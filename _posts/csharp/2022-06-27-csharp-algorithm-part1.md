---
title: C# - 알고리즘 - 합계(Sum), 최대(Max), 최소(Min), 평균(Average)
date: 2022-06-27 14:18:22 +/-09:00
category: [C_sharp/algorithm]
tag: [C_sharp, algorithm, linq]
---

```csharp
class Program
{
    static int[] scores = {77, 89, 95, 60, 91, 100, 96, 88, 75, 82 };
```
우리가 쓸 데이터다. 우리는 여기서 합계, 최대, 최소, 평균을 구할 것이다.

<b>Sum(합계)</b>

합계를 구하긴 구할 건데, 85점 이상인 녀석들만 골라서 합계를 구할 것이다. 그냥 구하면 너무 쉬우니까 말이다. 만약 그냥 구한다면 이하 코드에서 if문을 빼버리면 된다.

```csharp
class Sum
{
// 85점 이상인 학생들의 총점을 계산
int sum = 0;
public void SumResult()
    {
    for (int i = 0; i < scores.Length; i++)
        if (scores[i] > 85)
            sum += scores[i];

Console.WriteLine($"for문 합계: {sum}");
```

간단하다. 이걸로 끝? 아니다. LINQ를 이용하면 더욱 쉽게 구할 수 있다.

```csharp
Console.WriteLine($"LINQ 합계: {scores.Where(s => s >= 85).Sum()}");
```

---

<b>Max(최대)와 Min(최소)</b>

이거는 살짝 더 복잡하다. 물론 LINQ문은 간단히 해결해주긴 하지만, 일단 for문부터 만들어보자.

```csharp
public void MaxMinResult()
    {
    int max = 0; int min = 100;
    
    for (int i = 0; i < scores.Length; i++)
        if (max <= scores[i])
            max = scores[i];
        else
            continue;

    for (int i = 0; i < scores.Length; i++)
        if (min > scores[i])
            min = scores[i];
        else
            continue;

    Console.WriteLine($"for문 최고 점수 : {max}");
    Console.WriteLine($"for문 최소 점수 : {min}");
```
max의 초기 최댓값은 0, min의 초기 최소값은 100이다.

max는 배열을 순회할 때마다 해당 값이 자신을 넘어설 경우 max에 그 값을 대입시킨다.

min은 배열을 순회할 때마다 해당 값이 자신의 값보다 못미칠 경우 min에 그 값을 대입시킨다.

해당하지 않으면 둘 다 continue를 때려서 해당 i를 무시하게 한다.

이걸 LINQ로 한다면?

```csharp
Console.WriteLine($"LINQ 최고 점수 : {scores.Max()}");
Console.WriteLine($"LINQ 최소 점수 : {scores.Min()}");
```

---

<b>Average(평균)</b>

평균은 조금 더 복잡한 방법이지만 이 역시 LINQ를 쓰면 매우 간단하다.

```csharp
public void AverageResult()
{
    int avg0 = 0;
    
    for (int i = 0; i < scores.Length; i++)
    {
        avg0 += scores[i];
        if (i == scores.Length - 1)
            avg0 /= scores.Length;
    }
        
    Console.WriteLine($"for문 평균 점수 : {avg0}");
```

int avg0은 0의 초기값을 가진다. 배열을 순회할 때마다 값을 더하는 건 sum과 같다.

배열의 마지막에 도달했을 경우 배열의 길이로 나눠준다.

배열의 마지막에 도달할 때는 길이의 -1로 계산해준다. 왜냐하면 배열은 0부터 시작하기 때문이다.

```csharp
int avg1 = 0;
avg1 = scores.Sum() / scores.Length;
Console.WriteLine($"Sum / Length 평균 점수 : {avg1}");
```

하지만 이런 건 속 편하게 LINQ로 계산해버리면 딱이다. 물론 더 좋은 메소드도 있다.

```csharp
Console.WriteLine($"LINQ 평균 점수 : {scores.Average()}");
```

마지막으로 모든 걸 인스턴스화해서 출력시켜주면 된다.

```csharp
static void Main(string[] args)
{
    Sum sum = new Sum();
    sum.SumResult();
    MaxMin maxmin = new MaxMin();
    maxmin.MaxMinResult();
    Average average = new Average();
    average.AverageResult();
```

전체 코드는 아래 링크에 있다.

[https://github.com/shechren/CsharpAlgorithm/blob/main/SumMaxMinAverage](https://github.com/shechren/CsharpAlgorithm/blob/main/SumMaxMinAverage)

---