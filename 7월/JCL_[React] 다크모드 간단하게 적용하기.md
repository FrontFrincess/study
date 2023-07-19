# React 다크모드 간단하게 적용하기 



프로젝트들을 진행하면서 관통부터 지금까지 적용하겠다고 입벌구처럼 말만 하고 다닌 지 어언 n개월이 지났다. 별거 아니겠지? 하면서 한 번도 해보지 않아서 다크모드를 적용해보기로 했다!! 



다른 블로그 글들을 찾아보니 다크모드의 경우, 하기로 마음을 먹었다면 초기에 세팅을 해놓는 것이 가장 중요하다는 말들이 많았다. 또한, styled-component, scss, tailwindcss 등 여러 css 들로 다 가능하지만 가장 많이 사용하는 것은 styled component ! Styled Component 의 ThemeProvider 가 Global로 theme을 적용하기가 좋고 css 초기화를 위한 기능들이 잘 구현되어 있어 많이 쓴다고 한다. 

오늘은 styled component를 이용해서 다크모드를 적용해보겠다. 



시간이 없는 관계로 정말 정말 정말!! 간단하게 구현해보도록 하겠당 ..   

![스크린샷(5)](https://github.com/chaedev3/baekjoon/assets/109324466/5e4ced19-cf68-48f2-8e47-6552d9fe6847)

![스크린샷(4)](https://github.com/chaedev3/baekjoon/assets/109324466/79c8c0ee-ab7b-457d-8277-d5061a828939)

-> 구현하려고 하는 것은 버튼을 클릭하면 배경색, 글자색, 테두리 색을 포함하는 테마를 바꿔주는 것을 구현하는 것이다. 더 나아가 Recoil 을 이용해 새로고침을 해도 테마가 저장된 값으로 출력되도록 해보겠다. 



가장 먼저 React + Typescript + Styled Component 을 설치해준다.  

##### 0. 기본 설정 (React + Typescript + Styled Component 설치) 

```bash
# React + Typescript 
npx create-react-app dark_mode_example --template typescript 
# Styled Component
npm install --save styled-components
npm install @types/styled-components
```



##### 1. 가장 먼저, Typescript 를 사용했기 때문에 styled.d.ts 에 DefaultTheme에 대한 Type을 정의해주어야 한다.  

- 테마 변경에 배경색, 글자색, 테두리 색을 변경할 것이기 때문에 세 개를 넣어준다. 

```typescript
// styles/styled.d.ts 
import "styled-components"; 

declare module "styled-components" {
  export interface DefaultTheme {
    backgroundColor: string; 
    textColor: string;
    borderColor: string;
  }
}
```



##### 2. 라이트모드와 다크모드에 적용할 Theme 을 정의해준다.  

```typescript
// styles/theme.ts
// styled.d.ts 에서 정의한 styled-components 의 DefaultThme을 가져온다 
import { DefaultTheme } from "styled-components";

export const lightTheme: DefaultTheme = {
  backgroundColor: "#ffffff", 
  textColor: "#000000",
  borderColor: "#1e1e1e",
}

export const darkTheme: DefaultTheme = {
  backgroundColor: "#2F2F2F", 
  textColor: "#e5e5e5",
  borderColor: "#ffffff",
} 
```



##### 3. Styled-Component 의 ThemeProvider, createGlobalStyle 

Styled-Component 의 ThemeProvider 가 Global 하게 Theme 을 적용해준다. ThemeProvider 가 상단에 위치하면 그 자식들은 다 theme 속성을 가지게 된다.  



또한 createGlobalStyle을 이용해 전체적으로 적용할 수 있는 css 를 작성할 수 있습니다. 따로 파일을 만들어서 작성하고 최상단에서 import 해서 사용하는 것이 좋지만 이번에는 그냥 App.tsx 내에 작성하겠습니다. 

```typescript
//App.tsx
import { createGlobalStyle, ThemeProvider } from "styled-components";
import { darkTheme, lightTheme } from "./style/theme";
import { useState } from "react";

// createGlobalStyle 을 사용ㅇ하여 전체적으로 적용하고 싶은 css를 작성함
const GlobalStyle = createGlobalStyle`
body {
  color:${(props) => props.theme.textColor};
  background-color: ${(props) => props.theme.backgroundColor};
  border
}
p {
  text-align: center;
}
div {
  padding: 10px; 
  border: 2px solid ${(props) => props.theme.borderColor};
}
`;

function App() {
  // 버튼을 클릭하면 테마가 바뀌는 것을 구현 
  const [darkMode, setDarkMode] = useState(false);

  const darkModeHandler = () => {
    setDarkMode(!darkMode); 
  }; 

  return (
    <ThemeProvider theme={darkMode ? darkTheme : lightTheme}>
      <GlobalStyle />
      <button onClick={darkModeHandler}>
        클릭
      </button>
      <div>
        <p>다크모드 테스트</p>
      </div>
    </ThemeProvider>
  );
}

export default App;

```

- 하지만 이렇게 하게 되면 새로고침을 하면 darkMode 의 값이 false 라서 처음 모드는 밝은 모드로 뜨게 된다. 그렇기 때문에 전역 상태 관리가 필요하다. 





##### 4. Recoil 설치 

- 물론, localstoarge 에 저장하는 방법도 있지만 프로젝트를 진행하게 되면 recoil, redux 같은 전역 상태 관리 도구를 이용해 이 값을 저장할 것이기 때문에 recoil, recoil-persist을 이용해 진행해보겠다. 

```
npm install recoil 
npm install recoil-persist 
```



Recoil 을 깔았으니 index.tsx에 RecoilRoot를 삽입해준다 

```typescript
mport { RecoilRoot } from 'recoil';

const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLElement
);
root.render(
  <React.StrictMode>
    <RecoilRoot>
      <App />
    </RecoilRoot>
  </React.StrictMode>
);
```



atom 을 사용해 상태를 저장할 공간 만들어줌  

나는 새로고침해도 저장되게 하려고 recoil-persist를 사용했지만, 아직 recoil이 나온지 얼마 되지 않아 불안정한 부분이 있을 수 있어 effects_unstable을 추가해주었다. 

```typescript
// /recoil/atoms.ts 
import { atom } from "recoil";
import { recoilPersist } from "recoil-persist";

const { persistAtom } = recoilPersist(); 

export const isDarkAtom = atom({
    //state 이름 
    key:"isDark",
    // 기본값(기본값은 라이트모드)
    default: false,
    effects_UNSTABLE: [persistAtom], 
})
```



그 다음 원래 useState 를 이용해 기본값을 설정해주었던 것을 useRecoilValue 를 이용해 원래 있던 값을 가져오고 useState 을 이용해 바꾸었던 걸 useSetRecoilState 을 이용해 바꾸어준다.  

```typescript
// App.tsx
 const darkMode = useRecoilValue(isDarkAtom); 
 const setDarkMode = useSetRecoilState(isDarkAtom); 

 const darkModeHandler = () => {
    setDarkMode((prev) => !prev); 
 }; 
```



이렇게 하게 되면 새로고침을 해도 변하지 않게 된다 !! 
