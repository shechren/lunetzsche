---
title: Python - Future(1)
date: 2022-06-18 15:54:48 +/-09:00
category: [python/asynchronous]
tag: [python, future]
---

사전 설명을 길게 할 필요가 있다.

* Future는 대기 중인 작업을 큐에 넣고, 완료 상태를 조사하고, 결과(혹은 예외)를 가져올 수 있도록 캡슐화한다.

    일반적으로 Future에 대해 알아야 할 중요한 점은 여러분이나 나 같은 사람이 직접 객체를 생성하면 안 된다는 것이다. Future 객체는 concurrent.futures나 asyncio 같은 동시성 프레임워크에서만 배타적으로 생성해야 한다. 이유는 간단하다. Future는 앞으로 일어날 일을 나타내고, Future의 실행을 스케줄링한 후에만 생성된다. 예를 들어 Executor.submit() 메소드는 callable을 받아서 이 callable의 실행을 스케줄링하고, Future 객체를 반환한다.

    클라이언트 코드는 Future 객체의 상태를 직접 변경하면 안 된다. Future 객체가 나타내는 연산이 완료되었을 때, 동시성 프레임워크가 Future 객체의 상태를 변경하기 때문이다. 우리는 이 객체의 상태가 언제 바뀔지 제어할 수 없다.

    **{전문가를 위한 파이썬} 624P**

Future란 Parallelism(병렬성) 구조의 하나로(그럼에도 concurrent(병행성) 라이브러리에도 들어가 있다.) Python 3.2부터 사용할 수 있다. 스레딩과 멀티프로세싱을 한 곳에 묶어놓은 것으로 봐도 된다. Future는 비동기 실행을 위한 API를 고수준으로 작성하여 사용하기 쉽도록 개선한다.

GIL에 대해서도 적어두자. GIL은 global interpreter lock의 약자로 다수의 스레드가 하나의 자원에 엑세스될 때 모든 리소스를 잠가버린다. 따라서 GIL을 우회하고 싶다면 멀티프로세싱 혹은 CPython을 이용해야 한다.

이하에서 사용할 코드는 인프런 오리지널 파이썬 중급 과정에 자세히 나와 있으니 참고한다.

[인프런 오리지널 - 파이썬 중급](https://www.inflearn.com/course/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%A4%91%EA%B8%89-%EC%9D%B8%ED%94%84%EB%9F%B0-%EC%98%A4%EB%A6%AC%EC%A7%80%EB%84%90/dashboard)

제일 먼저 필요 모듈을 import한다.

```python
import time
from concurrent import futures
```
그리고 리스트 하나를 만든다.

```python
li = [1000, 100000, 10000000] #백, 십만, 천만
```

제네레이터 함수도 만든다.

```python
def gen(n):
    return sum(n for n in range(1, n+1))
```

우리는 li에 적힌 숫자 3개가 **각자** 1부터 시작해서 해당하는 숫자에 도달할 때까지 순회하며 1을 더할 것이다. 여기서 중요한 단어는 **각자**다.

```python
def main():
    worker = min(10, len(li))
```

이제 우리가 사용할 스레드 또는 프로세서의 수 표기를 위해 일꾼을 만들어주자. worker는 10과 len(li) 중 최솟값을 구해 우리에게 알려줄 것이다. 즉, li의 len()은 3이므로 worker = 3이 된다.

```python
#시작 시간
startTime = time.time()
```

시작 시간을 설정해준다. 아까 우리는 time을 임포트했다.

```python
#종료 시간
endTime = time.time() - startTime
```

종료 시간은 time에서 시작 시간을 빼면 된다. 간단하다.

이제 아까 임포트한 퓨처 라이브러리를 사용할 시간이다.

```python
with futures.ProcessPoolExecutor() as ex:
    #map은 작업 순서를 유지하고 즉시 실행한다.
    result = ex.map(gen, li)
```

map 함수의 첫 번째는 함수를 받고, 두 번째는 iter를 받는다. 첫 번째에 아까 만든 gen() 제네레이터 함수, 두 번째에 li를 넣는다.

그리고 그 값을 result로 받아온다...이게 끝이다. 정말로!

main() 함수 마지막 줄에 해당 내용을 추가해준다.

```python
print(f"\n 결과 : {list(result)} 시간: {endTime:.2f}s, 실행된 스레드 또는 프로세서: {worker}")
```

함수를 나오고 맨 아래에, main() 함수를 실행시키는 매직 메소드 하나를 적는다.

```python
if __name__ == "__main__":
    main()
```

실행 결과

**결과 : \[500500, 5000050000, 50000005000000\] 시간: 0.63s, 실행된 스레드 또는 프로세서: 3**

또다른 방법으로 실행하고 싶다면 ProcessPoolExecutor를 ThreadPoolExecutor로 바꾸면 된다. 즉, 프로세서를 스레드로 바꾸어 실행해보면 된다.

**결과 : \[500500, 5000050000, 50000005000000\] 시간: 0.52s, 실행된 스레드 또는 프로세서: 3**

두 번째 결과는 스레드로 실행한 결과다. 프로세서보다 미세하게 빠르다는 것을 알 수 있다. 다만 이건 상황에 따라 다르므로, 말 그대로 case by case니까 적재적소에 적용하는 것이 중요하다.

---