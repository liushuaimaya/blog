### SELECT 语句
  - DISTINCT: DISTINCTa, b FROM xxx, a, b 两列若不完全相同, 所有的行都会被检索出来
  - ||: 可以用来连接多个字段/字符串 形成新的计算字段
  - AS: 字段或值的替换名，最好使用
  - 聚集函数
    - SUM AVG MAX MIN COUNT，除 COUNT(*)外，均不计算 NULL
    - AVG 忽略值为 NULL 的列
    - COUNT(column), 对特定列计算，忽略 NULL 值
    - SUM SUM(item_price * quantity) AS total_price
### FROM
### WHERE 子句
  - 操作符: = < > >= <= <> != !> !< BETWEEN IS NULL IS NOT NULL
  - AND OR: 多个子句用 AND OR 连接, AND 优先级更高，可以用()组合
  - IN: IN 操作符用来指定条件范围，范围中的每个条件都可以进行匹配, IN 取一组由逗号分隔、括在圆括号中的合法值。
    - IN 可以包含其他 SELECT 语句，能更动态地建立 WHERE 子句
    - IN (SELECT ...)

  - NOT: 否定操作符，总是与其它操作符一起使用
  - LIKE: 只能用于字符串
    - %: 表示任何字符出现任意次数，可以匹配空字符，但是不能匹配 null
    - _: 匹配单个字符
    - []: 匹配[]内任意单个字符，各 DBMS 支持有限
### GROUP BY
  - GROUP BY a, b //按照 a, b 都一致进行分组
  - 一系列规则，见 P86
    - 如选择列（表达式），必须在group by 中使用，反之亦然
  - HAVING: 类似 WHERE，只不过 HAVING 过滤分组，WHERE 过滤单行
### ORDER BY 排序

  - DESC 只应用到直接位于其前面的列名
	- LIMIT 3,5 第 3 行开始 5 行, 且必须是最后一个子句

### 子查询
  - 使用子查询进行过滤
    - WHERE IN (SELECT ...)
    - 子查询只能查询单列
  - 使用子查询作为计算字段

### 联结表（JOIN）
  - 没有联结条件的表关系返回的结果为笛卡尔积
  - 要保证所有联结都有where子句，且保证where子句的正确性
    - 相等条件（WHERE A.xx = B.xx）默认等同于 内联结 INNER JOIN
  - INNER JOIN
    - FROM A INNER JOIN B ON ...
  - LEFT OUTER JOIN
    - 包括左表中没有关联行的行
  - FULL OUTER JOIN
    - 包括两个表中没有关联行的行(结果只出现一次)
  - 联结与聚集函数和GROUP BY一起使用
### 组合查询(UNION)
  - 多个SELECT语句之间用UNION连接
  - 各SELECT的列必须完全相同，即包含相同的列、表达式或聚集函数（顺序可以不同）
  - 各列类型必须兼容
  - UNION ALL: UNION过程会自动去掉重复的行，UNION ALL不会去掉重复行
  - ORDER BY 只能出现在最后一条SELECT语句之后，对所有结果进行排序

### INSERT:
  - 语法: 
    ```sql
      INSERT INTO
        TableName(header_01, header_02, header_03)
      VALUES
        (value1, value2, value3);
    ```
  - 空字段可以用NULL填充

### INSERT SELECT:
  - 语法:
    ```sql
      INSERT INTO
        Table1(header_01, header_02, header_03)
      SELECT header_01, header_02, header_03
      FROM Table2;
    ```
### SELECT INTO
  - 语法
  ```sql
    CREATE TABLE NewTable As
    SELECT * FROM OldTable;
  ```

### UPDATE & DELETE
  - UPDATE
    - 语法
    ```sql
      UPDATE Table1
      SET header_01 = 'foo',
          header_03 = 'foooo'
      WHERE header_02 = 'bar';
    ```
    - 删除某个字段可以 SET 该字段为 NULL

  - DELETE
    - 语法
    ```sql
      DELETE FROM Table1
      WHERE header_01 = 'foo';
    ```
    - 若要删除的某行数据中某个字段是其它表的外键，则会报错并中止

  - UPDATE/DELETE原则
    - 若果忘记了WHERE语句，则UPDATE和DELETE会更改/删除所有行
    - 保证每个表都有主键
    - UPDATE/DELETE前，应先用SELECT测试WHERE子句
    - 使用强制实施引用性完成的数据库，DBMS将不允许删除其数据与其它表相关联的行

### CREATE TABLE
  - 语法
  ```sql
    CREATE TABLE Table1(
      colomn_1 TEXT NOT NULL,
      colomn_2 INTEGER NOT NULL DEFAULT 1,
      colomn_3 NUMERIC NULL,
      ...
    );
  ```
  - NULL / NOT NULL
    - 不写NULL，默认为NULL
    - 设置为NOT NULL的列，更新、插入时不接受NULL值
    - 只有NOT NULL的列才可以设置为主键
  - DEFAULT
    - 设置为DEFAULT的列，插入时若不设置值，则有默认值，DEFAULT date('now'),设置默认为当前时间
### ALTER TABLE(更新表)
  - 对已有表修改复杂又不统一
  - 给已有表增加列是所有DBMS都支持的操作
    - ALTER TABLE Vendors 
      ADD vend_phone TEXT; 
  - 重命名表 RENAME / ALTER TABLE

###  使用视图
  - CREATE VIEW ProductCustomers AS SELECT ...