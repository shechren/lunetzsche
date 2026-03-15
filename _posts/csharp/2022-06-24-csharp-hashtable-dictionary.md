---
title: C# - 해시테이블(HashTable)과 딕셔너리(Dictionary)
date: 2022-06-24 06:33:02 +/-09:00
category: [C_sharp/basic]
tag: [C_sharp, hashtable, dictionary]
---

해시테이블은 Key와 Value로 이루어져 있다. 배열과 달리 숫자 인덱스가 아닌 Key 값을 호출해야 Value값을 볼 수 있다.

```csharp
using System;
using System.Collections;
```

해시테이블은 System.Collections 라이브러리에 들어있기에 반드시 이를 호출해줘야 한다.

해시테이블 예제를 만들어보자.

```csharp
Hashtable hs = new Hashtable();
hs.Add("좋은 아침입니다", "Good Morning");
hs.Add("안녕하세요", "Hello");
hs.Add("감사합니다", "Thank You");
hs.Add("잘자요", "Good Night");
```

이제 여기서 입력값을 Key로 변환해 해당하는 Value값을 찾아 반환해보자.

```csharp
Console.WriteLine("검색할 단어를 입력해주세요.");
string a = Console.ReadLine();
if (hs.Contains(a))
{
    Console.WriteLine(hs[a]);
}
```

위는 해시테이블.Contains(Key)로 Key 값을 찾으면 해시테이블[value]가 value값을 반환해주는 코드의 예시다.

---

딕셔너리 또한 컬렉션의 제네릭 라이브러리를 이용한다. 사용법은 리스트와 비슷하다.

해시테이블과의 차이점은 제네릭 형식으로 지정된다는 것이다. 그도 그럴 것이 키와 밸류값이 서로 다른 데이터형식일 수 있기 때문이다. 또한 해시테이블은 선언시 박싱이 일어나지만 딕셔너리는 박싱이 일어나지 않는다. (hashtable의 데이터 값은 object 타입으로 저장되기 때문이다.)

```csharp
using System.Collections.Generic;
```

딕셔너리는 다음과 같이 선언한다.

<b>Dictionary\<형식, 형식\> 이름</b>

리스트와 마찬가지로 인스턴스화해야 사용할 수 있다.

이제 다음 코드를 보자.

```csharp
class Cards
{
    public int number;

    public Cards(int number) { this.number = number; }
}
static void Main(string[] args)
{
    Dictionary<int, Cards> cards = new Dictionary<int, Cards>();
    for (int i = 1; i < 6; i++)
    {
        cards.Add(i, new Cards(i));
        Console.WriteLine(i);
    }
Console.WriteLine(cards[5]);
}
```

Cards 클래스는 생성자 Cards(인자 number)를 갖고 있다.

Dictionary cards를 생성해서 for문으로 카드를 5장 추가한 뒤 콘솔창에 출력해주었다.

마지막으로 5번 카드를 출력한다.

중요한 것은 5번 카드를 출력하는 것이지, 5번째(인덱스) 카드를 출력하는 것이 아니다. 딕셔너리는 인덱스가 존재하지 않는다.

그런데 5번째 카드의 반환값이 이상하다. 경로를 출력해주기만 하고 있다. 그것은 우리가 value의 반환값으로 class 형식을 출력했기 때문이다.

클래스는 지워버리고, 이번에는 value값에 string을 넣은 뒤 반환해보자.

```csharp
static void Main(string[] args)
{
    Dictionary<int, string> cards = new Dictionary<int, string>();
    for (int i = 1; i < 6; i++)
    {
        cards.Add(i, $"{i}번째 값입니다.");
    }
    foreach(KeyValuePair<int, string> card in cards)
        Console.WriteLine($"{Convert.ToString(card.Key)}은 {card.Value}");
}
```
<b>1은 1번째 값입니다.</b>
<b>2은 2번째 값입니다.</b>
<b>3은 3번째 값입니다.</b>
<b>4은 4번째 값입니다.</b>
<b>5은 5번째 값입니다.</b>

---