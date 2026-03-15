---
title: C# - 참조(ref와 out)
date: 2022-06-21 17:44:01 +/-09:00
category: [C#/basic]
tag: [C#, ref, out]
---
```csharp
static void Multiple(int a, int b, int result)
{
    result = a * b;
}
static void Main(string[] args)
{
    int number1 = 2;
    int number2 = 5;
    int result;

    Multiple(number1, number2, result);
    Console.WriteLine(result);
}
```

이런식으로 하려는데 출력 바로 직전 줄에서 에러가 난다. 왜일까?
result값에 문제가 있는데, Multiple 함수 내의 result와 Main 함수 내의 result가 다르기 때문이다.
이 경우 out 메소드를 붙여 매커니즘을 명확하게 한다.

![csharp-ref-out-1.jpg](/assets/postingImage/csharp-ref-out-1.jpg)

F10을 눌러보면 순서를 알 수 있다. Multiple 함수가 실행되어 out result가 넘어가고, 그 값이 대입되고 반환되어 돌아온다.
```csharp
static void Multiple(int a, int b, out int result)
{
    result = a * b;
    Console.WriteLine("출력 2");
}
static void Main(string[] args)
{
    int number1 = 2;
    int number2 = 5;
    int result;
    Console.WriteLine("출력 1.");

    Multiple(number1, number2, out result);
    Console.WriteLine(result);
}
```

출력 결과 정확한 값인 10이 나온다. 상술한 그림처럼 출력 1이 먼저 출력되고 출력 2가 그다음 출력되는 것도 같이 확인할 수 있다.
```csharp
internal class Program
{
    static void Number(int a, int b, int result)
    {
        result = b - a;
    }
    static void Main(string[] args)
    {
        int num1 = 1;
        int num2 = 4;
        int result = num1 - num2;
        Program.Number(num1, num2, result);
        Console.WriteLine(result);
    }
}
```

---

다음 코드를 보자.
무엇이 실행될까?
result = num1 - num2를 했으니 1-4가 실행될까?
아니면 result = b -a니까 4-1이 실행될까?
정답은 전자다. 결과로는 -3이 나온다 왜 그럴까?

![csharp-ref-out-2.png](/assets/postingImage/csharp-ref-out-2.png)

빨간 줄은 적용되지 않는다. 왜냐면 이미 result는 진짜로 3이 된 상태인데 거기다가 함수 내에서만 동작하는 -3을 넣어봤자 가짜 result인 것이다.
그럼 어떻게 해야 저 함수가 작동하게 할 수 있을까?

![csharp-ref-out-3.png](/assets/postingImage/csharp-ref-out-3.png)

방법은 간단하다. ref를 넣어 그것이 진짜로 Number 함수 내의 result를 참조하게 하면 되는 것이다. 이렇게 되어도 Main 함수 내의 num1 - num2는 실행된다. 다만 진짜로 참조가 된 ref result의 3이 덮어씌우게 되는 것이다.
WriteLine으로 출력을 해보면 그 과정을 더 확실히 알 수가 있게 된다.

---

ref는 사용하기 전에 초기화가 되고,
out은 초기화가 되진 않지만 이전 값들을 전부 무시한다는 특징이 있다.

다시 말하면 out은 할당이 되지 않은 상태이며, 메서드 내에서 할당이 된다.

ref는 변수를 수정할 때, out은 값을 만들어내 반환할 때 사용한다.

둘의 차이를 잘 기억하자.

---