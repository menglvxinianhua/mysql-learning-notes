
# MySQL事务
## MySQL事务
### 事务简介
##### 事务是把若干个逻辑工作单元作为一个整体进行处理：要么全部执行，要么全部不执行。

事务是一个或多个不可分割的工作单元，通常一个事务对应一个完整的业务系统操作。

- 一个完整的业务流程通常由多条 DML语句共同组合完成。
- 事务主要针对 DML语句，即 `INSERT`、`UPDATE`、`DELETE`。
- DDL、DCL 等语句通常会自动提交，不适合进行事务回滚操作。

### 事务的四大特性（ACID）

#### 原子性（Atomicity）

事务中的操作要么全部完成，要么全部不执行。

#### 一致性（Consistency）

事务执行前后，数据库都必须保持一致性状态。

一个事务应当将数据库从一个一致性状态转换到另一个一致性状态。

#### 隔离性（Isolation）

一个事务的执行不能被其他事务干扰。

多个事务并发执行时，各事务之间应相互隔离。

#### 持久性（Durability）

一个事务一旦提交，其对数据库中数据的改变就应当永久保存。

即使系统发生故障，已经提交的数据也不能丢失。

### 事务类型

#### 显式事务显式事务需要手动提交或回滚。

DML语句中的操作通常属于事务操作。

#### 隐式事务隐式事务由数据库自动提交，不需要手动处理，同时一般不具备回滚能力。

DDL、DCL 等语句通常属于隐式事务操作。

## 使用事务


### TCL语句
| TCL语句 |说明 |
| --- | --- |
| `START TRANSACTION` | 开启事务 |
| `COMMIT` | 提交事务 |
| `ROLLBACK` | 回滚事务 |

### 示例：创建账户表
```sql
CREATE TABLE account(
 id INT PRIMARY KEY AUTO_INCREMENT,
 cardnum VARCHAR(20) NOT NULL,
 username VARCHAR(30) NOT NULL,
 balance DOUBLE(10,2)
);
```

### 示例：添加账户数据
```sql
INSERT INTO account(cardnum, username, balance)
VALUES('123456789', '张三',2000);

INSERT INTO account(cardnum, username, balance)
VALUES('987654321', '李四',2000);
```

### 示例：转账事务
```sql
START TRANSACTION;

UPDATE accountSET balance = balance -200WHERE cardnum = '123456789';

UPDATE accountSET balance = balance +200WHERE cardnum = '987654321';

SELECT * FROM account;

COMMIT;
```

### 回滚事务当事务中的操作出现异常，或者需要撤销本次事务时，可以使用 `ROLLBACK`。

```sql
ROLLBACK;
```

- `COMMIT`：确认并永久保存本次事务中的所有操作。
- `ROLLBACK`：撤销本次事务中尚未提交的所有操作，使数据恢复到事务开始前的状态。

## 事务的并发问题多个事务同时访问同一批数据时，可能会产生数据不一致问题。

### 脏读脏读是指一个事务读取到了另一个事务尚未提交的数据。

如果另一个事务随后进行了回滚，那么前一个事务读取到的数据就是无效数据。

```text
事务 A读取了事务 B 未提交的数据。
事务 B发生异常并回滚。
事务 A读取到的数据与数据库最终数据不一致。
```

### 不可重复读不可重复读是指在一个事务中，多次读取同一条数据，得到的数据内容不一致。

通常是因为在两次读取之间，其他事务对该数据进行了修改并提交。

```text
事务 A 第一次读取某条数据。
事务 B 修改该数据并提交。
事务 A 再次读取同一条数据，发现数据内容发生变化。
```

### 幻读幻读是指在一个事务中，多次查询某个范围的数据时，前后查询到的数据总量不一致。

通常是因为在两次查询之间，其他事务新增或删除了符合查询条件的数据。

```text
事务 A 查询符合条件的数据。
事务 B 新增或删除符合条件的数据并提交。
事务 A 再次查询时，发现查询结果的记录数量发生变化。
```

##事务的隔离级别事务隔离级别用于控制多个并发事务之间的相互影响。

MySQL 提供了四种事务隔离级别：

- `READ UNCOMMITTED`
- `READ COMMITTED`
- `REPEATABLE READ`
- `SERIALIZABLE`

| 隔离级别 | 脏读 | 不可重复读 | 幻读 |
| --- | --- | --- | --- |
| `SERIALIZABLE` | Y | Y | Y |
| `REPEATABLE READ` | Y | Y | N |
| `READ COMMITTED` | Y | N | N |
| `READ UNCOMMITTED` | N | N | N |

> `Y` 表示可以解决该问题，`N` 表示不能解决该问题。

### 查看 MySQL 默认事务隔离级别
```sql
SELECT @@transaction_isolation;
```

### 设置事务隔离级别以下设置只对当前 Session 有效。

```sql
SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
```

```sql
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
```

```sql
SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```

```sql
SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```


