---
title: C# - 링큐(LINQ)
date: 2022-06-24 13:00:15 +/-09:00
category: [C_sharp/basic]
tag: [C_sharp, linq, lambda]
---

링큐는 <b>Language Integrated Query</b>의 약자다.

일단 지난 시간처럼 리스트를 만들어 불러오자.

```csharp
class OurClass
{
    public string name { get; set; }
    public int achievement { get; set; }
    public string favourite { get; set; }
}
static void Main(string[] args)
{
    List<OurClass> list = new List<OurClass>()
    { new OurClass { name = "Snow", achievement = 80, favourite = "Arts" },
        new OurClass { name = "Inar", achievement = 95, favourite = "Language" },
        new OurClass { name = "Lapis", achievement = 100, favourite = "Mathematics" },
        new OurClass { name = "Spade", achievement = 85, favourite = "Arts" },
        new OurClass { name = "Tess", achievement = 90, favourite = "Social" },
        new OurClass { name = "Rachel", achievement = 80, favourite = "Arts" } };
        }
```

위와 같은 리스트를 만들었다. 나는 85점 이상의 성적을 낸 친구들을 찾고 싶다.

링큐는 다음과 같이 사용할 수 있다.

<b>from - in - (where) - select</b>

list 안에(in) 있는 객체로부터(from) 성적이 85점 이상인 친구들을 조회한다(select)

```csharp
var GoodAchieve = from l in list where l.achievement >= 85 select l;
```

마지막으로 foreach문을 돌려 콘솔창에 출력한다.

```csharp
foreach (var select in GoodAchieve)
{
    Console.WriteLine(select.name);
}
```
이나르, 라피스, 스페이드, 테스가 조회된다.

---

1. 람다식을 이용해 모두의 평균 점수를 계산해보자.

```csharp
Console.WriteLine(list.Select(a => a.achievement).Average());
```

88.33 이 나온다.



2. 점수가 낮은 순서대로 이름을 정렬하자.

```csharp
var sorted = from l in list orderby l.achievement select l.name;
foreach (var s in sorted)
{ Console.WriteLine(s);}
```
l이라는 익명함수는 list에서 나왔는데 achievement의 순서대로 나열해서 name을 지정한다.

foreach문으로 출력.



3. Arts를 좋아하는 친구들의 수를 출력하자.

```csharp
var art = from a in list group a by a.favourite into grp select new { favourite = grp.Key, Count = grp.Count() };

list.Where(a => a.favourite == "Arts").ToList().ForEach(a => Console.WriteLine(a.name));
foreach (var s in art)
    if (s.favourite == "Arts")
        Console.WriteLine(s);
    else
        Console.WriteLine();
```
람다식으로 먼저 where문을 통해 Arts를 좋아하는 친구들의 이름을 출력한다.

foreach문을 통해 favourite == "Arts"가 성립할 경우 그 수를 반환하고 없을 경우 None을 반환한다.

---