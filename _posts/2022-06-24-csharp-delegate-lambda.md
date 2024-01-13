---
title: C# - 해시테이블(HashTable)과 딕셔너리(Dictionary)
date: 2022-06-24 09:13:38 +/-09:00
category: [C_sharp/basic]
tag: [C_sharp, delegate, lambada]
---

Delegate는 컴파일 시점이 아닌 프로그램 구동 시에 실행되며, 값이 아닌 코드를 매개변수에 넘기고 싶을 때 사용한다. 즉, 별도로 함수를 또 만들지 않고 한 번 사용하고 버리는 기능을 사용할 때 쓴다.
사용 방법은 다음과 같다.

<b>한정자 delegate 반환형식 대리자이름(매개변수)</b>

```csharp
private delegate int Calc(int a, int b);
```

보다시피 세미콜론(;)으로 문장이 끝맺어져 있다. 일반적인 함수와 달리 델리게이트 내에 다른 내용은 전혀 지정되지 않는다. 이 델리게이트는 매개변수 a와 b를 받는다.

이제 많고 많은 계산기를 구현할 시간이다.

```csharp
private static int Sum(int num1, int num2)
{
    return num1 + num2;
}
private static int Subt(int num1, int num2)
{
    return num1 - num2;
}
private static int Mult(int num1, int num2)
{
    return (num1 * num2);
}
private static int Div(int num1, int num2)
{
    return (num1 / num2);
}
private static int Rem(int num1, int num2)
{
    return (num1 % num2);
}
```

이제 Main함수 내에서 호출할 시간이다. 여기가 중요하다.

```csharp
static void Main(string[] args)
{
    Calc calc = Sum;
    Console.WriteLine(calc(1, 2));
    calc = Div;
    Console.WriteLine(calc(10, 2));
    calc = Rem;
    Console.WriteLine(calc(10, 3));
}
```

New Calc()로 인스턴스화가 아닌, calc로 인스턴스화한 뒤 다른 함수를 대입한다.

1 + 2는 3을 반환한다. 그다음에는 calc에 Div을 대입한다. 이번에는 5를 반환한다. 마지막으로 나머지인 1을 반환한다.

이런 식으로 델리게이트 하나에 여러 함수를 돌려쓸 수 있다.

 

델리게이트는 연결이 가능하다. 그것을 두고 Delegate Chaining, 델리게이트 체이닝이라고 부른다.

```csharp
private delegate void Calc(int a, int b);

private static void Sum(int num1, int num2)
{
    Console.WriteLine(num1 + num2);
}
private static void Subt(int num1, int num2)
{
    Console.WriteLine(num1 - num2);
}
private static void Mult(int num1, int num2)
{
    Console.WriteLine(num1 * num2);
}
private static void Div(int num1, int num2)
{
    Console.WriteLine(num1 / num2);
}
private static void Rem(int num1, int num2)
{
    Console.WriteLine(num1 % num2);
}
```

함수 자체에서 콘솔을 출력하게 바꿔보자.

```csharp
static void Main(string[] args)
{
    Calc calc = (Calc)Delegate.Combine
        (new Calc(Sum), new Calc(Div), new Calc(Rem));
    calc(10, 5);
}
```

그리고 다음과 같이 호출한다. Sum과 Div과 Rem을 묶은 뒤, 1번째 매개변수에 10, 2번째 매개변수에 5를 넣고 3가지 방법 모두를 동시에 계산하게 한다.

---

그런데 이렇게 일을 해도 결과적으로는 함수를 호출한 뒤, 델리게이트를 사용하는 것이 된다. 즉, 두 번 일하게 된다. 생각해보면 되게 귀찮고 비효율적인 일이다. 그래서 .NET 3.0부터는 람다식이 도입된다.

지금부터 기적을 체험할 시간이다.

```csharp
private delegate int Calc(int a, int b);

static void Main(string[] args)
{
    Calc calc = (a, b) => a + b;
    Console.WriteLine(calc(2, 5));
}
```

람다식은 다음과 같이 사용한다.

매개변수의 형태 => 리턴 형식;

Calc calc를 선언했으니 왼쪽은 그냥 calc가 된 것이다. (a, b)라는 매개변수는 a + b로 자동 치환된다.

이런 간단한 경우도 있지만, 람다식이 진가를 발휘하는 시간은 다름아닌 List 형식이다.

 

4명의 귀족들 사이에서 무기로 레이피어를 다루는 녀석을 찾을 것이다. 이나르와 스노우는 에스터크, 라피스는 레이피어, 스페이드는 스파타를 쓴다.

```csharp
public class Nobility
{ 
    public string Name { get; set; }
    public int Age { get; set; }
    public string Weapon { get; set; }
}
static void Main(string[] args)
{
    List<Nobility> list = new List<Nobility>()
    { new Nobility {Name = "Snow", Age = 2, Weapon = "Estoc"},
        new Nobility {Name = "Lapis", Age=16, Weapon = "Rapier"},
        new Nobility {Name = "Inar", Age= 18, Weapon = "Estoc"},
        new Nobility {Name = "Spade", Age=20, Weapon = "Spata"} };
        
    for (int idx = 0; idx < list.Count; idx++)
    {
        if (list[idx].Weapon == "Rapier")
            Console.WriteLine(list[idx].Name);
    }
}
```

for문을 돌려 이런 식으로 라피스를 찾을 수도 있지만, 람다식을 쓰면 코드를 더 간단하게 줄일 수 있다.

그 이전에 하나를 더 불러오자.

```csharp
using System.Linq;
```

링큐를 불러온 뒤 람다식을 시작한다.

자, 저 for문을 단 한 줄로 줄여서 요약하자면

```csharp
list.Where(answer => answer.Weapon == "Rapier").ToList().ForEach(answer => Console.WriteLine(answer.Name));
```

위와 같다.

대충 보면 뭐라는 건지 도저히 알 수 없다. 이래서 람다식은 남용해서는 결코 안 되고, 사용할 때마다 주석을 적어두는 것이 좋다.

일단 끊어서 하나씩 보자면 다음과 같다.

```csharp
list.Where(answer => answer.Weapon == "Rapier"). //Where문으로 조건을 축약한다.
    // list에서 파생된 answer 익명함수를 선언한다. Rapier 사용자를 찾는다.
    ToList().ForEach
    // Foreach문에서 answer 익명함수를 실행시킨다.
    // 출력(해당하는 사람의 이름)
    (answer => Console.WriteLine(answer.Name));
```
이렇게 주석을 달고 끊어서 쓰면 이해도 쉬워지고, 쓸데없는 선언 등을 지울 수 있어 간편하고 유용하다.

---