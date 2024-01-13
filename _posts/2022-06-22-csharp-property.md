---
title: C# - 속성(property)
date: 2022-06-22 10:10:34 +/-09:00
category: [C_sharp/basic]
tag: [C_sharp, get, set, property]
---

```csharp
{
    class ABC
    {
        private string abc = "ABC";

        public string getabc() { return abc; }
        public void setabc(string abc) { this.abc = abc; }//abc = value;
    }
    class Program
    {
        static void Main(string[] args)
        {
            ABC abc = new ABC();
            abc.getabc();
            abc.setabc("abc");
        }
    }
}
```

일반적으로 프로젝트를 진행할 경우 저 ABC란 변수를 함부로 고치게 놔둬서는 곤란하다. 누구나 접근할 수 있기에 잘못 불러올 가능성도 있을 뿐더러, 실제로 그것을 건드렸다간 const가 아닌 const를 건드릴 수도 있기 때문이다. 예컨대 기사의 체력이 100이 기본인데 거기서 우리가 입력한 체력 스탯만큼 증가한 값을 가진 기사가 초기값이라든지. 그런데 그걸 불러와서 2000으로 고친다? 이러면 안 되는 것이다.
그래서 일반적으로는 저것은 반드시 private로 두거나 혹은 상속 시에만 protected로 열어두고, get, set을 다음과 같이 열어둔다. set의 this.abc = abc;는 abc = value;와 같다고 주석을 달아놓았다.
 
근데,,, 코드가 너무 길어 가독성이 떨어진다. 이럴 때 더욱 코드를 줄일 수 있다.
```csharp
class ABC
{
    public string abc
    {
        get { return "안녕"; }
        set { abc = "안녕"; }
    }
}
class Program
{
    static void Main(string[] args)
    {
        ABC a = new ABC();
        Console.WriteLine(a.abc);
    }
}
```
<b>get은 ReadAble, set은 WritAble이다. 따라서 get만 있다면 ReadOnly, set만 있다면 WriteOnly가 되는 것이다.</b>
아무튼 우리는 코드를 획기적이진 않지만 어느 정도 줄이면서 가독성도 확보했다.
하지만 그래도 뭔가 부족하다.
```csharp
class ABC
    {
        public string abc
        {
            get; set;
        }
    }
class Program
{
    static void Main(string[] args)
    {
        ABC a = new ABC();
        a.abc = "안녕";
        Console.WriteLine(a.abc);
    }
}
```
프로퍼티의 마법은 여기서 끝이 아니다. get; set; 이 두 글자로 첫 번째 코드의 그 장황함을 모두 요약할 수 있다.
만약 ReadOnly로 만들고 싶으면 set은 지우고, WriteOnly로 만들고 싶으면 get을 지우면 된다. 둘 다 하고 싶으면 둘 다 하면 되는 것이다.

---