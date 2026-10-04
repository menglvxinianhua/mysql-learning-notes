### 1. 主键的概念主键是表中的一个特殊字段，用于唯一标识表中的一条记录。

主键字段具有以下特点：

- 主键值不能重复。
- 主键值不能为空。
- 一张表通常只能设置一个主键约束。
- 主键可以由一个字段组成，也可以由多个字段组成。
- 主键能够帮助数据库快速定位一条记录。

例如，在学生信息表中，学号通常具有唯一性，可以作为主键。

### 2. 单一主键单一主键是指使用一个字段作为主键。

示例：

```text
employee_id
```

如果 `employee_id` 的值不会重复，并且每条记录都有对应的编号，则可以将其设置为主键。

### 3. 联合主键联合主键是指使用多个字段共同作为主键。

联合主键中，单个字段的值可以重复，但多个字段组合起来的值不能重复。

例如：

```textstudent_id + course_id```

在选课表中：

- 一个学生可以选择多门课程。
- 一门课程也可以被多个学生选择。
- `student_id` 单独存在时可能重复。
- `course_id` 单独存在时也可能重复。
- 但是 `student_id` 和 `course_id` 的组合不能重复。

### 4. 添加主键约束
#### 1. 使用 SQL 添加主键基本语法：

```sql
ALTER TABLE 表名ADD PRIMARY KEY (列名);
```

示例：

将 `emp` 表中的 `employee_id` 字段设置为主键：

```sql
ALTER TABLE empADD PRIMARY KEY (employee_id);
```

#### 2. 添加联合主键基本语法：

```sql
ALTER TABLE 表名ADD PRIMARY KEY (列名1, 列名2);
```

示例：

```sql
ALTER TABLE student_courseADD PRIMARY KEY (student_id, course_id);
```

#### 3. 添加主键前的注意事项添加主键约束之前，应确保：

- 主键字段中的值不能重复。
- 主键字段中不能存在 `NULL` 值。
- 主键字段的数据能够唯一标识每条记录。
- 联合主键中，字段组合不能重复。

如果字段中已经存在重复值或空值，添加主键时可能会执行失败。

### 5.主键自增
#### 1. 自增的概念`AUTO_INCREMENT` 用于让整数类型的字段自动生成递增的数值。

自增字段通常与主键一起使用，例如：

```text
1、2、3、4、5……
```

这样在插入新记录时，可以不手动指定编号，MySQL 会自动生成编号。

#### 2. MySQL 自动增长的要求MySQL 中使用自动增长字段时，需要满足以下要求：

- 一张表中只能有一个自动增长字段。
- 自动增长字段必须是整数类型。
- 自动增长字段通常需要设置为主键或唯一键。
- 自动增长字段不能使用字符串类型。
- 如果删除设置了自动增长属性的主键约束，通常需要先去掉字段的自动增长属性。

#### 3. 使用 SQL 设置主键自增基本语法：

```sql
ALTER TABLE 表名MODIFY COLUMN 列名 整数类型 AUTO_INCREMENT;
```

示例：

将 `emp` 表中的 `employee_id` 字段设置为整数类型并开启自动增长：

```sql
ALTER TABLE emp MODIFY COLUMN employee_id INT AUTO_INCREMENT;
```

如果该字段还没有设置主键，可以先添加主键：

```sql
ALTER TABLE emp ADD PRIMARY KEY (employee_id);
```

然后设置自增：

```sql
ALTER TABLE emp MODIFY COLUMN employee_id INT AUTO_INCREMENT;
```



### 6.删除主键约束
#### 1. 使用 DDL语句删除主键
```sql
ALTER TABLE 表名 DROP PRIMARY KEY;
```

#### 2. 注意事项如果主键字段设置了 `AUTO_INCREMENT`，删除主键之前需要先取消字段的自动增长属性，然后再删除主键约束。

#### 3. 示例删除 `emp` 表中 `employee_id` 字段的主键约束。

```sql
--先取消 employee_id 的自动增长属性
ALTER TABLE emp MODIFY employee_id INT;
-- 再删除主键约束
ALTER TABLE emp DROP PRIMARY KEY;
```

---

