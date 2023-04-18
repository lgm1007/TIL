# MongoDB
* Document 지향적인 NoSQL 데이터베이스
### Document 의미
* RDBMS 의 레코드와 비슷한 개념
  * RDBMS 의 레코드보다 훨씬 더 유연하고 다양한 구조를 갖을 수 있다.
* BSON(Binary JSON) 형식으로 구성
* Document 샘플
```mongodb-json
{
    "_id": ObjectId("617eb2915f5de5c5b5ba3a5f"),
    "name": "Jason Joe",
    "age": 31,
    "address": {
        "city": "Pasadena, California",
        "country": "USA"
    },
    "phone-number": [
        "213-123-4567",
        "213-456-7890"
    ],
    "active": true
}
``` 
### Collection
* MongoDB Document의 그룹
  * Document 들이 Collection 내부에 위차하게 된다.
* RDBMS 의 테이블과 유사한 개념이지만 테이블과 달리 스키마를 따로 가지지 않는다.
### RDBMS 와의 비교
|RDBMS|MongoDB|
|---|---|
|Database|Database|
|Table|Collection|
|Tuple / Row|Document|
|Column|Key/Field|
|Table Join|Embedded Documents|
|Primary Key|Primary Key(_id)|
||**Database Server & Client**|
|mysqld|mongod|
|mysql|mongo|
### MongoDB 특징
* 스키마가 없기 떄문에 언제든지 필드를 추가하거나 삭제할 수 있다.
* 데이터를 복제하거나 분산 처리 기능을 내장하고 있어 대규모 데이터 처리 및 관리에 용이하다.
* 분산 처리 기능을 내장하고 있어, 고가용성과 스케일 아웃 기능을 쉽게 구현할 수 있다.
* 여러 언어와 프레임워크에서 사용할 수 있도록 다양한 드라이버를 제공한다.
### MongoDB 데이터 모델링
* **스키마 디자인 시 고려사항**
  * 객체들을 함께 사용하게 된다면 **한 Document에 합쳐 사용** (예: 게시물-댓글)
    * 그렇지 않으면 따로 사용하고, Join을 사용하지 않는 것을 확실하게 해둔다.
    * 데이터를 읽을 때 Join하는 게 아니라, 데이터를 작성할 때 Join 한다.
  * 사용자 요구에 따라 디자인한다.
* **데이터베이스 디자인 요구사항** (feat. ChatGPT)
  * ```
    사용자는 이름, 이메일, 비밀번호 필드를 가지고 있어야 한다.
    사용자는 여러 개의 주소를 가질 수 있다. 주소는 도시, 우편번호, 주소1, 주소2 필드를 가지고 있다.
    제품은 이름, 가격, 제조사 필드를 가지고 있어야 한다.
    제품은 여러 개의 리뷰를 가질 수 있다. 리뷰는 사용자 이름, 별점, 내용, 생성 시간 필드를 가지고 있다.
    ```
  * RDBMS 였다면, 사용자, 주소, 제품, 리뷰 4개의 테이블을 만들었어야 할 것이다.
  * MongoDB에서는 모든 객체를 하나의 Document에 작성한다.
  * ```mongodb-json
    {
        _id: ObjectId(USER_ID),
        name: USER_NAME,
        email: USER_EMAIL,
        password: USER_PASSWORD,
        address: {
            city: ADDRESS_CITY,
            zipcode: ADDRESS_ZIPCODE,
            fields: [ADDRESS_FIELD1, ADDRESS_FIELD2]
        }
    }
    ```
  * ```mongodb-json
    {
        _id: ObjectId(PRODUCT_ID),
        name: PRODUCT_NAME,
        price: NumberDecimal(PRODUCT_PRICE),
        company: PRODUCT_COMPANY,
        review: [
            {
                _id: ObjectId(REVIEW_ID1),
                username: USER_NAME,
                rated: REVIEW_RATED,
                content: REVIEW_CONTENT, 
                created: REVIEW_CREATED 
            },
            {
                _id: ObjectId(REVIEW_ID2),
                username: USER_NAME,
                rated: REVIEW_RATED,
                content: REVIEW_CONTENT, 
                created: REVIEW_CREATED 
            }
        ],
    }
    ```
### MongoDB 명령어
#### DDL (Data Definition Language)
1. use
   * 데이터베이스를 선택한다.
```
use myDatabase
```
2. show
   * 현재 서버에 존재하는 데이터베이스 리스트를 출력하거나 현재 선택된 데이터베이스의 컬렉션 리스트를 출력한다.
```
# 현재 서버에 존재하는 데이터베이스 나열
show dbs

# 현재 선택된 데이터베이스의 컬렉션 리스트 나열
show collections
```
3. db.createCollection()
   * 새로운 컬렉션을 생성하는 명령어
```
db.createCollection("<collectionName>")
```
4. db.dropCollection()
   * 지정한 컬렉션을 삭제하는 명령어
```
db.dropCollection("<collectionName">)
```
5. db.collection.createIndex()
   * 인덱스 생성 명령어
   * fieldName 필드를 1순위로 오름차순 정렬하는 인덱스 생성
```
db.collection.createIndex({ fieldName: 1 })
```
6. db.collection.dropIndex()
   * fieldName 필드를 1순위로 오름차순 정렬하는 인덱스 삭제
```
db.collection.dropIndex({ fieldName: 1 })
```
7. db.createView()
   * collectionName 컬렉션에서 fieldName 필드의 값이 "value"인 데이터만 뽑아내어 viewName이라는 뷰 생성
```
db.createView("viewName", "collectionName", [{ $match: { fieldName: "value" } }])
```
8. db.`<viewName>`.drop()
   * `viewName` 이라는 뷰를 삭제하는 명령어
```
db.<viewName>.drop()
```
9. db.createUser()
   * `username` 이라는 사용자 생성
   * 해당 사용자에 대해 databaseName 데이터베이스의 readWrite 권한 부여
```
db.createUser({ user: "username", pwd: "password", roles: [{ role: "readWrite", db: "databaseName" }] })
```
10. db.updateUser()
    * `username` 이라는 사용자의 pwd 값을 업데이트
```
db.updateUser("username", { pwd: "newPassword" })
```
11. db.dropUser()
    * `username` 사용자를 삭제
```
db.dropUser("username")
```
12. db.createRole()
    * `roleName` 이라는 역할 생성하고, 해당 역할에 대한 databaseName 데이터베이스의 collectionName 컬렉션에서 find, update 권한 부여
```
db.createRole({ role: "roleName", privileges: [{ resource: { db: "databaseName", collection: "collectionName" }, actions: ["find", "update"] }], roles: [] })
```
13. db.updateRole()
    * `roleName` 이라는 역할에 대해 `databaseName` 데이터베이스의 `collectionName` 컬렉션에서 find, insert 권한을 부여하도록 업데이트
```
db.updateRole({ role: "roleName", privileges: [{ resource: { db: "databaseName", collectionName: "collectionName" }, actions: ["find", "insert"] }]})
```
14. db.dropRole()
    * `roleName` 역할 삭제
```
db.dropRole("roleName")
```
#### DML (Data Manipulation Language)
1. db.collectionName.find()
   * 컬렉션에서 데이터를 검색한다.
   * 예시) `myCollection` 컬렉션에서 `name` 필드 값이 "John"인 객체 검색
```
db.myCollection.find({name: "John"})
```
2. db.collectionName.insert()
   * 컬렉션에 데이터를 삽입한다.
   * 예시) `myCollection` 컬렉션에 `{name:"John", age:30, email:"john@example.com"}` 객체 삽입
```
db.myCollection.insert({name:"John", age:30, email:"john@example.com"})
```
3. db.collectionName.update()
   * 컬렉션의 데이터를 업데이트한다.
   * 예시) `myCollection` 컬렉션에서 `name` 필드 값이 "John"인 객체의 `age` 필드 값을 35로 업데이트
```
db.myCollection.update({name:"John"}, {$set: {age:35}}, {multi:true})
```
4. db.collectionName.remove()
   * 컬렉션의 데이터를 삭제한다.
   * 예시) `myCollection` 컬렉션에서 `name` 필드 값이 "John"인 객체를 모두 삭제
```
db.myCollection.remove({name:"John"})
```
5. db.collectionName.aggregate()
   * 데이터를 집계하고 변환한다.
   * 예시) `myCollection` 컬렉션에서 `$group` 파이프라인 스테이지를 사용하여 `name` 필드 값을 그룹화하고, 각 그룹별로 `name` 값의 출현 횟수를 `count` 필드로 집계
```
db.myCollection.aggregate([{ $group: {_id:"$name", count: {$sum: 1}} }])
```
6. db.collectionName.mapReduce()
   * 데이터를 변환하고 집계한다.
   * 예시) `myCollection` 컬렉션에서 `myMapFunction` 맵 함수를 적용하여 데이터를 변환하고, `myReduceFunction` 리듀스 함수를 적용하여 집계한 결과를 `myOutputCollection` 컬렉션에 저장
```
db.myCollection.mapReduce(myMapFunction, myReduceFunction, {out: "myOutputCollection"})
```
#### DCL (Data Control Language)
* MongoDB에서는 DCL 이라는 명령어 집합이 별도로 제공되지는 않는다.
  * DCL은 주로 RDBMS에서 사용하는 명령어로, 데이터베이스 객체에 대한 권한을 관리하고 보안을 강화하는 데 사용된다.
* 대신 MongoDB는 **데이터베이스 사용자 및 권한을 관리**하는 기능을 제공

