---
title: C# - 예외(Exception)
date: 2022-06-24 21:54:22 +/-09:00
category: [C_sharp/basic]
tag: [C_sharp, exception]
---

서론 : 본문은 {이것이 C#이다} Chapter 12장을 많이 참고했다.

```csharp
static void Main(string[] args)
{
    int [] arr = { 1, 2, 3 };

    try {
        for (int i = 0; i < 10; i++)
            Console.WriteLine(arr[i]); }
    catch // IndexOutOfRangeException {
        Console.WriteLine("예외가 발생했다!"); }
    Console.WriteLine("프로그램 종료");
```

1, 2, 3이 순서대로 작동한 뒤, 4부터는 IndexOutOfRangeException이 뜬다. 자연히 예외가 발생하고, 프로그램이 뻗는다. 하지만 catch문이 있다면 예외에서 "예외가 발생했다!" 를 출력시키고, 프로그램을 강제로 종료시키며 마지막 문장을 출력시킨다.

---

이번에는 고의로 예외를 만들어볼 시간이다.

```csharp
static void Main(string[] args)
{
    try {
        throw new Exception("예외 투척~");  }
    catch (Exception ex) {
        Console.WriteLine(ex.Message); }
    Console.WriteLine("프로그램 종료");
}
```

try 문이 실행됨과 동시에 예외가 발생하고, 예외 ex를 받은 catch는 ex의 Message를 출력하고 프로그램을 종료시킨다.

또다른 방법으로도 예외를 발생시킬 수 있다. 이 문법은 C# 7.0부터 사용 가능하다.

```csharp
static void Main(string[] args)
{
    int? a = null;
    int b = a ?? throw new ArgumentNullException("이번에도 예외다~");
    try {
        Console.WriteLine(b); }
    catch (Exception ex) {
        Console.WriteLine(ex.Message); }
    Console.WriteLine("프로그램 종료");
}
```

예외 처리가 가장 중요한 건 다른 파일 또는 서버와의 연결에서인데, 만약 try 부분에서 모종의 이유(서버 다운, 파일 오류 등)로 예외가 실행되었다면, 그리고 그 상태에서 파일이나 서버와 연결된 상태라면 그 파일이나 서버는 그대로 펑~ 하게 되는 대참사가 일어날 수 있다. 그렇다고 예외 처리문에 파일 또는 서버를 닫는 문장을 넣어서 해결한다고 하기도 애매한 것이 예외문은 하나만 있는 게 아니라 곳곳에 산재해 있을 것이기 때문이다.

--- 

finally 문은 try 문이 실행된다면 종료시 반드시 실행된다. 값 반환(함수 종료) 혹은 throw(예외 발생) 시에도 실행된다. 즉, 파일이나 서버와의 연결을 안전하게 끊을 수 있다.

```csharp
static void Main(string[] args)
{
    int a = 11;
    int b = 0;
    try {
        Console.WriteLine($"나누기 시작. {a} 나누기 {b}는 = {a / b}"); }
    catch (DivideByZeroException ex) {
        Console.WriteLine("나눌 수 없음.");
        throw ex; }
    finally {
    Console.WriteLine("모든 코드가 종료됨."); }
```

나눌 수 없음.
처리되지 않은 예외: System.DivideByZeroException: 0으로 나누려 했습니다.
위치: ConsoleApp11.Program.Main(String[] args) 파일 (경로 생략) :줄 22
모든 코드가 종료됨.

---

예외는 다양하고 언제 어디서 발생할지 알 수 없다. 이를 대비하기 위해 우리는 예외만을 전문적으로 처리하는 예외 클래스를 만들 수 있다.
예외 클래스는 System.Exception; 라이브러리를 상속받는다.

---

C# 6.0부터 예외 조건을 만족하는 객체에 대해서만 예외 처리 코드를 처리할 수 있는 예외 필터(Exception Filter)가 생겼다.
방법은 catch() 문 뒤에 when 키워드를 적으면 된다.
이것들에 대한 이야기를 비롯 보다 자세한 내용은 {이것이 C#이다} 책을 참고하길 바란다.

---