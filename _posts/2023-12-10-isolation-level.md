---
title: Isolation Level
date: 2023-12-10 18:00:00 +0800
categories: [database]
tags: [database]
---

## Isolation Level

|                   | DIRTY READ | NON-REPEATABLE READ | PHANTOM READ | Default Level              |
|-------------------|------------|---------------------|--------------|----------------------------|
| READ UNCOMMITTED  | O          | O                   | O            |                            |
| READ COMMITTED    | X          | O                   | O            | ORACLE, PostgreSQL, MSSQL  |
| REPEATABLE READ   | X          | X                   | O            | MySQL, MariaDB (InnoDB 기준) |
| SERIALIZABLE READ | X          | X                   | X            |                            |

### READ UNCOMMITTED

READ UNCOMMITTED 격리 수준에서는 각 트랜잭션의 변경 내용이 COMMIT, ROLLBACK 여부와 관계 없이 다른 트랜잭션에서 조회 가능하다.

> Q. DIRTY READ 란 ?
>
> A. DIRTY READ는 다른 트랜잭션에서 처리한 작업이 완료되지 않았음에도 불구하고 다른 트랜잭션에서 볼 수 있게 되는 현상을 말한다.

READ UNCOMMITTED 수준에서는 ```DIRTY READ``` 를 허용 즉, 변경이 ROLL BACK 되거나 COMMIT 이 이루어 지지 않은 데이터를 조회 이후 정상
데이터로 판단하고 작업을 하기 때문에 데이터 정합성이 깨진다.

이러한 이유로 트랜잭션의 격리 수준으로 인정하지 않는다.

### READ COMMITTED

READ COMMITTED 격리 수준은 ```DIRTY READ``` 를 허용하지 않고, 온라인 서비스에서 가장 많이 사용하는 격리 수준이다.

READ COMMITTED 에서는 한 트랙잭션이 COMMIT, ROLLBACK 전까지 다른 트랙잭션은 UNDO 영역에 백업된 레코드 값을 참조하게
되면서 ```DIRTY READ```를 막는다.

> Q. UNDO 영역이란?
>
> A. UNDO 영역은 UPDATE 문장이나 DELETE와 같은 문장을 데이터를 변경했을 때 변경되기 전의 데이터를 보관하는 곳이다.
> INSERT 문장의 경우, 해당 데이터의 row id를 저장하고 이를 이용하여 물리적 메모리에 바로 접근할 수 있도록 보장한다.
>
> UNDO 영역은 트랜잭션의 롤백 대비용으로 사용하고, 트랜잭션의 격리 수준을 유지하면서 높은 동시성을 제공한다.

하지만, READ COMMITTED 격리 수준에서는 NON_REPEATABLE READ라는 부정합 문제가 존재한다.

> Q. REPEATABLE READ 란?
>
> A. 트랜잭션 내에서 동일한 SELECT 쿼리를 실행했을 때 항상 같은 결과를 보장해야 한다.

READ COMMITTED 에서는 A 트랙잭션에서 조회 로직이 여러번 있다고 가정할 때, B 트랙잭션에서 데이터 변경이후 A 트랙잭션에서 조회를 할 경우 조회 결과가 같지 않는
문제점이 있다.

이는 일반적인 웹 어플리케이션에서는 문제가 없을 수 있지만, 금전적인 처리의 경우 일치하지 않을 경우 문제가 발생할 수 있다.

### REPEATABLE READ

이 격리 수준에서는 MVCC를 이용해 READ COMMITTED 격리 수준에서 발생하는 NON-REPEATABLE READ 부정합이 발생하지 않는다.

> Q. MVCC 란?
>
> A. MVCC를 통해 트랜잭션이 롤백된 경우에 데이터를 복원할 수 있을 뿐만 아니라, 서로 다른 트랜잭션 간에 접근할 수 있는 데이터를 세밀하게 제어할 수 있다. 각각의 트랜잭션은 순차 증가하는 고유한 트랜잭션 번호가 존재하며, 백업 레코드에는 어느 트랜잭션에 의해 백업되었는지 트랜잭션 번호를 함께 저장한다.

REPEATABLE READ 수준에서는 트랜잭션 번호를 참고하여 자신보다 먼저 실행된 트랜잭션의 데이터만을 조회한다. 즉, 트랜잭션 작업 도중 현 트랜잭션 보다 이후 발생한
트랜잭션의 작업에 대해서는 무시한다.

REPEATABLE READ는 한 트랜잭션 내에서 동일한 결과를 보장하지만, 새로운 레코드가 추가되는 경우에 부정합이 생길 수 있다.

> Q. 자신보다 나중에 실행된 트랜잭션이 추가한 레코드는 무시한다면서 왜 새로운 레코드 추가되는 경우 부정합이 생길까?
>
> A. 일반적인 경우 발생하지 않지만 잠금이 사용될 경우 발생한다.

| Transaction A                                           | Transaction B                                           |
|---------------------------------------------------------|---------------------------------------------------------|
| SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ | SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ |
|                                                         | START TRANSACTION                                       |
|                                                         | SELECT * FROM table WHERE id >= 1                       |
| START TRANSACTION                                       |                                                         |
| UPDATE table SET name = 'new table' WHERE id = 1        |                                                         |
| COMMIT                                                  |                                                         |
|                                                         | SELECT * FROM member WHERE id >= 3 FOR UPDATE           |

SELECT FOR UPDATE를 이용해 쓰기 잠금을 하고 할 경우 데이터 조회가 언두 로그가 아닌 테이블에서 수행되기 때문에 Trasaction A 에서 UPDATE한 새로운
값이 나타난다..

### SERIALIZABLE

SERIALIZABLE 는 가장 엄격한 격리 수준으로, 모든 트랜잭션을 직렬화 하여 순차적으로 진행시키기 때문에 여러 트랜잭션이 동일한 레코드에 동시 접근할 수 없으므로, 어떠한
데이터 부정합 문제도 발생하지 않는다. 하지만 트랜잭션이 순차적으로 처리되어야 하므로 동시 처리 성능이 매우 떨어진다.

또한, 순수한 SELECT 작업에도 대상 레코드에 넥스트 키 락을 읽기 자금으로 걸기 때문에 다른 트랜잭션에서는 절대 추가/수정/삭제할 수 없다.
