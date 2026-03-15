---
title: C# - 이벤트(Event)
date: 2022-06-24 16:56:04 +/-09:00
category: [C_sharp/basic]
tag: [C_sharp, event]
---

{이것이 C#이다} 476P를 보면 다음과 같이 이벤트를 선언할 수 있다고 나와 있다.


1. 대리자(Delegate)를 선언한다. 이 대리자는 클래스 밖이건 안이건 상관없다.
2. 클래스 내에 1에서 선언한 대리자의 인스턴스를 event 한정자로 수식해서 선언한다.
3. 이벤트 핸들러를 작성한다. 이벤트 핸들러는 1에서 선언한 대리자와 일치하는 메소드면 된다.
4. 클래스의 인스턴스를 생성하고 이 객체의 이벤트에 3에서 작성한 이벤트 핸들러를 등록한다.
5. 이벤트가 발생하면 이벤트 핸들러가 호출된다.

이제 순서대로 따라해볼 시간이다. 해당 페이지에 나와있는 예제를 따라해보도록 하자.

```csharp
delegate void EventHandler(string message);
class EventTest
{
    public event EventHandler Awake;
}
class EventSetup
{
    public event EventHandler Awake;

    public void TaskStart(int number)
    {
        int temp = number % 10;

        if (temp != 0 && temp % 3 == 0)
        {
            Awake($"짝, {number}");
        }
    }
}
class MainApp
{
    static public void MyHandler(string message)
    {
        Console.WriteLine(message);
    }

    static void Main(string[] args)
    {
        EventSetup eventSetup = new EventSetup();
        eventSetup.Awake += new EventHandler(MyHandler);
    
        for (int i = 1; i < 30; i++)
            eventSetup.TaskStart(i);
    }
}
```

![csharp-event.png](/assets/postingImage/csharp-event.png)
매커니즘은 사진과 같다. 30 이하의 정수 중 3, 6, 9로 끝나는 수에서 짝이 외쳐지며 멈춘다. Python의 제네레이터(yield와 await)와 비슷한 역할을 하는 것이다.

---

순서를 다시 정리하면,

Delegate를 선언한 뒤

해당 Delegate를 EventTest class의 event (delegate이름) (event이름)으로 수식한 뒤

TaskStart() 함수에서는 이벤트 핸들러와 실행할 명령들을 적는다.

그리고 Main 함수 내에서 인스턴스화한 뒤 이벤트 핸들러를 등록하고,

for문으로 이벤트를 발생시키면 TaskStart의 Awake가 MyHandler를 오가며 메세지를 출력한다.

 

이벤트는 대리자에 event 키워드를 수식해서 선언한 것에 불과하지만, 이벤트는 public으로 선언되어 있다 한들 선언된 클래스 외부에서는 호출이 불가능하다. 따라서 대리자를 이용하는 것이다.

이벤트는 사용자 입력 등을 능동적으로 받을 때 사용하게 된다. 예컨대 우리가 ESC 버튼을 눌렀는데 ESC 버튼에 해당하는 명령어가 없다면 버튼을 눌러도 아무것도 작동하지 않을 테고, 명령어가 있어도 알림이 없으면 우리는 ESC 버튼이 눌렸는지도 모를 것이다. 따라서 우리는 이벤트를 통해 보다 능동적으로 프로그램에 개입할 수 있다.

---