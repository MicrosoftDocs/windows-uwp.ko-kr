---
title: Windows에서 Node.js 웹 프레임워크 시작
description: Windows에서 Node.js 웹 프레임워크를 시작하는 데 유용한 가이드입니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, nodejs 학습, windows의 노드, wsl의 노드, windows 기반 linux의 노드, windows에 노드 설치, vs code를 사용하는 nodejs, windows에서 노드를 사용하여 개발, windows에서 nodejs를 사용하여 개발, WSL에 노드 설치, Linux용 Windows 하위 시스템의 NodeJS
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: a8ce1d08136a74504e1b3bad26feadd61b72068f
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "72517792"
---
# <a name="get-started-with-nodejs-web-frameworks-on-windows"></a>Windows에서 Node.js 웹 프레임워크 시작

Windows에서 Next.js, Nuxt.js 및 Gatsby를 비롯한 Node.js 웹 프레임워크를 사용하는 데 유용한 단계별 가이드입니다.

## <a name="prerequisites"></a>필수 구성 요소

이 가이드에서는 다음을 포함하여 [WSL 2를 사용하여 Node.js 개발 환경 설치](./setup-on-wsl2.md) 단계를 이미 완료했다고 가정합니다.

- Windows 10 Insider Preview 빌드 18932 이상 설치
- Windows에서 WSL 2 기능을 사용하도록 설정
- Linux 배포판(이 예제에서는 Ubuntu 18.04) 설치. `wsl lsb_release -a`를 사용하여 확인할 수 있습니다.
- Ubuntu 18.04 배포판이 WSL 2 모드로 실행되는지 확인. (WSL은 배포판을 v1 또는 v2 모드로 실행할 수 있습니다.) PowerShell을 열고 `wsl -l -v`를 입력하여 확인할 수 있습니다.
- PowerShell에서 `wsl -s ubuntu 18.04`를 사용하여 Ubuntu 18.04를 기본 배포로 설정

## <a name="get-started-with-nextjs"></a>Next.js 시작

Next.js는 React.js, Node.js, Webpack 및 Babel.js를 기준으로 서버 렌더링 JavaScript 앱을 만들기 위한 프레임워크입니다. 기본적으로는 React에 대해 반복적으로 사용되며 모범 사례를 고려해서 생성된 프로젝트로, 별다른 구성 없이 간단하고 일관된 방식으로 "유니버설" 웹앱을 만들 수 있도록 합니다. 이러한 "유니버설" 서버 렌더링 웹앱을 "동일 구조"라고도 합니다. 즉, 코드가 클라이언트와 서버 간에 공유됩니다.

next, react 및 react-dom 설치를 비롯하여 Next.js 프로젝트를 만들려면

1. WSL 터미널을 엽니다(예: Ubuntu 18.04).

2. 새 프로젝트 폴더를 만들고(`mkdir NextProjects`) 해당 디렉터리를 입력합니다(`cd NextProjects`).

3. Next.js를 설치하고 다음과 같이 프로젝트를 만듭니다('my-next-app'을 원하는 앱 이름으로 바꿈). `npm create next-app my-next-app`

4. 패키지가 설치되면 새 앱 폴더로 디렉터리를 변경한 후(`cd my-next-app`) `code .`를 사용하여 VS Code에서 Next.js 프로젝트를 엽니다. 그러면 앱에 대해 만들어진 Next.js 프레임워크를 살펴볼 수 있습니다. VS Code는 VS Code 창의 왼쪽 아래에 있는 녹색 탭에 표시된 대로 WSL-Remote 환경에서 앱을 열었습니다. 즉, Windows OS에서 편집하기 위해 VS Code를 사용하지만 앱은 Linux OS에서 계속 실행됩니다.

    ![WSL-Remote 확장](../images/wsl-remote-extension.png)

5. Next.js가 설치되면 다음 세 가지 명령을 알고 있어야 합니다.

    - `npm run dev`: 핫 다시 로드, 파일 감시 및 작업 재실행을 통해 개발 인스턴스 실행
    - `npm run build`: 프로젝트 컴파일
    - `npm start`: 프로덕션 모드에서 앱 시작

    VS Code에서 통합된 WSL 터미널을 엽니다(**보기 > 터미널**). 터미널 경로가 프로젝트 디렉터리(`~/NextProjects/my-next-app$`)를 가리키는지 확인합니다. 그런 후 `npm run dev`를 사용하여 새 Next.js 앱의 개발 인스턴스를 실행해 봅니다.

6. 로컬 개발 서버가 시작되며, 프로젝트 페이지의 빌드가 완료되면 터미널에 "컴파일 성공 - [http://localhost:3000](http://localhost:3000)에서 준비 완료"가 표시됩니다. 이 localhost 링크를 선택하여 웹 브라우저에서 새 Next.js 앱을 엽니다.

    ![localhost:3000에서 실행되는 Next.js 앱](../images/next-app.png)

7. VS Code 편집기에서 `pages/index.js` 파일을 엽니다. `<h1 className='title'>Welcome to Next.js!</h1>` 페이지 제목을 찾아 `<h1 className='title'>This is my new Next.js app!</h1>`으로 변경합니다. 웹 브라우저가 여전히 localhost: 3000에서 열려 있는 상태에서 변경 내용을 저장하고 핫 다시 로드 기능이 변경 내용을 자동으로 컴파일하고 브라우저에서 업데이트합니다.

8. Next.js에서 오류를 처리하는 방법을 살펴보겠습니다. 제목 코드가 `<h1 className='title'>This is my new Next.js app!`과 같이 표시되도록 `</h1>` 닫는 태그를 제거합니다. 이 변경 내용을 저장하면 브라우저에 "컴파일하지 못함" 오류가 표시되고, 터미널에서 `<h1>`에 대한 닫는 태그가 필요하다고 표시됩니다. `</h1>` 닫는 태그를 바꾸고 저장한 후 페이지를 다시 로드합니다.

F5 키를 선택하거나 메뉴 모음에서 **보기 > 디버그**(Ctrl+Shift+D) 및 **보기 > 디버그 콘솔**(Ctrl+Shift+Y)로 이동하여 Next.js 앱에서 VS Code의 디버거를 사용할 수 있습니다. 디버그 창에서 기어 아이콘을 선택하면 디버깅 설정 정보를 저장하기 위한 시작 구성(`launch.json`) 파일이 생성됩니다. 자세히 알아보려면 [VS Code 디버깅](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)을 참조하세요.

![VS Code 디버그 창 및 launch.json 구성 아이콘](../images/vscode-debug-launch-configuration.png)

Next.js에 대한 자세한 내용은 [Next.js 문서](https://nextjs.org/docs)를 참조하세요.

## <a name="get-started-with-nuxtjs"></a>Nuxt.js 시작

Nuxt.js는 Vue.js, Node.js, Webpack 및 Babel.js를 기준으로 서버 렌더링 JavaScript 앱을 만들기 위한 프레임워크입니다. 이 파일은 Next.js를 토대로 생성되었습니다. 기본적으로 Vue에 대해 반복적으로 사용되는 프로젝트입니다. Next.js와 마찬가지로 모범 사례를 고려해서 생성되었으며, 별다른 구성 없이 간단하고 일관된 방식으로 "유니버설" 웹앱을 만들 수 있습니다. 이러한 "유니버설" 서버 렌더링 웹앱을 "동일 구조"라고도 합니다. 즉, 코드가 클라이언트와 서버 간에 공유됩니다.

통합된 서버 쪽 프레임워크, UI 프레임워크, 테스트 프레임워크, 모드, 모듈 및 linter 종류에 대한 일련의 질문과 대답을 포함하는 Nuxt.js 프로젝트를 만들기 위해서는 다음을 설치할 수 있습니다.

1. WSL 터미널을 엽니다(예: Ubuntu 18.04).

2. 새 프로젝트 폴더를 만들고(`mkdir NuxtProjects`) 해당 디렉터리를 입력합니다(`cd NuxtProjects`).

3. Nuxt.js를 설치하고 다음과 같이 프로젝트를 만듭니다('my-nuxt-app'을 원하는 앱 이름으로 바꿈). `npm create nuxt-app my-nuxt-app`

4. 이제 Nuxt.js 설치 관리자가 다음 질문을 표시합니다.
    - 프로젝트 이름: my-nuxtjs-app
    - 프로젝트 설명: 내 Nuxt.js 앱에 대한 설명입니다.
    - 작성자 이름: 내 GitHub 별칭을 사용합니다.
    - 다음과 같이 패키지 관리자를 선택합니다. Yarn 또는 **Npm** - 예제를 위해 NPM을 사용합니다.
    - 다음 중에서 UI 프레임워크를 선택합니다. 없음, Ant Design Vue, Bootstrap Vue 등 이 예제에서는 **Vuetify**를 선택하지만, Vue Community는 [이러한 UI 프레임워크에 대한 요약 비교를 ](https://vue-community.org/guide/ecosystem/ui-libraries.html#summary-tldr) 만들어 프로젝트에 가장 적합한 기능을 선택하는 데 도움을 줍니다.
    - 사용자 지정 서버 프레임워크 None, AdonisJs, Express, Fastify 중에서 선택합니다. 이 예제에서는 **없음**을 선택하지만, Dev.to 사이트에서 [2019-2020 서버 프레임워크 비교](https://dev.to/santypk4/introducing-the-best-10-node-js-frameworks-for-2019-and-2020-mcm)를 찾을 수 있습니다.
    - Nuxt.js 모듈을 선택합니다(스페이스바를 사용하여 모듈을 선택하거나 원하지 않는 경우 Enter 키 선택). Axios(HTTP 요청을 간소화하는 데 필요) 또는 [PWA 지원](https://pwa.nuxtjs.org/)(service-worker, manifest.json 등 추가) 이 예제를 위한 모듈은 추가하지 않도록 하겠습니다.
    - linting 도구인 **ESLint**, Prettier, Lint 스테이징 파일을 선택합니다. **ESLint**(코드를 분석하고 잠재적 오류를 경고하는 도구)를 선택합니다.
    - 테스트 프레임워크를 **없음**, Jest, AVA 중에서 선택합니다. 이 빠른 시작에서는 테스트를 진행하지 않을 것이므로 **없음**을 선택합니다.
    - 렌더링 모드를 **유니버설(SSR)** 또는 SPA(단일 페이지 앱) 중에서 선택합니다. 이 예제에서는 **유니버설(SSR)** 을 선택하겠지만, [Nuxt.js 문서](https://nuxtjs.org/guide#server-rendered-universal-ssr-)에 몇 가지 차이점이 나와 있습니다. 즉, SSR의 경우 앱의 서버 렌더링을 위해 Node.js 서버를 실행해야 하며, SPA는 정적 호스팅에 사용됩니다.
    - 개발 도구 **jsconfig.json**을 선택합니다(Intellisense 코드 완성을 위해 VS Code에서 권장됨).

5. 프로젝트를 만든 후에는 `cd my-nuxtjs-app`을 입력하여 Nuxt.js 프로젝트 디렉터리로 이동하고 `code .`를 입력하여 VS Code WSL-Remote 환경에서 프로젝트를 엽니다.

    ![WSL-Remote 확장](../images/wsl-remote-extension.png)

6. Nuxt.js가 설치되면 다음 세 가지 명령을 알고 있어야 합니다.

    - `npm run dev`: 핫 다시 로드, 파일 감시 및 작업 재실행을 통해 개발 인스턴스 실행
    - `npm run build`: 프로젝트 컴파일
    - `npm start`: 프로덕션 모드에서 앱 시작

    VS Code에서 통합된 WSL 터미널을 엽니다(**보기 > 터미널**). 터미널 경로가 프로젝트 디렉터리(`~/NuxtProjects/my-nuxt-app$`)를 가리키는지 확인합니다. 그런 후 `npm run dev`를 사용하여 새 Nuxt.js 앱의 개발 인스턴스를 실행해 봅니다.

6. 로컬 개발 서버가 시작됩니다(클라이언트 및 서버 컴파일에 대한 몇 가지 유용한 진행률 표시줄 표시). 프로젝트가 성공적으로 작성되면 터미널에 컴파일 소요 시간과 함께 "컴파일 성공"이 표시됩니다. 웹 브라우저에서 [http://localhost:3000](http://localhost:3000)을 가리켜 새 Nuxt.js app 앱을 엽니다.

    ![localhost:3000에서 실행되는 Nuxt.js 앱](../images/nuxt-app.png)

7. VS Code 편집기에서 `pages/index.vue` 파일을 엽니다. `<v-card-title class="headline">Welcome to the Vuetify + Nuxt.js template</v-card-title>` 페이지 제목을 찾아 `<v-card-title class="headline">This is my new Nuxt.js app!</v-card-title>`으로 변경합니다. 웹 브라우저가 여전히 localhost: 3000에서 열려 있는 상태에서 변경 내용을 저장하고 핫 다시 로드 기능이 변경 내용을 자동으로 컴파일하고 브라우저에서 업데이트합니다.

8. Nuxt.js에서 오류를 처리하는 방법을 살펴보겠습니다. 제목 코드가 `<v-card-title class="headline">This is my new Nuxt.js app!`과 같이 표시되도록 `</v-card-title>` 닫는 태그를 제거합니다. 이 변경 내용을 저장하면 브라우저 및 터미널에 `<v-card-title>`에 대한 닫는 태그가 없다는 것을 알려주는 컴파일 오류와 코드의 오류 위치를 나타내는 줄 번호가 표시됩니다. `</v-card-title>` 닫는 태그를 바꾸고 저장한 후 페이지를 다시 로드합니다.

F5 키를 선택하거나 메뉴 모음에서 **보기 > 디버그**(Ctrl+Shift+D) 및 **보기 > 디버그 콘솔**(Ctrl+Shift+Y)로 이동하여 Nuxt.js 앱에서 VS Code의 디버거를 사용할 수 있습니다. 디버그 창에서 기어 아이콘을 선택하면 디버깅 설정 정보를 저장하기 위한 시작 구성(`launch.json`) 파일이 생성됩니다. 자세히 알아보려면 [VS Code 디버깅](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)을 참조하세요.

![VS Code 디버그 창 및 launch.json 구성 아이콘](../images/vscode-debug-launch-configuration.png)

Nuxt.js에 대한 자세한 내용은 [Nuxt.js 가이드](https://nuxtjs.org/guide)를 참조하세요.

## <a name="get-started-with-gatsbyjs"></a>Gatsby.js 시작

Gatsby.js는 Next.js 및 Nuxt.js와 같이 서버 렌더링 방식과는 반대로, React.js를 기준으로 하는 정적 사이트 생성기 프레임워크입니다. 정적 사이트 생성기는 빌드 타임에 정적 HTML을 생성합니다. 서버는 필요하지 않습니다. Next.js 및 Nuxt.js는 런타임에 HTML을 생성합니다(새 요청이 들어올 때마다). 서버를 실행할 필요가 없습니다. 또한 Gatsby는 앱에서 데이터를 처리하는 방법(GraphQL 사용)을 나타내지만, Next.js 및 Nuxt.js에서는 이러한 사항을 사용자가 결정합니다.

Gatsby.js 프로젝트를 만들려면 다음을 수행합니다.

1. WSL 터미널을 엽니다(예: Ubuntu 18.04).
2. 새 프로젝트 폴더를 만들고(`mkdir GatsbyProjects`) 해당 디렉터리를 입력합니다(`cd GatsbyProjects`).
3. npm을 사용하여 Gatsby CLI를 설치합니다(`npm install -g gatsby-cli`). 일단 설치되면 `gatsby --version`을 사용하여 버전을 확인합니다.
4. Gatsby.js 프로젝트를 만듭니다(`gatsby new my-gatsby-app`).
5. 패키지가 설치되면 새 앱 폴더로 디렉터리를 변경한 후(`cd my-gatsby-app`) `code .`를 사용하여 VS Code에서 Gatsby 프로젝트를 엽니다. 그러면 VS Code의 파일 탐색기를 사용하여 앱에 대해 만들어진 Gatsby.js 프레임워크를 살펴볼 수 있습니다. VS Code는 VS Code 창의 왼쪽 아래에 있는 녹색 탭에 표시된 대로 WSL-Remote 환경에서 앱을 열었습니다. 즉, Windows OS에서 편집하기 위해 VS Code를 사용하지만 앱은 Linux OS에서 계속 실행됩니다.

    ![WSL-Remote 확장](../images/wsl-remote-extension.png)

6. Gatsby가 설치되면 다음 세 가지 명령을 알고 있어야 합니다.

    - `gatsby develop`: 핫 다시 로드를 사용하여 개발 인스턴스를 실행합니다.
    - `gatsby build`: 프로덕션 빌드를 만듭니다.
    - `gatsby serve`: 프로덕션 모드에서 앱을 시작합니다.

    VS Code에서 통합된 WSL 터미널을 엽니다(**보기 > 터미널**). 터미널 경로가 프로젝트 디렉터리(`~/GatsbyProjects/my-gatsby-app$`)를 가리키는지 확인합니다. 그런 후 `gatsby develop`를 사용하여 새 앱의 개발 인스턴스를 실행해 봅니다.

7. 새 Gatsby 프로젝트의 컴파일이 완료되면 터미널에 "브라우저에서 gatsby-starter-default를 볼 수 있습니다. [http://localhost:8000/](http://localhost:8000/)"이 표시됩니다. 웹 브라우저에서 빌드된 새 프로젝트를 보려면 이 localhost 링크를 선택합니다.

> [!NOTE]
> 터미널 출력에서 "브라우저 내 IDE인 GraphiQL에서 사이트의 데이터 및 스키마 [http://localhost:8000/___graphql](http://localhost:8000/___graphql)를 확인할 수 있다는 사실"도 알 수 있습니다. GraphQL은 API를 Gatsby에 기본 제공되는 이해하기 쉬운 IDE(GraphiQL)에 통합합니다. 사이트의 데이터와 스키마를 살펴보는 것 외에도 쿼리, 변경, 구독 등의 GraphQL 작업을 수행할 수 있습니다. 자세한 내용은 [GraphiQL 소개](https://www.gatsbyjs.org/docs/running-queries-with-graphiql/)를 참조하세요.

8. VS Code 편집기에서 `src/pages/index.js` 파일을 엽니다. `<h1 >Hi people</h1>` 페이지 제목을 찾아 `<h1 >Hi (Your Name)!</h1>`으로 변경합니다. 웹 브라우저가 여전히 localhost:8000에서 열려 있는 상태에서 변경 내용을 저장하고 핫 다시 로드 기능이 변경 내용을 자동으로 컴파일하고 브라우저에서 업데이트합니다.

    ![localhost:3000에서 실행되는 Gatsby.js 앱](../images/gatsby-app.png)

9. Next.js에서 오류를 처리하는 방법을 살펴보겠습니다. 제목 코드가 `<h1>Hi (Your Name)!`과 같이 표시되도록 `</h1>` 닫는 태그를 제거합니다. 이 변경 내용을 저장하면 브라우저에 "컴파일하지 못함" 오류가 표시되고, 터미널에서 `<h1>`에 대한 닫는 태그가 필요하다고 표시됩니다. `</h1>` 닫는 태그를 바꾸고 저장한 후 페이지를 다시 로드합니다.

F5 키를 선택하거나 메뉴 모음에서 **보기 > 디버그**(Ctrl+Shift+D) 및 **보기 > 디버그 콘솔**(Ctrl+Shift+Y)로 이동하여 Next.js 앱에서 VS Code의 디버거를 사용할 수 있습니다. 디버그 창에서 기어 아이콘을 선택하면 디버깅 설정 정보를 저장하기 위한 시작 구성(`launch.json`) 파일이 생성됩니다. 자세히 알아보려면 [VS Code 디버깅](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)을 참조하세요.

![VS Code 디버그 창 및 launch.json 구성 아이콘](../images/vscode-debug-launch-configuration.png)

Gatsby에 대한 자세한 내용은 [Gatsby.js 문서](https://www.gatsbyjs.org/docs/)를 참조하세요.
