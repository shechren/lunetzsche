---
title: Svelte.js (3) - 이벤트
date: 2023-11-27 18:56:16 +/-09:00
category: [javascript/svelte]
tag: [javascript, svelte]
---

- [**on click**](#on-click)
- [**on input**](#on-input)
- [**on focus**](#on-focus)
- [**on mouse**](#on-mouse)

# **on click**

Svelte에 VanillaJS를 더한 기준으로 DOM을 찾아 버튼에 연결시켜 이벤트를 수행한다면 다음과 같은 과정을 거쳐야 한다.

App.svelte
```html
<script>
	import { onMount } from 'svelte'
	let text = "안녕하세요!"
	onMount(() => {
		const but = document.querySelector('.myButton')
		but.addEventListener('click', () => text = '잘가세요!')
	})
</script>
<button class='myButton'>
	버튼
</button>
<h1> 
	{text} 
</h1>
```
onMount라는 걸 받고, onMount()는 콜백 함수를 받는다. 그 안에서 우리의 addEventListener를 실행시킬 수 있다.
하지만 svelte에서 우리가 이렇게 어려운 과정을 거칠 일은 없을 것이다. on Event를 사용한다면 말이다.

App.svelte
```html
<script>
	let text = "안녕하세요!"
</script>
<button on:click={() => {text = "잘 가세요!"}>
	버튼
</button>
<h1> 
	{text} 
</h1>
```
on Event는 AddEventListener로 실행되는 모든 이벤트를 받을 수 있다. on:click 메소드는 콜백 함수를 하나 받고, 그 안에 우리가 실행할 내용을 적어주면 된다. 물론 내용이 길다면 그 내용을 스크립트로 별도로 빼서 적용해도 문제 없다.

---

# **on input**

App.svelte
```html
<script>
	let myText
  function handleInput(event) {
    myText = event.target.value
  }
</script>
<input on:input={handleInput} />
<h1> 
	{myText} 
</h1>
```
on:input에 인풋 핸들러 함수를 넣고, event를 받는다.
event.target.value는 우리의 인풋 상자를 가리킨다. 따라서 우리는 이렇게 간단한 방법으로 인풋 값을 받아올 수 있다.

---

# **on focus**

App.svelte
```html
<script>
	let state
  function handleFocus() {
    state = "포커스가 있어요!"
  }

  function handleBlur() {
    state = "포커스가 나갔어요!"
  }
</script>
<h1> {state} </h1>
<input on:focus={handleFocus} on:blur={handleBlur} />
```
focus와 blur를 이용하면 포커스 유무에 따라 다양한 이벤트를 사용할 수 있다.

---

# **on mouse**
App.svelte
```html
<script>
	let boxSize = 200
	function me() {
    boxSize = 300
  }
	function ml() {
		boxSize = 200
	}
</script>
<div class='myBox'
	on:mouseenter={me}
	on:mouseleave={ml}
	style="width: {boxSize}px;height: {boxSize}px;"></div>

<style>
	.myBox {
		background-color: green;
	}

</style>
```

다양한 마우스 이벤트들도 처리할 수 있다. 위 코드는 마우스가 들어가면 div 크기를 키우고, 마우스가 나오면 div 크기를 원상복구시키는 동적 페이지를 만들 수 있다.

---