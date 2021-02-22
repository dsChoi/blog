---
title: "EffectivePython_03"
date: 2021-02-08T17:21:08+09:00
draft: true
draft: false
tags: ["python","파이썬", '이펙티브파이썬']  
---

# 2 리스트와 딕셔너리 (better way 11 ~ 14)

## Better way 11. 시퀀스를 슬라이싱하는 방법을 익혀라.

* 시퀀스를 여러조각(슬라이스(slice))으로 나누는 슬라이싱 구문이 있다. 

  ```python
   a = ['a', 'b','c', 'd', 'e', 'f', 'g', 'h']
   print('가운데 2개:' , a[3:5])
   print('마지막을 제외한 나머지:', a[1:7])
      
   >> 가운데 2개 : ['d', 'e']
   >> 마지막을 제외한 나머지: ['b','c', 'd', 'e', 'f', 'g']
  
  ```

* 리스트의 맨 앞부터 슬라이싱할 때는 시각적 잡음을 없애기 위해 0을 생략하라

  ```python
  assert a[:5] == a[0:5]
  ```

  

* 리스트의 끝까지 슬라이싱할 때는 쓸데없이 끝 인덱스를 적지말라.

  ```python
  assert a[5:] == a[5:len(a)]
  ```

* 리스트를 슬라이싱한 결과는 완전히 새로운 리스트이며, 원래 리스트에 대한 참조는 그대로 유지된다. 슬라이싱한 결과로 얻은 리스트를 변경해도 원래 리스트는 바뀌지 않는다.

  ```python
  b = a[3:]
  print('이전:', b)
  b[1] = 99
  print('이후:', b)
  print('변화없음:', a)
  >>>
  이전: ['d', 'e','f','g', 'h']
  이후: ['d', 99,'f','g', 'h']
  변화없음:  ['a', 'b','c', 'd', 'e', 'f', 'g', 'h']
  ```



>**기억해야 할 내용**
>
>* 슬라이싱 할때는 간결하게 하라. 시작 인덱스에 0을 넣거나, 끝 인덱스에 시퀀스 길이를 넣지 말라. 
>* 슬라이싱은 범위를 넘어가는 시작 인덱스나 끝 인덱스도 허용한다. 따라서 시퀀스의 시작이나 끝에서 길이를 제한하는 슬라이스(a[:20]) 이나 a[-20:] 를 쉽게 표현할 수 있다.
>* 리스트 슬라이스에 대입하면 원래 시퀀스에서 슬라이스가 가리키는 부분을 대입 연산자 오른쪽에 있는 시퀀스로 대치한다. 이때 슬라이스와 대치되는 시퀀스의 길이가 달라도 된다. 



## Better way 12. 스트라이드와 슬라이스를 한 식에 함께 사용하지 말라.

**스트라이드(stride)**

> 리스트[시작:끝:증가값]으로 ( 증가값으로 지정한) 일정한 간격을 두고 슬라이싱을 할 수 있는 특별한 구문



```python
x = ['빨강', '주황', '노랑', '초록', '파랑', '자주']
odds = x[::2]
evens = x[1::2]

print(odds)
print(evens)

>>>
['빨강',  '노랑', '파랑']
[ '주황', '초록', '자주']
```





```python
x = ['a', 'b','c','d','e','f','g','h']
x[::2] #['a','c','e','g']
x[::-2] #['h', 'f', 'd', 'b']
```



아래처럼 쓰면 헷갈린다.

```python
x[2::2] #['c', 'e', 'g']
x[-2::-2] #['g', 'e', 'c', 'a']
x[-2:2:-2] #['g', 'e']
x[2:2:-2] #[]
```

시작값이나 끝값을 증가값과 함께 사용하지 말것을 권한다.

시작이나 끝 인덱스와 함께 증가값을 사용해야 한다면 스트라이딩한 결과를 변수에 대입한 다음 슬라이싱하라.

```python
y = x[::2]
z = y[1:-1]
```



> **기억해야할 내용**
>
> * 슬라이스에 시작 , 끝, 증가값을 함께 지정하면 코드의 의미를 혼동하기 쉽다.
> * 시작이나 끝 인덱스가 없는 슬라이스를 만들 때는 양수 증가값을 사용하라, 가급적 음수 증가값은 피하라.
> * 한 슬라이스 안에서 시작, 끝 증가값을 함께 사용하지 말라. 세 파라미터를 모두 써야 하는 경우, 두번 대입을 사용(한 번은 스트라이딩, 한번은 슬라이싱) 하거나 itertools 내장 모듈의 islice 를 사용하라.




## Better way 13. 슬라이싱보다는 나머지를 모두 잡아내는 언패킹을 사용하라.

```python
car_ages = [0,9,4,8,7,20,19,1,6,15]
car_ages_descending = sorted(car_ages, reverse=True)
oldest, second_oldest = car_ags_descending
>>>
   Tracback...
  valueError: too many values to unpack (expected 2)
    
 ----
# 예전 코드

oldest = car_ages_descending[0]
second_oldest = car_ages_descending[1]
others = car_ages_descending[2:]
print(oldest, second_oldest, others)
>>>
20, 19 [15, 9, 8,7,6,4,1,0]

##
oldest, second_oldest, *others = car_ages_descending
print(oldest, second_oldest, others)
20, 19 [15, 9, 8,7,6,4,1,0]

oldest, *others, youngest = car_ages_descending
print(oldest,youngest, others)
20, 0 [19,15, 9, 8,7,6,4,1]


*others, second_youngest, youngest = car_ages_descending
print(youngest,second_youngest, others)
0, 1 ,[20, 19 ,15, 9, 8,7,6,4]


# 별표식만 사용할수 없다.
*others = car_ages_descending
>>>
Trackback...
SyntaxError: starred assignment target must be in a list or tuple

    
# 한 수준의 언패킹에서 두개의 별표식을 쓸수 없다.

first, *middle, *second_middle, last = car_ages_descending
>>>
Trackback...
SyntaxError: two starred expressions in assignment
```



> **1차이 나는 인덱스로 인한 오류(off-by-one error)**
>
> 3 이상 6 이하인 폐구간의 원소 개수는? 크기가 20인 리스트의 마지막 원소의 인덱스는? 13명을 네명씩 그룹으로 만들 때 전체 그룹의 숫자는? 이런식으로 개수를 따질 때 실수로 정답에 비해 1이 더 많거나 적은 답을 내놓는 경우. 
>
> 이런 오류를 off-by-one-error 라 한다. 리스트의 원소 개수를 세거나 어떤 작업을 정해진 수의 덩어리로 처리하기 위해 루프를 돌아야 하는 경우 이런 실수를 저지르기 쉽다.





> **기억해야 할 내용**
>
> * 언패킹 대입에 별표 식을 사용하면 언패킹 패턴에서 대입되지 않는 모든 부분을 리스트에 잡아낼 수 있다.
> * 별표 식은 언패킹 패턴의 어떤 위치에든 놓을 수 있다. 별표 식에 대입된 결과는 항상 리스트가 되며, 이 리스트에는 별표 식이 받은 값이 0개 또는 그 이상 들어간다.
> * 리스트를 서로 겹치지 않게 여러 조각으로 나눌 경우, 슬라이싱과 인덱싱을 사용하기보다는 나머지를 모두 잡아내는 언패킹을 사용해야  실수할 여지가 줄어든다.



## Better way 14. 복잡한 기준을 사용해 정렬할 때는 key 파라미터를 사용하라.

```python
print('미정렬:', repr(tools))
tools.sort(key=lambda x: x.name)
print('정렬:', tools)
>>>
미정렬 : [Tool('수준계', 3.5), Tool('해머', 1.25), Tool('스크류드라이버', 0.5),Tool('끌', 0.25)]
 정렬  : [Tool('끌', 0.25), Tool('수준계', 3.5), Tool('스크류드라이버', 0.5),Tool('해머', 1.25)]
    
    
tools.sort(key=lambda x: x.weight)
print('무게순 정렬:', tools)
>>>
무게순 정렬 :  [Tool('끌', 0.25) , Tool('스크류드라이버', 0.5) , Tool('해머', 1.25), Tool('수준계', 3.5)]
    
    
    
power_tools.sort(key=lambda x: (x.weight, x.name))
print(power_tools)


power_tools.sort(key=lambda x: x.name) #name 기준 오름차순
power_tools.sort(key=lambda x: x.weight, reverset=True) # weight 기준 내림차순

```



> **기억해야 할 내용**
>
> * 리스트 타입에 들어 있는 sort 메서드를 사용하면 원소 타입이 문자열, 정수 , 튜플 등과 같은 내장 타입인 경우 자연스러운 순서로 리스트의 원소를 정렬할 수 있다.
> * 원소 타입에 특별 메서드를 통해 자연스러운 순서가 정의돼 있지 않으면 sort 메서드를 쓸 수 없다. 하지만 원소 타입에 순서 특별 메서드를 정의하는 경우는 드물다.
> * sort 메서드의 key 파라미터를 사용하면 리스트의 각 원소 대신 비교에 사용할 객체를 반환하는 도우미 함수를 제공할 수 있다.
> * key 함수에서 튜플을 반환하면 여러 정렬 기준을 하나로 엮을 수 있다. 단항 부호 반전 연산자를 사용하면 부호를 바꿀 수 있는 타입이 정렬 기준인 경우 정렬 순서를 반대로 바꿀 수 있다.
> * 부호를 바꿀 수 없는 타입의 경우 여러 정렬 기준을 조합하려면 각 정렬 기준마다 reverse 값으로 정렬 순서를 지정하면서 sort 메서드를 여러번 사용해야 한다. 이때 정렬 기준의 우선순위가 점점 높아지는 순서로 sort 를 해야한다.



