# Hateoas
### Hateoas 정의
* **Hypermedia As The Engine Of Application State**
	* 하이퍼미디어를 애플리케이션의 상태를 관리하기 위한 메커니즘으로 사용한다는 의미
* REST api를 사용하는 클라이언트가 **전적으로 서버와 동적인 상호작용이 가능**하도록 하는 것을 의미
* 클라이언트가 서버로부터 어떠한 요청을 할 때, **요청에 필요한 URI를 응답에 포함시켜 반환하는 것을 가능**하게 함
### Hateoas 사용 예시
* 기존의 REST api 응답
```json
{
	"account_id": 1234,
	"salary": "3,500,000"
}
```
* Hateoas가 적용된 응답 (URI 정보 추가)
```json
{
	"account_id": 1234,
	"salary": "3,500,000",
	"link": [
		{
			"rel": "self",
			"href": "http://example.com/accounts/1234"
		},
		{
			"rel": "withdraw",
			"href": "http://example.com/accounts/1234/withdraw"
		},
		{
			"rel": "transfer",
			"href": "http://example.com/accounts/1234/transfer"
		}
	]
}
```
* 클라이언트 사이드에서 `rel` 이름으로 요청 URI를 사용할 경우, URI를 수정하게 될 때 클라이언트 사이드에서는 수정을 하지 않아도 된다는 장점이 있음

