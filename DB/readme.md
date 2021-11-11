# DataBase 총 정리

# JOIN
둘 이상의 Table을 연결해서 데이터를 검색(?)하는 방법
연결하려면 Table들이 적어도 하나의 Column을 공유하고 있어야 함
공유하는 Column을 Primary Key or Foreign Key로 사용

1. Inner Join (내부조인 == 교집합)
2. Left/Right Join (부분집합)
3. Outer Join (외부조인 == 합집합)
Oracle은 Outer Join이 있고 MySQL은 없어서 Left join + Right join으로 할 수 있다.

 
이 두 테이블을 기준으로 설명해보겠다.

# 1. Inner Join (A와 B의 공통적인 부분에 해당되는 것만 SELECT됨)
SELECT A.ID, A.ENAME, A.KNAME
FROM A INNER JOIN B
ON A.ID = B.ID
 

# 2. Left Join (Join 기준 왼쪽 – 현재는 A에 해당하는 것 전부 SELECT됨)
SELECT A.ID, A.ENAME, A.KNAME
FROM A LEFT OUTER JOIN B
ON A.ID = B.ID
 

# 2-1 Left Join 응용 (Join 기준 왼쪽 – 현재는 A에 해당하는 것만 SELECT됨)
SELECT A.ID, A.ENAME, A.KNAME
FROM A LEFT OUTER JOIN B
ON A.ID = B.ID
WHERE B.ID IS NULL
 
# 3. Right Join (Join 기준 오른쪽 – 현재 B에 해당하는 것 전부 SELECT됨)
SELECT A.ID, A.ENAME, A.KNAME
FROM A RIGHT OUTER JOIN B
ON A.ID = B.ID
 

# 3-1 Right Join 응용 (Join 기준 오른쪽 – 현재 B에 해당하는 것만 SELECT됨)
SELECT A.ID, A.ENAME, A.KNAME
FROM A RIGHT OUTER JOIN B
ON A.ID = B.ID
WHERE A.ID IS NULL
 







# 4. Outer Join (A table, B table이 가지고 있는 것 전부 SELECT됨)
SELECT A.ID, A.ENAME, A.KNAME
FROM A FULL OUTER JOIN B
ON A.ID = B.ID
 

# 4-1. Outer Join 응용 (A table만 가지고 있는 것 + B table만 가지고 있는 것 SELECT 됨)
SELECT A.ID, A.ENAME, A.KNAME
FROM A FULL OUTER JOIN B
ON A.ID = B.ID
WHERE A.ID IS NULL OR B.ID IS NULL
 





# Isolation Level (트랜잭션 격리수준)
동시에 여러 트랜잭션이 처리될 때, 트랜잭션끼리 얼마나 서로 고립되어 있는지를 나타내는 것
특정 트랜잭션이 다른 트랜잭션이 변경한 데이터의 조회 가능 여부를 결정하는 것

1. Read Uncommitted
2. Read Committed
3. Repeatable Read
4. Serializable
4번으로 갈 수록 트랜잭션간 고립 정도가 높아지고 성능이 떨어지는 것이 일반적이다.

# 1. Read Uncommitted (RDBMS 표준에서 격리수준으로 인정 X)
어떤 트랜잭션의 변경내용이 Commit이나 Rollback 상관없이 다른 트랜잭션에서 보인다.
-	데이터 정합성의 문제가 많이 생긴다.
-	
# 2. Read Committed (온라인 서비스에서 가장 많이 선택됨)
어떤 트랜잭션의 변경내용이 Commit되어야만 다른 트랜잭션에서 조회 가능하다.
-	정합성 문제가 해결된 것처럼 보이지만 Non Repeatable Read 문제 발생 가능
ex) 
1. B 트랜잭션에서 10번 사원의 나이 조회
2. 27살이 조회됨
3. A 트랜잭션에서 10번 사원의 나이를 27살에서 28살로 변경하고 Commit
4. B 트랜잭션에서 10번 사원의 나이 조회
5. 28살이 조회됨
-> 하나의 트랜잭션에서 똑같은 Select 수행한 경우 항상 같은 결과 반환해야 하지만 안되었다.


# 3. Repeatable Read (MySQL)
트랜잭션이 시작되기 전에 커밋한 내용에 대해서만 조회할 수 있는 격리수준이다.
1 - 10번 트랜잭션이 15번 사원 조회
2 - 12번 트랜잭션이 15번 사원 이름 변경하고 커밋
3 - 10번 트랜잭션이 15번 사원 다시 조회
4 - Undo 영역에 백업된 데이터 반환
자신의 트랜잭션 번호보다 낮은 트랜잭션 번호에서 변경된(Commit) 것만 보게 되는 것이다.

트랜잭션이 시작된 시점의 데이터를 일관되게 보여주는 것을 보장해야 한다.
한 트랜잭션 실행시간 길어질수록 해당 시간만큼 멀티 버전을 관리해야 한다.
하지만 사실 실 영향은 그리 크지 않아서 성능차이는 없다고 한다.
	DML 구문은 멀티버전 관리가 안되기 때문에 Update시 정합성 문제 발생 가능
	Phantom Read(커신 읽기)
	한 트랜잭션 내에서 같은 쿼리 2번 실행했는데 1번째에 없던 커신 레코드가 2번째 쿼리에서 나타나는 현상이 생긴다.
	원래 출력되지 않아야 하는데 Update문 영향을 받고 나서부터 출력된다.

# 4. Serializable
가장 단순하고 엄격한 격리수준
읽기 작업에도 공유 잠금을 설정하게 되고, 이러면 동시에 다른 트랜잭션이 변경 못하게 됨
동시처리 능력이 떨어지고, 성능저하가 발생한다.
