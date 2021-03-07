# mysql_sys.schema_unused_indexes-

SELECT 
    `t`.`OBJECT_SCHEMA` AS `object_schema`,
    `t`.`OBJECT_NAME` AS `object_name`,
    `t`.`INDEX_NAME` AS `index_name`
FROM
    (`performance_schema`.`table_io_waits_summary_by_index_usage` `t`
    JOIN `information_schema`.`STATISTICS` `s` ON (((CONVERT( `t`.`OBJECT_SCHEMA` USING UTF8) = `information_schema`.`s`.`TABLE_SCHEMA`)
        AND (CONVERT( `t`.`OBJECT_NAME` USING UTF8) = `information_schema`.`s`.`TABLE_NAME`)
        AND (CONVERT( `t`.`INDEX_NAME` USING UTF8) = `information_schema`.`s`.`INDEX_NAME`))))
WHERE
    ((`t`.`INDEX_NAME` IS NOT NULL)
        AND (`t`.`COUNT_STAR` = 0)
        AND (`t`.`OBJECT_SCHEMA` <> 'mysql')
        AND (`t`.`INDEX_NAME` <> 'PRIMARY')
        AND (`information_schema`.`s`.`NON_UNIQUE` = 1)
        AND (`information_schema`.`s`.`SEQ_IN_INDEX` = 1))
ORDER BY `t`.`OBJECT_SCHEMA` , `t`.`OBJECT_NAME`
