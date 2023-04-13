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

