---
title: Svelte.js (2) - 제어문
date: 2023-11-27 17:59:43 +/-09:00
category: [javascript/svelte]
tag: [javascript, svelte]
---

- [**~if**](#if)
- [**~each**](#each)
- [**~await**](#await)

# **~if**

App.svelte
```html
<script>
	let name = 'svelte'
	let myBool = false
</script>

<button on:click={() => myBool = !myBool}>
	myBool!
</button>
{#if myBool}
<h1>Hello {name}!</h1>
{:else}
<h1>please leave here!</h1>
{/if}
```

name은 마음대로 적는다. 중요한 건 중괄호 안 부분이다.
중괄호를 열고 \#이 있다면 그건 제어문의 시작 부분이다. :이 있다면 그것은 중간 부분이다. /이 있다면 끝 부분이다.
여느 프로그래밍 언어나 안 그런 게 없지만, if 다음에 올 부분은 조건문이다. 조건이 True면 if 내의 문장을 실행한다.
따라서 위에서는 버튼을 클릭할 때마다 문자의 내용이 바뀌게 된다.

---

# **~each**

App.svelte
```html
<script>
	let actors = ["Tom", "Nicole", "Will", "Amanda", "Liam"]
</script>

<ul>
{#each actors as actor}
	<ol>
		{actor}
	</ol>
{/each}
</ul>
```

for가 아닌 each라고 쓴다. 아마 forEach에서 이름을 따온 듯하다.
each iter-able as item 순서로 쓰면 된다. 즉, each 다음 이터러블 객체를 넣고, as 내가 순회할 i를 넣는다.
그다음 ol에서 i를 순회시키면 된다.
주의할 점은 ul의 경우 each문 바깥에 작성해야 한다는 점이다. 우리가 순회시킬 것은 배열의 요소이지, 배열 전체가 아니기 때문이다.

---

# **~await**

svelte에서 await을 사용한다면 아마도 이렇게 사용할 것이다.
App.svelte
```html
<script>
  let isLoading = false
  function fetchData() {
    isLoading = true
    const promise = new Promise(resolve => {
      setTimeout(() => {
        isLoading = false
        resolve()
      }, 2000)
    })
    promise.then(() => {
      console.log('Data fetched!')
    })
  }
</script>
<button on:click={fetchData} disabled={isLoading}>
  {#if isLoading}
    <span class="loading">Loading...</span>
  {:else}
    Fetch Data
  {/if}
</button>
<style>
  .loading {
    font-weight: bold;
    color: blue;
  }
</style>
```

역시나 누더기언어 자바스크립트 아니랄까봐, 너무나도 장황하다.
하지만 {#await}을 사용하면 어느 정도 간결한 문법으로 이를 줄일 수 있다.

App.svelte
```html
<script>
	let promise

	function fetchData() {
		promise = new Promise(resolve => {
			setTimeout(resolve, 2000)
		})}
</script>
<button on:click={fetchData} disabled={promise}>
	{#await promise}
		<span class="loading">Loading...</span>
	{:then}
		Fetch Data
	{/await}
</button>
<style>
  .loading {
    font-weight: bold;
    color: blue;
  }
</style>
```
일단 async function을 사용하지 않고 일반 function으로 바꾼다. 
{#if}가 아닌 {#await}를 쓴다. await 다음에는 실행시킬 promise가 온다.
then이 작동하면 그때는 버튼이 비활성화된다.
쉽게 말하면, 우리가 만든 promise는 말 그대로 2초간 대기만 한다. 그 트리거가 발생함과 동시에 버튼의 글자색이 파란색으로 바뀌고, 2초가 지나면 disabled된다.

---