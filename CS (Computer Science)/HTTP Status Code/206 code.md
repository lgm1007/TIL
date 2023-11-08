# 206 Code
* `206 Partial Content`
* 컨텐츠의 일부만을 보낸다.

### 부가 설명
* 클라이언트가 서버에 리소스의 특정 범위만 요청했을 때 사용
* 206 응답 코드는 요청이 성공적이었고, 요청의 `Range` 헤더에 지정된 데이터 범위가 응답 본문에 포함되어 있음을 나타낸다.
* 클라이언트가 단일 범위를 요청한 경우, 전체 응답의 `Content-Type`은 문서의 유형으로 설정되며 `Content-Range` 헤더가 제공된다.
  * 이 경우 `Content-Length`는 해당 범위의 바이트 크기를 나타낸다.
* 여러 범위가 요청되어 반환되는 경우, `Content-Type`은 `multipart/byteranges`로 설정되며 각 부분은 하나의 범위를 커버한다.
  * 각 부분은 자체 `Content-Type`과 `Content-Range`를 가지게 된다.

### 예시
* 클라이언트 측에서 보내는 `Range` 헤더 예시
* `video.mp4` 파일의 200번째 바이트부터 1000번째 바이트까지 범위를 요청
```vbnet
GET /video.mp4 HTTP/1.1
Host: example.com
Range: bytes=200-1000
```

* 서버의 응답 헤더 예시
* `Content-Range`는 전체 리소스 크기 중 요청받은 부분의 정확한 범위를 나타낸다.
```vbnet
HTTP/1.1 206 Partial Content
Date: web, 15 Nov 2015 06:25:24 GMT
Content-Type: video.mp4
Content-Range: bytes 200-1000/23000
Content-Length: 801 
```
