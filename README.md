# react 19

> [whats-new-in-react-19](https://vercel.com/blog/whats-new-in-react-19)

## react 19 컴파일 테스트

- 아직은 babel 플러그인만 있음.
- swc로는 불가능 -> swc로 누렸던 성능 이슈가 있을 수 있음

#### babel 컴파일 플러그인 없는 경우.

```js
export default defineConfig({
  plugins: [react({})],
})
```

![alt text](/images/1.png)

- 최적화 미 반영, (`memo`, `useCallback`, `useMemo`)

#### babel 컴파일 플러그인 없는 경우.

```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

const ReactCompilerConfig = { /* ... */ };

export default defineConfig({
  plugins: [react({
    babel: {
      plugins: [
        ["babel-plugin-react-compiler", ReactCompilerConfig],
      ],
    },
  })],
})
```

![alt text](/images/2.png)

- memo가 자동적으로 감싸져있음.
- 그리고 react devtool의 component에서 compiler가 자동으로 최적화했다는 문구를 확인할 수 있음.

![alt text](/images/3.png)

- dist의 build된 결과물을 보면 하단에 `react-compiler-runtime.production.js` 부분에 compiler를 통해서 최적화가 된 코드를 확인해볼 수 있다.

> react devtool performance는 아직 19버전 미지원

## Server Component (RSC)

> [react server component의 이해](https://vercel.com/blog/understanding-react-server-components-57brjqQf27QFQaFFm27gZ9)

#### 장점
- 초기 페이지 로드 시간: 서버에서 component를 렌더링하여 클라이언트로 전송되는 JavaScript 양을 줄여 초기 로드가 더 빨라진다. 또한 페이지가 클라이언트로 전송되기 전에 서버에서 데이터 쿼리가 시작되도록 한다.
- Code portability(코드 이식성): server component를 사용하면 개발자가 서버와 클라이언트에서 모두 실행될 수 있는 구성 요소를 작성할 수 있으므로 중복이 줄어들고 유지 관리가 개선되며 코드베이스 전체에서 논리를 더 쉽게 공유할 수 있다.
- SEO. 구성 요소의 서버 측 렌더링을 통해 검색 엔진과 LLM이 콘텐츠를 보다 효과적으로 크롤링하고 색인화할 수 있어 검색 엔진 최적화가 향상.
- 로컬 파일 시스템에 접근할 수 있으며 데이터 가져오기 및 네트워크 요청에 대한 짧은 지연 시간을 경험할 수 있음. (서버 리소스에 접근할 수 있음)
- 서버 컴포넌트에서 import 되는 모든 클라이언트 컴포넌트를 code splitting 포인트로 간주하기 때문에 더 이상 `React.lazy`로 메뉴얼 하게 명시하지 않아도 된다.
- 민감한 서버 자원의 데이터를 안전하게 사용할 수 있다.

#### 서버 컴포넌트와 서버 사이드 렌더링의 차이
- 서버 컴포넌트의 코드는 클라이언트로 전달되지 않음. 서버 사이드 렌더링의 모든 컴포넌트의 코드는 자바스크립트 번들에 포함되어 클라이언트로 전송된다.
- 서버 컴포넌트는 클라이언트 상태를 유지하며 refetch 될 수 있음. 서버 컴포넌트는 HTML이 아닌 특별한 형태로 컴포넌트를 전달하기 때문에 필요한 경우 포커스, 인풋 입력값 같은 클라이언트 상태를 유지하며 여러 번 데이터를 가져오고 리렌더링하여 전달할 수 있음. 하지만 SSR의 경우 HTML로 전달되기 때문에 새로운 refetch가 필요한 경우 HTML 전체를 리렌더링 해야 하며 이로 인해 클라이언트 상태를 유지할 수 없음.

#### 제한사항
- 서버 구성 요소는 이벤트 핸들러를 등록하고 클라이언트에서 트리거해야 하므로 상호 작용을 지원할 수 없음.
  - 예를 들어, 이벤트 핸들러는 `onClick` 클라이언트 구성 요소에서만 정의할 수 있음.
- 서버 구성요소는 대부분의 Hooks를 사용할 수 없음.
  - 서버 구성 요소는 렌더링 후 메모리에 유지되지 않으며 자체 상태를 가질 수 없음.
- 서버 구성 요소에서 클라이언트 구성 요소로 전달되는 Prop 값은 직렬화 가능해야 한다.
  - json형태로 직렬화 가능한 항목들
