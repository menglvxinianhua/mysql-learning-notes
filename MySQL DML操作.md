## 1. DML语句简介DML 是数据操作语言，主要用于操作表中的数据。

| DML语句 |作用 |
| --- | --- |
| `INSERT` | 添加数据 |
| `UPDATE` | 更新数据 |
| `DELETE` | 删除数据 |

> `TRUNCATE`也可以清空表中的数据，但它属于 DDL 操作。

---

## 添加数据（INSERT）

### 1. 指定字段插入数据
```sql
INSERT INTO 表名(列1, 列2, 列3, ...)
VALUES (值1, 值2, 值3, ...);
```

使用指定字段插入数据时，可以只为部分字段提供数据，未指定的字段将使用默认值；如果没有默认值，则使用 `NULL`，前提是该字段允许为空。

### 2. 示例向 `departments` 表中添加一条数据：

- 部门名称为 `market`
- 工作地点编号为 `1`

```sql
INSERT INTO departments(department_name, location_id)
VALUES ('market',1);
```

### 3. 完整插入
```sql 
INSERT INTO 表名VALUES (值1, 值2, 值3, ...);
```

完整插入时，提供的值必须与表中字段的顺序和数量一致。

### 4. 自增长主键的占位方式如果表中的主键字段设置了 `AUTO_INCREMENT`，完整插入时可以使用以下方式为主键占位：

- 使用 `DEFAULT`
- 使用 `NULL`
- 使用 `0`

```sql
INSERT INTO departmentsVALUES (DEFAULT, 'development',2);
```

```sql
INSERT INTO departmentsVALUES (NULL, 'human',3);
```

```sql
INSERT INTO departmentsVALUES (0, 'teaching',4);
```

> 使用 `DEFAULT`、`NULL` 或 `0`作为自增长主键的占位值时，MySQL 会自动生成新的自增长值。

---

# 默认值处理（DEFAULT）

## 1. 默认值的作用在 MySQL 中，可以使用 `DEFAULT` 为字段指定默认值。

如果插入数据时没有指定该字段的值，MySQL 会自动将默认值添加到该字段中。

## 2. 创建表时指定默认值
```sql
CREATE TABLE 表名 (
 列名 数据类型 DEFAULT 默认值,
 ...
);
```

### 示例创建 `emp3` 表：

- `emp_id`：主键、自动增长- `name`：姓名- `address`：地址，默认值为 `'Unknown'`

```sql
CREATE TABLE emp3 (
 emp_id INT PRIMARY KEY AUTO_INCREMENT,
 name VARCHAR(10),
 address VARCHAR(50) DEFAULT 'Unknown'
);
```

## 3. 修改表时添加默认值
```sql
ALTER TABLE 表名ADD COLUMN 列名 数据类型 DEFAULT 默认值;
```

### 示例为 `emp3` 表添加 `job_id` 字段，并设置默认值为 `0`。

```sql
ALTER TABLE emp3ADD COLUMN job_id INT DEFAULT0;
```

## 4. 插入数据时使用默认值
### 方式一：省略使用默认值的字段
```sql
INSERT INTO emp3(name)
VALUES ('admin');
```

此时，未指定的字段会使用对应的默认值。

### 方式二：完整插入时使用 `DEFAULT` 占位
```sql
INSERT INTO emp3VALUES (DEFAULT, 'oldliu', DEFAULT, DEFAULT);
```

---

# 更新数据（UPDATE）

## 1. UPDATE语法
```sql
UPDATE 表名SET 列名1 = 值1,
 列名2 = 值2 WHERE 条件;
```

### 示例将 `emp3` 表中 `emp_id` 为 `1` 的员工地址修改为 `Beijing`。

```sql
UPDATE emp3SET address = 'Beijing'
WHERE emp_id =1;
```

## 2. 注意事项`UPDATE`语句中的 `WHERE` 用于指定更新条件。

如果省略 `WHERE` 子句，表中的所有记录都会被更新。

```sql
-- 会更新 emp3 表中的所有记录
UPDATE emp3SET address = 'Beijing';
```

---

# 删除数据（DELETE）

## 1. DELETE语法
```sql
DELETE FROM 表名 WHERE 条件;
```

### 示例删除 `emp3` 表中 `emp_id` 为 `1` 的员工信息。

```sql
DELETE FROM emp3 WHERE emp_id =1;
```

## 2. 注意事项在 `DELETE`语句中，如果没有指定 `WHERE` 条件，将会删除表中的所有数据。

```sql
-- 删除 emp3 表中的所有数据
DELETE FROM emp3;
```

> `DELETE` 删除数据后，表结构仍然存在。

---

# 清空表数据（TRUNCATE）

## 1. TRUNCATE语法
```sql
TRUNCATE TABLE 表名;
```

### 示例删除 `emp3` 表中的所有数据。

```sql
TRUNCATE TABLE emp3;
```

## 2. DELETE 与 TRUNCATE 的区别
| 对比项 | `DELETE` | `TRUNCATE` |
| --- | --- | --- |
| 类型 | DML | DDL |
| 删除范围 | 可以根据 `WHERE` 条件删除部分数据 |只能删除整张表的数据 |
| 是否支持 `WHERE` | 支持 | 不支持 |
| 是否保留表结构 | 保留 | 保留 |
| 自增长值 | 通常不会重置 | 通常会重置 |
| 删除方式 |逐行删除数据 | 快速清空整张表 |
|事务回滚 | 在事务支持的情况下通常可以回滚 | 通常会隐式提交，回滚行为受存储引擎和事务环境影响 |

### 3.语句对比
```sql
-- 删除满足条件的数据
DELETE FROM emp3WHERE emp_id =1;
```

```sql
-- 删除表中的全部数据
DELETE FROM emp3;
```

```sql
-- 快速清空整张表
TRUNCATE TABLE emp3;
```

> 使用 `TRUNCATE` 前应确认表中的数据确实可以全部清空，因为它不支持 `WHERE` 条件。

## 4.清空表时DELETE 与TRUNCATE区别

- truncate是整体删除（速度较快），delete是逐条删除（速度较慢）；
- truncate不写服务器log，delete写服务器log，也就是truncate效率比delete高的原因；
- truncate是会重置自增值，相当于自增列会被列为初始值，又重新从1开始记录，而不是接着原来的值。而delete删除以后，自增值仍然会继续累加。