---
title: C# - 배열(Array) - (2)
date: 2022-06-23 17:10:34 +/-09:00
category: [C_sharp/basic]
tag: [C_sharp, list]
---

배열은 지정된 수만큼 메모리를 할당한다. 따라서 유동적이지 못하고, 부러 큰 배열을 사용했는데도 데이터가 작으면 할당되지 않은 메모리를 그만큼 낭비하게 된다.

 

우리는 그것을 해결하기 위해 동적 배열을 사용하는데, 그것을 두고 리스트(List)라고 부른다.

리스트의 선언은 다음과 같다.

<b>List\<데이터타입\> list;</b>

그리고 이를 사용하기 위해서는 제네릭을 선언해줘야만 한다.

<b>using System.Collections.Generic;</b>

리스트는 클래스이므로 새롭게 인스턴트를 선언해줘야만 한다.

<b>List\<int\> list = new List\<int\> { 1, 2, 3 };</b>

---

리스트는 내 마음대로 크기를 조절할 수 있다.

```csharp
list.Add(4);
```

이렇게 하면 1, 2, 3이 들은 리스트의 마지막에 4가 추가된다.

```csharp
List<int> list = new List<int> {  };
for (int i = 0; i < 5; i++)
    list.Add(i);
```

다음과 같이 리스트에 0부터 4까지 추가할 수도 있다.

```csharp
foreach (int i in list)
    Console.WriteLine(i);
```

0부터 4까지의 리스트를 출력하려면 위 코드를 쓰면 된다.



list.Insert()메소드도 있다. 첫 번째 인자는 인덱스, 두 번째 인자는 바꿀 변수를 넣는다.
```csharp
list.Insert(3, 100);
foreach (int i in list)
    Console.WriteLine(i);
```

3번째 인덱스에 100이 추가되고, 3은 4번째 인덱스로 밀리게 된다. 자연스레 4는 5번째, 5는 6번째에 출력된다.

하지만 이를 위해서는 3번째 인덱스 다음에 있는 녀석들의 인덱스를 전부 뒤로 미뤄야 하기 때문에 그다지 효율적인 방법이 아님을 알아둬야 한다.



이제 다시 100을 지우고 싶다면
```csharp
list.Remove(100);
```

을 쓰면 된다.

이렇게 값으로 지울 수도 있지만, 인덱스로 지울 수도 있다.
```csharp
list.RemoveAt(3);
```

이렇게 하면 처음처럼 0부터 4까지 출력된다.



Remove와 RemoveAt 또한 이후의 인덱스들을 전부 앞으로 당겨야 하기 때문에 Insert와 마찬가지로 그다지 효율적인 방법이 아니다.
```csharp
list.Clear();
```
.clear 메소드를 사용하면 리스트를 전부 지울 수도 있다.

---