---
title: C# - 추상(abstract) 클래스와 인터페이스(interface)
date: 2022-06-22 18:58:15 +/-09:00
category: C_sharp/basic]
tag: [C_sharp, abstract, interface]
---

```csharp
public abstract class A
    {
        public abstract string CW();

    }
    public class B : A
    {
        public override string CW()
        {
            string str = "상속받았어요.";
            return str;
        }
    }
```

abstract 클래스 A는 abstract string CW() 라는 함수를 갖고 있다. 그리고 class B는 클래스 A를 상속받았다.

이대로 쓰면 오류가 난다. abstract 클래스는 반드시 가진 abstract 함수들을 override로 상속하게 된다. 따라서 class B 또한 override string CW() 함수를 가지게 되었다.

CW() 함수는 string str을 반환하다. string str은 "상속받았어요." 라는 문자열을 출력한다.

이제 파생 클래스인 B를 인스턴스화해서 쓰면 된다.

```csharp
static void Main(string [] args)
    {
        B b = new B();
        string message = b.CW();
        Console.WriteLine(message);
    }
```
추상 클래스는 다른 추상 클래스를 상속할 수 있다. 이때 파생 클래스는 기본 클래스의 abstract 함수를 구현할 필요가 없다.

---

인터페이스는 클래스와 비슷하지만 전혀 다르다. 인터페이스는 함수, 이벤트, 인덱서, 프로퍼티만 가질 수 있다. 다른 요소는 가질 수 없다.

```csharp
interface Imyinterface
    {
        void Ifunction();
    }
    ```
인터페이스를 상속하는 클래스는 선언된 모든 함수와 프로퍼티를 구현해야 하는데, 함수들은 반드시 public 한정자를 가져야만 한다. 그렇지 않으면 인터페이스를 상속받을 수 없다는 이야기.

```csharp
class MyClass : Imyinterface
{
    public void Ifunction()
    {
        string a = "여기는 인터페이스가 아닌 클래스이기에 string a 정의가 가능해요.";
        Console.WriteLine(a);
    }
}
```
이제 인스턴스화한 뒤 실행을 해보자.

```csharp
static void Main(string[] args)
    {
        MyClass myClass = new MyClass();
        myClass.Ifunction();
    }
```

---
 
![csharp-deadly-diamond.png](/assets/postingImage/csharp-deadly-diamond.png)

위와 같은 상황은 죽음의 다이아몬드(the deadly diamond of death)라고 불린다. 하나의 클래스는 하나의 클래스만 상속받을 수 있다.

사람은 간다는 함수를 갖고 있다. 사람을 상속받은 바퀴는 구른다는 함수를 갖고 있다. 사람을 상속받은 날개는 난다는 함수를 갖고 있다. 여기까지는 문제없다.


이 둘을 상속받은 탈것은 바퀴의 구른다를 상속받아야 할지, 날개의 난다를 상속받아야 할지 알 수가 없다. 그거는 컴퓨터는 물론 개발하는 사람도 모른다. 하지만 인터페이스는 다중 상속이 가능하다. 따라서 이러한 죽음의 다이아몬드를 어느 정도 피해갈 수 있는 것이다.

---