---
title: Python - Future(2)
date: 2022-06-18 17:02:43 +/-09:00
category: [python/asynchronous]
tag: [python, future]
---

이전 글의 코드 중 일부는 재사용할 것이다. 일단 필요 모듈 임포트부터 다시 해보자.

```python
import time
from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor, wait, as_completed
```

우리는 저기서 wait와 as_completed를 알아야 한다. 그걸 위해 리스트도 살짝 고쳐서 다시 가져온다.

```python
li = [1000, 100000, 100000000] #백, 십만, 1억
```

저 수들을 각각 A, B, C라고 해보자. 지금은 보나마나 ABC 순서대로 일이 끝마쳐질 테지만, 실제로 우리가 코드를 사용할 적에는 어떤 게 먼저 끝날지는 모른다. 게다가 갑작스런 상황 등으로 아예 일이 실패할 수도 있다. 우리는 이것의 여부를 반환할 것이다.

main 함수 내에 새로운 로컬 변수를 선언한다.

```python
fut_li = []
```
ProcessPoolExecutor() 부분 이하를 수정할 것이다.

```python
#ThreadPoolExecutor()로 바꿔도 된다.
with ProcessPoolExecutor() as ex:
    for work in li:
        #future 반환
        future = ex.submit(gen, work)
        #스케줄링
        fut_li.append(future)
        #스케줄링 확인
        print(f"Scheduled for {work, future}")
        print()
```

work는 li를 돌 것이다. submit함수를 map 함수처럼 매핑해준다. map함수는 어떻게 매핑했었지? (함수, iter)다. 여기서는 iter 자리에 work를 넣은 뒤 future 변수에 값을 넣는다.

마지막으로 future 변수를 fut_li 리스트에 추가한다.

---

```python 
# wait 결과 출력
result = wait(fut_li, timeout=1)
print(f"일이 끝났어요! {str(result.done)}")
print([future.result() for future in result.done])
print(f"일이 안 끝났어요! {str(result.not_done)}")
```

wait을 먼저 출력해볼 것이다. 1초 안에 끝난 일은 done, 1초가 넘어간 일은 not_done으로 취급된다. result는 str로 변환해서 출력해줘야 한다.

```python
#종료 시간
endTime = time.time() - startTime

    print(f"\n 결과  시간: {endTime:.2f}s, 실행된 스레드 또는 프로세서: {worker}")

if __name__ == "__main__":
    main()
```

위 문장은 전의 코드에서 그대로 복사해와서 그대로 사용한다. 이제 출력해보자.

**Scheduled for (1000, &lt;Future at 0x2a84e201810 state=running&gt;)**  
  
**Scheduled for (100000, &lt;Future at 0x2a84e202ad0 state=running&gt;)**  
  
**Scheduled for (100000000, &lt;Future at 0x2a84e2028c0 state=pending&gt;)**  
  
**일이 끝났어요! {&lt;Future at 0x2a84e201810 state=finished returned int&gt;, &lt;Future at 0x2a84e202ad0 state=finished returned int&gt;}**  
**\[500500, 5000050000\]**  
**일이 안 끝났어요! {&lt;Future at 0x2a84e2028c0 state=running&gt;}**  
  
**결과  시간: 5.35s, 실행된 스레드 또는 프로세서: 3**

앞의 둘은 1초 안에 계산이 완료된 것이고, 뒤의 하나는 1초 안에 끝나지 않은 것이다.

따라서 1초를 기준으로 일의 성패를 가늠할 수 있게 된다.

---

wait부분을 전부 ctrl + / 로 블록 처리한다. 이제는 as\_completed를 출력할 차례다. 방법은 iter 부분에 wait(iter) 대신 as\_completed(iter)를 넣으면 된다.

```python
#as_completed 결과 출력
for future in as_completed(fut_li):
    result = future.result()
    done = future.done()
    cancelled = future.cancelled

    print(f"퓨처 as_completed 결과 : {result, done}")
    print(f"퓨처 as_completed 캔슬 : {cancelled}")
```

그리고 각종 정보들을 받아오자. result는 결과값, done은 성공, cancelled는 실패다.

Scheduled for (1000, &lt;Future at 0x1f8559d1750 state=running&gt;)  
  
Scheduled for (100000, &lt;Future at 0x1f8559d2a10 state=running&gt;)  
  
Scheduled for (100000000, &lt;Future at 0x1f8559d2ce0 state=pending&gt;)  
  
**퓨처 as_completed 결과 : (500500, True)**  
**퓨처 as_completed 캔슬 : &lt;bound method Future.cancelled of <Future at 0x1f8559d1750 state=finished returned int&gt;>**  
**퓨처 as_completed 결과 : (5000050000, True)**  
**퓨처 as_completed 캔슬 : &lt;bound method Future.cancelled of <Future at 0x1f8559d2a10 state=finished returned int&gt;>**  
**퓨처 as_completed 결과 : (5000000050000000, True)**  
**퓨처 as_completed 캔슬 : &lt;bound method Future.cancelled of <Future at 0x1f8559d2ce0 state=finished returned int&gt;>**  
  
**결과  시간: 5.16s, 실행된 스레드 또는 프로세서: 3**

일이 끝나는 순서대로 프린트문이 출력된다. 앞의 두 개는 1초 뒤에 출력된 반면 마지막 1억짜리는 5초 뒤에 출력되었다. 다행히도 실패한 일은 없었다.

---

정리하면, wait(iter)를 쓰면 우리가 지정해놓은 시간 내에서 성패를 가늠할 수 있다.

as_completed(iter)를 쓰면 일이 끝나는 순서대로 값을 반환한다. 다시 말해 A, B, C가 동시에 진행되어 A가 5분 걸리고 C가 1시간 걸리면 기존에는 모두 1시간이 걸렸을 테지만, 이제부터는 A는 5분만에 값을 반환하게 된다는 것이다. 물론 C 또한 동시에 실행되면서 말이다.

---