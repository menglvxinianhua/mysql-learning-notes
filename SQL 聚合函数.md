# #聚合函数

## 聚合函数用于对一组数据进行计算，并返回一个结果

常见聚合函数：

- `AVG()`：求平均值。
- `SUM()`：求和。
- `MIN()`：求最小值。
- `MAX()`：求最大值。
- `COUNT()`：统计行数。

> [!note]
> 除 `COUNT(*)` 外，大多数聚合函数会忽略 `NULL` 值。

---

## AVG 与 SUM 函数
### AVG 函数`AVG(arg)` 用于对分组数据求平均值。

- `arg` 参数通常是数值类型的列或表达式。

### SUM 函数`SUM(arg)` 用于对分组数据求和。

- `arg` 参数通常是数值类型的列或表达式。

### 示例：计算指定岗位员工的平均薪水与薪水总和
```sql
SELECT AVG(salary), SUM(salary)
FROM employeesWHERE job_id LIKE '%REP%';
```

---

## MIN 与 MAX 函数
### MIN 函数`MIN(arg)` 用于求分组中的最小值。

参数可以是：

- 数字
- 字符
- 日期
### MAX 函数`MAX(arg)` 用于求分组中的最大值。

参数可以是：

- 数字
- 字符
- 日期
### 示例：查询最早和最晚的入职时间
```sql
SELECT MIN(hire_date), MAX(hire_date)
FROM employees;
```

---

## COUNT 函数`COUNT` 函数用于返回分组中的行数。

| 函数形式 |说明 |
| --- | --- |
| `COUNT(*)` | 返回查询结果中的所有行数，包括重复行和包含空值的行。 |
| `COUNT(expr)` | 返回 `expr` 不为 `NULL` 的行数。 |
| `COUNT(DISTINCT expr)` | 返回 `expr` 不为 `NULL` 且不重复的值的数量。 |

---

## DISTINCT关键字`DISTINCT` 用于去除重复值。

在聚合函数中，`COUNT(DISTINCT expr)` 用于统计非空且不重复的值的数量。

### 示例：统计80号部门中有佣金的员工人数
```sql
SELECT COUNT(commission_pct)
FROM employeesWHERE department_id =80;
```

### 示例：统计员工表中不同部门的数量
```sql
SELECT COUNT(DISTINCT department_id)
FROM employees;
```

---

## 聚合函数中处理 NULL 值可以使用 `IFNULL` 将空值替换为指定值后再进行计算。

### 示例：将没有佣金的员工佣金按0计算，再求平均佣金
```sql
SELECT AVG(IFNULL(commission_pct,0))
FROM employees;
```

---


