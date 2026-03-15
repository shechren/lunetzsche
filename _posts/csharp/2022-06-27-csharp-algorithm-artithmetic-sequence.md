---
title: C# - 알고리즘 - 등차수열(Artithmetic Sequence)
date: 2022-06-27 18:05:25 +/-09:00
category: [C_sharp/algorithm]
tag: [C_sharp, algorithm]
---

```csharp
namespace Algorithm2
{
	public class ArtithmeticSequence
	{
		int[] arr1 = new int[15];
		int[] arr2 = new int[20];
		int[] arr3 = new int[50];


		public void AS()
		{
```
이번에 쓸 알고리즘의 데이터다. 보다시피 총 3가지 데이터 내에서 결과를 뽑아낼 것이다.

```csharp
		static void Main(string[] args)
		{
			ArtithmeticSequence seq = new ArtithmeticSequence();
			seq.AS();
		}
	}
}
```

AS()라는 하나의 메소드로 전부 뽑아낼 거니까 미리 작성해준다.


1. 1부터 15까지 출력하라.

```csharp
foreach (int i in arr1)
{
    arr1[i] += 1;
    Console.WriteLine(arr1[i]);
}
Console.WriteLine("---------------");
```

foreach문을 이용한다. for문을 이용해도 상관없지만 귀찮으니 여기선 이렇게 한다. arr1[i]에 1을 더해주면 된다.

2. 20 이하의 수 중 4의 배수를 출력하라

```csharp
for (int i = 0; i < arr2.Length + 1; i++)
    if (i % 4 == 0 && i != 0)
        Console.WriteLine(i);
Console.WriteLine("---------------");
```

arr2.Length에 1을 더해주는 이유는 배열은 0부터 시작하기 때문이다.

4로 나눠서 0이 되는 숫자를 출력한다. 단, 0은 제외한다.

3. 50 이하의 수 중에서 3의 배수를 적는다. 단, 9의 배수는 제외한다.

```csharp
    for (int i = 0; i < arr3.Length + 1; i++)
        if (i % 3 == 0 && i != 0 && i % 9 != 0)
            Console.WriteLine(i);					
}
```

2와 같은 방식으로 한다. 여기서 9로 나눈 수를 제외한다.

---