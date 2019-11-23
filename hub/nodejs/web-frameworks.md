---
title: Windows에서 node.js 웹 프레임 워크 시작
description: Windows에서 node.js 웹 프레임 워크를 시작 하는 데 도움이 되는 가이드입니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, node.js, windows 10, microsoft, learning NodeJS, windows의 노드, windows의 노드, windows에서 노드 설치, windows에서 노드 설치, windows에서 노드를 사용 하 여 개발, windows에서 NodeJS를 사용 하 여 개발, windows에서 노드 설치, windows의 NodeJS Linux 용 하위 시스템
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: a8ce1d08136a74504e1b3bad26feadd61b72068f
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517792"
---
# <a name="get-started-with-nodejs-web-frameworks-on-windows"></a>Windows에서 node.js 웹 프레임 워크 시작

Windows에서 node.js, Nuxt 및 Gatsby를 비롯 한 node.js 웹 프레임 워크를 사용 하 여 시작 하는 데 도움이 되는 단계별 가이드입니다.

## <a name="prerequisites"></a>필수 구성 요소

이 가이드에서는 다음을 포함 하 여 [WSL 2를 사용 하 여 node.js 개발 환경을 설정](./setup-on-wsl2.md)하는 단계를 이미 완료 했다고 가정 합니다.

- Windows 10 Insider Preview 빌드 18932 이상 버전을 설치 합니다.
- Windows에서 WSL 2 기능을 사용 하도록 설정 합니다.
- Linux 배포 (Ubuntu 18.04)를 설치 합니다. 다음을 사용 하 여이를 확인할 수 있습니다 `wsl lsb_release -a`
- Ubuntu 18.04 배포가 WSL 2 모드로 실행 되 고 있는지 확인 합니다. WSL은 v1 또는 v2 모드에서 배포를 실행할 수 있습니다. PowerShell을 열고 다음을 입력 하 여이를 확인할 수 있습니다 `wsl -l -v`
- PowerShell을 사용 하 여 기본 배포로 Ubuntu 18.04을 다음으로 설정 합니다. `wsl -s ubuntu 18.04`

## <a name="get-started-with-nextjs"></a>Next.js 시작

다음 js는 응답 .js, node.js, Webpack 및 Babel를 기반으로 서버에서 렌더링 된 JavaScript 앱을 만들기 위한 프레임 워크입니다. 이는 기본적으로 가장 적합 한 방식으로 "범용" 웹 앱을 만들 수 있으며,이는 거의 모든 구성을 사용 하 여 간단 하 고 일관 된 방식으로 "범용" 웹 앱을 만들 수 있도록 하는 것입니다. 이러한 "범용" 서버 렌더링 웹 앱을 "isomorphic" 라고도 합니다. 즉, 코드가 클라이언트와 서버 간에 공유 됩니다.

다음 .js 프로젝트를 만듭니다. 여기에는 다음 설치, 반응 및 반응-dom이 포함 됩니다.

1. WSL 터미널을 엽니다 (ie. Ubuntu 18.04).

2. 새 프로젝트 폴더 만들기: `mkdir NextProjects` 하 고 해당 디렉터리를 입력 합니다. `cd NextProjects`.

3. 다음 js를 설치 하 고 프로젝트를 만듭니다 (' 내-다음-앱 '을 앱 호출을 원하는 것으로 대체). `npm create next-app my-next-app`.

4. 패키지가 설치 되 면 새 앱 폴더 `cd my-next-app`디렉터리를 변경 하 고 `code .`를 사용 하 여 VS Code에서 다음 .js 프로젝트를 엽니다. 그러면 앱에 대해 만들어진 다음 .js 프레임 워크를 살펴볼 수 있습니다. VS Code은 VS Code 창의 왼쪽 아래에 있는 녹색 탭에 표시 된 대로 WSL-원격 환경에서 앱을 열었습니다. 즉, Windows OS에서 편집을 위해 VS Code를 사용 하는 동안에도 Linux OS에서 응용 프로그램을 실행 하 고 있습니다.

    ![WSL-원격 확장](../images/wsl-remote-extension.png)

5. 다음에는 다음 .js를 설치할 때 알아야 할 세 가지 명령이 있습니다.

    - 핫 다시 로드, 파일 감시 및 작업이 다시 실행 되는 개발 인스턴스를 실행 하는 `npm run dev`.
    - 프로젝트 컴파일에 대 한 `npm run build`입니다.
    - 프로덕션 모드에서 앱을 시작 하는 `npm start`.

    VS Code ( **> 터미널 보기**)에서 통합 된 wsl 터미널을 엽니다. 터미널 경로가 프로젝트 디렉터리 (`~/NextProjects/my-next-app$`)를 가리키는지 확인 합니다. 그런 다음을 사용 하 여 새 다음 .js 앱의 개발 인스턴스를 실행 해 보세요. `npm run dev`

6. 로컬 개발 서버가 시작 되 고 프로젝트 페이지의 빌드가 완료 되 면 터미널에 " [http://localhost:3000](http://localhost:3000)에서 성공적으로 컴파일 되었습니다."가 표시 됩니다. 이 localhost 링크를 선택 하 여 웹 브라우저에서 새로운 다음 .js 앱을 엽니다.

    ![Localhost에서 실행 되는 다음 .js 앱: 3000](../images/next-app.png)

7. VS Code 편집기에서 `pages/index.js` 파일을 엽니다. `<h1 className='title'>Welcome to Next.js!</h1>` 페이지 제목을 찾아 `<h1 className='title'>This is my new Next.js app!</h1>`변경 합니다. 웹 브라우저가 여전히 localhost: 3000에 열려 있는 상태에서 변경 내용을 저장 하 고 핫 다시 로드 기능이 브라우저에서 자동으로 컴파일되고 변경 내용을 업데이트 하는지 확인 합니다.

8. 다음 js에서 오류를 처리 하는 방법을 살펴보겠습니다. 제목 코드가 다음과 같이 표시 되도록 `</h1>` 닫는 태그를 제거 합니다. `<h1 className='title'>This is my new Next.js app!`. 이 변경 내용을 저장 하면 "컴파일 실패" 오류가 브라우저에 표시 되 고 터미널에서 `<h1>`에 대 한 닫는 태그가 필요 함을 알 수 있습니다. `</h1>` 닫는 태그를 바꾸고 저장 하 고 페이지를 다시 로드 합니다.

F5 키를 선택 하거나 메뉴 모음에서 **> 디버그** (Ctrl + Shift + D) 및 **보기 > 디버그 콘솔** (Ctrl + shift + Y)로 이동 하 여 다음 .js 앱에서 VS Code의 디버거를 사용할 수 있습니다. 디버그 창에서 기어 아이콘을 선택 하면 디버깅 설정 정보를 저장 하기 위한 시작 구성 (`launch.json`) 파일이 생성 됩니다. 자세히 알아보려면 [VS Code 디버깅](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)을 참조 하세요.

![VS Code 디버그 창 및 시작 json 구성 아이콘](../images/vscode-debug-launch-configuration.png)

다음 .js에 대해 자세히 알아보려면 [다음 .js 문서](https://nextjs.org/docs)를 참조 하세요.

## <a name="get-started-with-nuxtjs"></a>Nuxt.js 시작

Nuxt은 Vue, node.js, Webpack 및 Babel를 기반으로 서버 렌더링 JavaScript 앱을 만들기 위한 프레임 워크입니다. 다음 .js를 통해 아이디어를 받았습니다. 기본적으로 Vue에 대 한 프로젝트 상용구입니다. 다음 .js와 마찬가지로 모범 사례를 고려 하 여이 기능을 사용할 수 있으며, 모든 구성으로 간단 하 고 일관 된 방법으로 "범용" 웹 앱을 만들 수 있습니다. 이러한 "범용" 서버 렌더링 웹 앱을 "isomorphic" 라고도 합니다. 즉, 코드가 클라이언트와 서버 간에 공유 됩니다.

Nuxt 프로젝트를 만들기 위해 설치 하려는 통합 서버 쪽 프레임 워크, UI 프레임 워크, 테스트 프레임 워크, 모드, 모듈 및 linter의 종류에 대 한 일련의 질문에 대 한 대답을 포함 합니다.

1. WSL 터미널을 엽니다 (ie. Ubuntu 18.04).

2. 새 프로젝트 폴더 만들기: `mkdir NuxtProjects` 하 고 해당 디렉터리를 입력 합니다. `cd NuxtProjects`.

3. Nuxt를 설치 하 고 프로젝트를 만듭니다 (' Nuxt-앱 '을 앱 호출을 원하는 것으로 대체). `npm create nuxt-app my-nuxt-app`

4. 이제 Nuxt 설치 관리자가 다음 질문을 표시 합니다.
    - 프로젝트 이름: 내 nuxtjs-앱
    - 프로젝트 설명: 내 Nuxt 앱에 대 한 설명입니다.
    - 작성자 이름: 내 GitHub 별칭을 사용 합니다.
    - Yarn 또는 **Npm** 패키지 관리자를 선택 합니다. 예를 들어 Npm를 사용 합니다.
    - UI 프레임 워크: 없음, Ant Design Vue, 부트스트랩 Vue 등을 선택 합니다. 이 예에서는 **Vuetify** 를 선택 합니다. 하지만 Vue 커뮤니티는 [이러한 UI 프레임 워크를 비교](https://vue-community.org/guide/ecosystem/ui-libraries.html#summary-tldr) 하 여 프로젝트에 가장 적합 한 방법을 선택 하는 데 도움이 되는 훌륭한 요약을 만들었습니다.
    - 사용자 지정 서버 프레임 워크 (없음, AdonisJs, Express, Fastify 등)를 선택 합니다. 이 예에서는 **없음** 을 선택 하겠습니다. 하지만 Dev.to 사이트에서 [2019-2020 서버 프레임 워크 비교](https://dev.to/santypk4/introducing-the-best-10-node-js-frameworks-for-2019-and-2020-mcm) 를 찾을 수 있습니다.
    - Nuxt 모듈 (스페이스바를 사용 하 여 모듈을 선택 하거나 원하지 않는 경우 입력): Axios (HTTP 요청 간소화) 또는 [PWA 지원](https://pwa.nuxtjs.org/) (서비스 작업자, 매니페스트. json 파일 등)을 선택 합니다. 이 예제에 대 한 모듈을 추가 하지 않도록 하겠습니다.
    - Lint tools: **Eslint**, Prettier, 보풀이 없는 파일을 선택 합니다. **Eslint** (코드를 분석 하 고 잠재적 오류를 경고 하는 도구)를 선택 하겠습니다.
    - 테스트 프레임 워크를 선택 합니다. **없음**, JEST, ava. 이 빠른 시작에서 테스트에 대해 다루지 않으므로 **없음** 을 선택 합니다.
    - 렌더링 모드 **(유니버설 (SSR)** 또는 단일 페이지 앱 (SPA))를 선택 합니다. 예제에서는 **유니버설 (SSR)** 를 선택 합니다. 그러나 [Nuxt 문서](https://nuxtjs.org/guide#server-rendered-universal-ssr-) 는 정적 호스팅을 위해 앱과 SPA를 실행 하는 데 사용 되는 node.js 서버를 필요로 하는 SSR의 차이점을 가리킵니다.
    - 개발 도구 선택: **jsconfig.json** (Intellisense 코드 완성이 작동 하도록 VS Code에 권장)

5. 프로젝트를 만든 후에는 Nuxt 프로젝트 디렉터리를 입력 `cd my-nuxtjs-app` `code .`를 입력 하 여 VS Code WSL-원격 환경에서 프로젝트를 엽니다.

    ![WSL-원격 확장](../images/wsl-remote-extension.png)

6. Nuxt가 설치 되 면 다음 세 가지 명령을 알고 있어야 합니다.

    - 핫 다시 로드, 파일 감시 및 작업이 다시 실행 되는 개발 인스턴스를 실행 하는 `npm run dev`.
    - 프로젝트 컴파일에 대 한 `npm run build`입니다.
    - 프로덕션 모드에서 앱을 시작 하는 `npm start`.

    VS Code ( **> 터미널 보기**)에서 통합 된 wsl 터미널을 엽니다. 터미널 경로가 프로젝트 디렉터리 (`~/NuxtProjects/my-nuxt-app$`)를 가리키는지 확인 합니다. 그런 다음를 사용 하 여 새 Nuxt 앱의 개발 인스턴스를 실행 해 봅니다. `npm run dev`

6. 로컬 개발 서버가 시작 됩니다 (클라이언트 및 서버 컴파일에 대 한 몇 가지 쿨 진행률 표시줄 표시). 프로젝트가 성공적으로 작성 되 면 터미널에 컴파일 소요 시간과 함께 "컴파일 성공"이 표시 됩니다. 웹 브라우저가 [http://localhost:3000](http://localhost:3000) 새 Nuxt 앱을 열도록 지정 합니다.

    ![Localhost: 3000에서 실행 중인 Nuxt 앱](../images/nuxt-app.png)

7. VS Code 편집기에서 `pages/index.vue` 파일을 엽니다. `<v-card-title class="headline">Welcome to the Vuetify + Nuxt.js template</v-card-title>` 페이지 제목을 찾아 `<v-card-title class="headline">This is my new Nuxt.js app!</v-card-title>`변경 합니다. 웹 브라우저가 여전히 localhost: 3000에 열려 있는 상태에서 변경 내용을 저장 하 고 핫 다시 로드 기능이 브라우저에서 자동으로 컴파일되고 변경 내용을 업데이트 하는지 확인 합니다.

8. Nuxt에서 오류를 처리 하는 방법을 살펴보겠습니다. 제목 코드가 다음과 같이 표시 되도록 `</v-card-title>` 닫는 태그를 제거 합니다. `<v-card-title class="headline">This is my new Nuxt.js app!`. 이 변경 내용을 저장 하면 코드에서 오류를 찾을 수 있는 줄 번호를 사용 하 여 `<v-card-title>`에 대 한 닫는 태그가 없다는 것을 알 수 있도록 브라우저와 터미널에 컴파일 오류가 표시 됩니다. `</v-card-title>` 닫는 태그를 바꾸고 저장 하 고 페이지를 다시 로드 합니다.

F5 키를 선택 하거나 메뉴 모음에서 **> 디버그** (Ctrl + Shift + D) 및 **보기 > 디버그 콘솔** (Ctrl + shift + Y)로 이동 하 여 Nuxt 앱에서 VS Code의 디버거를 사용할 수 있습니다. 디버그 창에서 기어 아이콘을 선택 하면 디버깅 설정 정보를 저장 하기 위한 시작 구성 (`launch.json`) 파일이 생성 됩니다. 자세히 알아보려면 [VS Code 디버깅](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)을 참조 하세요.

![VS Code 디버그 창 및 시작 json 구성 아이콘](../images/vscode-debug-launch-configuration.png)

Nuxt에 대해 자세히 알아보려면 [Nuxt 가이드](https://nuxtjs.org/guide)를 참조 하세요.

## <a name="get-started-with-gatsbyjs"></a>Gatsby.js 시작

Gatsby는 다음 .js 및 Nuxt와 같이 서버에 렌더링 되는 것과는 반대로, 응답 .js를 기반으로 하는 정적 사이트 생성기 프레임 워크입니다. 정적 사이트 생성기는 빌드 시 정적 HTML을 생성 합니다. 서버가 필요 하지 않습니다. 다음 .js 및 Nuxt는 런타임에 HTML을 생성 합니다 (새 요청이 들어올 때마다). 서버를 실행 하려면 서버를 실행 해야 합니다. 또한 Gatsby는 앱에서 데이터를 처리 하는 방법 (GraphQL 사용)을 지정 하는 반면, 다음 js 및 Nuxt는 해당 결정을 그대로 둡니다.

Gatsby 프로젝트를 만들려면 다음을 수행 합니다.

1. WSL 터미널을 엽니다 (ie. Ubuntu 18.04).
2. 새 프로젝트 폴더 만들기: `mkdir GatsbyProjects` 하 고 해당 디렉터리를 입력 합니다. `cd GatsbyProjects`
3. Npm를 사용 하 여 Gatsby CLI: `npm install -g gatsby-cli`를 설치 합니다. 설치 되 면 `gatsby --version`를 사용 하 여 버전을 확인 합니다.
4. Gatsby 프로젝트를 만듭니다. `gatsby new my-gatsby-app`
5. 패키지가 설치 되 면 새 앱 폴더 `cd my-gatsby-app`디렉터리를 변경 하 고 `code .`를 사용 하 여 VS Code에서 Gatsby 프로젝트를 엽니다. 이렇게 하면 VS Code의 파일 탐색기를 사용 하 여 앱에 대해 생성 된 Gatsby 프레임 워크를 살펴볼 수 있습니다. VS Code은 VS Code 창의 왼쪽 아래에 있는 녹색 탭에 표시 된 대로 WSL-원격 환경에서 앱을 열었습니다. 즉, Windows OS에서 편집을 위해 VS Code를 사용 하는 동안에도 Linux OS에서 응용 프로그램을 실행 하 고 있습니다.

    ![WSL-원격 확장](../images/wsl-remote-extension.png)

6. Gatsby가 설치 되 면 다음 세 가지 명령을 알고 있어야 합니다.

    - 핫 다시 로드를 사용 하 여 개발 인스턴스를 실행 하는 `gatsby develop`.
    - 프로덕션 빌드를 만드는 `gatsby build`.
    - 프로덕션 모드에서 앱을 시작 하는 `gatsby serve`.

    VS Code ( **> 터미널 보기**)에서 통합 된 wsl 터미널을 엽니다. 터미널 경로가 프로젝트 디렉터리 (`~/GatsbyProjects/my-gatsby-app$`)를 가리키는지 확인 합니다. 그런 다음를 사용 하 여 새 앱의 개발 인스턴스를 실행 해 봅니다 `gatsby develop`

7. 새 Gatsby 프로젝트의 컴파일이 완료 되 면 터미널이 "브라우저에서 Gatsby를 볼 수 있습니다. 기본값을 볼 수 있습니다. [http://localhost:8000/](http://localhost:8000/). " 웹 브라우저에서 빌드된 새 프로젝트를 보려면이 localhost 링크를 선택 합니다.

> [!NOTE]
> 또한 터미널 출력을 통해 "브라우저 내 IDE"를 GraphiQL 사이트의 데이터와 스키마를 탐색할 수 있습니다 [http://localhost:8000/___graphql](http://localhost:8000/___graphql). "라는 사실을 알 수 있습니다. GraphQL는 Api를 Gatsby에 기본 제공 되는 GraphiQL (자체 문서화 IDE)로 통합 합니다. 사이트의 데이터와 스키마를 탐색 하는 것 외에도 쿼리, 변경이, 구독 등의 GraphQL 작업을 수행할 수 있습니다. 자세한 내용은 [GraphiQL 소개](https://www.gatsbyjs.org/docs/running-queries-with-graphiql/)를 참조 하세요.

8. VS Code 편집기에서 `src/pages/index.js` 파일을 엽니다. `<h1 >Hi people</h1>` 페이지 제목을 찾아 `<h1 >Hi (Your Name)!</h1>`변경 합니다. 웹 브라우저가 여전히 localhost: 8000로 열려 있는 상태에서 변경 내용을 저장 하 고 핫 다시 로드 기능이 브라우저에서 자동으로 컴파일되고 변경 내용을 업데이트 하는지 확인 합니다.

    ![Localhost: 3000에서 실행 중인 Gatsby 앱](../images/gatsby-app.png)

9. 다음 js에서 오류를 처리 하는 방법을 살펴보겠습니다. 제목 코드가 다음과 같이 표시 되도록 `</h1>` 닫는 태그를 제거 합니다. `<h1>Hi (Your Name)!`. 이 변경 내용을 저장 하면 "컴파일 실패" 오류가 브라우저에 표시 되 고 터미널에서 `<h1>`에 대 한 닫는 태그가 필요 함을 알 수 있습니다. `</h1>` 닫는 태그를 바꾸고 저장 하 고 페이지를 다시 로드 합니다.

F5 키를 선택 하거나 메뉴 모음에서 **> 디버그** (Ctrl + Shift + D) 및 **보기 > 디버그 콘솔** (Ctrl + shift + Y)로 이동 하 여 다음 .js 앱에서 VS Code의 디버거를 사용할 수 있습니다. 디버그 창에서 기어 아이콘을 선택 하면 디버깅 설정 정보를 저장 하기 위한 시작 구성 (`launch.json`) 파일이 생성 됩니다. 자세히 알아보려면 [VS Code 디버깅](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)을 참조 하세요.

![VS Code 디버그 창 및 시작 json 구성 아이콘](../images/vscode-debug-launch-configuration.png)

Gatsby에 대 한 자세한 내용은 [Gatsby 문서](https://www.gatsbyjs.org/docs/)를 참조 하세요.
