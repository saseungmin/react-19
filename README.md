# react 19

## react 19 컴파일 테스트

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

![alt text](image.png)

- build된 결과물을 보면 하단에 `react-compiler-runtime.production.js` 부분에 compiler를 통해서 최적화가 된 코드를 확인해볼 수 있다.

> react devtool performance는 아직 19버전 미지원
