---
title: Mathematics - 순열(Permutation)과 조합(Combination)
date: 2023-11-22 22:28:54 +/-09:00
category: [mathematics]
tag: [mathematics, probability, statistics]
---

- [**순열(Permutation)**](#순열)
- [**조합(Combination)**](#조합)
- [**Quiz 1**](#quiz-1)
- [**Quiz 2**](#quiz-2)

# **순열**

a명의 사람이 있다. b명의 사람을 뽑아 한 줄에 늘어놓고 싶다. 경우의 수는

$$ _a P_b = \frac{a!}{(a-b)!} $$

가 된다.

쉽게 말해서, 5명의 사람 중 3명을 줄 세우고 싶다면,

$$ _5 P_3 = \frac{5!}{(5-3!)} = \frac{5!}{2!} = \frac{5\cdot4\cdot3\cdot2\cdot1}{2\cdot1} = 60 $$

이렇게 60가지의 경우의 수가 있다. 이것을 <b>순열(Permutation)</b>이라고 한다.

---

# **조합**

이번에도 a명의 사람 중 b명을 뽑아 한 줄에 세울 것이다. 하지만 줄을 서는 순서에 신경을 쓰지 않을 것이다.
이 경우를 <b>조합(Combination)</b>이라고 한다.

$$ _a C_b $$

이것을 <b>이항계수</b>라고 부르기도 한다.

$$  _a C_b = \frac{_a P_b}{b!} = \frac{a!}{b!(a-b)!} $$

다시, 5명의 사람이 있는데 3명을 세울 것이다. 순서는 무시한다.

$$  _5 C_3 = \frac{_5 P_3}{3!} = \frac{5!}{3!(5-3)!} = \frac{120}{6\cdot2} = 10 $$

이렇게 10가지 경우의 수가 나오게 된다.

---

# **Quiz 1**

젤리의 하루 권장 섭취량은 3개에서 6개다. 6일간 젤리를 먹을 경우 내가 먹을 젤리의 개수 경우의 수는?

$$ _6 C_4 = \frac{_6 P_4}{4!} = \frac{6!}{4!(6-4)!} = \frac{720}{24\cdot2} = 15 $$

---

# **Quiz 2**

10명의 학생 중에서 3명을 선발하여 1위, 2위, 3위를 결정하는 경우의 수는?

$$ _{10} P_3 = \frac{10!}{3!(10-3)!} = \frac {10!}{6! \cdot 7!} $$

10 팩토리얼을 어떻게 구하냐고 물어볼 수도 있다. 그런데 6! × 7!은 이미 구해져 있다.
그럼 우리는 8 × 9 × 10만 하면 10!을 구할 수 있다.

$$ = \frac{6! \cdot 7! \cdot 720}{6! \cdot 7!} = 720 $$

---