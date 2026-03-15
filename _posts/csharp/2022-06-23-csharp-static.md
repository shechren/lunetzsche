---
title: C# - static 한정자
date: 2022-06-23 10:49:16 +/-09:00
category: [C_sharp/basic]
tag: [C_sharp, static]
---

static 한정자로 변수/함수/클래스를 지정하면 프로그램 내를 통틀어 단 한 개만 존재함을 의미한다.
```csharp
public static void StaticA() 
{
    Console.WriteLine(staticString);
}
```

만약 클래스가 static이라면 그 이하 변수와 함수 또한 static을 부여받아야 한다.

```csharp
static class A
    {
        static string staticString = "정적 한정자";

        public static void StaticA() 
        {
            Console.WriteLine(staticString);
        }
    }
```

호출할 때는 인스턴스화를 할 필요 없이 그냥 클래스 자체로 접근하면 된다.

```csharp
static void Main(string[] args)
{
    A.StaticA();
}
```

---