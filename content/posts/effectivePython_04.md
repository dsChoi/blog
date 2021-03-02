---
title: "파이썬코딩의 기술"
date: 2021-02-08T17:21:08+09:00
draft: true
draft: false
tags: ["python","파이썬", '이펙티브파이썬']  
---

# 2 리스트와 딕셔너리 (better way 15 ~ 18)

## Better way 15. 딕셔너리 삽입 순서에 의존할 떄는 조심하라



python 3.5 이전 버전에서는 딕셔너리 구현이 내장 hash 함수와 파이썬 인터프리터가 시작할 때 초기화되는 난수 씨앗값을 사용하는 해시 테이블 알고리즘으로 만들어졌기 때문에 입력 순서와 출력순서가 달라진다. 

python 3.6 부터는 딕셔너리가 삽입 순서를 보존하도록 동작이 개선되었다.

```python
baby_names = {
	'cat': 'kitten' , 
	'dog': 'puppy'
}
print(baby_names)

>>>  3.5 
{'dog':'puppy' , 'cat' : 'kitten'}

>> 3.6
{'cat' : 'kitten' , 'dog' :'puppy'}
```



> **기억해야 할 내용**
>
> * 파이썬 3.7 부터 dic 인스턴스에 들어 있는 내용을 이터레이션할 때  키를 삽입한 순서대로 돌려받는다는 사실에 의존할 수 있다.
> * 파이썬은 dict 는 아니지만 딕셔너리와 비슷한 객체를 쉽게 만들 수 있게 해준다. 이런 타입의 경우 키 삽입 순서가 그대로 보존된다고 가정할 수 없다.
> * 딕셔너리와 비슷한 클래스를 조심스럽게 다루는 방법으로는 dict 인스턴스의 삽입 순서보존에 의존하지 않고 코드를 작성하는 방법, 실행 시점에 명시적으로 dict 타입을 검사하는 방법, 타입 애너테이션과 정적 분석(static analysis) 을 사용해 dict 값을 요구하는 방법이 있다.



## Better way 16. in 을 사용하고 딕셔너리 키가 없을 때 KeyError 를 처리하기보다는 get을 사용하라





```python
if key in counters:
    count  = counters[key]
else : 
    count = 0
 
counters[key] = count + 1

try:
    count = coutners[key]
except keyError:
    count = 0

counters[key] = count + 1

// 아래처럼 get default 형식이 코드가 간결하고 좋다.

count = counters.get(key, 0)
counters[key] = count + 1


```



> **기억해야 할 내용**
>
> * 딕셔너리 키가 없는 경우를 처리하는 방법으로는 in 식을 사용하는 방법, KeyError 예외를 사용하는 방법, get 메서드를 사용하는 방법, setdefault 메서드를 사용하는 방법이 있다.
> * 카운터와 같이 기본적인 타입의 값이 들어가는 딕셔너리를 다룰 때는 get 메서드가 가장 좋고, 딕셔너리에 넣을 값을 만든느 비용이 비싸거나 만드는 과정에 예외가 발생할 수 있는 경우에도 get 메서드를 사용하는 편이 낫다.
> * 해결하려는 문제에 dict 의 setdefault 메서드를 사용하는 방법이 가장 적합해 보인다면 setdefault 대신 defaultdict  를 사용할지 고려해보라.



## Better way 17. 내부 상태에서 원소가 없는 경우를 처리할 때는 setdefault 보다 defaultdic 를 사용하라.



```python
visits = {
	'미국' : {'뉴욕', '로스엔젤레스'}, 
	'일본' : {'하코네'}
}



//짧게 사용 가능
visits.setdefault('프랑스', set()).add('칸')

//이전 방법

if (japan := visits.get('일본')) is None:
    visits['일본'] = japan = set()

jpan.add('교토')

print >>
{'미국' : {'뉴욕', '로스엔젤레스'}, '일본' : {'하코네'} , '프랑스' : {'칸'}}


```



**직접 딕셔너리 생성을 제어하기**



setdefault 호출의 복잡도를 감춰주며, 프로그래머에거 더 나은 인터페이스를 제공한다 

단점은 호출할때마다 set() 인스턴스를 만들기 때문에 효율적이지 않고 setdefault 라는 메서드 이름은 코드를 처음 읽는 사람은 코드 동작이해가 어렵기 때문이다.

```python
class Visits: 
    def __init__(self):
        self.data = {}
    def add(self, country, city):
        city_set = self.data.setdefault(country, set())
        city_set.add(city)
        
       
visits = Vists()
visits.add('러시아', '예카테린부르크')
visits.add('탄자니아', '잔지바르')
print(visits.data)

>>
{'러시아' : {'예카테린부르크'}, '탄자니아':{'잔지바르'}}


```



**collections 의 defaultdict 을 사용하기**

```python
from collections import defaultdict

class Vists:
    def __init__(self)
    	self.data = defaultdict(set)
        
    def add(self, country, city)
    	self.data[country].add(city)

visits = Vists()
visits.add('영국', '바스')
visits.add('영국', '런던')
print(visits.data)
        
 >>
defaultdict(<class 'set'>, {'영국' : {'바스', '런던'}}
  
```



>**기억해야 할 내용**
>
>* 키로 어떤 값이 들어올지 모르는 딕셔너리를 관리해야 하는데 collections 내장 모듈에 있는 defaultdict 인스턴스가 여러분의 필요에 맞아떨어진다면 defaultdict 를 사용하라.
>* 임의의 키가 들어 있는 딕셔너리가 여러분에게 전달됐고 그 딕셔너리가 어떻게 생성됐는지 모르는 경우, 딕셔너리의 원소에 접근하려면 우선 get을 사용해야 한다. 하지만 setdefault 가 더 짧은 코드를 만들어내는 몇가지 경우에는 setdefault 를 사용하는것도 고려해볼 만하다. 





## Better way 18. `__missing__` 을 사용해 키에 따라 다른 디폴트 값을 생성하는 방법을 알아두라.



setdefault 와 defaultdict 모두 사용하기 어려운 경우

```python
pictures = {}
path = 'profile_1234.png'

if (handle := pictures.get(path)) is None:
    try: 
        handle = open(path, 'a+b')
    except OSError:
        print(f'경로를 열수 없습니다: {path}')
        raise
    else:
        pictures[path] = handle
        
handle.seek(0)
image_data = handle.read()
        
```





이 코드는 문제가 많다. 

파일 핸들을 만드는 내장 함수인 open 이 딕셔너리에 경로가 있는지 여부와 관계없이 항상 호출된다.  이로 인해 같은 프로그램상에 존재하던 열린 파일 핸들과 혼동될 수 잇는 새로운 파일 핸들이 생길수도 있다. open 이 예외를 던질 수 있으므로 이 예외를 처리해야한다. 하지만 이 예외를 같은 줄에 있는 setdefault 가 던지는 예외와 구분하지 못할 수도 있다.

```python
try:
    handle = picture.setdefault(path, open(path, 'a+b'))
except OSError:
    print(f'경로를 열수 없습니다: {path}')
    raise
else:
	handle.seek(0)
	image_data = handle.read()
```



```python
from collections import defaultdict

def open_picture(profile_path):
    try:
        return open(profile_path, 'a+b')
    except OSError:
        print(f'경로를 열수 없습니다: {profile_path}')
	    raise
        
        
pictures = defaultidc(open_picture)
handle = pictures[path]
handle.seek(0)
image_data = handle.read()

>>>
Traceback...
TypeError : open(picture() missing 1 required positional
argument : 'profile_path'
                 

```





```python
class Pictures(dict):
    def __missing__(self, key)
    	value = open_picture(key)
        self[key]  = value
        return value
    
pictures = Pictures()
handle = pictures[path]
handle.seek(0)
image_data = handle.read()



```

pictures[path] 라는 딕셔너리 접근에서 path 가 딕셔너리에 없으면 `__missing__` 메서드가 호출된다.  이 메서드는 키에 해당하는 디폴트 값을 생성해 딕셔너리에 넣어준 다음에 호출한 쪽에 그 값을 반환해야 한다. 그 후 딕셔너리에서 같은 경로에 접근하면 이미 해당 원소가 딕셔너리에 들어 있으므로 `__missing__`  이 호출되지 않든다. 

> **기억해야 할 내용**
>
> * 디폴트 값을 만드는 계싼 비용이 높거나 만드는 과정에서 예외가 발생할 수 있는 상황에서는 dict 의 setdefault 메서드를 사용하지 말라.
> * defaultdict 에 전달되는 함수는 인자를 받지 않는다. 따라서 접근에 사용한 키 값에 맞는 디폴트 값을 생성하는 것은 불가능하다.
> * 디폴트 키를 만들 때 어떤 키를 사용했는지 반드시 알아야 하는 상황이라면 직접 dict 의 하위 클래스와 `__missing__` 메서드를 정의하면 된다.



