---
title: Python - 데코레이터(Decorator) 
date: 2022-06-16 13:59:09 +/-09:00
category: [python/advanced]
tag: [python, decorator]
---

데코레이터의 장점은 중복을 제거하고 코드를 간결하게 하며 공통된 함수(로깅, 프레임워크, 유효성 체크)를 작성할 수 있게 한다. 단점이라면 가독성이 악화되고 디버깅이 어려워지기는 하지만 같은 함수를 반복적으로 불러와야 할 때 매우 효율적인 방법이다.


이하는 인프런의 파이썬 중급 오리지널에 나오는 코드를 살짝 수정한 것이다.


[우리를 위한 프로그래밍 : 파이썬 중급 (Inflearn Original) - 인프런 | 강의](https://www.inflearn.com/course/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%A4%91%EA%B8%89-%EC%9D%B8%ED%94%84%EB%9F%B0-%EC%98%A4%EB%A6%AC%EC%A7%80%EB%84%90/dashboard)
```python
import time

def perfomance(function):
#클로저 형식 진입
  def perfomance\_2(\*args):
    st = time.perf\_counter()
    result = function(\*args)
    et = time.perf\_counter() - st
    name = function.\_\_name\_\_
    arg\_str = ", ".join(repr(arg) for arg in args)
    #결과
    print("[%0.5fs %s(%s) -> %r" % (et, name, arg\_str, result))
    return result
  return perfomance\_2 #클로저 마무리
```

데코레이터는 보다시피 클로저와 일급함수의 기능을 사용하므로 이를 사용하기 위해선 일급함수와 클로저에 대한 선행학습이 되어 있어야 한다.

```python
@perfomance
def time\_func(seconds):
  time.sleep(seconds)

@perfomance
def sum\_func(\*numbers):
  return sum(numbers)

데코레이터의 호출 방법은 간단하다. 함수 바로 윗줄에 @(골뱅이) 이후 사용할 일급 함수의 이름을 적는다. 그게 전부다.

time\_func(2)
sum\_func(1, 2, 3)

그리고 마지막으로 함수를 호출한다. 1번째 함수는 우리가 넣은 수의 시간만큼 잠을 자는, 그러니까 대기하는 함수. 두 번째 함수는 우리가 넣은 수를 더하는 함수다.

결과:

2.00489s time\_func(2) -> None 
0.00000s sum\_func(1, 2, 3) -> 6

1번째 함수는 아무것도 반환하지 않으며, 2초간 쉬었지만 0.00489초가 더 걸렸다.
2번째 함수에서는 1 + 2 + 3인 6을 반환했는데, 0.00000초도 되지 않는 짧은 시간 내에 모든 계산을 완료했다.
우리가 원하는 시간대에 원하는 데코레이터를 넣으면 사용자 데이터(통계) 또는 런타임 값 등을 유용하게 알아낼 수 있다.

--- 
