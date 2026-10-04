## 等值连接
### 等值连接的概念
等值连接是指使用等号 `=`作为连接条件，将多个表中具有相同关联值的记录连接起来。

在员工表与部门表中：

| 表名 |关联字段 | 键类型 |
| --- | --- | --- |
| `EMPLOYEES` | `department_id` | 外键 FK |
| `DEPARTMENTS` | `department_id` | 主键 PK |

连接关系为：

```sqlemployees.department_id = departments.department_id```

其中：

- `EMPLOYEES.department_id` 是外键。
- `DEPARTMENTS.department_id` 是主键。
- 外键的值引用对应部门的主键值。

### 等值连接的特点- 多表等值连接的结果通常是多个表的交集部分。
- 连接 `n` 个表时，至少需要 `n -1` 个连接条件。
- 多表连接通常没有固定的主次顺序。
- 建议为表设置别名，以提高 SQL 的可读性和编写效率。
- 连接条件一般写在 `WHERE` 子句中。

### 使用等值连接返回记录
```sql
SELECT employees.employee_id,
 employees.last_name,
 employees.department_id,
 departments.department_id,
 departments.location_idFROM employees,
 departmentsWHERE employees.department_id = departments.department_id;
```

上述查询中：

- `employee_id`、`last_name`、`department_id` 来自 `EMPLOYEES` 表。
- `department_id`、`location_id` 来自 `DEPARTMENTS` 表。
- `WHERE` 子句用于指定两个表之间的等值连接条件。

## 使用 AND 添加搜索条件除了连接条件外，还可以在 `WHERE` 子句中通过 `AND` 添加一个或多个筛选条件。

```sql
SELECT e.last_name,
 d.department_nameFROM employees e,
 departments dWHERE e.department_id = d.department_id AND e.last_name = 'King';
```

上述查询先连接员工表和部门表，再筛选姓氏为 `King` 的员工。

## 限制不明确的列名当多个表中存在同名字段时，必须在列名前加上表名或表别名，以避免歧义。

例如，`EMPLOYEES` 表和 `DEPARTMENTS` 表中都可能包含 `department_id` 字段。

```sql
employees.department_iddepartments.department_id
```

### 使用表前缀的原因- 在多表查询中明确列属于哪个表。
- 避免同名字段导致 SQL 报错。
- 提高 SQL语句的可读性。
- 在部分情况下有助于数据库优化查询。
- 可以使用列别名区分不同表中同名的列。

```sql
SELECT e.department_id AS employee_department_id,
 d.department_id AS department_department_idFROM employees e,
 departments dWHERE e.department_id = d.department_id;
```

## 使用表别名表别名用于简化表名书写，使多表查询更加清晰。

```sql
SELECT e.employee_id,
 e.last_name,
 e.department_id,
 d.department_id,
 d.location_idFROM employees e,
 departments dWHERE e.department_id = d.department_id;
```

其中：

| 原表名 | 表别名 |
| --- | --- |
| `employees` | `e` |
| `departments` | `d` |

### 表别名使用原则
- 表别名应尽量简短且容易识别。
- 表别名的有效范围是当前查询语句。
- 表别名定义在 `FROM` 子句中，并紧跟在表名后面。
- 定义表别名后，应在查询中使用该别名引用对应表的字段。

## 多于两个表的连接连接多个表时，表的数量与连接条件数量的关系如下：

```text
连接 n 个表，至少需要 n -1 个连接条件。
```

例如，连接员工表、部门表和地点表时：

| 表名 |关联字段 |
| --- | --- |
| `EMPLOYEES` | `department_id` |
| `DEPARTMENTS` | `department_id`、`location_id` |
| `LOCATIONS` | `location_id` |

连接关系为：

```sql
e.department_id = d.department_idd.location_id = l.location_id
```

### 示例：查询 King 所在的部门名称
```sql
SELECT d.department_nameFROM employees e,
 departments dWHERE e.department_id = d.department_id AND e.last_name = 'King';
```

### 示例：查询员工姓名、部门名称和城市
```sql
SELECT e.last_name,
 d.department_name,
 l.cityFROM employees e,
 departments d,
 locations lWHERE e.department_id = d.department_id AND d.location_id = l.location_id;
```

## 非等值连接
### 非等值连接的概念非等值连接是指连接条件不使用等号 `=`，而使用范围比较运算符进行连接。

常见运算符包括：

- `BETWEEN ... AND ...`
- `>`
- `<`
- `>=`
- `<=`

例如，员工表中的工资 `salary` 可以与工资等级表中的最低工资和最高工资进行范围匹配。

| 表名 |相关字段 |
| --- | --- |
| `EMPLOYEES` | `salary` |
| `JOB_GRADES` | `lowest_sal`、`highest_sal`、`grade_level` |

连接条件为：

```sql
e.salary BETWEEN j.lowest_sal AND j.highest_sal
```

### 使用非等值连接返回记录
```sql
SELECT e.last_name,
 e.salary,
 j.grade_levelFROM employees e,
 job_grades jWHERE e.salary BETWEEN j.lowest_sal AND j.highest_sal;
```

该查询用于根据员工工资查询其对应的工资等级。

### 工资等级表示例
```sql
CREATE TABLE job_grades (
 lowest_sal INT,
 highest_sal INT,
 grade_level VARCHAR(3)
);
```

```sql
INSERT INTO job_grades VALUES (1000,2999, 'A');
INSERT INTO job_grades VALUES (3000,4999, 'B');
INSERT INTO job_grades VALUES (5000,7999, 'C');
INSERT INTO job_grades VALUES (8000,12000, 'D');
```

工资等级数据如下：

| 最低工资 |最高工资 | 工资等级 |
| --- | --- | --- |
|1000 |2999 | A |
|3000 |4999 | B |
|5000 |7999 | C |
|8000 |12000 | D |

## 自连接
### 自连接的概念自连接是指一个表与它本身进行连接。

自连接通常用于处理同一张表中存在上下级关系、父子关系或层级关系的数据。

在员工表中：

| 别名 | 表示的角色 |
| --- | --- |
| `worker` | 员工 |
| `manager` | 员工的经理 |

关联关系为：

```sql
worker.manager_id = manager.employee_id
```

其中：

- `worker.manager_id` 表示员工的经理编号。
- `manager.employee_id` 表示经理本人的员工编号。
- 两个别名实际上都指向 `EMPLOYEES` 表。

### 连接一个表到它本身
```sql
SELECT worker.last_name,
 manager.last_nameFROM employees worker,
 employees managerWHERE worker.manager_id = manager.employee_id;
```

该查询可以获取每位员工及其对应经理的姓名。

### 使用列别名区分员工和经理
```sql
SELECT worker.last_name AS worker_name,
 manager.last_name AS manager_nameFROM employees worker,
 employees managerWHERE worker.manager_id = manager.employee_id;
```

### 示例：查询 Fox 的经理
```sql
SELECT worker.last_name,
 manager.last_nameFROM employees worker,
 employees managerWHERE worker.manager_id = manager.employee_id AND worker.last_name = 'Fox';
```
