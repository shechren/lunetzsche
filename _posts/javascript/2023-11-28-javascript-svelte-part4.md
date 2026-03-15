---
title: Svelte (4) - 상태 관리 -(1)
date: 2023-11-28 10:40:11 +/-09:00
category: [javascript/svelte]
tag: [javascript, svelte]
---

- [**components**](#components)
- [**트리-구조**](#트리-구조)
- [**스토어**](#스토어)

# **components**

두 개의 파일을 만든다. App2에서는 each문을 출력하고, App에서 그 컴포넌트를 가져오고 싶다. 하지만 배열은 App의 배열을 쓸 것이다. 이 경우 다음과 같이 할 수 있다.
App.svelte
```html
<script>
	import App2 from './App2.svelte'
	const nameList = ["이나르", "에스카", "스노우", "스페이드"]
</script>

<App2 nameList={nameList}/>
```
App2.svelte
```html
<script>
	export let nameList
</script>

<ui>
	{#each nameList as name}
		<li>
			{ name }
		</li>
	{/each}
</ui>
```
const로 배열을 만들었으나, export에서는 let으로 배열을 받는다.
nameList를 name으로 순회하고, 그 nameList를 다시 App으로 가져와서 받는다. 당연한 얘기겠지만 App2 nameList={nameList}에서, nameList와 {nameList}는 서로 다르다.

여기서 App은 App2의 부모 컴포넌트, App2는 자식 컴포넌트다.
우리는 App2 컴포넌트를 단순히 꺼내 쓰는 것만으로도 반복작업을 쉽게 할 수 있다.

---

# **트리-구조**

자바스크립트만이 아니라 여느 웹 구조건 다 그렇지만, 기본적으로는 트리 구조를 유지한다.

![javascript-svelte-tree](/assets/postingImage/javascript-svelte-tree.png)

만약 a1에서 A로 데이터를 보낸다면 자식 노드에서 부모 노드로 데이터를 올리는 것이다. 이 경우 <b>event</b>를 통해 데이터를 올리면 된다.
A에서 a1에서 데이터를 보낸다면 부모 노드에서 자식 노드로 데이터를 내리는 것이다. 이 경우 <b>props</b>를 통해 데이터를 내리면 된다. 다시 말하지만 이것은 svelte만이 아니라 리액트, 뷰, 플루터 등 모든 웹에서 다 같은 개념이다.

그런데, a1에서 B로 데이터를 보내고 싶다면? 다이렉트로는 가는 길이 없고, 그렇다고 다이렉트로 가는 길을 만들어뒀다간 여러가지로 꼬이고 난장판이 될 수밖에 없다. A랑 루트를 거쳐서 가는 길이 있다고는 해도 그 길을 다 거쳤다가는 A와 루트에 쓸데없는 코드를 여럿 작성할 수밖에 없다.

이를 개선하기 위해 절대다수의 웹 프레임워크에서는 <b>상태 관리 라이브러리</b>를 쓴다. 특징이라 한다면, 스벨트는 이러한 라이브러리가 기본적으로 내장되어 있다. 우리는 스벨트의 스토어 기능을 이용할 것이다.

---

# **스토어**

스토어는 말 그대로 상태 관리를 위한 라이브러리라고 생각하면 된다. A와 B가 있고 Store가 있다 가정하면, A와 B에서 스토어를 불러오고, 스토어의 내용을 수정해주면 A에도 B에도 즉각 적용되게 할 수 있다. 이를 통해 리액티브한 페이지를 더욱 쉽게 구성할 수 있다.

App.svelte
```html
<script>
	import { myStore } from './store.js'
	import App2 from './App2.svelte'

</script>
<input bind:value={$myStore} />
<App2 />
```

App에서는 인풋창 하나와 App2를 불러올 것이다.

App2.svelte
```html
<script>
	import { myStore } from './store.js'
</script>
<h1> {$myStore} </h1>
```

App2에서는 h1 태그 하나만 표기할 것이다.

```store.js
import { writable } from 'svelte/store'

export let myStore = writable('')
```

주의할 점은 store.js다. 즉, <b>자바스크립트 파일</b>이라는 점이다.
writable을 import하는 것이 첫째고,
둘째는 변수 하나를 writable로 선언한다. 이때 초기값은 괄호 안에 설정해준다.

이제 값을 불러오기 위해서는 App과 App2 모두 myStore를 import한 뒤, $ 기호를 통해 바인딩해주면 된다.

---