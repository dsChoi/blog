---
title: "파이썬 코딩의 기술 better way 19 ~ 22"
date: 2021-03-09T22:22:08+09:00
draft: false
showDate: true
tags: ["python","파이썬", '이펙티브파이썬']  
---


# 3 함수 (better way 19 ~ 22)


## Better way 19. 함수가 여러 값을 반환하는 경우 절대로 네 값 이상을 언패킹하지 말라

```python
def get_stats(numbers):
    minimum = min(numbers)
    maximum = max(numbers)
    return minimum, maximum

lengths = [63, 73, 72, 60, 67, 66, 71, 61, 72 , 70]

minimum , maximum= get_stats(lengths) # 반환값이 두개

print(f'최소 : {minimum}, 최대 : {maximum}')

```

```python
lengths = [63, 73, 72, 60, 67, 66, 71, 61, 72, 70]

def get_avg_ratio(numbers):
    average = sum(numbers) / len(numbers)
    scaled = [x / average for x in numbers]
    scaled.sort(reverse= True)
    return scaled

longest, *middle, shortest = get_avg_ratio(lengths)

print(f'최대길이: {longest:>4.0%}')
print(f'최소길이: {shortest:>4.0%}')
```



```python
def get_stats(numbers):
  minimum = min(numbers)
  maxium = max(numbers)
  count = len(numbers)
  average = sum(numbers) / count

  sorted_numbers = sorted(numbers)
  middle = count // 2
  if count % 2 == 0:
      lower = sorted_numbers[middle-1]
      upper = sorted_numbers[middle]
      median = (lower + upper) / 2
  else:
      median = sorted_numbers[middle]

  return minimum, maxium, average, median, count

minimum, maxium, average, median, count =  get_stats(lengths)

print(f'최소 길이: {minimum}, 최대길이: {maxium}')
print(f'평균: {average}, 중앙값: {median}, 개수: {count}')

#최소 길이: 60, 최대길이: 73
#평균: 67.5, 중앙값: 68.5, 개수: 10
```



**문제점**

```
* 모든 반환 값이 수이기 때문에 순서를 혼동하기 쉽다. 
* 함수를 호출하는 부분과 반환 값을 언패킹하는 부분이 길고 여러가지 방법으로 줄을 바꿀수 있어 가독성이 나빠진다.
* 함수가 여러값을 반환하거나 언패킹할때 값이나 변수를 네 개 이상 사용하면 안된다. (값을 3개까지만 쓰자)
* 이보다 많은 값을 언패킹해야 한다면 경량 클래스나 namedtuple 을 사용하고 함수도 이런 값을 반환하게 만드는 것이 낫다.
```

> **기억해야 할 내용**
>
> * 함수가 여러 값을 반환하기 위해 값들을 튜블에 넣어서 반환하고, 호출하는 쪽에서 파이썬 언패킹 구문을 쓸 수 있다. 
> * 함수가 반환한 여러 값을 모든 값을 처리한는 별표 식을 사용해 언패킹할 수도 있다.
> * 언패킹 구문에 변수가 네 개 이상 나오면 실수하기 쉬우므로 변수를 네 개 이상 사용하면 안된다. 대신 작은 클래스를 반환하거나 namedtuple 인스턴스를 반환하라.



## better way 20. None 을 반환하기보다는 예외를 발생시켜라. 



```python
def carefule_divide(a,b):
    try:
        return a/b
    except ZeroDivisionError:
        return None

x, y = 1, 0
result = carefule_divide(x, y)
if result is None :
    print('잘못된 입력') 

```



실수할 가능성을 줄이는 방법 1. 

```python
def carefule_divide(a,b):
        try:
            return True, a/b
        except ZeroDivisionError:
            return False, None

x, y = 0, 5
success, result = carefule_divide(x, y)
if not success:
   print('잘못된 입력')

```

실수할 가능성을 줄이는 방법 2. 

```python
def carefule_divide(a,b):
        try:
            return  a/b
        except ZeroDivisionError as e:
            return ValueError('잘못된 입력')

x, y = 5, 2
try:
    result = carefule_divide(x, y)
except ValueError:
    print('잘못된 입력')
else:
    print('결과는 %.1f 입니다.' % result) # 결과는 2.5 입니다.

```



* 함수의 반환 값이 항상 float 이라 지정 가능

* 호출자가 어떤 Exception 을 잡아내야 할지 결정할때 문서를 참조할 것으로 예상하고, 발생시키는 예외를 문서에 명시해야한다.

* 입력, 출력, 예외적인 동작이 모두 명확해졌고, 호출자가 이 함수를 잘못 처리할 가능성이 매우 낮아짐

  



**독스트링과 타입 애너테이션까지 포함시킨 코드**

```python
def carefule_divide(a: float, b: float) -> float:
    """a를 b로 나눈다.
    
    Raises: 
        ValueError: b가 0이어서 나눗셈을 할 수 없을 때
    """
    
    try:
        return a/b
    except ZeroDivisionError as e:
        return ValueError('잘못된 입력')
```



> **기억해야할 내용**
>
> * 특별한 의미를 표시하는 None 을 반환하는 함수를 사용하면 None 과 다른 값(0이나 빈 문자열) 이 조건문에서 False 로 평가될 수 있기 때문에 실수하기 쉽다.
> * 특별한 상황을 표현하기 위해 None 을 반환하는 대신 예외를 발생시켜라. 문서에 예외 정보를 기록해 호출자가 예외를 제대로 처리하도록 하라.
> * 함수가 특별한 경우를 포함하는 그 어떤 경우에도 절대로 None 을 반환하지 않는 다는 사실을 타입 애너테이션으로 명시할수 있다.



## better way 21. 변수 영역과 클로저의 상호작용 방식을 이해하라.



```python
def sort_priority(values, group):
    def helper(x):
        if x in group:
            return (0,x)
        return (1,x)
    values.sort(key=helper)

## 이 함수는 입력이 간단하면 잘 동작한다.
numbers = [8,3,1,2,5,4,7,6]
group = {2,3,5,7}
sort_priority(numbers, group)
print(numbers)

>>
[2, 3, 5, 7, 1, 4, 6, 8]
```



이 함수가 예상대로 작동하는 세 가지 이유

* 파이썬이 클로저를 지원 

  * 클로저란 자신이 정의된 영역 밖의 변수를 참조하는 함수다. 클로저로 인해 도우미 함수가 sort_priority 함수의 group 인자에 접근할 수 있다.

* 파이썬에서 함수가 일급시민 객체임

  * 일급시민 객체란 ? 이를 직접 가리킬수 있고, 변수에 대입하거나 다른 함수에 인자로 전달 할 수 있으며, 식이나 IF 문에서 함수를 비교하거나 함수에서 반환하는 것 등이 가능하다는 것을 의미

* 파이썬에는 시퀀스(튜플 포함)를 비교하는 구체적인 규칙이 있음

  * 파이썬은 시퀀스를 비교할 때 0번 인덱스에 있는 값을 비교한 다음, 이 값이 같으면 다시 1번 인덱스에 있는 값을 비교한다. 
  * 이런식으로 순서대로 원소를 비교해 두 값이 같으면 그다음 원소로 넘어가는 작업을 시퀀스의 모든 원소를 다 비교하거나 결과가 정해질 때까지 계속한다. 이로 인해 helper 클로저가 반환하는 튜플이 서로 다른 두 그룹을 정렬하는 기준 역할을 할 수 있다.

  

```python
def sort_priority2(nubmers, group):
    found = False
    def helper(x):

        if x in group:
            found = True
            return (0,x)
        return (1,x)
    numbers.sort(key=helper)
    return found
>>
발견: False
[2, 3, 5, 7, 1, 4, 6, 8]

def sort_priority2(nubmers, group):
    found = False
    def helper(x):
        nonlocal found
        if x in group:
            found = True
            return (0,x)
        return (1,x)
    numbers.sort(key=helper)
    return found


found = sort_priority2(numbers, group)
print('발견:', found)
print(numbers)

>>

발견: True
[2, 3, 5, 7, 1, 4, 6, 8]
```



nonlocal 사용 방식이 복잡해지면 도우미 함수로 상태를 감싸는 편이 낫다. 



```python
class Sorter:
    def __init__(self, group):
        self.group = group
        self.found = False
    def __call__(self,x):
        if x in self.group:
            self.found = True
            return (0,x)
        return (1,x)
sorter = Sorter(group)
numbers.sort(key=sorter)
assert sorter.found is True

print(numbers)
```

> **기억해야 할 내용**
>
> * 클로저 함수는 자신이 정의된 영역 외부에서 정의된 변수도 참조할 수 있다.
> * 기본적으로 클로저 내부에 사용한 대입문은 클로저를 감싸는 영역에 영향을 끼칠수 없다.
> * 클로저가 자신을 감싸는 영역의 변수를 변경한다는 사실을 표시할 때는 nonlocal 문을 사용하라.
> * 간단한 함수가 아닌 경우에는 nonlocal 을 사용하지 마라.



## better way 22. 변수 위치 인자를 사용해 시각적인 잡음을 줄여라.

위치 인자를 가변적으로 받을 수 있으면 함수 호출이 깔끔해지고 시각적 잡음도 줄어든다. 



가변적인 위치 인자를 받는 데는 두 가지 문제점이 있다.

1. 선택적인 위치 인자가 함수에 전달되기 전에 항상 튜플로 변환된다는 것

   1. 함수를 호출하는 쪽에서 제너레이터 앞에 *연산자를 사용하면 제너레이터의 모든 원소를 얻기 위해 반복한다는 뜻

      ```python
      def my_generator():
          for i  in range(10):
              yield i
      def my_func(*args):
          print(args)
          
      it = my_generator()
      
      my_func(*it)
      >>
      (0, 1, 2, 3, 4, 5, 6, 7, 8, 9)
      ```

      

   2. 함수에 새로운 위치 인자를 추가하면 해당 함수를 호출하는 모든 코드를 변경해야 한다는 것



```python
def log(sequence, message, *values):
    if not values:
        print(f'{sequence} - {message}')
    else:
        values_str = ', '.join(str(x) for x in values)
        print(f'{sequence} - {message}: {values_str}')
        
log(1,'좋아하는 숫자는', 7,33)        
log(1, '안녕')

log('좋아하는 숫자는', 7,33) # 예전 방식 코드는 깨짐
```

예전 방식의 코드를 보면 예외가 발생하지 않고 코드가 작동할 수도 있기 때문에 이런 버그는 추적하기가 어렵다. 

이런 가능성을 완전히 없애려면 *args 를 받아들이는 함수를 확장할 때는 키워드 기반의 인자만 사용해야 한다. 

더 방어적으로 프로그래밍하려면 타입 애너테이션을 사용해도 된다.

>**기억해야 할 내용**
>
>* def 문에서 *args 를 사용하면 함수가 가변 위치 기반 인자를 받을 수 있다.
>* *연산자를 사용하면 가변 인자를 받는 함수에서 시퀀스 내의 원소를 전달할 수 있다.
>* 제너레이터에 *연산자를 사용하면 프로그램이 메모리를 모두 소진하고 중단될 수 있다.
>* *args 를 받는 함수에 새로운 위치 기반 인자를 넣으면 감지하기 힘든 버그가 생길 수 있다.