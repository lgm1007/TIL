# List Comprehension
### 기본 문법
**1. 일반적인 배열과 리스트의 활용**
* 기본적으로 List Comprehension은 **리스트를 쉽게, 짧게 한 줄로** 만드는 파이썬 문법
* `[(아이템 데이터를 활용한 값) for (Iterator 아이템 변수) in (순회할 Iterator 값)`
* List Comprehension 사용 전 예제
```python
size = 10
arr = [0] * size
for i in range(len(size)):
    arr[i] = i * 2
```
* List Comprehension 사용 후
```python
size = 10
arr = [i * 2 for i in range(size)]
print(arr)

# [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
```
* List Comprehension 다른 예제 - 원소를 제곱하여 새로운 리스트 만들기
```python
arr = [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
new_arr = [n * n for n in arr]
print(new_arr)

# [0, 4, 16, 36, 64, 100, 144, 196, 256, 324]
```
**2. 조건문으로 필터링**
* List Comprehension은 if 조건문을 사용하여 생성되는 리스트 원소값을 필터링할 수 있다.
* List Comprehension 필터링 예제 - 짝수만 원소로 갖게 하기
```python
size = 10
arr = [n for n in range(1,11) if n % 2 == 0]
print(arr)

# [2, 4, 6, 8, 10]
```
* 필터링 예제 2 - 2의 배수이고(AND), 3의 배수인 수만 원소로 갖게 하기 
```python
arr = [n for n in range(1, 31) if n % 2 == 0 if n % 3 == 0]
print(arr)

# [6, 12, 18, 24, 30]
```
* 필터링 예제 3 - 2의 배수이거나(OR), 3의 배수인 수만 원소로 갖게 하기
```python
arr = [n for n in range(1, 16) if n % 2 == 0 or n % 3 == 0]
print(arr)

# [2, 3, 4, 6, 8, 9, 10, 12, 14, 15]
```
### 다른 구조로 확장하기
**1. `set` 만들기**
* `set`은 생성기호 `{}` 사용
```python
new_set = {n ** 2 for n in range(10)}
print(new_set)

# {0, 1, 64, 4, 36, 9, 16, 49, 81, 25}
# 0~9 까지의 정수를 제곱한 값
# 집합 특성으로 순서가 일정하지 않다.
```
**2. `dictionary` 만들기**
* `dictionary`는 for문 앞에 `key: value` 쌍을 작성하고, 사용하는 변수를 2개로 둔다.
```python
# 내장 모듈 string에서 영어 소문자 상수 가져오기
from string import ascii_lowercase as lowercase

# zip 내장함수: key, value로 사용할 값들의 묶음을 병렬로 배치
new_dict = {c: n for c, n in zip(lowercase, range(1, 27))}
print(new_dict)

# {'a': 1, 'b': 2, 'c': 3, ..., 'x': 24, 'y': 25, 'z': 26}
```
**3. `tuple` 만들기**
* `tuple`은 생성기호 `()` 사용
```python
# tuple 자료구조로 컴프리헨션 식을 넣어 생성해준다.
# 그렇지 않고 그냥 (컴프리헨션 식) 으로 작성하면 Generator를 생성하게 된다.
new_tuple = tuple(n for n in range(1, 10))
print(new_tuple)

# (1, 2, 3, 4, 5, 6, 7, 8, 9)
```