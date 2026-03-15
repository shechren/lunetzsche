---
title: C# - 문자열 슬라이싱
date: 2022-06-24 21:02:36 +/-09:00
category: [C_sharp/basic]
tag: [C_sharp, slice]
---

```csharp
static void Main(string[] args)
{
    string str = "    abcdefg,hijklmn,opqrstu,abcdefg    ";
    Console.WriteLine("e의 위치 " + str.IndexOf("e"));
    Console.WriteLine("마지막 a의 위치 " + str.LastIndexOf("a"));
    Console.WriteLine("10번째 단어부터 시작 " + str.Substring(10));
    Console.WriteLine("5 ~ 9번째 단어 " + str.Substring(5, 9));
    Console.WriteLine("선행 공백 제거" + str.Trim() + "후행 공백 제거");
    Console.WriteLine("단어를 바꾼다." + str.Replace("abcdefg", "바꾼 단어"));

    string[] spl = str.Split(','); // 1글자니까 작은따옴표를 붙인다.
    foreach (string a in spl) Console.WriteLine("쉼표 기준으로 나눈다." + a);
}
```
출력 결과:

e의 위치 8
마지막 a의 위치 28
10번째 단어부터 시작 g,hijklmn,opqrstu,abcdefg
5 ~ 9번째 단어 bcdefg,hi
선행 공백 제거abcdefg,hijklmn,opqrstu,abcdefg후행 공백 제거
단어를 바꾼다.    바꾼 단어,hijklmn,opqrstu,바꾼 단어
쉼표 기준으로 나눈다.    abcdefg
쉼표 기준으로 나눈다.hijklmn
쉼표 기준으로 나눈다.opqrstu
쉼표 기준으로 나눈다.abcdefg

---