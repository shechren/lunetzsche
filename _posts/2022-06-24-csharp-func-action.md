---
title: C# - Func와 Action
date: 2022-06-25 11:14:14 +/-09:00
category: [C_sharp/basic]
tag: [C_sharp, func, action, lambda]
---

하나의 익명 메소드 또는 람다를 선언하기 위해 매번 별개의 대리자(delegate)를 선언하는 것도 일이라면 일이다. 닷넷은 Func와 Action이라는 대리자 또한 지원한다. Func는 반환값이 있고, Action은 반환값이 없다.(void)

---

본문의 코드들은 Main 함수 내의  void A() 함수에서 실행시켰다. 전역에서 실행시키고 싶을 경우 public delegate void를 붙여주면 된다.

Func를 사용한 예시를 보자.

```csharp
Func<int, int, int> func1 = (int a, int b) => a + b;
Console.WriteLine(func1(1, 2));
```

Func 대리자는 총 16개의 입력값을 받을 수 있다. 그리고 추가로 반환값을 하나 더 받는데, 다시 말해 총 17개의 값을 적을 수 있고, 몇 개를 적건 마지막 값이 반환값으로 되는 것이다.

위에서는 input으로 2개의 int를 받았고 return으로 int를 돌려준다. Func1로 선언된 Func는 람다식을 가지는데 a와 b라는 매개변수가 있고 그것은 a+b를 리턴한다.

다시 말해 a와 b를 매개변수로 받으면 a+b를 리턴하는 녀석이 Func 대리자다.

위의 func1은 당연히 1+2니까 3을 반환한다.

```csharp
Func<int, string> func2 = (int x) => x + "이 입력되었어요.";
Console.WriteLine(func2(3));
```

입력값과 반환값의 데이터 형식이 다를 수도 있다. 나는 3을 입력하고, 반환값을 string으로 하면 Func는 자동으로 반환값의 형식을 int가 아닌 string으로 변환해준다.

```csharp
Func<int> func3 = () => 100;
Console.WriteLine(func3());
```

입력값이 없다면 다음과 같이 표기한다. 이 경우 반환값이 int 100이므로 무조건 저것을 반환한다.

---

Action은 조금 다르다. Func처럼 17개를 입력할 수 있지만 역시나 16개까지의 입력을 받을 수 있다. 나머지 하나는 입력값이 없는 것이다. 

본문의 코드들은 Main 함수 내의 void A() 함수에서 실행시켰다. 전역에서 실행시키고 싶을 경우 public delegate void를 붙여주면 된다.
```csharp
Action act1 = () => Console.WriteLine("Action1");
act1();
```

"Action"을 출력하는 함수다. 입력값도, 반환값도 없다.

```csharp
int actInt1 = 0;
Action<int> act2 = (c) => actInt1 = c * c;
act2(3);
Console.WriteLine(actInt1);
```

인자를 하나만 받는 Action이다. actInt1에 3을 제곱한 수를 대입한다. 다시 말하지만 이 수는 act2에 반환하는 것이 아니다. act2는 반환값이 void이며 입력값만 받는다. 쉽게 말해 WriteOnly인 것이다.

actInt1의 값을 출력해보면 당연히 9가 반환됨을 알 수 있다.

```csharp
float actFlt2 = 3.0f;
Action<float, float> act3 = (d, e) => actFlt2 = d - e;
act3(2.5f, 1.6f);
Console.WriteLine(actFlt2);
```
인자를 2개 받는 Action이다.

---