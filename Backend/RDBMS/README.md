# RDBMS

오라클DB 위주로 서술하고 나머지는 차후에 추가



(주의) 이 문서는 실무에서 RDBMS를 이용하는 방법의 실전 위주 정보를 정리한 자료임.

DB 이론과 관련된 정보는 [DB](../DB/README.md)

데이터베이스 이론 관련 정보는 [DB](https://github.com/jonghoonok/CS_Study/tree/main/DB)



참고자료

- 이지훈 : 오라클로 배우는 데이터베이스 입문
- Silver DBA Oracle Database Administration



[TOC]

이거 목차 링크 정리해야함

  * [**1. Intro**](#--1-intro--)
    + [1.1. 데이터베이스](#11-------)
    + [1.2. 관계형 데이터 베이스](#12------------)
    + [1.3. 오라클 데이터베이스](#13-----------)
  * [**2. DQL**](#--2-dql--)
    + [2.1. SELECT문의 기본 형식](#21-select--------)
    + [2.2. WHERE](#22-where)
    + [2.3. Oracle function](#23-oracle-function)
    + [2.4. 다중행 함수와 데이터 그룹화](#24----------------)
    + [2.5. JOIN](#25-join)
      - [INNER JOIN](#inner-join)
      - [OUTER JOIN](#outer-join)
    + [2.6. Subquery](#26-subquery)
  * [**3. DML, DDL**](#--3-dml--ddl--)
    + [3.1. DML](#31-dml)
    + [3.2. Transaction, Session](#32-transaction--session)
      - [isolation level](#isolation-level)
    + [3.3. DDL](#33-ddl)
    + [3.4. Object](#34-object)
    + [3.5. 제약 조건](#35------)
    + [3.6. 권한 관리](#36------)
  * [**4. PL/SQL**](#--4-pl-sql--)
    + [4.1. PL/SQL 기초](#41-pl-sql---)
    + [4.2. Record, Collection](#42-record--collection)
    + [4.3. 커서와 예외 처리](#43----------)
    + [4.4. 저장 서브프로그램](#44----------)





## **1. Intro**

> 데이터베이스와 RDBMS 기본 개념





### 1.1. 데이터베이스





데이터베이스란?

- 데이터와 정보
  - 데이터: 어떤 필요에 의해 수집했지만 아직 특정 목적을 위해 평가하거나 **정제하지 않은 자료**
  - 정보: 수집한 데이터를 어떤 목적을 위해 **분석하거나 가공한 것**
- 효율적인 데이터 관리: **데이터베이스의 필요성** 대두
  - 데이터를 통합하여 관리
  - 일관된 방법을 관리
  - 데이터의 누락, 중복 제거
  - 여러 사용자가 공동으로 실시간 사용 가능
- 데이터베이스
  - 특정 목적을 위해 여러 사람이 공유하여 사용할 수 있으며 효율적 관리와 검색을 위해 **구조화한 데이터 집합**
- 파일 시스템
  - DB등장 이전에 데이터를 관리하던 방식
  - 각각의 응용 프로그램에 필요한 데이터를 각각 저장: 중복 및 누락 발생 가능
    - 데이터의 통합 관리 필요성 대두: **DBMS**의 등장





DBMS란?

- Database Management System
- 여러 목적으로 사용할 데이터의 접근/관리 등 업무를 **DBMS가 전담**함
  - 하나의 DBMS를 통해 관리: 데이터를 관리하는 방식이 통합됨
  - 중복 및 누락 방지: 하나의 소프트웨어가 데이터를 관리
  - 공동으로 실시간 사용 가능: 데이터가 응용프로그램에 독립적이므로 프로그램 업데이트 시에도 사용 가능





데이터 모델

- 데이터를 저장하는 방식을 정의해 놓은 개념 모형

- 계층형 데이터 모델

  - 트리 구조를 활용
  - 부모 자식 관계 - 1:N

- 네트워크형 데이터 모델

  - 그래프 구조를 기반으로 함
  - 자식 개체가 여러 부모 개체를 가질 수 있음 - M:N

- 객체 지향형 데이터 모델

  - 데이터를 독립된 개체로 구성하여 관리

- **관계형 데이터 모델**

  - 데이터 간 관계에 초점

    - 각 데이터의 **독립 특성**만을 규정하여 데이터 묶음을 나눔
    - 중복이 발생할 수 있는 데이터는 별개의 **relation**으로 정의하여 연결

  - 구성 요소

    - | 이름                   | 설명                                   | 대응(구현)되는 개념  |
      | ---------------------- | -------------------------------------- | -------------------- |
      | 개체(entity)           | 데이터화하려는 사물/개념의 정보 단위   | 테이블(**relation**) |
      | 속성(attribute)        | 데이터의 종류, 특성, 상태              | 열(column)           |
      | **관계(relationship)** | 개체 간 또는 속성 간의 연관성을 나타냄 | 외래키               |






### 1.2. 관계형 데이터 베이스





관계형 데이터베이스

- 관계형 데이터 모델을 바탕으로 만들어진 데이터베이스
  - 테이블(table)로 이루어져 있으며, 이 테이블은 키(key)와 값(value)의 **관계**를 나타냄
- 관계형 데이터베이스를 관리하는 시스템은 **RDBMS**(Relational Database Management System)이라 함
  - MS-SQL, MySQL, MariaDB, PostgreSQL, DB2 등





SQL

- Structured Query Language
- RDBMS에서 데이터를 다루고 관리하는 데 사용하는 **질의 언어**
  - 조회, 데이터 조작, 객체 조작 등
  - DQL(Data Query Language)
    - 데이터를 원하는 방식으로 **조회**하는 명령어
  - DML(Data Manipulation Language)
    - **데이터를 저장, 수정, 삭제**하는 명령어
  - DDL(Data Definition Language)
    - 테이블을 포함한 여러 **객체를 생성, 수정, 삭제**하는 명령어
  - TCL(Transaction Control Language)
    - **트랜잭션 데이터의 영구 저장, 취소** 등과 관련된 명령어
  - DCL(Data Control Language)
    - **데이터 사용 권한**과 관련된 명령어





관계형 데이터베이스의 구성 요소

- 테이블
  - 2차원 표 형태의 데이터 저장 공간
  - 행: 저장하려는 **하나의 개체를 구성하는 여러 값**을 가로로 늘어뜨린 것
  - 열: 저장하려는 데이터를 **대표하는 이름과 공동 특성**
- 키
  - 수많은 데이터를 구별할 수 있는 유일한 값
  - 슈퍼키, 후보키, 기본키, 보조키(대체키), 외래키로 분류할 수 있음 





키의 종류

- 슈퍼키
  - 튜플을 **유일하게 식별**하기 위해서 사용되는 **속성들의 집합**
  - 예) 학생들의 정보를 나열한 테이블에서 학생 식별에 학번과 주민번호가 사용될 수 있음
    - 각각은 학생을 유일하게 식별할 수 있고 또 이 둘의 조합 역시 학생을 유일하게 식별할 수 있음
    - 학번, 주민번호, 학번 + 주민번호 세 가지가 슈퍼키로 사용될 수 있음
- 후보키
  - 슈퍼키이면서 **최소성을 만족시키는** 키
    - 모든 레코드를 유일하게 식별하는 데 꼭 필요한 속성만으로 되어있을 것
  - 예) 학번과 주민번호는 굳이 조합하지 않아도 모든 튜플을 유일하게 식별할 수 있음
    - 학번, 주민번호 두 가지가 후보키로 사용될 수 있음
- **기본키**
  - 후보키 중에 **개인 정보 노출이 가장 적은 데이터를 선정**하는 것이 일반적임
  - 예) 학번과 주민번호 중 학번을 기본키로 채택
  - 값이 중복되지 않아야 하고 NULL값을 가질 수 없음
- 보조키
  - 후보키 중에 기본키로 지정되지 않은 값: 속성은 기본키와 같음
- 외래키
  - 특정 테이블에 포함되어 있으면서 **다른 테이블의 기본키**로 지정된 키
  - 예) 학생 정보 테이블에 있는 학과 코드: 학과 정보 테이블과 학생 정보 테이블을 이어줌
- 복합키
  - 여러 열을 조합하여 기본키 역할을 할 수 있게 만든 키
  - 예) 과목 정보 테이블에서 (과목코드+교수+강의시간) 조합으로 수업을 유일하게 식별함 





오라클 데이터베이스

- 대표적인 상용 관계형 데이터베이스 제품
- 자료형
  - 여러 자료형이 있지만 보통 **VARCHAR2, NUMBER, DATE** 세 가지만을 사용함
- 객체
  - 테이블: 데이터를 저장하는 장소
  - 인덱스: 테이블 검색 효율을 높이기 위해 사용
  - 뷰: 데이터를 논리적으로 연결하여 하나의 테이블**처럼** 사용하게 해줌
  - 시퀀스: 일련 번호를 생성해 줌
  - 시노님: 오라클 객체의 **별칭**을 지정함
  - 프로시저: 프로그래밍 연산 및 기능 수행, return 값 없음
  - 함수: 프로그래밍 연산 및 기능 수행, return 값 있음
  - 패키지: 관련 있는 프로시저와 함수를 보관함
  - 트리거: 데이터 관련 작업의 연결 및 방지 관련 기능 제공
- PL/SQL
  - SQL과 별도로 데이터 관리를 위해 만들어진 프로그래밍 언어
  - 프로그래밍 언어에서 제공하는 요소를(변수, 조건문, 반복문) 사용하여 데이터를 관리할 수 있음





### 1.3. 오라클 데이터베이스







## **2. DQL**

> SELECT 문을 이용하여 데이터베이스를 조회, 분석하기





데이터를 조회하는 3가지 방법

- SELECTION
  - **행 단위**로 원하는 데이터를 조회
- PROJECTION
  - **열 단위**로 원하는 데이터를 조회
  - SELECTION, PROJECTION을 함께 사용하여 상세하게 조회할 수도 있음
- JOIN
  - **두 개 이상의 테이블을 연결하여 하나의 테이블처럼 조회**
  - SELECT문을 사용할 때 자주 사용됨





### 2.1. SELECT문의 기본 형식





`SELECT [조회할 열1 이름], [열2], ..., [열N] FROM [조회할 테이블 이름];`

- 조회할 열 이름에 `*`를 넣으면 전체 열 조회 가능: `SELECT * FROM EMP;`
- 띄어쓰기와 줄 바꿈 활용
  - 명령 수행에 영향을 주지 않기 때문에 가독성을 위해 **SELECT와 FROM을 다른 줄에 적는 것 권장**





중복 제거

- **DISTINCT**를 이용하여 중복을 제거하고 특정 데이터 종류만 확인 가능

  - ```SQL
    SELECT DISTINCT DEPTNO
    	FROM EMP;
    ```

- **ALL**을 이용하면 반대로 중복을 제거하지 않고 그대로 출력함

  - ```SQL
    SELECT ALL DEPTNO
    	FROM EMP;
    ```





별칭 설정

- SELECT 절에 명시한 열 이름이 아니라 별칭이(alias) 출력되도록 할 수 있음

  - 편의를 위해 + 해당 열이 어떻게 도출되었는지 노출되지 않기 위해 사용함

- **별칭을 하나씩 지정**하는 방법과 **연산식을 사용**하는 방법 존재

  - 연산식을 사용하여 연수입 출력하기(월급*12 + 상여)

  - ```SQL
    SELECT ENAME, SAL, SAL*12+COMM, COMM
    	FROM EMP;
    ```

- 별칭을 지정하는 방법

  - 한 칸 띄우기
  - 한 칸 띄우고 큰 따옴표
  - **한 칸 띄우고 AS** : 일반적으로 사용하는 방법
    - `SELECT ENAME, SAL, SAL*12+COMM AS ANNSAL, COMM FROM EMP;`
    - SQL문에 따옴표 사용이 좋지 않기 때문: 프로그래밍 언어에서 문자열로 ("SQL") 사용 시 에러 발생
    - 또 AS를 사용하면 어떤 단어가 별칭인지 알아보기 편하다는 장점도 존재
  - 한 칸 띄우고 AS 다음에 큰 따옴표 





정렬

- **ORDER BY**를 사용하여 출력 데이터를 정렬함

- SELECT문의 제일 마지막에 `ORDER BY [정렬하려는 열(여러 개 가능)] [정렬 옵션]` 을 붙여 사용

  - 정렬 옵션은 **ASC**(기본 값), **DESC** 존재

  - ```SQL
    SELECT *
    	FROM EMP
    ORDER BY SAL;
    ```

  - **여러 개 조건을 붙이면 먼저 오는 열을 우선 정렬**하고 값이 같으면 그 다음 열 기준으로 정렬

    - `ORDER BY DEPTNO DESC, ENAME;`

- 정렬은 느리기 때문에 **가능하면 사용하지 않을 것**을 권장





### 2.2. WHERE





WHERE

- **특정 조건을 기준으로** 원하는 행을 출력

  - WHERE절에는 조건식이 들어감: 조건식이 참인 경우에만 출력

- FROM 다음에 WHERE 절을 이용함

  - ```SQL
    SELECT *
    	FROM EMP
    WHERE DEPTNO = 30;
    ```

  - SQL문에서는 동등 연산자가 ==가 아니라 =임




WHERE문과 함께 사용하는 연산자

- **논리 연산자 AND, OR**을 이용하여 **여러 개의 조건식 적용도 가능**: 조건식 갯수는 무제한

  - 여러 개 조건식이 묶여 있을 때 NOT을 이용하여 한번에 뒤집는 경우 많음

- **IN**을 이용하여 조건식 갯수를 줄일 수 있음

  - OR을 여러 개 사용한 경우

  - ```SQL
    SELECT *
    	FROM EMP
    WHERE JOB = 'MANAGER'
       OR JOB = 'SALESMAN'
       OR JOB = 'CLERK';
    ```

  - IN을 사용한 경우

  - ```SQL
    SELECT *
    	FROM EMP
    WHERE JOB IN ('MANAGER', 'SALESMAN', 'CLERK');
    ```

- **BETWEEN A AND B**를 이용하여 조회하기

  - ```SQL
    SELECT *
    	FROM EMP
    WHERE SAL BETWEEN 2000 AND 3000;
    ```

- 일부 문자열이 포함된 데이터를 조회할 때 사용하는 **LIKE**

  - ENAME 열 값이 대문자 S로 시작하는 데이터를 조회하라는 SQL문

  - ```SQL
    SELECT *
    	FROM EMP
    WHERE ENAME LIKE 'S%';
    ```

  - 와일드 카드: ( `%, _` ) 특정 문자 또는 문자열을 대체함

    - 조회 성능에 영향을 미침

  - **NOT LIKE**를 이용하여 일부 문자열을 배제하여 조회하는 것도 가능

- **IS NULL**을 이용해 데이터가 NULL인 행을 조회하기

  - ```SQL
    SELECT *
    	FROM EMP
    WHERE COMM IS NULL;
    ```

  - NULL은 **비교 연산자로 비교하면 결과 값도 NULL**이 되기 때문에 IS NULL을 이용해야 함

- **집합 연산자**

  - 두 개 이상의 SELECT문의 결과 값을 연결함
  - **열 갯수와 각각의 열의 자료형이 일치해야**(열 이름은 달라도 ok)연결 가능
  - UNION: 합집합으로 묶음, 중복 제거
  - UNION ALL: 합집합으로 묶고 중복 제거하지 않음
  - MINUS: 차집합 - 먼저 작성한 SELECT문의 결과에서 다음 결과를 빼 줌
  - INTERSECT: 교집합





연산자 우선순위

- 산술연산자 곱하기, 나누기
- 산술연산자 더하기, 빼기
- 대소 비교 연산자
- 비교 연산자
  - `IS (NOT) NULL`, `(NOT) LIKE`, `(NOT) IN`
- `BETWEEN `연산자
- 논리 부정 연산자 `NOT`
- 논리 연산자 `AND`
- 논리 연산자 `OR`





### 2.3. Oracle function





오라클 함수란?

- 특정한 결과 값을 얻기 위해 데이터를 입력할 수 있는 특수 명령어
- 내장 함수
  - 오라클이 기본으로 제공하고 있는 함수
  - single-row function
    - 문자 함수
    - 숫자 함수
    - 날짜 함수
    - 형 변환 함수
  - multiple-row function
- 사용자 정의 함수





#### 2.3.1. 문자 함수







문자 함수란?

- 문자 데이터를 가공하거나 문자 데이터로부터 특정 결과를 얻고자 할 때 사용하는 함수





대-소문자 변경

- 문자열 데이터에서 특정 문자열을 포함하는 데이터를 조회할 때 유용함
  - 대소문자 양식을 통일하여 조회할 수 있음
- `UPPER(string)`
  - 문자열을 모두 대문자로 변경
- `LOWER(string)`
  - 문자열을 모두 소문자로 변경
- `INITCAP(string)`
  - 첫 글자는 대문자로, 나머지는 소문자로 변경





문자열 길이

- `LENGTH(string)`
  - 문자열의 길이를 반환함
- `LENGTHB(string)`
  - 문자열의 바이트 갯수를 반환함





문자열 추출, 인덱싱

- `SUBSTR(string, index, len)`
  - 문자열에서 index로부터 len길이의 문자열을 추출해 리턴함
  - **맨 앞 글자의 index가 1임**에 주의
    - 음수 index도 가질 수 있음
    - 근데 방향을 바꾸는 것은 안됨
  - len을 지정하지 않으면 끝까지 추출
- `INSTR(string, char, index1, index2)`
  - 문자열에서 찾으려는 문자를 지정해 위치를 리턴함
    - 없으면 0을 리턴
    - 비교 연산자와 함께 이용하면 와일드 카드로 조회하는 것과 같이 사용할 수 있음
  - index1 : 위치 찾기를 시작할 위치(optional)
    - 음수로 지정하면 오른쪽에서 왼쪽으로 검색할 수 있음
  - index2 : 시작 위치에서 찾으려는 문자가 몇 번째인지 지정(optional)





문자 대체

- `REPLACE(string, char1, char2)`
  - char1을 찾아(필수) char2(optional)로 대체함
  - char2를 지정하지 않으면 그냥 지워버림
- `LPAD(string, digit, char)`
  - 문자열의 길이가 digit보다 작으면 나머지 왼쪽 공간을 char로 채움
  - char를 지정하지 않으면 공백으로 채움
- `RPAD(string, digit, char)`
  - 문자열의 길이가 digit보다 작으면 나머지 오른쪽 공간을 char로 채움





문자열 합치기

- `CONCAT(string1, string2)`
  - 두 개의 문자열 데이터를 하나의 데이터로 연결





문자 지우기

- `TRIM(option, char FROM string)`
  - 삭제 option으로 위치를 지정할 수 있음
    - LEADING : 앞서 가는 문자들(왼쪽)을 지움
    - TRAILING : 따라오는 문자들(오른쪽)을 지움
    - BOTH : 양쪽 모두 지움
    - 지정 안 하면 기본적으로 BOTH
  - 삭제할 문자 char를 지정하지 않으면 공백이 제거됨
- `LTRIM(string, chars)`
  - 왼쪽에서 chars에 포함되는 문자들을 제거
  - chars를 지정하지 않으면 공백이 제거됨
- `RTRIM(string, chars)`
  - 오른쪽에서 chars에 포함되는 문자들을 제거





#### 2.3.2. 숫자 함수





반올림, 버림

- `ROUND(숫자, 반올림 위치)`
  - 지정된 숫자의 **특정 위치**에서 반올림한 값을 반환
    - 소숫점 몇 째 자리까지 남기는지
    - 1로 하면 첫 째 자리에서 반올림하는게 아니라 첫 째 자리까지 표시함
  - 위치를 지정하지 않으면 기본값 0으로 취급
- `TRUNC(숫자, 버림 위치)`
  - 지정된 숫자의 **특정 위치**에서 버린 값을 반환
  - ROUND와 같은 방식





MAXMIN, MINMAX

- `CEIL(숫자)`
  - 입력된 숫자보다 큰 정수 중 가장 작은 것을 반환
- `FLOOR(숫자)`
  - 입력된 숫자보다 작은 정수 중 가장 큰 것을 반환





나머지

- `MOD(피제수, 제수)`
  - 피제수를 제수로 나눈 나머지를 반환





#### 2.3.3. 날짜 함수





`SYSDATE`

- 오라클 데이터베이스 서버가 놓인 OS의 현재 날짜와 시간을 보여줌





날짜 간 연산

- 날짜 - 날짜
  - 두 날짜 데이터 간의 **일수 차이**
  - 날짜 + 날짜는 지원하지 않음
- `MONTHS_BETWEEN(날짜1, 날짜2)`
  - 두 날짜 데이터 간의 개월 수 차이를 구함
  - 날짜1이 더 최근이면 양수로, 반대면 음수로 출력





날짜와 숫자의 연산

- 날짜 + 숫자
  - 날짜 데이터에 숫자만큼 일수를 더한 날짜
- 날짜 - 숫자
  - 날짜 데이터에 숫자만큼 일수를 뺀 날짜
- `ADD_MONTHS(날짜, 개월 수)`
  - 날짜 데이터에 지정한 개월 수 이후의 날짜 데이터를 반환





돌아오는 요일, 달의 마지막 날짜 구하기

- `NEXT_DAY(날짜 데이터, 요일 문자)`
  - 날짜 데이터를 기준으로 **돌아오는 요일**의 날짜를 출력
- `LAST_DAY(날짜 데이터)`
  - 날짜 데이터를 기준으로 **해당 날짜가 속한 달**의 마지막 날짜를 출력





반올림, 버림

- `ROUND(날짜데이터, 반올림 기준 포맷)`
  - 소수점 위치 정보 대신 반올림의 기준이 될 **포맷 값**을 지정해 줌
  - [포맷 값](https://docs.oracle.com/cd/B28359_01/olap.111/b28126/dml_commands_1029.htm#OLADM780) : 날짜 데이터를 표시하는 형식
- `TRUNC(날짜데이터, 버림 기준 포맷)`





#### 2.3.4. 형 변환 함수





오라클의 형 변환

- implicit : 숫자처럼 생긴 문자 데이터는 숫자로 변환하기가 가능
  - 예) `EMPNO + '500'`
- explicit : 그 외의 경우에는 명시적으로 형 변환이 필요
  - 문자를 중심으로 숫자 또는 날짜 데이터의 변환이 가능함





숫자, 날짜를 문자로

- `TO_CHAR(날짜 데이터, 출력되길 원하는 문자 형태)`
  - 주로 날짜를 문자 데이터로 변환하는 데 사용됨
  - 문자 형태에는 날짜 데이터 포맷을 입력하면 됨
    - 예) `SELECT TO_CHAR(SYSDATE, 'YYYY/MM/DD HH24:MI:SS')`
  - 숫자를 문자 데이터로 변환하는 경우 숫자 데이터 출력 형식을 지정해야 함





문자를 숫자, 날짜로

- `TO_NUMBER(문자열 데이터, 숫자 형태)`
- `TO_DATE(문자열 데이터, 날짜 형태)`





#### 2.3.5. 기타 함수





NULL 처리 함수

- `NVL(검사할 데이터, NULL일 경우 반환할 데이터)`
  - 검사할 데이터가 NULL이 아니면 그대로 반환함
- `NVL2(검사할 데이터, NULL이 아닐 경우 반환할 데이터 ,NULL일 경우 반환할 데이터)`
  - 예) `NVL2(COMM, 'O', 'X')`





`DECODE`

- switch-case 조건문과 비슷하게 사용할 수 있음

- 기준이 되는 데이터를 먼저 지정한 후 해당 데이터 값에 따라 다른 결과 값을 내보내는 함수

  - ```sql
    SELECT EMPNO, ENAME, JOB, SAL,
    	DECODE(JOB,
              	'MANAGER', SAL*1.1,
              	'SALESMAN', SAL*1,05,
              	'ANALYST', SAL,
              	SAL*1.03) AS UPSAL		-- 조건과 일치한 경우가 없을 때
    FROM EMP;
    ```

- **조건별로 동일한 자료형의 데이터를 반환해야 함**





`CASE`

- DECODE와 마찬가지로 기준 데이터를 명시하는 방식, 기준 데이터를 지정하지 않는 방식이 있음
- IF문과 비슷한 느낌으로 사용 가능
- **조건별로 동일한 자료형의 데이터를 반환해야 함**





### 2.4. 다중행 함수와 데이터 그룹화





다중행 함수

- 여러 행을 바탕으로 하나의 결과 값을 도출해 내는 함수
  - 다중행 함수를 사용한 SELECT 절에는 여러 행이 결과로 나올 수 있는 열을 함께 사용할 수 없음
- `SUM(옵션, 합계를 구할 열이나 데이터)`
  - 옵션으로 DISTINCT, ALL을 선택할 수 있음
  - DISTINCT를 선택하면 같은 결과 값을 가진 데이터는 합계에서 한 번만 사용
  - 아무것도 선택하지 않으면 디폴트로 ALL이 선택됨
- `COUNT(옵션, 갯수를 구할 열이나 데이터)`
  - SUM과 마찬가지로 DISTINCT, ALL을 선택할 수 있음
- `MAX(옵션, 최댓값을 구할 열이나 데이터)`
  - DISTINCT, ALL을 선택할 수 있음
  - 날짜 및 문자 데이터도 사용할 수 있음 : 날짜는 최근일 수록 큼
- `MIN(옵션, 최솟값을 구할 열이나 데이터)`
  - DISTINCT, ALL을 선택할 수 있음
- `AVG(옵션, 평균값을 구할 열이나 데이터)`
  - DISTINCT, ALL을 선택할 수 있음





`GROUP BY`

- 다중행 함수의 결과를 **특정 열 값을 기준으로 묶어서 출력**하고 싶을 때 사용함

  - "**Project tuples into subsets** and calculate aggregater against each subset"

  - 예) 각 부서별 평균 급여 출력하기

    - ```sql
      SELECT AVG(SAL), DEPTNO
      	FROM EMP
      GROUP BY DEPTNO;
      ```

- 다중행 함수를 사용하지 않은 일반 열은 GROUP BY 절에 명시하지 않으면 SELECT절에 사용할 수 없음

  - 예) `SELECT ENAME, DEPTNO, AVG(SAL)`





`HAVING`

- GROUP BY 절을 통해 **그룹화된 결과 값의 범위를 제한**하는 데 사용함

- WHERE절은 출력 대상 행을 제한하고, HAVING절은 **그룹화된 대상을 출력에서 제한**함

  - WHERE절에서는 그룹화된 데이터를 제한하는 조건식을 지정할 수 없음

  - WHERE절은 연산이 수행되기 전에 필터링을 하고, HAVING절은 연산 결과를 받아서 걸러내기 때문

  - ```sql
    SELECT DEPTNO, JOB, AVG(SAL)
    	FROM EMP
    WHERE AVG(SAL) > 2000			-- 오류 발생
    GROUP BY DEPTNO, JOB;
    ```





그룹화 관련 함수

- `ROLLUP`

  - 그룹화된 열을 지정하여(여러 개 지정 가능) 해당 열의 합계를 출력

  - 지정한 열을 소그룹부터 대그룹의 순서로 각 그룹별 결과를 출력하고 마지막에 총 합계를 출력

  - ```sql
    SELECT fact_1_id,
           fact_2_id,
           SUM(sales_value) AS sales_value
    FROM   dimension_tab
    GROUP BY ROLLUP (fact_1_id, fact_2_id)
    ORDER BY fact_1_id, fact_2_id;
    
     FACT_1_ID  FACT_2_ID SALES_VALUE
    ---------- ---------- -----------
             1          1     4363.55
             1          2     4794.76
             1          3     4718.25
             1          4     5387.45
             1          5     5027.34
             1               24291.35
             2          1     5652.84
             2          2     4583.02
             2          3     5555.77
             2          4     5936.67
             2          5     4508.74
             2               26237.04
                             50528.39
    ```

- `CUBE`

  - 그룹화된 열을 지정하여 해당 열의 합계를 출력
  - 지정한 모든 열에서 가능한 조합의 결과를 모두 출력
    - ROLL UP은 N+1개의 조합이 출력되고, CUBE는 2^N개의 조합이 출력됨

- `GROUPING SETS`

  - 그룹화된 열을 지정하여 해당 열의 합계를 출력
  - ROOLUP, CUBE와 달리 계층별로 분류하지 않고 각각 따로 그룹화한 후 연산을 진행





### 2.5. JOIN



![join](https://miro.medium.com/max/1932/0*Mu_d-mJMmaVX-j0P)





JOIN이란?

- 중복을 없애기 위해서 **정규화**한 테이블들에서 원하는 결과를 도출하기 위해 **다시 조합**하는 것
- 적어도 하나의 칼럼을 서로 공유하고 있는 테이블들을 연결하여 **데이터 검색에 활용**함
  - 보통 공통된 값인 PK 및 FK 값을 사용하여 JOIN
  - JOIN 연산자에 따라, FROM절의 JOIN 형태에 따라서 구별함
- 집합 연산자는 SELECT문의 결과 값을 세로로, JOIN은 가로로 연결한 것이라고 볼 수 있음





JOIN의 종류

- 연산자에 따른 분류
  - EQUI JOIN
    - 테이블을 연결한 후에 출력 행을 각 테이블의 특정 열에 일치하는 데이터를 기준으로 선정
    - `=`연산자 사용
  - NON EQUI JOIN
    - EQUI JOIN 이외의 방식
    - 비교 연산자나 BETWEEN A AND B 연산자를 사용
- FROM 절의 JOIN 형태에 따른 분류
  - INNER JOIN : JOIN 조건에서 **값이 일치하는 행만** 반환
  - OUTER JOIN : JOIN 조건에서 **한쪽 값이 없더라도** 행을 반환





JOIN의 동작 방식

- JOIN이 어떤 방식으로 동작하고 있는지는 **실행계획**에서 확인 가능
  - 적절하지 않은 경우 오라클 힌트를 이용하여 변경 가능
- NESTED LOOP JOIN : 중첩 for문과 같은 방식으로 **JOIN 조건을 반복적으로 체크**하면서 수행
  - INNER TABLE의 INDEX를 이용하지 않으면 굉장한 비효율이 발생함
  - 1:N관계를 가지는 경우 1인 쪽이 OUTER TABLE로 설정되는 것이 좋음 
  - 예)걸그룹 테이블과 멤버 이름 테이블을 그룹명 기준으로 JOIN할 때 그룹을 기준으로 멤버 이름 테이블을 scan하면서 멤버 이름을 가져와야 함
- SORT MERGE JOIN : 두 테이블을 각각 JOIN 컬럼 기준으로 sort한 후 JOIN을 수행
  - INNER TABLE에 적절한 INDEX가 없어서 NESTED LOOP JOIN을 사용하기 비효율적인 경우에 이용
  - 범위로 JOIN을 하는 경우에도 (NON EQUI JOIN) 적절함
  - sorting은 PGA영역에서 수행됨 : 경합이 발생하지 않아 성능 상 이점이 있음
- HASH JOIN
  - OUTER TABLE을 **Build Input**으로 삼아서 Hash 영역에 저장하고, INNER TABLE의 JOIN 컬럼을 기준으로 해시 함수를 적용하여 해시 테이블을 탐색하면서 JOIN을 수행
  - EQUI JOIN만 가능함
  - INNER TABLE이 대용량이고 OUTER TABLE이 작을 때(Hash영역 크기 제한) 좋음
  - 해시 키 값으로 사용되는 컬럼에 중복이 적어야 함
  - 배치에서 쓰면 좋음





#### INNER JOIN

![INNER JOIN](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile9.uf.tistory.com%2Fimage%2F99799F3E5A8148D7036659)

EQUI JOIN

- 기준 테이블과 JOIN할 테이블의 **칼럼 값들이 서로 정확하게 일치하는 경우** 겹치는 값들을 가져오는 것

- WHERE 절에 `=`  동등 연산자를 사용하여 JOIN 조건을 명시

- 일반적으로 PK, FK로 지정된 칼럼을 JOIN으로 많이 사용함

- EQUI JOIN의 성능을 높이려면 **INDEX 기능을 사용하는 것이 좋음**

  - 조건이 일치하는지 확인하기 위해 매번 FULL SCAN을 하면 굉장히 비효율적이기 때문

- 예) 테이블 A와 B에서 각각 이름과 나이를 가져옴 

  - ```sql
    SELECT A.NAME, B.AGE
    FROM EX_TABLE A				--FROM EX_TABLE A, JOIN_TABLE B로 쓰면 JOIN구문 생략 가능
    EQUI JOIN JOIN_TABLE B		--EQUI JOIN을 INNER JOIN으로 변경 가능
    ON A.NO_EMP = B.NO_EMP
    ```

  - 사원 번호가 같은 사람에 대해 조회 : 사원 별 나이-이름 테이블이 된다





NATURAL JOIN

- 두 테이블의 **동일한 이름을 가지는 칼럼을 모두 JOIN하는 것**

- EQUI JOIN에서 동일한 속성이 두 번 나타나게 되는데, **중복을 제거하여 같은 속성을 한번만 표기**함

  - ```sql
    SELECT A.NAME, B.AGE
    FROM EX_TABLE A NATURAL JOIN JOIN_TABLE B
    ```

  - USING문을 사용하면 칼럼을 선택해서 JOIN할 수 있음

  - ```sql
    SELECT A.NAME, B.AGE
    FROM EX_TABLE A JOIN JOIN_TABLE B
    USING No_EMP;
    ```

- 테이블 별칭(alias)을 사용하면 오류가 발생함





NON EQUI JOIN

- 조인 대상 테이블의 **어떤 칼럼 값도 일치하지 않을 때** 사용하며 '=' 이외의 연산자를 사용

- 사용 빈도가 매우 낮다고 함

- 연산자 : BETWEEN AND, IS NULL, IS NOT NULL, IN, NOT IN, < , >, >=, <=

- ```sql
  SELECT [테이블명1.]속성명, [테이블명2.]속성명 ....
  FROM 테이블명1, 테이블명2..
  WHERE (NON EQUI JOIN 조건)	--JOIN 키워드는 안 쓰고 그냥 조인함
  ```





SELF JOIN

- **같은 테이블 내에서** 2개의 속성을 연결하여 EQUI JOIN을 하는 것

  - 주의할 점 : JOIN할 때 **별칭을 반드시 입력**해주어야 함
  - JOIN할 테이블을 복사하여 조인하는 방법도 있으나 자원이 낭비되기 때문에 별칭 지정으로 해결

- 한 테이블 내에 이미 종속 관계가 있을 때 사용(학번 - 학생 이름 - 선배의 학번)

- ```sql
  SELECT a.S_NO AS 학번, a.S_NAME AS 이름, b.S_NAME AS 선배
  FROM STUDENT a, STUDENT b
  WHERE a.S_SENIOR = b.S_NO;
  ```





#### OUTER JOIN

![LEFT OUTER JOIN](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile6.uf.tistory.com%2Fimage%2F997E7F415A81490507F027)

LEFT OUTER JOIN

- JOIN의 왼쪽에 표기된 데이터를 기준으로 OUTER JOIN을 수행함

- 예) 학생 테이블과 학과 테이블에서 학과 코드 값이 같은 튜플을 JOIN하여 출력

  - ```sql
    SELECT s.S_NO, s.S_NAME, s.D_CODE, d.D_NAME
    FROM STUDENT s LEFT OUTER JOIN DEPARTMENT d
    ON s.D_CODE = d.D_CODE;
    ```

  - INNER JOIN과 달리 **학과 정보가 NULL인 학생도 출력됨**





RIGHT OUTER JOIN

- JOIN의 왼쪽에 표기된 데이터를 기준으로 OUTER JOIN을 수행함

- LEFT OUTER JOIN이랑 다른 것은 위치 뿐이니 위 예제를 다른 문법으로 해보자

  - ```sql
    SELECT s.S_NO, s.S_NAME, s.D_CODE, d.D_NAME
    FROM STUDENT s, DEPARTMENT d
    WHERE s.D_CODE(+) = d.D_CODE;	--LEFT OUTER JOIN의 경우 오른쪽에 (+)를 붙이면 됨
    ```

  - 이번엔 학생이 없는(학생 정보가 NULL인 학과)도 출력됨





FULL OUTER JOIN

- LEFT OUTER JOIN과 RIGHT OUTER JOIN의 결과를 합집합으로 처리한 결과와 동일함

- LEFT, RIGHT OUTER JOIN과 다르게 JOIN을 명시하는 경우만 가능

  - ```sql
    SELECT s.S_NO, s.S_NAME, s.D_CODE, d.D_NAME
    FROM STUDENT s FULL OUTER JOIN DEPARTMENT d
    ON s.D_CODE = d.D_CODE;
    ```

  - 학과코드가 입력 안 된 학생이나 학과명이 없는 학과코드도 모두 출력됨





CROSS JOIN

- Cartesian Product라고도 하며 곱집합을 반환함

- 두 테이블 간에 연결될 수 있는 모든 경우의 수를 산출하여 나타내는 JOIN

- ```sql
  SELECT A.COLOR, B.SIZE
  FROM EX_TABLE A
  CROSS JOIN JOIN_TABLE B
  ```

![cross join](https://www.essentialsql.com/wp-content/uploads/2016/08/Cross-Join-Two-Tables-to-Get-Combinations.png?ezimgfmt=ng%3Awebp%2Fngcb11%2Frs%3Adevice%2Frscb11-1)





### 2.6. Subquery





## **3. DML, DDL**



### 3.1. DML







### 3.2. Transaction, Session

트랜잭션이란?

- 하나의 그룹으로 처리되어야 하는 여러 **쿼리들을 묶은 작업의 논리적 단위**
  - 트랜잭션은 DML 쿼리들의 조합으로 구성됨
  - 예) 송금 트랜잭션 : 돈 보내는 사람의 잔고 UPDATE + 받는 사람의 잔고 UPDATE
  - 쿼리들을 논리적으로 그룹화하기 위해 **커밋**과 **롤백**이 이루어 짐
- 트랜잭션은 한 **작업의 완전성**을 보장함
  - 트랜잭션 내의 모든 처리대상 명령문들이 반드시 완전히 수행되어야 함
  - 어느 한 문장이라도 에러가 발생한다면 트랜잭션으로 묶인 전체 명령문은 모두 취소



트랜잭션의 성질 ACID

- Atomicity
  - 트랜잭션 중간에 어떠한 문제가 발생한다면 트랜잭션에 해당하는 어떠한 작업 내용도 수행되어서는 안되며 아무런 문제가 발생되지 않았을 경우에만 모든 작업이 수행되어야 함
- Consistency
  - 트랜잭션은 발생 전후에 **데이터의 일관성**을 보장해야 함
  - 데이터의 일관성 : 한 데이터를 서로 다른 장소에서 참조했을 때 조회 결과가 일치하는 것
- Isolation
  - 각 트랜잭션은 서로 간섭 없이 실행되어야 함
  - isolation이 보장되지 않으면 트랜잭션이 원래 상태로 되돌아갈 수 없음
- Durability
  - 트랜잭션이 종료되면 영구적으로 DB에 작업의 결과가 저장되어야 함
  - 이를 위해 트랜잭션 종료시 커밋 또는 롤백을 수행함



커밋과 롤백

- 트랜잭션을 논리적인 작업의 단위로 구분하기 위해  커밋과 롤백을 이용함
  - 이를 통해 **데이터 무결성**을 보장할 수 있음
- 커밋
  - 트랜잭션으로 묶인 모든 쿼리가 실행되어 성공하면 트랜잭션의 결과를 DB에 **영구적으로 저장**하는 작업
  - 영구적으로 변경하기 전에 데이터의 변경사항을 확인할 수 있음
  - DDL과 DCL 명령문이 수행되면 자동으로 커밋됨
- 롤백
  - 트랜잭션의 쿼리가 하나라도 실패하면 **실행 결과를 모두 취소하고 DB를 트랜잭션 실행 전의 상태로 되돌리는 것**
  - 컴퓨터가 다운되거나 SQL 프로그램이 비정상 종료되면 자동으로 롤백됨



트랜잭션의 복구

- UNDO
  - 사용자가 했던 작업을 반대로 진행하여 원상태로 되돌림
  - 롤백 작업 시에는 UNDO를 이용함
- REDO
  - 사용자가 했던 작업을 그대로 반복하여 진행
  - DB서버에는 기본적으로 모든 작업이 REDO에 기록됨 : 장애가 발생해도 안 날아감
- 장애 복구의 흐름
  1. REDO 데이터를 이용해서 마지막 Check point부터 장애까지의 버퍼 캐시를 복구
  2. UNDO를 이용해서 커밋되지 않은 데이터를 모두 롤백하여 복구 완료



DB 회복 기법

- Log 기반 회복 기법
- 지연 갱신 회복 기법
  - Write 연산을 하지 않고 로그에 DB변경 내역만 저장 : **write 연산은 트랜잭션 완료시 로그를 보고 수행**
  - 트랜잭션 완료시 장애 발생 : write을 아직 하지 않았기 때문에 REDO만 수행
  - 트랜잭션 미완료시 장애 발생 : 로그 무시
- 즉시 갱신 회복 기법
  - 즉시 DB 변경(write 연산 수행) + 로그에 기록
  - 장애 발생 시 로그에 기반하여 **UNDO 실행**
- 체크포인트 회복기법
  - 체크 포인트를 지정하여 장애발생시 체크포인트까지 UNDO 실행 후 다시 REDO 실행
- 그림자 페이징 회복 기법 
  - 하드디스크에 그림자 페이지를 만들고 저장
  - 장애 발생시 하드디스크에 있는 페이지로 주메모리 페이지 변경
  - 장애 미발생시 그림자 페이지 테이블은 삭제



데이터 무결성

- 데이터의 정확성, 일관성, 유효성을 유지하는 것
- 4가지 종류가 있음
  - 개체 **무결성** (Entity integrity) 
    - 모든 테이블이 기본 키 (primary key)로 선택된 필드 (column)를 가져야 함
    - 기본키로 선택된 필드는 빈 값을 허용하지 않음
  - 참조 **무결성** (Referential integrity) 
    - 서로 참조 관계에 있는 두 테이블의 데이터는 항상 일관된 값을 유지함
    - (참조되는 테이블의)기본키와 (참조하는 테이블의)참조키 간의 관계는 항상 유지되어야 함
    - FK의 값은 NULL이거나 참조하는 테이블의 PK 값 중 하나여야 함
  - 도메인 **무결성** (Domain integrity)
    - 테이블에 존재하는 필드의 무결성을 보장하기 위한 것으로 올바른 데이터가 입력됐는지를 체크하는 것
    - 예) 성별 속성의 도메인은 남, 여로 그 외의 값은 입력 불가
  - **사용자 정의 무결성** 규칙 (User-defined Integrity)
    - 사용자가 정의한 무결성 규칙으로 개체, 참조, 도메인 무결성에 포함되지 않는 것



참조 규칙

- 참조되는 테이블에 변경이 있을 때 **참조 무결성을 지키기 위해** SQL에 설정되어 있는 규칙
  - 실수로 관련 데이터가 삭제되거나 수정되는 것을 막아줌
- 참조 무결성 강화 규칙으로 다음과 같이 5가지의 설정이 가능함
  - CASCADE
    - 참조되는 테이블의 레코드를 삭제할 때, 참조하는 테이블의 레코드가 **자동으로 삭제**됨
    - 참조되는 테이블의 기본 키 필드값이 바뀌면 참조하는 테이블의 필드값도 변경됨
  - RESTRICT
    - 참조하는 테이블에 일치하는 레코드가 있다면 참조되는 테이블의 레코드를 갱신/삭제할 수 없음
  - NO ACTION
    - 참조되는 테이블에 대해 갱신 또는 삭제를 했을 때, SQL문장 실행 종료 시에 FK 제약을 만족하는지 체크하고 만족하면 SQL문 실행이 성공, 아니면 실패하도록 함
  - SET NULL
    - 참조되는 테이블의 행이 갱신 또는 삭제되면 참조하는 테이블에서 대응되는 행의 FK 값이 NULL이 됨
  - SET DEFAULT
    - SET NULL과 동일한 경우에 FK 값이 NULL이 아니라 DEFAULT값이 됨



#### isolation level

여러 트랜잭션이 동시에 DB에 접근할 때 어떻게 제어할지에 대한 설정

- isolation level에 따라 **성능을 얻기 위해서 정합성을 얼마나 포기할 것인지** 정함
- 데이터 정합성과 성능(동시성)은 trade-off 관계를 가지고 있음
  - ACID를 엄격히 지키면 성능 저하
  - ACID를 완벽히 지키는 4레벨부터(Serializable) 지키지 않는 1레벨까지 4단계 존재함



Read-Uncommitted

- 트랜잭션이 커밋 되기 전에 데이터가 변경된 내용을 다른 트랜잭션이 조회 가능
- **Dirty Read**, Non-Repeatable Read, Phantom Read 발생 가능
  - 한 트랜잭션이 수정한 데이터를 다른 트랜잭션이 조회한 후 롤백이 일어난 경우
  - 무효가 된 데이터를 읽고 처리했기 때문에 문제가 생김



Read-Committed

- 커밋이 완료된 트랜잭션의 변경 사항만 다른 트랜잭션에서 조회 가능
- 아직 커밋되지 않았다면 변경 전 데이터를, 커밋됐다면 변경 후의 데이터를 읽음
- **Non-Repeatable Read**, Phantom Read 발생 가능
  - 한 트랜잭션에서 수정하고 커밋한 데이터를 다른 트랜잭션에서 커밋 이전과 이후에 각각 조회한 경우
  - 두 번의 조회에 대해 값이 다르다는 문제가 생김



Repeatable-Read

- 한 트랜잭션이 조회한 데이터는 항상 동일함을 보장
- 조회를 한 번 했으면 트랜잭션 종료시까지 다른 트랜잭션이 변경하거나 삭제 불가
- **Phantom Read** 발생 가능
  - 어떤 범위의 레코드를 읽었을 때 첫 번째 쿼리에선 없던 데이터가 새로 생긴 경우



Serializable

- 한 트랜잭션에서 사용하는 데이터를 다른 트랜잭션에서 접근 불가



### 3.3. DDL





### 3.4. Object





### 3.5. 제약 조건





### 3.6. 권한 관리







## **4. PL/SQL**



### 4.1. PL/SQL 기초





### 4.2. Record, Collection





### 4.3. 커서와 예외 처리





### 4.4. 저장 서브프로그램