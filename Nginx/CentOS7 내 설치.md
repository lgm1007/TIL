# CentOS 7에 Nginx 설치
### 1. yum 외부 저장소 추가
* yum 저장소에는 Nginx가 없기 때문에 외부 저장소를 추가해줘야 한다.
```
$ cd /etc/yum.repos.d/
$ vi nginx.repo
```
* /etc/yum.repos.d/ 경로에 nginx.repo 파일을 추가하고 다음과 같이 작성한다.
```
[nginx]
name=nginx repo
baseUrl=http://nginx.org/packages/centos/7/$basearch/
gpgcheck=0
enabled=1
```
* OS 버전이 다르다면 버전에 맞게 숫자를 수정하면 된다.
### 2. yum install
* yum install 실행
```
$ yum install -y nginx
```
### 3. 방화벽 포트 개방
```
$ firewall-cmd --permanent --zone=public --add-port=8089/tcp
success
$ firewall-cmd --reload
success
$ firewall-cmd --list-ports
21/tcp 5000/tcp 5001/tcp 8089/tcp
```
### 4. Nginx 포트 설정
```
$ vi /etc/nginx/conf.d/default.conf
```
```
server {
        listen      80;

        root         /usr/share/nginx/html;
        index index.html index.htm index.php;

        # Load configuration files for the default server block.
        # include /etc/nginx/default.d/*.conf; 

        location / {
        }

        location ~ \.php$ {
            fastcgi_split_path_info ^(.+\.php)(/.+)$;
            fastcgi_pass   unix:/var/run/php-fpm/php-fpm.sock;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }
        location ~ /\.ht {
             deny  all;
        }
        error_page 404 /404;
        location = /404 { # php 404 redirect
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }
}
```
* 이후 Nginx로 실행하고자 하는 WAS 주소를 location 내 `proxy_pass` 설정으로 입력한다.
### 5. Nginx 데몬 실행
```
$ systemctl start nginx
$ systemctl enable nginx
Created symlink from /etc/systemd/system/multi-user.target.wants/nginx.service to /usr/lib/systemd/system/nginx.service.
```
### 6. Nginx 실행 확인
* 정상적으로 실행되었다면, default.conf 에 listen 으로 설정한 포트번호로 접속하면 nginx 시작 페이지가 보여진다.
* **http://[서버 ip]:[포트번호]**
