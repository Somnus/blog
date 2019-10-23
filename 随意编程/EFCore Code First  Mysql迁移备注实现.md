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
            private static string b { get; set; } = "Framework.Website.Models";
            private static string c { get; set; } = GetPath();
            private static List<DbDescription> list { get; set; }

            public static string GetDescription(string table, string column = "")
            {
                if (list == null || list.Count() == 0)
                    list = GetDescription();

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
                            if (y != null && y.Count > 0)
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

            private static List<DbDescription> GetDescription()
            {
                var columnDscriptAllList = new List<DbDescription>();

                var folder = new DirectoryInfo(c);
                var files = folder.GetFiles("*.cs");
                foreach (var item in files)
                {
                    DbDescription dbDescription = new DbDescription();

                    var a = File.ReadAllText(item.FullName, Encoding.UTF8);
                    var b = GetSubString2(a, "{", "}");

                    var columnDscriptList = new List<DbDescription>();
                    var c = GetSubString2(b, "{", "}").Replace("\r", "");
                    string[] slipt = { "public" };
                    var d = c.Split(slipt, StringSplitOptions.None);
                    for (int i = 0; i < d.Length - 1; i++)
                    {
                        columnDscriptList.Add(new DbDescription());
                    }
                    for (int i = 0; i < columnDscriptList.Count; i++)
                    {
                        var x = d[i].IndexOf("<summary>");
                        var y = d[i].LastIndexOf("</summary>");

                        columnDscriptList[i].Name = d[i + 1].Split(' ')[2];
                        columnDscriptList[i].Description = (x > 0 && y > 0) ? d[i].Substring(x + 9, y - x - 10).Replace("\r", "").Replace(" ", "").Replace("\n", "").Replace(@"///", "") : "";
                    }
                    dbDescription.Column = columnDscriptList;

                    var e = GetSubString1(b, "{");
                    var f = GetSubString3(e, "class");
                    dbDescription.Name = f.Split(' ')[1];

                    var g = e.IndexOf("<summary>");
                    var h = e.LastIndexOf("</summary>");
                    dbDescription.Description = (g > 0 && h > 0) ? e.Substring(g + 9, h - g - 10).Replace("\r", "").Replace(" ", "").Replace("\n", "").Replace(@"///", "") : "";

                    columnDscriptAllList.Add(dbDescription);
                }
                return columnDscriptAllList;
            }
            private static string GetSubString1(string str, string end)
            {
                var b = str.IndexOf(end);
                var t = str.Substring(0, b - 1);
                return t;
            }
            private static string GetSubString2(string str, string start, string end)
            {
                var a = str.IndexOf(start);
                var b = str.LastIndexOf(end);
                var t = str.Substring(a + 1, b - a - 1);
                return t;
            }
            private static string GetSubString3(string str, string start)
            {
                var b = str.IndexOf(start);
                var t = str.Substring(b, str.Length - b);
                return t;
            }

            private static string GetPath()
            {
                string Delimiter = string.Empty;
                var path = AppDomain.CurrentDomain.BaseDirectory;
                var flag = path.IndexOf("/bin");
                if (flag > 0)
                    Delimiter = "/";//如果可以取到值，修改分割符
                path = path.Substring(0, path.IndexOf(Delimiter + "bin"));
                string parentPath = path.Substring(0, path.LastIndexOf(Delimiter));
                parentPath = parentPath.Substring(0, parentPath.LastIndexOf("\\"));
                return parentPath + "\\" + b;
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
