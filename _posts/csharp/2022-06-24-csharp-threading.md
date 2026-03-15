---
title: C# - 스레딩(Threading) - (1)
date: 2022-06-25 12:37:10 +/-09:00
category: [C_sharp/asyncronous]
tag: [C_sharp, asyncronous]
---

기본적으로 모든 프로그램은 프로세스 안에서 동작한다. 하나의 프로세스에 하나의 스레드가 할당되면 단일 스레드, 여러개의 스레드가 할당되면 멀티 스레드라고 부른다. 지금 만들 프로그램은 지금껏 만든 프로그램들과 다른, 멀티 스레딩 프로그램이다.

```csharp
using System.Threading;
```
가장 먼저 System.Threading을 선언한다.

```csharp
static Thread thread1; // static으로 수식해주는 이유는
static Thread thread2; // void Thr() 내에서 다른 클래스로 접근하기 위함이다.
static void Main(string[] args)
{
    Thr(); // 실행한다.
    void Thr() // 실행할 함수 Thr()
    {
        thread1 = new Thread(Test1);
        thread1.Start();
        thread2 = new Thread(Test2);
        thread2.Start();
    }
    void Test1()
    {
        int a = 0;
        while (true)
        {
            if (a > 5) thread1.Abort(); // 5번 실행하고 스레드를 멈춘다.
            Console.WriteLine("1번 스레드를 실행합니다." + a);
            Thread.Sleep(1000); // 1초간 대기
            a++;
        }
    }
    void Test2()
    {
        int b = 0;
            while (true)
            {
                if (b > 10) thread2.Abort(); // 10번 실행하고 스레드를 멈춘다.
                Console.WriteLine("2번 스레드를 실행합니다." + b);
                Thread.Sleep(300); // 0.3초간 대기
                b++;
        }
    }
}
```

출력 시 서로 다른 두 개의 스레드가 동시에 실행되어, 첫 스레드는 5초 뒤에, 두 번째 스레드는 3초 뒤에 종료됨을 알 수 있다.

Abort()문은 그다지 사용이 추천되지 않는다. 스레딩이 무한대로 반복되는 형태라면 외부에서 변수 등을 바꾸는 방향으로 개입되어야 한다. Abort문으로 해당 스레드가 잠겨버리고 다른 스레드가 컨텍스트 스위칭을 하지 못하게 되면 순식간에 프로세스 전체가 마비되는 꼴이 된다.


멀티 스레딩은 응답성이 높고 자원 공유가 수월하다. 그리고 결정적으로 경제성이 좋다. 이미 프로세스에 할당된 메모리와 자원을 사용하기 때문에 별도의 비용을 더 지불하지 않는다. 하지만 멀티 스레딩은 구현하기가 매우 까다롭고, 여러 스레드 중 하나에만 문제가 생겨도 기반 프로세스가 죽어버리는 문제가 발생할 수 있다. 또한 작업 간 전환(context switching)이 자주 일어나 비용이 많이 소모된다. 즉, 멀티 스레딩이 많이 사용될수록 성능이 떨어진다. 컨텍스트 스위칭은 파이썬 등 다른 비동기 프로그램에서도 많이 살펴볼 수 있는 용어다.

---