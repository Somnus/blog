## CREATE TABLE
    /*一般创建*/
    CREATE [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name
        (create_definition,...)
        [table_options]
        [partition_options]

    /*表结构及数据复制*/
    CREATE [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name
        [(create_definition,...)]
        [table_options]
        [partition_options]
        [IGNORE | REPLACE]
        [AS] query_expression

    /*表结构，索引结构复制*/
    CREATE [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name
        { LIKE old_tbl_name | (LIKE old_tbl_name) }
## INSERT INTO
    INSERT [LOW_PRIORITY | DELAYED | HIGH_PRIORITY] [IGNORE]
        [INTO] tbl_name
        [PARTITION (partition_name [, partition_name] ...)]
        [(col_name [, col_name] ...)]
        {VALUES | VALUE} (value_list) [, (value_list)] ...
        [ON DUPLICATE KEY UPDATE assignment_list]

    INSERT [LOW_PRIORITY | DELAYED | HIGH_PRIORITY] [IGNORE]
        [INTO] tbl_name
        [PARTITION (partition_name [, partition_name] ...)]
        SET assignment_list
        [ON DUPLICATE KEY UPDATE assignment_list]

    INSERT [LOW_PRIORITY | HIGH_PRIORITY] [IGNORE]
        [INTO] tbl_name
        [PARTITION (partition_name [, partition_name] ...)]
        [(col_name [, col_name] ...)]
        SELECT ...
        [ON DUPLICATE KEY UPDATE assignment_list]

    value:
        {expr | DEFAULT}

    value_list:
        value [, value] ...

    assignment:
        col_name = value

    assignment_list:
        assignment [, assignment] ...
