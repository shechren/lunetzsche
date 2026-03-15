---
title: C# - 배열(Array) - (2)
date: 2022-06-23 16:00:45 +/-09:00
category: [C_sharp/basic]
tag: [C_sharp, array]
---

다차원 배열을 이용하면 다양한 상황에서 이용할 수 있다. 예를 들어 미로를 만들 수도 있고, 게임에서 미니맵을 만들 수도 있다.

사용법은 배열과 유사하다. 단지 쉼표만 붙을 뿐이다.

<b>데이터형식[ , ] 배열이름 = new 데이터형식[크기];</b>

```csharp
int[,] a = new int[5, 5] {
{ 1, 2, 3, 4, 5 },
{ 6, 7, 8, 9, 10 },
{ 11, 12, 13, 14, 15 },
{ 16, 17, 18, 19, 20 },
{ 21, 22, 23, 24, 25 } };
```

조금 무식한 방법...을 통해서 5 * 5 배열을 만들었다. 하지만 이렇게 만들면 인덱스 이상의 의미는 없어보인다.

배열을 조금 건드려보자.

```csharp
int[,] a = new int[5, 5] {
{ 0, 1, 0, 1, 0 },
{ 1, 1, 1, 1, 1 },
{ 1, 1, 1, 1, 1 },
{ 0, 1, 1, 1, 0 },
{ 0, 0, 1, 0, 0 } };
```

이제 하트 모양이 되었다. 정말로 이 하트를 콘솔에 출력해보자.

```csharp
void ShowMe()
  {
      var firstcolour = Console.ForegroundColor;
      for (int x = 0; x < a.GetLength(0); x++)
      {
          for (int y = 0; y < a.GetLength(1); y++)
          {
              if (a[x, y] == 1)
                  Console.ForegroundColor = ConsoleColor.Cyan;
              else
                  Console.ForegroundColor = ConsoleColor.Yellow;
              Console.Write("○");
          }
          Console.WriteLine();
      }
      Console.ForegroundColor = firstcolour;
  }
  ShowMe();
```
var firstcolour = 마지막 텍스트를 첫 번째 기본 디폴트 색상으로 되돌리기 위해 사용한다.

```csharp
for (int x = 0; x < a.GetLength(0); x++)
{
    for (int y = 0; y < a.GetLength(1); y++)
    {
      if (a[x, y] == 1)
          Console.ForegroundColor = ConsoleColor.Cyan;
      else
          Console.ForegroundColor = ConsoleColor.Yellow;
      Console.Write("○");
    }
    Console.WriteLine();
}
Console.ForegroundColor = firstcolour;
```
왼쪽 제일 위의 ○(동그라미)부터 for문을 시작한다. 주의할 점은 .Length가 아닌 .GetLength를 사용해야 한다.

아까 우리는 하트에 1을 적었다. 1을 적은 곳에는 녹청색이, 아닌 곳(0을 적은 곳)에는 노란색이 들어오게 했다.

그상태로 동그라미를 출력한다.

for문을 한 차례 나오면 줄을 넘기기 위해 콘솔창을 1줄 갱신해준다.

마지막으로 for문을 전부 나오면 아까 지정한 firstcolour로 되돌린다.

![csharp-array-1.png](/assets/postingImage/csharp-array-1.png)

출력 결과는 위와 같다.

---