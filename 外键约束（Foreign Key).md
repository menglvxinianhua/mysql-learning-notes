### 1. 外键的作用外键用于建立两个表之间的关联关系。

例如：

- `employees` 表中的 `department_id` 指向 `departments` 表中的 `department_id`
- 员工表中的部门编号必须在部门表中存在- 外键可以保证数据的参照完整性
### 2. 外键关系示例
| 子表 | 外键字段 | 主表 | 被参照字段 |
| --- | --- | --- | --- |
| `employees` | `department_id` | `departments` | `department_id` |

### 3. 添加外键约束
```sql
ALTER TABLE 表名ADD CONSTRAINT约束名FOREIGN KEY (列名)
REFERENCES参照表名(参照列名);
```

### 4. 创建部门表
```sql
CREATE TABLE departments (
 department_id INT,
 department_name VARCHAR(30),
 location_id INT);
```

### 5. 为字段添加主键和自动增长
```sql
ALTER TABLE departmentsADD PRIMARY KEY (department_id);
```

```sql
ALTER TABLE departmentsMODIFY department_id INT AUTO_INCREMENT;
```

### 6. 为 `emp` 表添加部门编号字段
```sql
ALTER TABLE empADD COLUMN dept_id INT;
```

### 7. 为 `dept_id` 添加外键约束
```sql
ALTER TABLE empADD CONSTRAINT emp_fkFOREIGN KEY (dept_id)
REFERENCES departments(department_id);
```

---

## 三、删除外键约束

### 1. 使用 DDL语句删除外键
```sql 
ALTER TABLE 表名DROP FOREIGN KEY约束名;
```

### 2. 示例删除 `emp` 表中名为 `emp_fk` 的外键约束。

```sql
ALTER TABLE empDROP FOREIGN KEY emp_fk;
```

> 删除外键时，需要使用外键约束的名称，而不是外键字段的名称。

---
