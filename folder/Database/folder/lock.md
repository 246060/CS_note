# Mysql Lock
[참조](https://offbyone.tistory.com/225)

- [Mysql Lock](#mysql-lock)
  - [1. Table-Level Locking](#1-table-level-locking)
  - [2. Row-Level Locking](#2-row-level-locking)
  - [3. Optimistic Lock(낙관적인 잠금)](#3-optimistic-lock낙관적인-잠금)

두 명의 사용자가 재고가 하나밖에 없는 제품을 조회하고, 동시에 구매를 한 상황입니다. 서버로 요청이 넘어가면 서버에서는 다음 세 단계를 거쳐 구매를 처리한다고 가정해 봅니다.

  1) 재고 확인
  2) 재고 감소
  3) 구매정보 입력

재고를 확인하고 구매할 수 있다고 판단한 후에 재고를 감소하기 전에 다른 요청을 처리하는 쓰레드 또는 프로세스가 재고를 확인하게 되면 둘 다 구매 할 수 있다고 판단하게 되고 구매 처리가 되어 버립니다.
 

MySQL은 테이블이 MyISAM 인 경우와 InnoDB 인 경우 서로 다른 방법을 사용할 수 있습니다.

## 1. Table-Level Locking

`MyIASM`인 경우는 `테이블 수준의 잠금` 기능 사용  
다중 사용자 환경에서는 사용하지 않는 것이 좋습니다.  

테이블에 10개의 제품이 있으면 하나의 제품을 구매하는 동안에 다른 9개도 구매를 하지 못하게 되는 것입니다.

테이블 잠금은 READ lock 과 WRITE lock 이 있습니다.

- `READ lock` 을 걸면 UNLOCK 하기 전까지 `다른 세션에서 읽을 수는 있지만 입력, 수정, 삭제는 할 수 없고 대기`하게 됩니다.

```sql
mysql> LOCK TABLES tb_stock READ;
...
mysql> UNLOCK TABLES;
```

- `WRITE lock` 을 걸면 UNLOCK 하기 전까지 `다른 세션에서는 읽을 수도 없고 대기`하게 됩니다.

```sql
mysql> LOCK TABLES tb_stock WRITE;
...
mysql> UNLOCK TABLES;
```

- 테이블 잠금이 유지되는 동안 잠긴 테이블에 대해서만 액세스 할 수 있습니다. 여러 테이블을 읽을 경우 모든 테이블의 잠금을 하나의 LOCK TABLES 문에 모두 나열 해야 합니다.

```sql
mysql> LOCK TABLES tb_stock WRITE, tb_order READ;
```

- 하나의 테이블의 lock을 얻고, 다른 테이블을 조회하려고 하면 에러가 발생

```sql
mysql> LOCK TABLES tb_stock READ;
Query OK. 0 rows affected(0.00 sec)

mysql> SELECT COUNT(*) FROM tb_stock;
+----------+
| COUNT(*) |
+----------+
|        3 |
+----------+
1 row in set (0.00 sec)


mysql> SELECT COUNT(*) FROM tb_order;

ERROR 1100 (KY000): Table 'tb_order' was not locked with LOCK TABLES
```


## 2. Row-Level Locking

MySQL 테이블이 `InnoDB`라면 `행 수준의 잠금`을 사용  
테이블내의 하나의 행을 읽고, 수정할 때 다른 행들에 대한 액세스는 허용  

- 트랜잭션을 시작하고 `FOR UPDATE 문`을 사용하면 `조회된 행`에 대해서는 commit 또는 rollback 으로 트랜잭션이 종료될 때까지 `조회, 입력, 수정, 삭제가 모두 블록` 됩니다.

- MySQL이 `조회된 행을 기억하는 방법`이 `인덱스 범위를 기억`하는 것이므로 `조건 필드에 인덱스가 없으면 전체 테이블에 lock 이 걸립니다.`


```sql
mysql> START TRANSACTION;
mysql> SELECT * FROM tb_stock WHERE id=1 FOR UPDATE;
...
mylsq> COMMIT;

```

- `공유모드`로 잠그면 `다른 세션에서 읽기는 가능하고 수정, 삭제는 못하도록 할 수 있습니다.`

```sql
mysql> START TRANSACTION;
mysql> SELECT * FROM tb_stock WHERE id=1 LOCK IN SHARE MODE;
...
mylsq> COMMIT;
```

- `Row-Level Lock 을 사용할 때는 데드락(Dead-Lock) 이 발생하지 않도록 주의`해야 합니다. 하나의 Row 를 잠그고, 잠긴 다른 Row 가 풀릴 때까지 서로 기다리는 코드를 작성하게 되면 서로 록을 해제 할때까지 무한히 대기하는 데드락이 발생할 수 있습니다. `참고로 Table-Level Lock 은 데드락이 발생하지 않습니다.`





## 3. Optimistic Lock(낙관적인 잠금)
  = 레코드에 버저닝 방식으로 동시성을 제어 하는 방식
  = 논리적 트랜잭션을 위해 추가 필드를 사용할 수 도 있음

낙곽적인 잠금은 실제로 Lock 을 얻어서 잠그는 것이 아니고 단일 쿼리는 원자적이므로 하나의 쿼리가 실행되는 동안에는 끼어들 수 없다는 것을 이용하여 동시성을 제어하는 방법 

보통 다음과 같은 개념입니다. 업데이트와 체크를 동시에 한 쿼리로 수행하는 것입니다.

1) 테이블에 정수형 필드를 하나 두고 조회 시 그 값을 읽어 옵니다.

```sql
SELECT quantity, opt_flag 
FROM tb_stock 
WHERE id = 1;
```

2) 업데이트시 이 값이 조건절에 걸어 업데이트합니다.

```sql
UPDATE tb_stock 
SET quantity = quantity - 1, opt_flag = opt_flag + 1 
WHERE opt_flag = '조회시값' AND id = 1;
```

3) 업데이트 쿼리의 affected row 가 0이면 조회 당시의 opt_flag 가 변경되었을 것입니다. 이것은 내가 조회하고 업데이트하기 전에 누가 먼저 업데이트 했다는 것을 뜻하므로 실패로 처리합니다.

위의 쇼핑몰에서 구매하는 예를 사용하여 좀더 현실적으로 작성해 보면 다음과 같을 것입니다.

```sql
UPDATE tb_stock 
SET quantity = quantity - 1 
WHERE quantity - 1 >= 0 AND id = 1;
```

하나를 구매해서 재고를 하나 감소시켜도 그 값이 음수가 되지 않는다면 성공적으로 구매를 할 수 있는 것이 되겠습니다. 업데이트에 성공하면 주문 정보를 입력하면 됩니다.

웹 프로그래밍의 경우 세션이 물리적으로 유지되지 않으므로 조회 당시에 물건을 구매할 수 있다고 보여도, 구매 버튼을 누르면 다른 사람이 먼저 구매 해 버려서 구매 할 수 없는 상황이 발생할 수 있습니다. 이런 상황을 받아들일 수 있는 응용에서는 낙관적인 잠금이 빠른 성능을 유지하면서도 동시성을 제어할 수 있는 좋은 방법이 됩니다.



