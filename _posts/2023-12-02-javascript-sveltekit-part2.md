---
title: SvelteKit - (2)
date: 2023-12-03 12:24:15 +/-09:00
category: [javascript/svelte]
tag: [javascript, svelte, sveltekit, mariadb, tailwind]
---

- [**구조**](#구조)
- [**Routing**](#routing)
- [**Request와 Response**](#request와-response)

# **구조** 

SvelteKit의 구조는 상당히 특이하다. 지금까지의 라우팅은 파일을 대상으로 했다면, 우리는 폴더를 대상으로 라우팅을 할 것이다. 왜냐하면 한 폴더에 하나의 Svelte 파일, 하나의 JS 파일만 가지기 때문이다.
그래서 예컨대 './api/server.js' 에 액세스하기 위해서 뒤에 있는 '/server.js' 를 생략한다는 것이다.
또한 이 모든 건 routes 폴더 아래에 있어야 한다는 점 또한 잊지 말자.
대신 우리는 경로 내의 파일에 +page.svelte 또는 +server.js와 같은 네이밍을 통해 구분해준다.

![sveltekit-structure.png](/assets/postingImage/sveltekit-structure.png)

+page.svelte가 routes 폴더 바로 아래에 있다면 SvelteKit은 자동적으로 이 페이지를 실행시킨다. 따라서 routes/+page.svelte는 우리의 메인이자 홈 화면이 되는 것이다.

---

# **Routing**

![sveltekit-tag-a.png](/assets/postingImage/sveltekit-tag-a.png)

나는 홈 화면을 로그인 페이지로 하기로 했다. 간단한 과제니까 말이다. css의 경우 tailwind에서 적당한 걸 골라서 약간 손을 봐주었다.
여기서 중요한 건 회원가입 버튼을 누르면 회원가입 페이지로 넘어가야 한다. 회원가입 파일...이 아닌 경로(경로+파일)는 /SignUp 폴더에서 해결하기로 했다.

그럼 라우팅과 뭐 여러가지 설정을 하고 import를 해줘야 하지 않느냐 묻는다면 정상이다. 하지만 SvelteKit의 사용법은 많이 다르다.

```html
<button class="tailwind_tags..."><a href="/SignUp">회원가입</a></button>
```
회원가입 버튼의 button 태그다. class는 tailwind css때문에 그런 거니 신경쓰지 않아도 된다. 중요한 건 button 태그 내에 a 태그가 또 있다는 것이다. 이 a 태그를 통해 버튼을 누르는 순간 우리의 페이지는 /SignUP 경로로 이동되며, 자동으로 그 안의 +page.svelte를 찾아준다.

---

# **Request와 Response**

여기까지는 쉬웠지만 지금부터가 진짜 난관이다. 어떤 블로그를 가건 이것에 대한 정보가 없거나 혹은 개판으로 적어놨기 때문이다. 그 블로그 하는 사람들은 자기들도 그걸 못 읽을 거라고 나는 확신한다. 아무튼,

routes 폴더 안에 test 폴더를 만들고, +server.js 파일을 만든다.

```javascript
export const GET = async () => {
  return new Response(JSON.stringify({message: "안녕하세요!"}, {status: 200}))
}
```

GET은 이미 네이밍된 메소드 이름이다. 보면 알겠지만 함수 표현식이다. ES6권장은 함수 표현식 Svelte 권장은 함수 선언문, 여기서 다시 함수 표현식 + 화살표 함수를 쓴다고 해서 이상하게 볼 것은 없다. 자바스크립트는 원래 근본이 없어서 권장 문법을 무시하는 언어다.

Response도 이미 지정된 메소드인데, 우리는 저 메시지를 반환받기 위해 postman을 쓸 것이다.

![sveltekit-request-get.png](/assets/postingImage/sveltekit-request-get.png)

우리가 원하는 메시지가 반환된 걸 볼 수 있다.

이번에는 같은 폴더에서 POST 요청을 보내볼 것이다. POST 요청을 받으면 그 메세지 그대로 출력해보자.

![sveltekit-request-post.png](/assets/postingImage/sveltekit-request-post.png)

반환 Response가 없어 에러가 뜨긴 했지만 아무튼 우리가 원하던 값이 잘 나옴을 확인할 수 있다.

---