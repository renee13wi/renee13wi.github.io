---
title: gatsby
date: "2020-12-22T22:12:03.284Z"
description: gatsby로 기술 블로그 시작하기
---

GatsbyJS는 React 기반의 정적 페이지 생성기로
가공할 정보를 GraphQL에서 가져와서 빌드 시점에 정적 페이지를 만들어내는 방식이며,
각 페이지 정보들이 모두 배포 시점에 생성되어, 따로 서버가 필요하지 않다는 장점이 있습니다.

##### gatsby-cli 전역 설치하기
```bash
npm install —global gatsby-cli
```

##### 사이트 생성하기 : gatsby new [프로젝트 폴더명]
```bash
gatsby new my-gatsby-blog https://github.com/gatsbyjs/gatsby-starter-blog
```

gatsby theme는 기술 블로그형이 목적이고 시작 하기 수월한 gatsby-starter-blog theme로 정했습니다.
더 많은 Theme는 [gatsby theme](https://www.gatsbyjs.com/starters/?v=2) 참고하면 됩니다.

다음 명령어를 통해 localhost:8000로 접속 가능합니다.
```bash
npm run develop
```

##### 프로젝트 구조
```bash
├── .cache // Gatsby의 내부 캐시
├── content
│   ├── assets
│   └── blog
│       ├── 1 unit folder
│
├── node_modules
├── public // gatsby build Output
├── src
│   ├── components
│   ├── pages // 파일 이름과 폴더 이름을 경로로 따르는 페이지
│   ├── templates
│   ├── normalize.css
│   ├── style.css
│
├── static
│   ├── favicon.ico
│   ├── robots.txt
│
├── .gitignore
├── .prettierignore
├── .prettierrc
├── gatsby-browser.js
├── gatsby-config.js
├── gatsby-node.js
├── LICENSE
├── package-lock.json
├── package.json
└── README.md
```
- `/node_modules` : 이 디렉토리에는 프로젝트가 의존하는 모든 코드 모듈 (npm 패키지)이 자동으로 설치됩니다.
- `gatsby-config.js`
    Gatsby 사이트의 기본 구성 파일입니다.
    여기서 사이트 제목 및 설명, 포함 할 Gatsby 플러그인 등 사이트 (메타 데이터)에 대한 정보를 지정할 수 있습니다.
- `gatsby-browser.js`
    Gatsby 브라우저 API의 사용이 있는 경우 사용합니다.
- `gatsby-node.js`
    Gatsby 노드 API 사용이 있을 경우의 사용법을 정의합니다.
- `gatsby-ssr.js`
    Gatsby 서버 사이드 렌더링 API 사용이 있을 경우의 사용법 정의합니다.
- `package.json`
    메타 데이터 (프로젝트 이름, 작성자 등)와 같은 항목이 포함 된 Node.js 프로젝트용 파일입니다.
    npm이 프로젝트에 설치할 패키지를 아는 방법입니다.

[참고 사이트 : gatsby-starter-blog](https://github.com/gatsbyjs/gatsby-starter-blog)

##### GitHub Pages 배포 방법 : github.io 주소 사용하기
GitHub Pages에 배포하기 위해서는 gh-pages라는 패키지를 사용해야 합니다.
```bash
npm install gh-pages --save-dev
```

**GitHub Pages를 만드는 방법은 2가지입니다.**
1. github.io 메인을 사용 할 경우 - `https://[사용자명].github.io`
2. repository를 사용 할 경우 - `https://[사용자명].github.io/[Repository이름]`

**1. github.io 메인을 사용 할 경우**

Git Repository 이름을 [사용자명].github.io로 생성합니다.  
그리고 패키지 내의 package.json 파일에 아래 부분을 추가합니다.
```bash
{
  "scripts": {
    "deploy": "gatsby build && gh-pages -d public -b master"
  }
}
```

**2. repository를 사용 할 경우**

[package.json]
```bash
{
  "scripts": {
    "deploy": "gatsby build --prefix-paths && gh-pages -d public"
  }
}
```
[gatsby-config.js]
```bash
module.exports = {
  pathPrefix: "/[Repository 이름을 넣으시면 됩니다.]"
}
```

**GitHub Pages 배포하기**
```bash
npm run deploy
```

---

##### gatsby 새 페이지 생성

`src/page/*.js`는 자동으로 새페이지가 생성됩니다.  
`src/page/about.js` 에서 새 파일을 만들고 다음 코드를 새 파일에 복사 한 후 저장합니다.
```js
import React from "react"

export default function About() {
  return(
    <div>
	    <h1 style={{ color: `red`}}>About Gatsby</h1>
    </div>
  )
}
```

`http://localhost:8000/about/` 이동


##### 하위 구성 요소 사용

홈페이지의 정보가 커져서 다시 작성했다고 가정해보겠습니다.  
하위 구성 요소를 사용하여 UI를 재사용 가능한 조각으로 나눌 수 있습니다. 
1. src/components에 header.js 새 파일을 작성합니다.
2. header.js에 다음 코드를 추가합니다.

```javascript
import React from "react"

export default function Header() {
  return <h1>This is a header.</h1>
}
```

3. header.js를 about.js에 가져오기 위해 about.js 파일을 수정합니다.

```javascript
import React from "react"
import Header from "../components/header"

export default function About() {
  return(
    <div>
      <Header />
      <h1 style={{ color: `red`}}>About Gatsby</h1>
    </div>
  )
}
```
불러온 header에 `This is a header.`라고 읽히고 싶지 않다면

4. src/components/header.js 에서 다음과 같이 변경합니다.

```javascript
import React from "react"

export default function Header(props) {
  return <h1>{props.headerText}</h1>
}
```

5. src/pages/about.js 에서 다음과 같이 변경합니다.

```javascript
import React from "react"
import Header from "../components/header"

export default function About() {
  return (
    <div style={{ color: `teal` }}>
      <Header headerText="About Gatsby" />
      <p>Such wow. Very React.</p>
    </div>
  )
}
```

**props란?**

react컴포넌트를 UI 재사용 가능한 코드 조각으로 정의했습니다.  
이러한 재사용 가능한 조각을 동적으로 만들려면 다른 데이터를 제공 할 수 있어야합니다.  
"props"라는 입력으로 이를 수행합니다.  
Props은 React 컴포넌트에 제공되는 속성입니다.

---

##### 참고사이트
[gatsby blog tutorial](https://www.gatsbyjs.com/docs/tutorial/part-one/)  
[gatsby theme](https://www.gatsbyjs.com/starters/?v=2)  
[gatsby-starter-blog](https://github.com/gatsbyjs/gatsby-starter-blog)  
[React tutorial](https://ko.reactjs.org/docs/getting-started.html)  
[Medium : Gatsby 로 Blog 만들기](https://medium.com/@pks2974/gatsby-%EB%A1%9C-blog-%EB%A7%8C%EB%93%A4%EA%B8%B0-ac3eed48e068)