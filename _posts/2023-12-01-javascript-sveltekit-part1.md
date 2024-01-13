---
title: SvelteKit - (1)
date: 2023-12-01 20:40:00 +/-09:00
category: [javascript/svelte]
tag: [javascript, svelte, sveltekit, tailwind]
---

[https://tailwindcss.com/docs/guides/sveltekit](https://tailwindcss.com/docs/guides/sveltekit)

- [**서론**](#서론)
- [**SvelteKit Install**](#sveltekit-install)
- [**What should I do next?**](#what-should-i-do-next)

# **서론**

서론에 약간 잡소리를 적어놓겠다.
아무리 신기술이라 해도 그렇지, 지들이 React를 해도 그렇지,
알아먹을 수 있게 적어놓은 블로그가 국내외를 통틀어 제대로 된 곳이 하나도 없었다.
절대다수가 React와 비슷하다고 퉁치고 넘어가고,
차라리 적지 말고 시간낭비나 막아주던가.
그래서 이번 포스팅은 정말로 내가 심혈을 기울여서 직접 프로젝트를 진행하면서 적는 것이다.
감사의 말은 필요없고, 대신 여러분이 포스팅할 적에 읽는 사람 배려 좀 했으면 좋겠다.
자기가 읽어도 모를 말들을 대체 왜 쓰는 건지 도통 이해가 가지 않는다.

# **SvelteKit Install**

일단 디렉토리를 만들자.

```terminal
npm init svelte@next
```
새로운 디렉토리에 만든다면 이 뒤에 디렉토리 이름을 붙여준다.
1. Where should we create your project? (leave blank to use current directory)
어디에 프로젝트를 생성할 것인가에 대해 묻는다. 그냥 진행할 시 현재 디렉토리에 생성한다.
2. Add type checking with TypeScript?
타입스크립트를 사용하냐는 질문이다.
  -> Yes, using JavaScript with JSDoc comments : 자바스크립트를 쓴다.
  -> Yes, using TypeScript syntax : 타입스크립트를 쓴다.
3. Add ESLint for code linting?
ESLint를 통한 코드 문법 맞춤을 사용하냐는 질문이다. ESLint는 써본 사람들은 알겠지만 엄청난 역겨움을 자랑한다. 그래도 대개의 프로젝트에선 쓰는 경향이 있다.
4. Add Prettier for code formatting?
프리티어를 통한 코드 가독성 통일을 사용하냐는 질문이다. 여기서도 Yes를 눌러준다. 개인 프로젝트라면 No를 해도 상관없다.
5. Add Playwright for browser testing?
크로뮴, 파이어폭스, 웹킷 등을 통합해서 테스팅을 제공하는 Playwright를 사용하겠냐는 질문이다.
6. Add Vitest for unit testing?
만든 애플리케이션을 번들링하겠냐는 질문이다. 그 역겨운 Babel을 써본 사람들이라면 이 또한 무슨 의미인지 알 것이다. 역시나 여기서도 Yes를 누를 수밖에 없다.

```terminal
yarn
yarn run dev
```
만악의 근원 npm의 시간이 끝났다. 이제 yarn의 시간이 왔다.

---

# **What should I do next?**

일단 서버를 구동시켜보고 나면 이 SvelteKit이라는 게 일반적인 세팅을 대부분 해놓았다는 사실을 알 수 있다. 그런데, 이제 뭘 하면 되지?
뭘 해야 할지 곰곰히 생각해보자. 일단 백엔드는 이 녀석이 자동으로 구성해주니 express.js는 필요가 없다. 대개는 다 준비되어 있는 상태다.
내 과제에서는 MariaDB를 쓰라는 명령이 떨어진 고로.. 일단 tailwind CSS와 MariaDB가 생각난다.
npm 사용을 권장하지만, 이하부터는 본인은 yarn을 쓸 것이다.

```terminal
yarn tailwindcss postcss autoprefixer mariadb
npx tailwindcss init -p
```

그다음 svelte.config.js에 들어가서 파일을 고쳐준다.

```javascript
import adapter from '@sveltejs/adapter-static';
import { vitePreprocess } from '@sveltejs/kit/vite';
/** @type {import('@sveltejs/kit').Config} */

export default {
  kit: {
    adapter: adapter()
  },
  preprocess: vitePreprocess()
};
```

tailwind.config.js도 고쳐준다.

```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: ['./src/**/*.{html,js,svelte,ts}'],
  theme: {
    extend: {}
  },
  plugins: []
};
```

components와 routes는 자주 쓰는 경로니까 달러 표시로 간략하게 연결할 수 있도록 해두었다.

vite.config.js
```javascript
import { sveltekit } from '@sveltejs/kit/vite'

/** @type {import('vite').UserConfig} */
const config = {
  plugins: [sveltekit()],
  resolve: {
    alias: {
			$routes: '/src/routes',
      $components: '/src/routes/components',
      $assets: '/src/assets',
			$resources: '/src/resources/css',
    },
  },
}

export default config
```


---