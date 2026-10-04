## 一、SELECT语句的功能

###### SELECT语句用于从数据库中查询并返回数据。

使用一个 SELECT语句，可以完成以下操作：

- **列选择**：选择数据表中需要查询的列。
- **行选择**：选择数据表中需要查询的行。
- **连接**：将多个数据表中的数据集合起来进行查询。

## 二、SELECT语句的基本语法
```sql
SELECT * | [DISTINCT] column | expression [alias], ...
FROM table;
```

### 语法说明
|语法元素 |说明 |
| --- | --- |
| `SELECT` | 确定需要查询的列 |
| `*` |选择表中的所有列 |
| `DISTINCT` | 去除查询结果中的重复行 |
| `column` | 指定需要查询的列名 |
| `expression` | 指定表达式 |
| `alias` | 为列或表达式指定别名 |
| `FROM table` | 确定数据来源表 |

### SELECT 子句
###### SELECT 子句用于确定需要显示的一列或多列数据。

###### FROM 子句FROM 子句用于确定查询数据来源于哪一张表。

## 三、准备测试数据

###### 如果数据库中没有测试数据，可以将课程提供的 `data.sql` 文件导入 Navicat，然后执行 SQL 文件中的数据创建和插入语句。

基本操作步骤：

1. 打开 Navicat，并连接到 MySQL 数据库。
2. 选择需要导入数据的数据库。
3. 右键单击数据库，选择运行 SQL 文件。
4. 选择课程提供的 `data.sql` 文件。
5. 执行 SQL 文件。
6. 刷新数据库对象，查看导入的数据表。

# 查询中的列选择
## 一、查询所有列使用星号 `*` 可以查询数据表中的所有列。

```sql
SELECT *
FROM departments;
```

也可以写成一行：

```sql
SELECT * FROM departments;
```

### 说明 `*` 表示数据表中的所有列。
- `departments` 是需要查询的数据表。
- 查询结果会显示 `departments` 表中的所有字段和数据。

## 二、查询指定的列可以在 SELECT 子句中直接写出需要查询的列名。

```sql
SELECT department_id, location_idFROM departments;
```

也可以写成一行：

```sql
SELECT department_id, location_id FROM departments;
```

### 说明 SELECT 后面写需要显示的列名。
- 多个列名之间使用英文逗号 `,` 分隔。
- 列名的书写顺序决定查询结果中列的显示顺序。

## 三、查询列并使用列别名列别名用于为查询结果中的列重新命名，使结果更容易理解。

```sql
SELECT department_name AS nameFROM departments;
```

也可以省略 `AS`：

```sql
SELECT department_name nameFROM departments;
```

### 列别名的基本语法
```sql
SELECT column_name AS aliasFROM table_name;
```

其中：

- `column_name`：原始列名。
- `alias`：查询结果中显示的新名称。
- `AS`：用于指定列别名，可以省略。

# 查询中的算术表达式
## 一、算术运算符算术表达式可以对数值型列进行计算，也可以由列名、常量和算术运算符组成。

| 运算符 |说明 |
| --- | --- |
| `+` | 加法 |
| `-` | 减法 |
| `*` |乘法 |
| `/` | 除法 |

### 算术表达式的基本组成
```text
列名 + 常量列名 - 常量列名 * 常量列名 / 常量
```

算术表达式可以用于：

- 计算工资、奖金等数据。
- 调整查询结果的显示方式。
- 根据原始数据生成新的计算列。
- 对多个数值进行组合计算。

## 二、使用算术运算符例如，查询员工姓名、工资以及工资增加300 后的结果：

```sql
SELECT last_name, salary, salary +300FROM employees;
```

查询结果会包含以下信息：

- 员工姓名 `last_name`
- 原始工资 `salary`
- 工资增加300 后的工资## 三、运算符的优先级MySQL 中算术运算符的优先级如下：

1. 乘法和除法优先于加法和减法。
2. 乘法和除法之间优先级相同，按照从左到右的顺序计算。
3. 加法和减法之间优先级相同，按照从左到右的顺序计算。
4. 括号中的表达式优先计算。

### 优先级顺序
```text
* /
+ -
```

### 示例一：先乘法，后加法
```sql
SELECT employee_id, last_name,12 * salary +100FROM employees;
```

实际计算顺序：

```text(12 * salary) +100```

该表达式表示先将月工资乘以12，再加上100。

### 示例二：使用括号改变计算顺序
```sql
SELECT employee_id, last_name,12 * (salary +100)
FROM employees;
```

实际计算顺序：

```text
12 * (salary +100)
```

该表达式表示先将工资增加100，再将结果乘以12。

### 两个表达式的区别
| 表达式 |计算顺序 | 含义 |
| --- | --- | --- |
| `12 * salary +100` | `(12 * salary) +100` | 年薪增加100 |
| `12 * (salary +100)` | `12 * (salary +100)` | 月薪增加100 后再计算年薪 |

# NULL 值
## 一、NULL 的含义`NULL` 表示一个值未被分配、未知或者不适用。

`NULL`不是以下内容：

- 不是数字 `0`。
- 不是空字符串 `''`。
- 不是空格。
- 不是一个已经确定的具体值。

### NULL 与0、空字符串的区别
| 内容 | 含义 |
| --- | --- |
| `NULL` | 未知、未分配或不适用 |
| `0` | 数值零 |
| `''` | 空字符串 |
| 空格 | 一个或多个空格字符 |

## 二、查询包含 NULL 的列
```sql
SELECT last_name, job_id, salary, commission_pctFROM employees;
```

其中，`commission_pct` 列可能包含 `NULL` 值。

## 三、NULL 对算术表达式的影响如果算术表达式中的任意一个操作数为 `NULL`，那么整个算术表达式的结果也是 `NULL`。

```sql
SELECT last_name,12 * salary * commission_pctFROM employees;
```

如果某位员工的 `commission_pct` 为 `NULL`，则：

```text12 * salary * NULL = NULL```

因此，查询结果中的计算列会显示为空。

### 注意事项进行算术计算时，如果参与计算的列可能包含 `NULL`，需要特别注意查询结果可能为空。

# MySQL 中的别名
## 一、列别名列别名可以为查询结果中的列指定一个更容易理解的名称。

### 基本语法
```sql
SELECT column_name AS aliasFROM table_nameWHERE condition;
```

例如，为 `last_name` 指定别名 `name`：

```sql
SELECT last_name AS nameFROM employees;
```

也可以省略 `AS`：

```sql
SELECT last_name nameFROM employees;
```

### 使用别名的作用
-  修改查询结果中列的显示名称。
- 让查询结果更容易阅读。
- 为算术表达式生成有意义的列名。
- 简化较长或复杂的列名。

## 二、为算术表达式指定别名
```sql
SELECT last_name, salary, salary +300 AS new_salaryFROM employees;
```

其中：

- `salary +300` 是算术表达式。
- `new_salary` 是该表达式的列别名。

也可以省略 `AS`：

```sql
SELECT last_name, salary, salary +300 new_salaryFROM employees;
```

## 三、表别名表别名用于为数据表指定一个临时名称，通常用于简化 SQL语句或区分多个数据表中的同名列。

### 基本语法
```sql
SELECT table_alias.column_nameFROM table_name table_aliasWHERE condition;
```

例如：

```sql
SELECT emp.last_nameFROM employees emp;
```

其中：

- `employees` 是原始表名。
- `emp` 是表别名。
- `emp.last_name` 表示查询 `employees` 表中的 `last_name` 列。

### 使用 AS 指定表别名表别名也可以使用 `AS`：

```sql
SELECT emp.last_nameFROM employees AS emp;
```

### 表别名的作用- 简化较长的表名。
- 在多表查询中区分不同数据表。
- 明确列所属的数据表。
- 避免多个表存在同名列时产生歧义。

# SELECT 查询示例汇总
## 查询数据表中的所有列
```sql
SELECT *
FROM departments;
```

## 查询指定列
```sql
SELECT department_id, location_idFROM departments;
```

## 使用列别名
```sql
SELECT department_name AS nameFROM departments;
```

## 使用算术表达式
```sql
SELECT last_name, salary, salary +300FROM employees;
```

## 使用括号改变运算顺序
```sql
SELECT employee_id, last_name,12 * (salary +100)
FROM employees;
```

## 查询可能包含 NULL 的列
```sql
SELECT last_name, job_id, salary, commission_pctFROM employees;
```

## 为算术表达式指定别名
```sql
SELECT last_name, salary, salary +300 AS new_salaryFROM employees;
```

## 使用表别名
```sql
SELECT emp.last_nameFROM employees emp;
```


# 去重、条件筛选与排序
## 去除重复行：DISTINCT`SELECT` 查询默认会返回所有符合条件的行，其中可能包含重复行。

使用 `DISTINCT` 可以去除查询结果中的重复行。

```sql
SELECT DISTINCT 列名FROM 表名;
```

示例：查询 `employees` 表中不重复的部门 ID。

```sql
SELECT DISTINCT department_idFROM employees;
```

> `DISTINCT` 会根据 `SELECT` 后指定列的组合判断结果是否重复。

distinct 对一整条的结果集进行相同的重复的剔除，而不是对查询到的某一个列进行剔除。比如：

|数据A | 数据B |
| --- | --- |
| 30 | 90 |
| 30 | 60 |
虽然数据A列的内容是重复了但是数据A和数据B组合不算重复就没办法剔除单行的重复

## 行选择：WHERE`WHERE` 子句用于过滤查询返回的行。

`WHERE` 子句跟在 `FROM` 子句之后；当条件结果为 `TRUE` 时，该行会被返回。

```sql
SELECT * | 列名FROM 表名WHERE 条件;
```

示例：查询指定部门的部门名称和地点 ID。

```sql
SELECT department_name, location_idFROM departmentsWHERE department_id =4;
```
