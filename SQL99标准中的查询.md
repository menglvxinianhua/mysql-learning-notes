## SQL99 标准中的查询
MySQL5.7 支持 SQL99 标准语法。

SQL99 中常用的连接类型包括：

- 交叉连接：`CROSS JOIN`
- 自然连接：`NATURAL JOIN`
- 内连接：`INNER JOIN`
- 外连接：`LEFT OUTER JOIN`、`RIGHT OUTER JOIN`
- 全外连接：`FULL OUTER JOIN`（MySQL 不直接支持）

---

#交叉连接 CROSS JOIN## 概念`CROSS JOIN` 子句用于生成两个表的笛卡尔积。

笛卡尔积表示：一个表中的每一行，都会与另一个表中的每一行进行组合。

两个表之间的连接条件始终为真，因此不需要指定 `ON` 条件。

## 语法
```sql
SELECT 字段列表FROM 表1CROSS JOIN 表2;
```

## 示例查询员工表与部门表的所有组合：

```sql
SELECT last_name, department_nameFROM employeesCROSS JOIN departments;
```

查询两个表的全部字段及其所有组合：

```sql
SELECT *
FROM employeesCROSS JOIN departments;
```

>交叉连接返回的数据量 = 表1行数 × 表2行数。> 因此，在数据量较大时应谨慎使用。

---

# 自然连接 NATURAL JOIN
## 概念`NATURAL JOIN` 用于基于两个表中**所有同名字段**自动建立连接。

特点：

- 自动查找两个表中名称相同的列。
- 所有同名列都会作为连接条件。
- 若两个表的同名字段数据类型不兼容，会导致错误。
- 使用时不需要编写 `ON`连接条件。

## 语法
```sql
SELECT 字段列表FROM 表1NATURAL JOIN 表2;
```

## 示例`departments` 表与 `locations` 表中都包含 `location_id` 字段，因此可以通过自然连接查询部门及其所在城市：

```sql
SELECT department_id, department_name, location_id, cityFROM departmentsNATURAL JOIN locations;
```

## 等价写法上述自然连接等价于使用旧式连接语法并在 `WHERE` 中指定条件：

```sql
SELECT d.department_id,
 d.department_name,
 d.location_id,
 l.cityFROM departments d, locations lWHERE d.location_id = l.location_id;
```

## 示例：员工与部门自然连接
```sql
SELECT first_name, department_nameFROM employeesNATURAL JOIN departments;
```

## 注意事项
- 若两张表有多个同名字段，`NATURAL JOIN` 会把这些字段全部用作连接条件。
- 由于连接条件由系统自动决定，实际开发中通常更推荐使用 `JOIN ... ON ...`，以避免因字段重名而产生非预期结果。

---

# 内连接 INNER JOIN
## 概念`INNER JOIN` 返回两个表中**满足连接条件的记录**。

可以将其理解为两个表数据的交集：

- 只保留能够匹配上的记录。
- 无法匹配的记录不会出现在查询结果中。

## 语法
```sql
SELECT 字段列表FROM 表1INNER JOIN 表2ON连接条件;
```

`INNER`关键字可以省略，以下写法效果相同：

```sql
SELECT 字段列表FROM 表1JOIN 表2ON连接条件;
```

## 使用 ON 子句连接两张表查询员工及其所属部门的信息：

```sql
SELECT e.employee_id,
 e.last_name,
 e.department_id,
 d.department_id,
 d.location_idFROM employees eJOIN departments dON e.department_id = d.department_id;
```

## 使用 ON 子句连接三张表查询员工编号、所在城市和部门名称：

```sql
SELECT e.employee_id,
 l.city,
 d.department_nameFROM employees eJOIN departments dON e.department_id = d.department_idJOIN locations lON d.location_id = l.location_id;
```

## 示例：筛选指定员工查询姓氏为 `Fox` 的员工及其部门名称：

```sql
SELECT e.employee_id,
 e.salary,
 d.department_nameFROM employees eINNER JOIN departments dON e.department_id = d.department_idWHERE e.last_name = 'Fox';
```

---

# 外连接 OUTER JOIN## 概念在 SQL:1999 标准中，普通连接通常只返回符合连接条件的记录，也就是内连接的结果。

外连接除了返回匹配成功的记录外，还会保留某一侧或两侧未匹配的记录。

未能匹配到连接条件的数据也称为**孤立数据**（Orphan Data）。

外连接包括：

- 左外连接：`LEFT OUTER JOIN`
- 右外连接：`RIGHT OUTER JOIN`
- 全外连接：`FULL OUTER JOIN`

---

# 左外连接 LEFT OUTER JOIN## 概念左外连接会返回：

- 左表中的全部记录；
- 两表中满足连接条件的记录；
-右表中无法匹配的字段以 `NULL` 显示。

即使左表中的某条记录在右表中没有匹配项，该记录仍会被保留。

## 语法
```sql
SELECT 字段列表FROM 左表LEFT OUTER JOIN右表ON连接条件;
```

`OUTER`关键字可以省略：

```sql
SELECT 字段列表FROM 左表LEFT JOIN右表ON连接条件;
```

## 示例查询所有员工及其部门名称：

```sql
SELECT e.last_name,
 e.department_id,
 d.department_nameFROM employees eLEFT OUTER JOIN departments dON e.department_id = d.department_id;
```

该查询会返回：

- 能够找到对应部门的员工信息；
- 没有对应部门的员工信息；
- 对于没有匹配部门的员工，`department_name` 显示为 `NULL`。

---

#右外连接 RIGHT OUTER JOIN## 概念右外连接会返回：

-右表中的全部记录；
- 两表中满足连接条件的记录；
- 左表中无法匹配的字段以 `NULL` 显示。

即使右表中的某条记录在左表中没有匹配项，该记录仍会被保留。

## 语法
```sql
SELECT 字段列表FROM 左表RIGHT OUTER JOIN右表ON连接条件;
```

`OUTER`关键字同样可以省略：

```sql
SELECT 字段列表FROM 左表RIGHT JOIN右表ON连接条件;
```

## 示例查询所有部门及对应的员工信息：

```sql
SELECT e.last_name,
e.department_id,
d.department_nameFROM employees eRIGHT OUTER JOIN departments d ON e.department_id = d.department_id;
```

该查询会返回：

- 有员工的部门及对应员工；
- 没有员工的部门；
- 对于没有员工的部门，员工相关字段显示为 `NULL`。

---

# 全外连接 FULL OUTER JOIN## 概念全外连接会返回：

- 左表中的全部记录；
- 右表中的全部记录；
- 两表中能够匹配的记录进行合并；
- 任意一侧无法匹配的数据，另一侧字段以 `NULL` 显示。

## MySQL 中的限制MySQL 不支持 `FULL OUTER JOIN`语法。

可以通过将左外连接与右外连接的查询结果使用 `UNION` 合并，模拟全外连接。

## UNION 与 UNION ALL
|关键字 |作用 |
| --- | --- |
| `UNION` | 合并多个查询结果，并自动去除重复记录，效果类似于 `DISTINCT`。 |
| `UNION ALL` | 合并多个查询结果，并保留全部记录，包括重复记录。 |

## 模拟全外连接的语法
```sql
(
 SELECT 字段列表 FROM 表1 LEFT OUTER JOIN 表2 ON连接条件)
UNION(
 SELECT 字段列表 FROM 表1 RIGHT OUTER JOIN 表2 ON连接条件);
```

## 示例查询员工与部门信息，并保留双方无法匹配的数据：

```sql
(
 SELECT e.last_name,
 d.department_name FROM employees e LEFT OUTER JOIN departments d ON e.department_id = d.department_id)
UNION(
 SELECT e1.last_name,
 d1.department_name FROM employees e1 RIGHT OUTER JOIN departments d1 ON e1.department_id = d1.department_id);
```

---

