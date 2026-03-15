---
title: Mathematics - 확률변수와 확률분포
date: 2024-04-21 17:23 +/-09:00
category: [mathematics]
tag: [mathematics, distribution]
---

- [**확률변수**](#확률변수)
- [**분포**](#분포)
  - [**이산확률분포**](#이산확률분포)
    - [**이항 분포 (Binomial Distribution)**](#이항분포)
    - [**푸아송 분포 분포 (Poisson Distribution)**](#푸아송분포)
    - [**기하 분포 (Geometric Distribution)**](#기하분포)
  - [**연속확률분포**](#연속확률분포)
    - [**정규 분포 (Normal Distribution)**](#정규분포)
    - [**표준 정규 분포 (Standard Normal Distribution)**](#표준정규분포)
    - [**t 분포 (t-Distribution)**](#t분포)
    - [**카이제곱 분포 (Chi-squared Distribution)**](#카이제곱분포)
    - [**F 분포 (F-Distribution)**](#f분포)

---

# **확률변수**

이산확률변수(discrete random variable)란 셀 수 있는 유한 또는 무한의 값들을 취할 수 있는 변수를 말한다.
예를 들어 주사위 던지기, 동전 던지기를 n회 반복했을 경우의 결과가 있다.

연속형 확률변수(continuous random variable)는 특정 구간 내에서 모든 값을 취할 수 있는 변수를 말한다. 이러한 변수는 주로 실수 범위의 값을 가지며, 그 값들 사이에는 무한히 많은 가능한 값들이 존재한다.
예를 들어 특정인의 키나 몸무게 등이 속한다.

# **분포**

확률론과 통계학에서 어떤 확률변수가 취할 수 있는 값과 그 값이 나타날 확률을 설명하는 개념을 분포라고 부른다. 확률변수가 연속적인 값들을 가질 수 있는 경우에는 확률밀도함수(probability density function, PDF)로, 이산적인 값들을 가질 수 있는 경우에는 확률질량함수(probability mass function, PMF)로 표현된다.

# **확률분포**

확률변수가 취하는 값들의 집합이 자연수의 부분 집합과 일대일로 대응한다면 이산확률분포(discrete probability distribution)가 되고, 실수의 구간을 이루면 연속확률분포(continuous probability distribution)가 된다.

## **이산확률분포**

### **이항분포**
이항 분포(binomial distribution)는 실험 결과가 성공 또는 실패라는 두 가지 경우가 있는 경우에 사용된다.

$$ P(X = k) = \binom{n}{k} p^k (1-p)^{n-k} $$

 - ex: 동전 던지기의 결과

### **푸아송분포**
푸아송 분포(Poisson Distribution)는 특정 시공간에서 발생하는 이벤트의 횟수를 나타낼 때 적합하다.

$$ P(X = k) = \frac{e^{-\lambda} \lambda^k}{k!} $$

### **기하분포**
 - ex: 1시간동안 매장에 방문한 손님의 수

기하분포 (Geometric Distribution)는 첫 성공이 발생할 때까지의 실패 횟수를 나타낸다.

$$ P(X = k) = (1-p)^{k-1} p $$
 - ex: 복권 당첨이 되기까지 걸린 횟수

## **연속확률분포**

### **정규분포**

평균 μ를 중심으로 종 모양의 대칭을 이루는 분포다. 가장 널리 사용된다.

$$ f(x|\mu, \sigma^2) = \frac{1}{\sigma\sqrt{2\pi}} e^{-\frac{(x-\mu)^2}{2\sigma^2}} $$

### **표준정규분포**

정규 분포의 특수한 형태다. 평균이 0이며 분산이 1이다. 통계적 검정 및 표준화 점수 계산에 이용된다. **z분포** 라고도 부른다.

$$ f(z) = \frac{1}{\sqrt{2\pi}} e^{-\frac{z^2}{2}} $$

### **t분포**

작은 표본(30이하)에서 모표준편차가 알려지지 않은 상태에서 정규분포의 표본을 추정할 때 사용된다. 자유도 v에 따라 모양이 달라지며, 𝑣가 높아질수록 표준정규분포에 근사한다.

$$ f(t|v) = \frac{\Gamma(\frac{v+1}{2})}{\sqrt{v\pi}\Gamma(\frac{v}{2})} (1+\frac{t^2}{v})^{-\frac{v+1}{2}} $$

### **카이제곱분포**

주로 독립식 검정 또는 적합도 검정에서 사용된다.자유도 𝑘의 제곱합으로 계산되며, 크로스테이블에서 기대치와 관측치의 차이를 분석하는 데 사용된다.

$$ f(x|k) = \frac{x^{k/2-1} e^{-x/2}}{2^{k/2} \Gamma(k/2)} $$

### **F분포**

두 분산의 비율을 검정하는데 사용한다.

$$ f(x|d_1, d_2) = \frac{\Gamma(\frac{d_1+d_2}{2}) (d_1/d_2)^{d_1/2} x^{d_1/2-1}}{\Gamma(\frac{d_1}{2}) \Gamma(\frac{d_2}{2}) (1+(d_1/d_2)x)^{(d_1+d_2)/2}} $$


---