# 분산 시스템: CPU Bound & I/O Bound
### CPU Bound
* CPU Bound는 프로세스가 진행될 때 **CPU 사용 기간이 I/O Waiting 보다 많은 경우**이다.
* 주로 행렬 곱 연산, 고속 연산을 수행할 때 발생하며 CPU 성능에 영향을 미친다.
### I/O Bound
* I/O Bound는 프로세스가 진행될 때 **I/O Waiting 시간이 많은 경우**이다.
* 파일 쓰기, 디스크 작업, 네트워크 통신을 할 때 주로 나타내며 작업에 의한 병목(다른 시스템과 통신할 때 나타남)에 의해 작업 속도가 결정된다.
### CPU Bound 성능 : CPU 성능 향상에 따른 작업 처리 성능
* CPU의 성능이 향상되거나 개수가 추가되면 CPU Bound의 작업 처리 성능이 향상된다.
* 따라서 CPU Bound의 성능 향상을 위해 **scale-up이 주로 사용**된다.
### I/O Bound 성능 : 스레드 성능 향상에 따른 작업 처리 성능
* 스레드 개수를 늘리거나 동시성을 활용하여 I/O Bound의 작업 처리 성능이 향상된다.
* 따라서 I/O Bound 성능 향상을 위해 **scale-out이 주로 사용**된다.
### 병렬 프로그래밍 방법
#### Multiprocessing 방식
* Multiprocessing 방식은 Multiple process를 사용하며 고가용성 (CPU) Utilization와 같은 CPU Bound 어플리케이션 처리에 적합하다.
#### Multithreading
* Multithreading 방식은 Single 및 Multi process, Multiple threads 를 사용하며 I/O Bound 중에서 빠르게 처리해야 하는 애플리케이션에 적합하다.
#### Async I/O
* Async I/O 방식은 Single process, Single thread 를 사용하며 I/O Bound 중 천천히 처리해도 되는 애플리케이션에 적합하다.
