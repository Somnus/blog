重写Mysql下sql脚本生成器

    using Framework.NetCore.Extensions;
    using Framework.NetCore.Models;
    using Microsoft.EntityFrameworkCore.Metadata;
    using Microsoft.EntityFrameworkCore.Migrations;
    using Microsoft.EntityFrameworkCore.Migrations.Operations;
    using Pomelo.EntityFrameworkCore.MySql.Infrastructure.Internal;
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using System.Reflection;
    using System.Text;

    namespace Framework.NetCore.EfDbContext
    {

        public class MyMigrationsSqlGenerator : MySqlMigrationsSqlGenerator
        {
            public MyMigrationsSqlGenerator(
                MigrationsSqlGeneratorDependencies dependencies,
                IMigrationsAnnotationProvider migrationsAnnotations,
                IMySqlOptions mySqlOptions)
                : base(dependencies, migrationsAnnotations, mySqlOptions)
            {
            }

            protected override void Generate(MigrationOperation operation, IModel model, MigrationCommandListBuilder builder)
            {
                base.Generate(operation, model, builder);

                if (operation is CreateTableOperation || operation is AlterTableOperation)
                    CreateTableComment(operation, model, builder);

                if (operation is AddColumnOperation || operation is AlterColumnOperation)
                    CreateColumnComment(operation, model, builder);
            }

            /// <summary>
            /// Create table comment.
            /// </summary>
            /// <param name="operation"></param>
            /// <param name="builder"></param>
            private void CreateTableComment(MigrationOperation operation, IModel model, MigrationCommandListBuilder builder)
            {
                string tableName = string.Empty;
                string description = string.Empty;
                if (operation is AlterTableOperation)
                {
                    var t = operation as AlterColumnOperation;
                    tableName = (operation as AlterTableOperation).Name;
                }

                if (operation is CreateTableOperation)
                {
                    var t = operation as CreateTableOperation;
                    var addColumnsOperation = t.Columns;
                    tableName = (operation as CreateTableOperation).Name;

                    foreach (var item in addColumnsOperation)
                    {
                        CreateColumnComment(item, model, builder);
                    }
                }
                description = DbDescriptionHelper.GetDescription(tableName);

                if (tableName.IsNullOrWhiteSpace())
                    throw new Exception("Create table comment error.");

                var sqlHelper = Dependencies.SqlGenerationHelper;
                builder
                .Append("ALTER TABLE ")
                .Append(sqlHelper.DelimitIdentifier(tableName).ToLower())
                .Append(" COMMENT ")
                .Append("'")
                .Append(description)
                .Append("'")
                .AppendLine(sqlHelper.StatementTerminator)
                .EndCommand();

            }

            /// <summary>
            /// Create column comment.
            /// </summary>
            /// <param name="operation"></param>
            /// <param name="builder"></param>
            private void CreateColumnComment(MigrationOperation operation, IModel model, MigrationCommandListBuilder builder)
            {
                //alter table a1log modify column UUID VARCHAR(26) comment '修改后的字段注释';
                string tableName = string.Empty;
                string columnName = string.Empty;
                string columnType = string.Empty;
                string description = string.Empty;
                if (operation is AlterColumnOperation)
                {
                    var t = (operation as AlterColumnOperation);
                    tableName = t.Table;
                    columnName = t.Name;
                    columnType = GetColumnType(t.Schema, t.Table, t.Name, t.ClrType, t.IsUnicode, t.MaxLength, t.IsFixedLength, t.IsRowVersion, model);
                }

                if (operation is AddColumnOperation)
                {
                    var t = (operation as AddColumnOperation);
                    tableName = t.Table;
                    columnName = t.Name;
                    columnType = GetColumnType(t.Schema, t.Table, t.Name, t.ClrType, t.IsUnicode, t.MaxLength, t.IsFixedLength, t.IsRowVersion, model);
                    description = DbDescriptionHelper.GetDescription(tableName, columnName);
                }

                if (columnName.IsNullOrWhiteSpace() || tableName.IsNullOrWhiteSpace() || columnType.IsNullOrWhiteSpace())
                    throw new Exception("Create columnt comment error." + columnName + "/" + tableName + "/" + columnType);

                var sqlHelper = Dependencies.SqlGenerationHelper;
                builder
                .Append("ALTER TABLE ")
                .Append(sqlHelper.DelimitIdentifier(tableName).ToLower())
                .Append(" MODIFY COLUMN ")
                .Append(columnName)
                .Append(" ")
                .Append(columnType)
                .Append(" COMMENT ")
                .Append("'")
                .Append(description)
                .Append("'")
                .AppendLine(sqlHelper.StatementTerminator)
                .EndCommand();

            }
        }


        public class DbDescriptionHelper
        {
            public static string b { get; set; } = "Framework.Website.Models";
            public static string c { get; set; } = @"C:\Users\fxy75\Downloads\pos3.0\Framework.Website.Models";

            public static List<DbDescription> list { get; set; }


            public static string GetDescription(string table, string column = "")
            {
                if (list == null || list.Count() == 0)
                {
                    list = GetDescription();
                }

                if (!string.IsNullOrWhiteSpace(table))
                {
                    if (string.IsNullOrWhiteSpace(column))
                    {
                        var x = list.FirstOrDefault(p => p.Name == table);
                        if (x != null)
                            return x.Description;
                        return string.Empty;
                    }
                    else
                    {
                        var x = list.FirstOrDefault(p => p.Name == table);
                        if (x != null)
                        {
                            var y = x.Column;
                            if (y.IsNotNull())
                            {
                                var z = y.FirstOrDefault(p => p.Name == column);
                                if (z != null)
                                    return z.Description;
                            }
                        }
                        return string.Empty;
                    }
                }
                else
                    return string.Empty;
            }

            public static List<DbDescription> GetDescription()
            {
                var d = new List<DbDescription>();

                var e = Assembly.Load(b);
                var f = e?.GetTypes();
                var g = f?
                    .Where(t => t.IsClass
                    && !t.IsGenericType
                    && !t.IsAbstract
                    && t.GetInterfaces().Any(m => m.GetGenericTypeDefinition() == typeof(IBaseModel<>))
                    ).ToList();

                foreach (var h in g)
                {
                    var i = new DbDescription();

                    var j = c + "\\" + h.Name + ".cs";
                    var k = File.ReadAllText(j);
                    k = k.Substring(k.IndexOf("{") + 1, k.LastIndexOf("}") - k.IndexOf("{") - 1).Replace("\n", "");

                    var l = k.Substring(k.IndexOf(" {") + 2, k.LastIndexOf(" }") - k.IndexOf(" {") - 1).Replace("\n", "");
                    string[] slipt = { "}\r" };
                    var m = l.Split(slipt, StringSplitOptions.None).ToList();

                    var n = new List<DbDescription>();
                    foreach (var o in m)
                    {
                        var p = o.Replace("///", "");

                        var q = p.IndexOf("<summary>");
                        var r = p.LastIndexOf("</summary>");

                        var s = p.IndexOf("public");
                        var t = p.IndexOf("{");

                        var u = (q > 0 && r > 0) ? p.Substring(q + 9, r - q - 10).Replace("\r", "").Replace(" ", "") : "";
                        var v = (s > 0 && t > 0) ? p.Substring(s, t - s).Split(' ')[2] : "";

                        n.Add(new DbDescription()
                        {
                            Description = u,
                            Name = v
                        });
                    }

                    var w = k.Substring(0, k.IndexOf("{\r") - 1);
                    w = w.Replace("///", "");

                    var x = w.IndexOf("<summary>");
                    var y = w.LastIndexOf("</summary>");
                    var z = (x > 0 && y > 0) ? w.Substring(x + 9, y - x - 10).Replace("\r", "").Replace(" ", "") : "";

                    d.Add(new DbDescription()
                    {
                        Name = h.Name,
                        Description = z,
                        Column = n
                    });
                }
                return d;
            }
        }

        public class DbDescription
        {
            public string Name { get; set; }
            public string Description { get; set; }
            public List<DbDescription> Column { get; set; }
        }
    }

EFCore DbContext中替换IMigrationsSqlGenerator

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            {
                optionsBuilder
                    .UseMySql(_option.ConnectionString)
                    .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
                
                base.OnConfiguring(optionsBuilder);
            }