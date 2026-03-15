---
title: C# - Generic(제네릭)
date: 2022-06-23 19:12:42 +/-09:00
category: [C_sharp/basic]
tag: [C_sharp, generic]
---

제네릭은 일반화라고 부르며 우리가 말하는 모든 데이터 형식 또는 함수의 타입에 대해 우리가 원하는 타입으로 고정시킬 수 있다.

```csharp
class A<T>
{
    T[] array = new T[10];
}
class B<T>
{
    public T Addition(T result)
    {
        return result;
    }
}
static void Main(string[] args)
{
    A<int> a1 = new A<int>();
    A<string> a2 = new A<string>();
    B<string> b = new B<string>();
    Console.WriteLine(b.Addition("안녕하세요!"));
}
```
클래스 A는 T 타입이다. 왜 T 타입이냐면 우리가 T 타입으로 선언했기 때문이다. A에는 배열[]이 들어갔고, array[]는 총 10개다.


클래스 B 또한 T 타입이다. 그 이하 Addition 함수 또한 T 타입이다. 반환되는 Result도 T 타입이다.


마지막에 인스턴스를 선언할 때 우리가 원하는 형식을 그제서야 대입한다.

a1은 int 타입이고 a2는 string 타입이다.

b는 string 타입이고, 매개변수에 "안녕하세요!"를 넘겨서 출력시켰다.

---