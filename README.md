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

![alt text](/images/2.png)

- memo가 자동적으로 감싸져있음.
- 그리고 react devtool의 component에서 compiler가 자동으로 최적화했다는 문구를 확인할 수 있음.

> react devtool performance는 아직 19버전 미지원
