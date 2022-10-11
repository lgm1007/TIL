# 빌드 관리 도구 - Maven과 Gradle
### 빌드 관리 도구란?
* **빌드** : 프로젝트에서 작성한 java 소스와 프로젝트 내에 필요한 각종 xml, properties, jar 파일들을 JVM이나 WAS가 인식할 수 있도록 패키징 해주는 과정
* 프로젝트 생성, 테스트 빌드, 배포 등의 작업을 위한 전용 프로그램
* 빌드 도구 설정파일에 필요한 라이브러리 종류와 버전들, 종속성 정보를 명시하여 필요한 라이브러리들을 설정파일을 통해 자동으로 다운로드해주고 이를 간편하게 관리해주는 도구
### Maven
* Java용 프로젝트 관리도구로써 Apache와 Ant 대안으로 만들어졌다. 
* 빌드 중인 프로젝트, 빌드 순서, 다양한 외부 라이브러리 종속성 관계를 **pom.xml 파일에 명시**
* 외부저장소에서 필요한 라이브러리와 플러그인을 다운로드한 다음, 로컬 시스템의 캐시에 모두 저장한다.
### Gradle
* Apache Maven과 Apache Ant에서 볼 수 있는 개념들을 사용하는 대안으로써 나온 프로젝트 빌드 관리 도구 (완전한 오픈소스)
* Groovy 언어를 사용한 `Domain-specific-language` 를 사용
  * 설정파일을 xml을 사용하는 Maven 보다 코드가 훨씬 간결함
* 큰 규모로 예상되는 multi-project 빌드를 도울 수 있도록 디자인됨
* 프로젝트의 어느 부분이 업데이트 되었는지 알 수 있기 때문에, 빌드에 점진적으로 추가할 수 있음
* 업데이트가 이미 반영된 빌드의 부분은 더 이상 재실행되지 않음
### Maven VS Gradle
* Gradle은 **작업 의존성 그래프**를 기반으로 하는 반면, Maven은 **고정적이고 선형적인 단계의 모델**을 기반으로 함
* Gradle은 **incremental build**를 허용함
  * **incremental build** : 어떤 task가 업데이트되었고 안되었는지를 체크 
  * 이미 업데이트된 태스크에 대해서는 작업이 실행되지 않으므로 빌드 시간이 훨씬 단축됨
    * 빌드 규모가 점점 커질수록 빌드시간의 차이도 Maven과 비교하여 꽤 격차가 벌어질 수 있을 것
* Gradle은 **concurrent 안전한 캐시**를 사용함
  * 2개 이상의 프로젝트에서 동일한 캐시를 사용할 경우, 서로 overwrite되지 않도록 checksum 기반의 캐시를 사용하고 캐시를 repository와 동기화시킬 수 있다.
* 고도로 사용자 정의된 빌드를 작성하기 위해서는 **커스터마이징이 간편한** Gradle을 사용하는 것이 훨씬 나음
  * 예1) Maven 설정 코드
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-checkstyle-plugin</artifactId>
    <version>2.12.1</version>
    <executions>
        <execution>
            <configuration>
                <configLocation>config/checkstyle/checkstyle.xml</configLocation>
                <consoleOutput>true</consoleOutput>
                <failsOnError>true</failsOnError>
            </configuration>
            <goals>
                <goal>check</goal>
            </goals>
        </execution>
    </executions>
</plugin>
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>findbugs-maven-plugin</artifactId>
    <version>2.5.4</version>
    <executions>
        <execution>
            <goals>
                <goal>check</goal>
            </goals>
        </execution>
    </executions>
</plugin>
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-pmd-plugin</artifactId>
    <version>3.1</version>
    <executions>
        <execution>
            <goals>
                <goal>check</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```
  * 예2) Gradle 설정 코드
```groovy
apply plugin:'java'
apply plugin:'checkstyle'
apply plugin:'findbugs'
apply plugin:'pmd'
version ='1.0'
repositories {
    mavenCentral()
}
dependencies {
    testCompile group:'junit', name:'junit', version:'4.11'
}
```
