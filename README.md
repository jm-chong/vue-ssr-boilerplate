# Vue SSR Boilerplate
## Overview
### Prologue
Vue나 React을 통해 웹사이트를 만들어 본 이후에 한번쯤 SSR (Server Side Rendering)을 구현해 보고 싶은 생각이 들게 된다. Client에서 HTML을 그리는 SPA방식은 첫 로딩시점에 화면이 깜박 거리는 현상과 함께 SEO(Search Engine Optimization)문제로, 검색엔진에 노출이 어려워 지게 된다. 

Nuxt등 SSR을 지원해 주는 많은 도구들이 있지만, 필요한 설정이나 Node 서버 쪽 상세구현과 함께 작업을 진행, 또는 Java, PHP등 JS가 아닌 Backend에 올려서 사용하는 등등..([Usage non-Node.js Environment](https://ssr.vuejs.org/guide/non-node.html)) 여러가지 이유로 SSR을 직접 구현해 보고 싶은 니즈가 있을거라 생각된다.

Vue SSR 구현을 위해 여러가지 Boilerplate를 Google링 해보았으나, 생각보다 딱 맞는(?) 소스가 잘 검색되지 않아서, 이번 기회에 직접 한번 만들어 보기로 하였다.

### Why not Nuxt?
Nuxt는 그 자체로 매우 훌륭한 Framework이다. Vue의 생태계(vue-router, vuex, ssr...)를 매우 쉽게 이용할 수 있고, 빠르게 비즈니스 로직을 개발하기에 적합하다.

하지만 세부적인 셋팅의 커스텀이 필요하거나, Node 서버사이드를 구성하기에는 다소 아쉬운 부분이 있었다.

Server Side Rendering 을 기술스택을 하나하나 쌓아 올라가면서 만들다 보면, Vue생태계에 대한 이해와 더불어 가능한 모든 곳에 Customize가 가능할 것으로 기대된다. 
### Why not vue-cli
Nuxt와 유사한 이유로 Vue-cli 또한 사용하지 않았다. 
All-in-one Package가 가져다 주는 잇점보다, 하나하나 쌓아 올라가면서 (From Scratch 라는 표현을 주로 사용한다..) Vue 생태계에 대한 이해도가 높아지기를 기대해 본다. 

### Framework
Using Koa : Express.js 와 유사한 비동기 middleware를 지원하는 Node.js Framework 이다. 

Express.js 
```js
app.get('/', function(req, res) {
	res.send("Hello Express")
}
```

Koa.js
```js
app.use(async ctx => {
  ctx.body = 'Hello Koa!';
});
```

가장 큰 장점은 'async/await'를 쉽게 지원한다는데 있다고 생각한다. DB에 접근하거나, 다른 API에 대한 요청등 비동기 처리가 많은 서버작업의 특성상 비동기를 쉽게 다룰수 있는것은 큰 장점으로 보인다.

### Bundle
Webpack4
Reference 가 풍부하고, 세부적인 환경 구성이 가능한, webpack을 사용하였다. Server Side Rendering 을 지원하면서 편리한 개발 환경 구성을 위한 `webpack-dev-middleware`, `webpack-hot-middleware` 미들웨어도 같이 사용하였다.

덕분에 서버 하나만 띄우고도 개발모드에서의 hmr등의 기능을 마음껏 누릴 수 있었다.

세부적으로는 Vue는 HMR로 소스코드 변경을 반영하였고,
Node.js Server-side는 nodemon을 통해 소스코드 변경이 바로 반영되도록 하였다. 

### Typescript
JS의 단점을 보완하기 위해 타입스크립트를 추가하였다. Vue 2.x 의 Typescript 지원이 아직 완벽하지는 않지만. Type Checking 이 가져다 주는 이점을 활용하고 싶었다.
Vue 3 에서는 Core를 Typescript 로 작성하고 있어, TS호환성이 크게 향상될것으로 기대된다.

Webpack config 파일또한 [Typescript](https://webpack.js.org/configuration/configuration-languages/#typescript) 로 작성하였다 (Intellij 에서의 alias 추적등의 일부 기능지원이 어려워져서 썩 권장하고 싶지는 않다.)

#### Vue Typescript 
Vue의 Typescript 는 크게 `Vue.extend({})` 방식과, `Vue class component` 의 두가지 방식이 존재한다. Boilerplate에서는 `extend`방식으로 구현하였다. `class`방식이 Typescript 의 Type Inference 가 더 잘되긴 하지만, Vue 3에서 [class API를 지원하지 않기로](https://github.com/vuejs/rfcs/pull/17#issuecomment-494242121) 방향이 정해지면서 이후 호환성을 고려해서 `class`방식은 사용하지 않았다. 물론 이후에도 'vue-class-component'에 대한 지속적인 지원은 이뤄진다고 하니, `class`방식으로 개발해도 이후 호환성에 큰 문제는 없을것으로 생각된다.

### Linting
Lint는 코드의 일관성을 유지시켜 주고, `syntax`수준의 오류를 사전에 미리 잡아주는 유용한 도구이다. `prettier`를 사용한다면, 문법적 오류가 아닌 `indent`나 `trailing Comma`등을 자동으로 수정해주어, 여러사람이 코드를 작성 하더라도 일관된 스타일을 유지시켜주어, 코드의 가독성을 크게 높여줄 수 있다.
Boilerplate 에서는 아래의 rule들을 적용하였다
  - `airbnb-base`
  - `plugin:@typescript/eslint/recommended`
  - `plugin:vue/recommended`
  - `prettier/@typescript/eslint`
  - `plugin:prettier/recommended`
  - `prettier/vue`
  - 
#### Why not use TSLint?
Typescript Linter로 TSLint를 많이 사용하지만, boilerplate에서는 ESLint 에 Typescript 룰을 적용하는것으로 대체하였다. TSLint가 ESLint 와 중복되는 부분들이 있어서 TSLint를 만든 palantir 가 [Medium Blog](https://medium.com/palantir/tslint-in-2019-1a144c2317a9)를 통해 ESLint + Typescript rule 의 사용을 권장하였다. 

#### ESLint : vue + TS + Prettier
사용하는 Framework 이 여러가지이다 보니, ESLint 룰을 정리하는것 또한 만만치 않은 작업이었다. [도움이된 글](https://www.robertcooper.me/using-eslint-and-prettier-in-a-typescript-project)을 참고해서 잘 동작하도록 만들 수 있었다. 

## Directory Structure

## How SSR (Server-Side-Rendering) Works?

## vue-renderer

## devMiddleware, hotMiddleware

## entry-client, entry-server, bundle-renderer

## 참고
- [Vue SSR Guide](https://ssr.vuejs.org/)
- [vue-hackernews](https://github.com/vuejs/vue-hackernews-2.0)
- [vue-enterprise-boilerplate](https://github.com/chrisvfritz/vue-enterprise-boilerplate)
