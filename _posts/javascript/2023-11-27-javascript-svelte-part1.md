---
title: Svelte.js (1) - 입문
date: 2023-11-27 14:24:49 +/-09:00
category: [javascript/svelte]
tag: [javascript, svelte]
---

- [**환경설정**](#환경설정)
- [**Hello Svelte!**](#hello-svelte)
- [**더하기**](#더하기)
- [**컴포넌트화**](#컴포넌트화)

# **환경설정**

```terminal
npm create svelte@latest my-app
```

을 입력하고 Svelte-kit을 통한 설치를 시작한다.

Library project - Javascript - (ESLint, Prettier, Playwright, Vitest) 를 선택한다.

git repository에 my-svelte-app을 만들어주고 npm과 git 설정을 시작한다.


```terminal
cd my-app
npm install
git init
git add .
git commit -m 'first-commit'
git remote add origin my-svelte-app
```

Svelte는 기본적으로 다음과 같은 구조를 따른다.
App.svelte

```html
<script>
  // Javascript Area
</script>

<style>
  /* CSS Area */
</style>

<!-- HTML Area -->
```

App.svelte가 없으면 새로 만들어준다.

[https://svelte.dev/tutorial/](https://svelte.dev/tutorial/)

자세한 문법은 이 사이트에 가서 하나씩 공부하도록 한다.

---

# **Hello Svelte!**

```html
<script>
	let name = 'hi'
</script>

<h1> {name} </h1>
```

script의 내용은 name에 'hi' 를 할당했다. h1태그에는 중괄호 안에 name을 넣었다. 이렇게 되면 name은 자동으로 html에서 js를 향해 단방향 바인딩이 성립된다. 따라서 h1 태그에서 hi라는 문자가 출력된다.

# **더하기**

```html
<script>
	let firstIn = 0
	let secondIn = 0
	let result = 0
  $: {
    result = parseInt(firstIn) + parseInt(secondIn)
  }
	
</script>

<input bind:value={firstIn} />
<input bind:value={secondIn} />
<h1> {result} </h1>
```

첫 입력값, 두 번째 입력값, 결괏값을 초기화한다.
\$: {} 는 리액티브한 결괏값을 바로 얻어낼 수 있다. 중괄호 안에 값을 집어넣으면 된다.
input에는 bind:value를 해주는데, 중괄호만 적으면 단방향만 바인드되지만, bind를 해주는 순간 양방향 바인드가 된다.

# **컴포넌트화**

App.svelte
```html
<script>
	import App2 from './App2.svelte'
	
</script>
<h1> 안녕하세요! </h1>
<App2 />
```

App2.svelte
```html
<script>
	let hello = "hello!"
</script>
<h2> {hello} </h2>
```

App2의 h2를 가져와서 표기했다.

---