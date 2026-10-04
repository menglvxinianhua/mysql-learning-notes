## 一、SQL语言简介SQL（Structured Query Language，结构化查询语言）是一种数据库查询和程序设计语言，主要用于存取数据、查询数据、更新数据以及管理关系型数据库系统。

## 二、SQL 能做什么SQL 可以用于：

- 面向数据库执行查询- 向数据库中插入新的记录- 更新数据库中的数据- 从数据库中删除记录- 创建数据库中的新表- 在数据库中创建存储过程- 在数据库中创建视图- 设置表、存储过程和视图的访问权限
##  三、SQL 标准SQL 标准主要由 ANSI 和 ISO 等组织制定和维护。

## 四、SQL语言分类SQL语言按照功能可以分为以下几类：

| 分类  | 英文全称                                | 主要功能             | 常用关键字                                       |
| --- | ----------------------------------- | ---------------- | ------------------------------------------- |
| DQL | Data Query Language，数据查询语言          | 查询数据库中的数据        | `SELECT`、`FROM`、`WHERE`、`ORDER BY`、`HAVING` |
| DML | Data Manipulation Language，数据操纵语言   | 对表中的数据进行增加、修改和删除 | `INSERT`、`UPDATE`、`DELETE`                  |
| DDL | Data Definition Language，数据定义语言     | 定义和管理数据库对象       | `CREATE`、`ALTER`、`DROP`                     |
| DCL | Data Control Language，数据控制语言        | 管理数据库用户权限        | `GRANT`、`REVOKE`                            |
| TCL | Transaction Control Language，事务控制语言 | 管理事务             | `COMMIT`、`ROLLBACK`、`SAVEPOINT`             |

### 1. DQL：数据查询语言DQL主要用于从数据库表中查询和获取数据。

常用关键字：

- `SELECT`：指定要查询的字段- `FROM`：指定数据来源的表- `WHERE`：指定查询条件- `ORDER BY`：对查询结果进行排序- `HAVING`：对分组后的结果进行筛选
### 2. DML：数据操纵语言DML主要针对表中的数据进行操作。

常用语句：

- `INSERT`：添加数据- `UPDATE`：更新数据- `DELETE`：删除数据###3. DDL：数据定义语言DDL主要用于定义和管理数据库对象。

数据库对象包括：

- 数据库（Database）
- 数据表（Table）
- 索引（Index）
- 视图（View）
- 存储过程（Procedure）
- 触发器（Trigger）

常用语句：

- `CREATE`：创建数据库对象- `ALTER`：修改数据库对象- `DROP`：删除数据库对象###4. DCL：数据控制语言DCL主要用于管理用户权限。

- `GRANT`：授予用户某种权限- `REVOKE`：收回用户已经获得的权限###5. TCL：事务控制语言TCL主要用于管理事务。

- `COMMIT`：提交事务- `ROLLBACK`：回滚事务- `SAVEPOINT`：设置事务保存点## 五、DML 与 DDL 的区别> [!NOTE]
> DML 针对的是表中的数据；DDL 针对的是数据库对象。

| 对比项目 | DML | DDL |
|---|---|---|
| 操作对象 | 表中的数据 | 数据库对象 |
|主要功能 | 增加、修改、删除数据 | 创建、修改、删除数据库对象 |
| 常用关键字 | `INSERT`、`UPDATE`、`DELETE` | `CREATE`、`ALTER`、`DROP` |
| 示例 | 添加一条员工记录 | 创建一张员工表 |

## 六、SQL语言语法规则- SQL语句不区分大小写。
- SQL关键字建议使用大写。
- SQL语句可以写成一行，也可以分成多行书写。
- SQL语句通常以分号 `;`结尾。

示例：

```sql
SELECT*
FROM employeesWHERE employee_id =1;
```

上面的 SQL语句也可以写成一行：

```sql
 SELECT * FROM employees WHERE employee_id =1;
```
