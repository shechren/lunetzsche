---
title: SvelteKit - (3)
date: 2023-12-05 20:55:39 +/-09:00
category: [javascript/svelte]
tag: [javascript, svelte, sveltekit, mariadb, tailwind]
---

- [**회원가입 만들기**](#회원가입-만들기)
- [**관리자 권한 부여**](#관리자-권한-부여)
- [**Layout**](#layout)
- [**Dynamic Route**](#dynamic-route)

# **회원가입 만들기** 

```html
<div class="tailwind css...">
  <div class="tailwind css...">
    <form class="space-y-6" method="POST" action='?/login' use:enhance={signIn}>
      <div>
        <label for="email" class="tailwind_css...">이메일</label>
        <div class="mt-2">
          <input id="email" name="email" value={form?.email ?? ""} type="email" autocomplete="email"class="tailwind_css...">
        </div>
      </div>
      <div>
        <div class="tailwind css...">
          <label for="password" class="tailwind css...">비밀번호</label>
        </div>
        <div class="mt-2">
          <input id="password" name="password" value={form?.password ?? ""} type="password" autocomplete="current-password" class="tailwind_css...">
        </div>
      </div>
      <div class="signButtons">
        <div class="signInButton">
          <button class="tailwind_css...">로그인</button>
        </div>
        <div class="signUpButton">
          <button class="tailwind_css..."><a href="/SignUp">회원가입</a></button>
        </div>
      </div>
    </form>
    <div class="message">
      <p> {message} </p>
    </div>
  </div>
</div>
<style>
  .signButtons {
    display: flex;
    justify-content: space-between;
  }
</style>
```
tailwind css를 이용해서 간단하게 css를 만들었다. 자세한 css 내용은 tailwind에서 찾길 바란다.(tailwind 코드이기에 라이선스 이유로 공개할 수 없다.)
let form으로 변수를 하나 만들어두고,
form 에는 이렇게 적는다.
```javascript
method="POST" action='?/login' use:enhance={signIn}
```

그리고 스크립트 영역에는 다음과 같이 적는다.
```javascript
import '$resources/app.css'
import { enhance } from '$app/forms'
import { goto } from '$app/navigation';

const signUp = ({form}) => {
  return async ({ result, update }) => {
    if (result.status === 200) {
      await goto('/', {replaceState:true});
    }
  };
}
```

이제 +page.server.js를 적어야 한다. +page.server.js는 내 +page.svelte의 백엔드에서 작동한다. js파일이기에 프론트엔드에서는 작동하지 않는다.

```javascript
import { createPool } from 'mariadb';
import query from '$routes/sql';

export const actions = 
 { create: async ({request}) => {
  try {
    const body = await request.formData();
    const email = body.get('email')
    const password = body.get('password')

    const pool = createPool({
      host: 'localhost',
      user: 'root',
      password: 'test1234',
      port: 5000,
      database: 'svelte_service'
    });
    const connection = await pool.getConnection();

  try {
    await connection.beginTransaction();
    await pool.query(query.createDatabase);
    await pool.query(query.useDatabase);
    await pool.query(query.createAccountTable);
    await pool.query(query.insertAccount, [[email], [password]]);
    await connection.commit();
  } catch (error) {
    await connection.rollback();
    throw new Error(error)
  } finally {
    await connection.release();
    await pool.end();
  }
    return {
      success: true
    }}
    catch (err) {
    return err
  }
}}
```
formData는 심볼로 들어 있어서 get() 메소드로 가져와야 한다.
과제가 mariaDB를 사용하라고 해서... mariaDB를 사용할 것이다.
mariaDB의 pool Connection을 받고, transaction을 시작한다.
그리고 정해진 이름에 따라 쿼리를 넣는다. 쿼리는 sql 폴더의 자바스크립트 파일에서 가져올 것인데, insert의 경우 배열로 해당하는 정보를 넣어야 한다.
return문의 경우 type을 success로 반환하는 것이 아니기 때문에 잘못 입력된 자료는 반드시 에러로 반환해줘야 한다.

---

# **관리자 권한 부여**

```javascript
// +page.svelte
const signIn = ({form}) => {
  return async ({ result, update }) => {
    if (result.data.role === 'owner') {
      const owner = result.data.code.toString()
      await goto(`/Home?${owner}`)
    } else if (result.data.type === 'success') {
      await goto('/Home', {replaceState:true});
    }
    else if (result.data.type === 'invalid') {
      message = '잘못된 정보입니다.'
    }
  };
}

// +page.server.js
// 중략
if (email==='test' && password==='test1234') {
  return {
    type: 'success',
    role: 'owner',
    code: 'test1234'
  }
} else if (result.length > 0) {
  return {
    type: 'success'
  }
} else {
  throw new Error('result error')
}
// 후략
```
관리자 계정에는 특별히 리턴값을 더해서 검사해주고, /Home 뒤에 / 를 붙이고 그 뒤에 관리자만의 코드를 넣어준다.

---

# **Layout**

레이아웃은 말 그대로 레이아웃이다. 해당하는 레이아웃을 고정시켜 둘 적에 편리하다. 파일명을 +layout.svelte로 짓고, 원래 페이지가 들어갈 공간은 slot 으로 지정해두면 된다.

![sveltekit-layout.png](/assets/postingImage/sveltekit-layout.png)

필자는 nav tag를 레이아웃으로 설정하여 고정했다. 사진에서 보다시피 a 태그도 넣을 수 있고, svelte 파일이기에 당연히 스크립트도 넣을 수 있다.

---

# **Dynamic Route**

우리는 로그인 시 우리만의 고유 아이디를 넣어 값을 식별해주기로 했다. 이렇게 하면 /Home 하위 디렉토리들의 명칭이 전부 다 개성적으로 작성되기에 우리는 감당할 수 없다. 또한, 생각해보면 포스팅이 늘어날 때마다 포스팅에 정수를 더해서 구분한다 해도 그 많은 포스팅마다 개별적인 라우팅을 부여해줄 수는 없는 일이다.

Dynamic Route는 이 문제점을 해결해준다. 사용법은 간단한데, 디렉토리 이름에 대괄호를 붙이는 것이다.

![sveltekit-dynamic-route.png](/assets/postingImage/sveltekit-dynamic-route.png)

사진에서 보다시피 [id] 값이 바로 dynamic route다. 이 값을 얻기 위해서는 다음과 같은 방법이 사용된다.

```javascript
import { page } from '$app/stores'

const id = $page.params.id
```
대략 이런 식이다. 

---