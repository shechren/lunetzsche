---
title: C# - 다형성(polymorphism)
date: 2022-06-22 15:17:46 +/-09:00
category: [C_sharp/basic]
tag: [C_sharp, class, partial, sealed, virtual, override]
---

sealed 메소드는 오버라이딩이 되지 않도록 봉인하는 역할을 한다. sealed class는 <b>다른 클래스를 상속받을 수 없다.</b>
```csharp
class A
  {
  public string name3 = "B로 상속이 불가능. B는 sealed 클래스이기 때문";
  }
sealed class B
  {
    public string name = "안녕";
    public string name2 = "안녕 2";
  }
  static void Main(string[] args)
  { 
    B b = new B();
    Console.WriteLine(b.name);
    Console.WriteLine(b.name2 = "안녕22");
  }
```
이제 오버라이드를 설명할 것이다. 우리는 class A를 만들고 이것을 다른 클래스들에 나눠줄 것이다. 방법은 A를 사용하면서 B와 C의 기능을 덮어씌우는 것이다. 쉽게 말해 A가 들은 폴더에 B와 C를 가져오면 A, B, C 모두 들은 것처럼 말이다.

---

```csharp
class A
    {
        public virtual void Me()
        {
            Console.WriteLine("Me를 나눠줄 거야.");
        }
    }
```
먼저 virtual 클래스를 하나 선언한다. 그리고 함수 Me를 만들었다. 우리는 이것을 모든 파생 클래스에서 실행할 것이다.
```csharp
class B : A
    {
        public override void Me()
        {
            base.Me();
            Console.WriteLine("첫 번째 오버라이드.");
        }
    }
  class C : A
    {
        public override void Me()
        {
            base.Me();
            Console.WriteLine("두 번째 오버라이드.");
        }
    }
    static void Main(string[] args)
    {
        B b = new B();
        C c = new C();
        b.Me();
        c.Me();
    }
```
B와 C를 만들고, override 메소드를 붙였다. 덮어씌운다는 것이다. A도 실행시킬 거니까 당연히 base.Me로 함수도 가져온다.

마지막으로 Main 함수 내에서 B와 C를 인스턴스화하면 끝난다.

"Me를 나눠줄 거야"

"첫 번째 오버라이드."

"Me를 나눠줄 거야"

"두 번째 오버라이드"

가 출력된다.

---

중첩 클래스(Nested Class)는 클래스 내의 클래스를 말한다. 클래스 외부에는 공개하지 않을 형색이나 현재 클래스의 일부로 표현할 클래스를 만들 때 사용한다.

```csharp
internal class Program
{
    private string str = "private가 선언된 전역변수";
class A
    {
        public class LittleA
        {
            public void OurTasks()
            {
                Program program = new Program();
                program.str = "여기서도 쓸 수 있어!";
                Console.WriteLine(program.str);
            }
        }
    }
    static void Main(string[] args)
    {
        A.LittleA a = new A.LittleA();
        a.OurTasks();
    }
}
```
전역변수 str은 private로 선언되었지만 중첩 클래스 LittleA의 함수 OurTasks에서 값이 덮어씌워지고 출력된다.

---

분할 클래스(partial class)는 여러 번에 나눠서 구현하는 클래스다. 쉽게 말해서 같은 이름을 여러번 선언할 수 있다. 별다른 기능은 없고 클래스가 아주 길게 늘어진다면 파일을 나누더라도 여러 개의 클래스지만 같은 이름이라면 하나의 클래스로 퉁쳐서 불러올 수 있다.

사용법은 class 앞에 partial을 붙이고, 같은 이름의 클래스를 여럿 나누면 된다.

```csharp
partial class A
    {
        public void A1() { }
    }
    partial class A
    {
        public void A2() { }
    }
    partial class A
    {
        public void A3() { }
    }
```

---