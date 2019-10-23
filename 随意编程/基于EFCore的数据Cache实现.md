    using Microsoft.EntityFrameworkCore;
    using Microsoft.Extensions.Caching.Memory;
    using System;
    using System.Collections.Concurrent;
    using System.Collections.Generic;
    using System.ComponentModel.DataAnnotations;

    namespace FXY_NetCore_DbContext
    {
        public class DefaultContext
        {
            /// <summary>
            /// a queue to save the handle,if will be empted when call savechanges().
            /// </summary>
            private ConcurrentQueue<CacheEntity> CacheQueue { get; set; }

            /// <summary>
            /// databse context.
            /// </summary>
            private DbContext Context { get; set; }

            /// <summary>
            /// netocre inner cache.
            /// </summary>
            private IMemoryCache Cache { get; set; }

            /// <summary>
            /// the time of cache's life cycle
            /// </summary>
            private int ExpirtTime { get; set; } = 10;

            /// <summary>
            /// entity context.
            /// </summary>
            /// <param name="context">database context</param>
            /// <param name="cache">cache</param>
            /// <param name="expirtTime">expirt time,default 60 sencond.</param>
            public DefaultContext(DbContext context, IMemoryCache cache, int expirtTime = 10)
            {
                CacheQueue = new ConcurrentQueue<CacheEntity>();
                Context = context;
                Cache = cache;
                ExpirtTime = expirtTime;
            }

            /// <summary>
            /// add entity to database context and add the handle to the queue.
            /// <para>it will be excuted when call Savechange().</para>
            /// </summary>
            /// <typeparam name="TEntity"></typeparam>
            /// <param name="entity"></param>
            public void Add<TEntity>(TEntity entity)
                where TEntity : class, new()
            {
                Context.Add(entity);
            }

            /// <summary>
            /// add entity list to database context and add the handle to the queue.
            /// <para>it will be excuted when call Savechange().</para>
            /// </summary>
            /// <typeparam name="TEntity"></typeparam>
            /// <param name="entities"></param>
            public void AddRange<TEntity>(params TEntity[] entities)
                where TEntity : class, new()
            {
                foreach (var item in entities)
                    Add(item);
            }

            /// <summary>
            /// remove entity to database context and add the handle to the queue.
            /// <para>it will be excuted when call Savechange().</para>
            /// </summary>
            /// <typeparam name="TEntity"></typeparam>
            /// <param name="entity"></param>
            public void Remove<TEntity>(TEntity entity)
                where TEntity : class, new()
            {
                bool reesult = Enqueue(entity);
                if (reesult)
                    Context.Remove(entity);
            }

            /// <summary>
            /// remove entity list to database context and add the handle to the queue.
            /// <para>it will be excuted when call Savechange().</para>
            /// </summary>
            /// <typeparam name="TEntity"></typeparam>
            /// <param name="entities"></param>
            public void RemoveRange<TEntity>(params TEntity[] entities)
                where TEntity : class, new()
            {
                foreach (var item in entities)
                    Remove(item);
            }

            /// <summary>
            /// update entity to database context and add the handle to the queue.
            /// <para>it will be excuted when call Savechange().</para>
            /// </summary>
            /// <typeparam name="TEntity"></typeparam>
            /// <param name="entity"></param>
            public void Update<TEntity>(TEntity entity)
                where TEntity : class, new()
            {
                bool reesult = Enqueue(entity);
                if (reesult)
                    Context.Update(entity);
            }

            /// <summary>
            /// update entity list to database context and add the handle to the queue.
            /// <para>it will be excuted when call Savechange().</para>
            /// </summary>
            /// <typeparam name="TEntity"></typeparam>
            /// <param name="entities"></param>
            public void UpdateRange<TEntity>(params TEntity[] entities)
                where TEntity : class, new()
            {
                foreach (var item in entities)
                    Update(item);
            }

            /// <summary>
            /// attach entity to database context add the handle to the queue.
            /// <para>it will be excuted when call Savechange().</para>
            /// </summary>
            /// <typeparam name="TEntity"></typeparam>
            /// <param name="entities"></param>
            public void Attach<TEntity>(TEntity entity)
                where TEntity : class, new()
            {
                bool reesult = Enqueue(entity);
                if (reesult)
                    Context.Attach(entity);
            }

            /// <summary>
            /// attach entity list to database context add the handle to the queue.
            /// <para>it will be excuted when call Savechange().</para>
            /// </summary>
            /// <typeparam name="TEntity"></typeparam>
            /// <param name="entities"></param>
            public void AttachRange<TEntity>(params TEntity[] entities)
                where TEntity : class, new()
            {
                foreach (var item in entities)
                    Attach(item);
            }


            /// <summary>
            /// update cache and database.
            /// <para>update cache at first,if update cache is failed,return false,else commit the changes to database.</para>
            /// </summary>
            /// <returns></returns>
            public bool SaveChanges()
            {
                bool result = Dequeue();
                if (result)
                    result = Context.SaveChanges() > 0;
                return result;
            }

            /// <summary>
            /// single query.
            /// <para>find it in the cache first,return if find it,otherwise search it in database by efcore.</para>
            /// </summary>
            /// <typeparam name="TEntity"></typeparam>
            /// <param name="key"></param>
            /// <returns></returns>
            public TEntity Get<TEntity>(string key)
                where TEntity : class, new()
            {
                var result = GetCache<TEntity>(key);
                if (result == null)
                {
                    result = Context.Find<TEntity>(key);
                    var cacheEntity = GetCacheEntity(result);
                    AddCache(cacheEntity);
                }
                else
                {
                    var cacheEntity = GetCacheEntity(result);
                    UpdateCache(cacheEntity);
                }
                return result;
            }

            /// <summary>
            /// collection query.
            /// <para>do not allow fuzzy query</para>
            /// </summary>
            /// <typeparam name="TEntity"></typeparam>
            /// <param name="keys"></param>
            /// <returns></returns>
            public List<TEntity> Get<TEntity>(string[] keys)
            where TEntity : class, new()
            {
                var result = new List<TEntity>();
                foreach (var item in keys)
                    result.Add(Get<TEntity>(item));
                return result;
            }

            #region private

            #region cache queue

            /// <summary>
            /// add the handle to the context queue.
            /// </summary>
            /// <param name="model"></param>
            /// <param name="handleEnum"></param>
            private bool Enqueue(object model)
            {
                CacheEntity entity = GetCacheEntity(model);
                if (CacheQueue.TryPeek(out CacheEntity cacheEntity1))
                    return false;
                else
                {
                    CacheQueue.Enqueue(entity);
                    return CacheQueue.TryPeek(out CacheEntity cacheEntity2);
                }
            }

            /// <summary>
            /// update the changes to cache,and remove it from the cache queue.
            /// <para>include add,delete and update.</para>
            /// </summary>
            /// <returns></returns>
            private bool Dequeue()
            {
                bool check = false;
                bool dequeue = CacheQueue.TryDequeue(out CacheEntity cacheEntity);
                if (dequeue)
                    check = RemoveCache(cacheEntity);
                else
                    check = false;
                return check;
            }

            #endregion

            #region cache core

            /// <summary>
            /// add cache
            /// </summary>
            /// <param name="cacheEntity"></param>
            /// <returns></returns>
            private bool AddCache(CacheEntity cacheEntity)
            {
                bool check;
                Cache.Set(cacheEntity.key, cacheEntity.Value, new TimeSpan(0, 0, ExpirtTime));
                check = Cache.Get(cacheEntity.key) != null;
                return check;
            }

            /// <summary>
            /// remove cache.
            /// </summary>
            /// <param name="cacheEntity"></param>
            /// <returns></returns>
            private bool RemoveCache(CacheEntity cacheEntity)
            {
                bool check;
                Cache.Remove(cacheEntity.key);
                check = Cache.Get(cacheEntity.key) == null;
                return check;
            }

            /// <summary>
            /// update cache.
            /// </summary>
            /// <param name="cacheEntity"></param>
            /// <returns></returns>
            private bool UpdateCache(CacheEntity cacheEntity)
            {
                bool check = RemoveCache(cacheEntity);
                if (check)
                    check = AddCache(cacheEntity);
                return check;
            }

            /// <summary>
            /// get cache by key.
            /// </summary>
            /// <typeparam name="TEntity"></typeparam>
            /// <returns></returns>
            private TEntity GetCache<TEntity>(string key)
                where TEntity : class, new()
            {
                Cache.TryGetValue(key, out object value);
                return value as TEntity;
            }

            #endregion

            #region other

            /// <summary>
            /// get private cache entity.
            /// </summary>
            /// <param name="model"></param>
            /// <param name="handleEnum"></param>
            /// <returns></returns>
            private CacheEntity GetCacheEntity(object model)
            {
                var key = GetModelKey(model);
                var entity = new CacheEntity()
                {
                    Value = model,
                    key = key
                };
                return entity;
            }

            /// <summary>
            /// get the key of a entity.
            /// </summary>
            /// <param name="model"></param>
            /// <returns></returns>
            private string GetModelKey(object model)
            {
                string key = "";
                var type = model.GetType().GetProperties();
                foreach (var item in type)
                {
                    if (item.GetCustomAttributes(typeof(KeyAttribute), true).Length > 0)
                    {
                        key = item.GetValue(model).ToString();
                        break;
                    }
                }
                return key;
            }

            #endregion
            #endregion
        }

        /// <summary>
        /// a entity to handle cache
        /// </summary>
        public sealed class CacheEntity
        {
            /// <summary>
            /// cache key
            /// </summary>
            [Key]
            public string key { get; set; }

            /// <summary>
            /// cache value
            /// </summary>
            public object Value { get; set; }
        }
    }
