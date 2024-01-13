---
title: C# - 만능 변수
date: 2022-06-20 19:05:20 +/-09:00
category: [C_sharp/basic]
tag: [C_sharp, var, object, dynamic]
---

C#에는 언제 바뀔지 모르는 DataType이 있다. 프로그래밍을 하다 보면 int가 string이 될 수도 있고, list가 될 수도 있다. 그걸 그때마다 형식 변환하기 귀찮을 때도 있다. 그것을 성능상의 저하를 감안하고라도 사용할 경우 다음과 같은 해답이 존재한다.

<b>var, object, dynamic</b>

일단 테스트를 해보자.

```csharp
//bool 테스트
var isTrue = true;
object isTrue2 = false;
dynamic isTrue3 = true;

//str 테스트
var str1 = "나는 var";
object str2 = "나는 string";
dynamic str3 = "나는 dynamic";

//int 테스트
var int1 = 1;
object int2 = 2;
dynamic int3 = 3;
```

모두 빨간줄 없이 정상적으로 뜬다는 것을 알 수 있다.

여기서 int 형식을 string 형식으로 바꿔보자.
```csharp
int1 = "나는 var";
int2 = "나는 string";
int3 = "나는 dynamic";
```

"나는 var" 에 빨간 밑줄이 그어짐을 알 수 있다. var는 처음 타입에서 다른 타입으로 바꿀 수 없다. 이 부분에서 const와 비슷하다. const는 상수를 정해놓는다. const string = "스노우" 는 한 번 입력하면 다른 것을 대입할 수 없다. var에 처음 들어간 것이 string이라면 string으로 변할 수는 있어도 다른 것으로는 변할 수 없다.

```csharp
int a = int1;
int b = int2;
int c = int3;
```

이 경우 object인 int2에 밑줄이 그어진다. object는 Convert. 메소드를 쓰는 게 아닌 이상 다른 데이터 형식에 대입할 수 없다. 또한 object 변수는 데이터에 관계 없이 저장할 수 있지만 해당 데이터가 무슨 타입인지는 모른다.

dynamic의 경우 실행되는 시점(런타임)에서 DataType을 검사한다. 다시 말해서 컴파일 단계에서는 오류가 나도 오류를 띄우지 않는다.

.NET Framework 3.5 이전에는 object만이 존재했고 object는 성능상에 부정적인 영향을 준다. 그리고 3.5 초기에 var가 등장한다. 하지만 다른 데이터 타입으로 변경할 수 없음을 깨달은 개발자들이 아쉬움을 표하자 .NET framework 4.0에서 dynamic이 등장한다. 그래도 dynamic이라고 전부 만능은 아닌 것이 상술했듯이 디버깅의 문제가 있기에 코딩에서 어려움이 증가한다.

---