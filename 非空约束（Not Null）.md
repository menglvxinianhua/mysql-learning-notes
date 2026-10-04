

### 1. 非空约束的作用非空约束用于保证字段不能为空。

如果字段设置了 `NOT NULL`，插入数据时必须为该字段提供值。

### 2. 添加非空约束
```sql
ALTER TABLE 表名MODIFY 列名 数据类型 NOT NULL;
```

### 3. 示例为 `emp` 表中的 `salary` 字段添加非空约束。

```sql
ALTER TABLE empMODIFY salary FLOAT(8,2) NOT NULL;
```

---

## 删除非空约束
### 1. 使用 DDL语句删除非空约束 
```sql
ALTER TABLE 表名MODIFY 列名 数据类型 NULL;
```

### 2. 示例删除 `emp` 表中 `salary` 字段的非空约束。

```sql
ALTER TABLE empMODIFY salary FLOAT(8,2) NULL;
```

---
