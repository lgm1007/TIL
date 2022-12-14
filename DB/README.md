# Database
* 여러 사람에 의해 공유되어 사용될 목적으로 통합되어 관리되는 데이터의 집합
* 다수의 사용자가 사용하는 데이터들의 공유와 운영을 위해 저장해놓는 공간
* 즉 자료 파일을 조직적으로 통합하여 자료 항목의 중복을 없애고 자료를 구조화하여 기억시켜 놓은 자료의 집합체
### Database 사용 이유
* 프로그램을 개발하다보면 프로그램 사용자들에 의해 만들어진 데이터나 프로그래머가 필요에 의해 만든 데이터 등 필연적으로 많은 데이터들이 만들어지는데, 데이터베이스를 사용하지 않으면 해당 데이터들은 프로그램을 종료하는 순간 모두 사라져버린다.
  * 이런 문제를 방지하기 위해 데이터들을 보관해놓을 공간이 필요해지면서 데이터베이스에 필요한 데이터들을 저장하여 사용하면서 사용하게 되었다.
### Database 용어
* **식별자(Identifier)** : 여러 개의 집합체를 담고 있는 관계형 데이터베이스에서 각각의 구분할 수 있는 논리적인 개념
  * 식별자의 특징
    * 유일성 : 하나의 릴레이션에서 모든 행의 서로 다른 키 값을 가져야 한다.
    * 최소성 : 꼭 필요한 최소한의 속성들로만 키를 구성해야 한다.
* **튜플(Tuple)** : 테이블에서 행을 의미, 레코드(Recode) 혹은 로우(Row)라고도 한다.
  * 튜플의 특징
    * 릴레이션에서 같은 값을 가질 수 없다.
    * 튜플의 수는 카디널리티(Cardinality)라고 한다.
* **어트리뷰트(Attribute)** : 테이블에서 열을 의미, 칼럼(Column) 이라고도 한다.
  * 어트리뷰트의 특징
    * 어트리뷰트의 수는 디그리(Degree)라고 한다.
### Database 쿼리 문법 종류
1. **데이터 정의 언어(DDL)** : 관계형 데이터베이스의 구조를 정의
    * 쌍, 속성, 관계 인덱스 파일 위치 등 데이터베이스 고유의 특성을 포함
2. **데이터 조작 언어(DML)** : 데이터베이스 검색, 등록, 삭제, 갱신을 하기 위해 사용하는 데이터베이스 언어
    * 데이터베이스의 검색 및 업데이트 등 데이터 조작을 위해 사용
3. **데이터 제어 언어(DCL)** : 데이터베이스에서 데이터에 대한 엑세스의 제어를 위한 데이터베이스 언어
    * 박탈, 연결, 권한 부여, 질의, 자료 삽입, 갱신, 삭제 등
