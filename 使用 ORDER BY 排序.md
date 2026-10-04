`ORDER BY` 子句用于对查询结果进行排序。

- `ASC`：升序排序，默认值。（ascend)
- `DESC`：降序排序。(descend)
- `ORDER BY` 必须位于 `SELECT` 语句的最后。

```sql
SELECT last_name, job_id, department_id, hire_date
FROM employees
ORDER BY hire_date;
```

`SELECT` 语句的逻辑执行顺序如下：

1. `FROM` 子句
2. `WHERE` 子句
3. `SELECT` 子句
4. `ORDER BY` 子句

### 默认升序排序

以下两种写法效果相同，均按照工资升序排序。

```sql
SELECT employee_id, last_name, salary
FROM employees
ORDER BY salary;
```

```sql
SELECT employee_id, last_name, salary
FROM employees
ORDER BY salary ASC;
```

### 降序排序

查询员工 ID 和姓名，并按照姓名降序排序。

```sql
SELECT employee_id, last_name
FROM employees
ORDER BY last_name DESC;
```

## 使用列别名排序

可以在 `ORDER BY` 中使用 `SELECT` 子句中定义的列别名。

示例：查询员工 ID、姓名和年薪，并按照年薪升序排序。

```sql
SELECT employee_id, last_name, salary * 12 AS annsal
FROM employees
ORDER BY annsal;
```

## 多列排序

`ORDER BY` 可以指定多个排序列，排序优先级由列出现的先后顺序决定。

```sql
SELECT last_name, department_id, salary
FROM employees
ORDER BY department_id, salary DESC;
```

上述查询的排序规则为：

1. 先按照 `department_id` 升序排序。
2. 当 `department_id` 相同时，再按照 `salary` 降序排序。

`ORDER BY` 中可以使用未出现在 `SELECT` 列表中的列。

```sql
SELECT department_id, salary
FROM employees
ORDER BY department_id ASC, salary DESC;
```