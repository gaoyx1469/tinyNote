#大表数据UPDATE
1. 背景
   * 数据库ORACLE，版本11.2.0.3.0
   * 主表TABLEA 数据量百万级 字段26列
   * 更新数据表TABLEB 数据量十万级 字段2列
   * 根据TABLEB中第一个字段，在TABLEA中匹配相同的字段（主键），将TABLEA中某列更新为TABLEB中第二列

2. 实现
    * 使用游标
    ```
   BEGIN 
        FOR cur IN (
            SELECT a.aid aid,b.bcol2 col2 FROM TABLEA a,TABLEB b
            WHERE a.aid = b.bid and a.coln != b.col2
        )LOOP
        UPDATE TABLEA c set c.coln = cur.col2 WHERE c.aid = cur.aid;
        END LOOP;
   END;
   COMMIT;
   ```