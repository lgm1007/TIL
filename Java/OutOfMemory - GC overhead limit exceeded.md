# java.lang.OutOfMemoryError: GC overhead limit exceeded
### 오류가 발생하는 이유
* CPU 사용량 98% 이상 GC를 진행하는 경우
  * 한정된 Heap Memory 안에서 GC 대상의 객체는 없지만 새롭게 생성되는 객체가 메모리를 할당하지 못할 경우 GC의 시간이 길어지게 된다.
* GC 진행 후 메모리가 2% 미만이 복구된 경우
### 해결방법
1. GC overhead limit exceeded Option 끄기
    * 추천하는 방법은 아님
    * 원인을 분석하는 편이 해결 방법이 될 수 있음
   ```
   -XX:-UseGCOverheadLimit
   ```
2. heap 메모리 설정
    * 제일 간단하면서 해결하기 쉬운 방법
    * 데이터를 읽는 양이 많을 경우
    * -Xmx : 최대 Heap Memory 설정
    * -Xms : 최소 Heap Memory 설정
   ```
   java -Xmx4G -Xms1024m
   ```
3. heap dump를 통해 메모리 사용량 체크/분석

