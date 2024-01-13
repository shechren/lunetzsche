---
title: C# - 접근 한정자
date: 2022-06-22 12:20:35 +/-09:00
category: [C_sharp/basic]
tag: [C_sharp, public, protected, private]
---

| 접근 제한자       | 설명                                                                           |
|------------------|--------------------------------------------------------------------------------|
| public           | 클래스의 내부/외부 모두에서 접근 가능.                                       |
| protected        | 외부에서 접근 불가, 파생 클래스에서는 접근 가능.                              |
| private          | 클래스의 내부에서만 접근 가능.                                                |
| internal         | 같은 어셈블리에서만 public으로 접근 가능. 다른 어셈블리에서는 private와 같다. |
| protected internal | 같은 어셈블리에서만 protected로 접근 가능. 다른 어셈블리에서는 private와 같다. |
| private protected | 같은 어셈블리 클래스의 상속받은 클래스 내부에서만 접근 가능.                     |

만약 아무것도 지정하지 않을 경우 모든 클래스의 멤버는 private로 자동 지정된다.
```csharp
public class Test1
{
    public string test1;
}
private class Test3
{
    public string test3 = "private입니다.";
    private string sectret = "수정할 수 없습니다.";
}
static void Main(string[] args)
{
    Test1 test1 = new Test1();
    Console.WriteLine(test1.test1 = "public입니다.");
    Test3 test3 = new Test3();
    Console.WriteLine(test3.test3);
}
```

Test2는 Protected를 설명하기 위해 조금 이따가 쓸 것이다.

Public은 Main함수 내에 인스턴스를 만들어서 즉시 호출 가능하다. private 또한 마찬가지다. 하지만 private은 Test1처럼 값을 수정할 수는 없다. 일종의 ReadOnly인 셈이다.

이제 Protected를 추가해보자.

```csharp
private class Test2
{
    protected string test2;
}
private class Test3 : Test2
{
    public string test3 = "private입니다.";
    private string sectret = "수정할 수 없습니다.";
    public string A()
    {
        return test2 = "protected입니다.";
    }
}
static void Main(string[] args)
{
    Test3 test3 = new Test3();
    Console.WriteLine(test3.A());
    Console.WriteLine(test3.test3);
}
```

private class Test3은 Test2를 상속받았다.

protected string test2는 상속받은 클래스 내에서 사용할 수 있다. 즉시 대입은 안 되고, 함수 A()를 선언한 뒤 거기서 test2를 반환해보도록 했다.

상속받은 클래스, 다시 말해 파생 클래스에서는 public과 같은 권한을 가짐을 알 수 있다.

---