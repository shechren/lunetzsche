---
title: C# - 생성자(Constructors)
date: 2022-06-21 16:51:45 +/-09:00
category: [C_sharp/basic]
tag: [C_sharp, 생성자]
---

생성자는 클래스를 초기화할 때 사용한다.

```csharp
using System;

namespace ConsoleApp10
{
    internal class Program
    {
        struct Name
        {
            public string a { get; set; } // 이름
            public int b { get; set; }  // 나이
            public string c { get; set; } // 비고
        public Name(string a, int b, string c)
            {
                this.a = a; this.b = b; this.c = c;
            }
        }
        static void Main(string[] args)
        {
            Name name = new Name("스노우", 2, "꼬마");
            Console.WriteLine($"{name.a}, {name.b}, {name.c}");
        }
    }
}
```

생성자는 하나의 클래스에 하나만 들어갈 수 있다. 반드시 클래스의 이름과 일치해야 하며, 아무것도 return하지 않는다. 생성자 앞에 적을 수 있는 문장은 상속 메소드 뿐이다.
생성자는 자신의 클래스에 있는 내용을 상속받을 수 있다.

```csharp
struct Name
      {
          public string a { get; set; }
          public int b { get; set; }  
          public string c { get; set; }
      public Name(string a, int b, string c) // 생성자
          {
              this.a = a; this.b = b; this.c = c;
          }
      }
```
생성자는 자신의 클래스에 있는 내용을 상속받을 수 있다.
여기서 this.가 무엇을 가리키는지, 각 변수가 무엇을 가리키는지 헷갈릴 수 있다.


이렇게 표시해두면 이해가 바로 쉽다. 역시 백문이 불여일견이다.

```csharp
static void Main(string[] args)
      {
          Name name = new Name("스노우", 2, "꼬마"); //생성자 호출
          Console.WriteLine($"{name.a}, {name.b}, {name.c}");
      }
```
이제 생성자를 호출하면 된다. 호출하는 방법은 다음과 같다. 생성자 생성자의 변수명 = New 생성자(매개변수)
위에서는 Name이 생성자니까 Name(생성자) name(생성자의 변수명), = New Name(New 생성자)가 되는 것이다. 그리고 그 생성자에 매개변수를 받아왔다. 무엇을? 위에 적은 a, b, c를 말이다.
그리고 우리는 name이라는 변수명을 name.변수명으로 간단히 호출할 수 있게 된다.

---