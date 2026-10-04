### 1. 唯一约束的作用唯一约束用于保证字段中的数据不能重复。

例如，为员工姓名添加唯一约束后，`name` 字段中不能出现重复值。

### 2. 添加唯一约束
```sql
ALTER TABLE 表名ADD CONSTRAINT约束名UNIQUE(列名);
```

### 3. 示例为 `emp` 表中的 `name` 字段添加唯一约束。

```sql
ALTER TABLE empADD CONSTRAINT emp_ukUNIQUE(name);
```

---

## 删除唯一约束
### 1. 使用 DDL语句删除唯一约束
```sql
ALTER TABLE 表名DROP KEY约束名;
```

### 2. 示例删除 `emp` 表中名为 `emp_uk` 的唯一约束。

```sql
ALTER TABLE empDROP KEY emp_uk;
```

> 在 MySQL 中，唯一约束通常以唯一索引的形式保存，因此也可以使用删除索引的方式删除唯一约束。

---
