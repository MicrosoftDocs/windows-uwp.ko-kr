---
title: 초보자를 위한 Windows에서 NodeJS 사용 시작
description: 초보자가 Windows에서 Node.js 개발을 시작하는 데 도움이 되는 가이드입니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, nodejs 학습, windows 노드, 초보자를 위한 windows 노드, windows 노드로 개발, windows에서 nodejs로 개발
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 5737316ae2de0520e5443f69cefaec25679a228f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166637"
---
# <a name="get-started-using-nodejs-on-windows-for-beginners"></a>초보자를 위한 Windows에서 Node.js 사용 시작

Node.js를 처음 사용하는 경우 이 가이드의 몇 가지 기본 사항으로 시작할 수 있습니다.

## <a name="prerequisites"></a>사전 요구 사항

이 가이드에서는 다음을 포함하여 [네이티브 Windows에서 Node.js 개발 환경 설치](./setup-on-windows.md) 단계를 이미 완료했다고 가정합니다.

- Node.js 버전 관리자를 설치합니다.
- Visual Studio Code를 설치합니다.

Windows에서 직접 Node.js를 설치하는 것은 최소한의 설정으로 기본 Node.js 작업을 수행하는 가장 간단한 방법입니다.

Node.js를 사용하여 프로덕션 애플리케이션을 개발할 준비가 되면(일반적으로 Linux 서버에 배포하는 경우) [WSL2를 사용하여 Node.js 개발 환경을 설정](./setup-on-wsl2.md)하는 것이 좋습니다. Windows 서버에서 웹앱을 배포할 수 있지만, [Linux 서버를 사용하여 Node.js 앱을 호스트](https://azure.microsoft.com/develop/nodejs/)하는 것이 훨씬 더 일반적입니다.

## <a name="types-of-nodejs-applications"></a>Node.js 애플리케이션 유형

Node.js는 웹 애플리케이션을 만드는 데 주로 사용되는 JavaScript 런타임입니다. 다시 말해서 애플리케이션의 백 엔드를 작성하는 데 사용되는 JavaScript의 서버 쪽 구현입니다. 많은 Node.js 프레임워크로 프런트 엔드를 처리할 수도 있습니다. Node.js를 사용하여 만들 수 있는 작업의 몇 가지 예는 다음과 같습니다.

- **SPA(단일 페이지 앱)** : 브라우저 내에서 작동하는 웹앱이며, 새 데이터를 가져오는 데 사용할 때마다 페이지를 다시 로드할 필요가 없습니다. SPA의 몇 가지 예로는 소셜 네트워킹 앱, 메일 또는 지도 앱, 온라인 텍스트 또는 그리기 도구 등이 있습니다.
- **RTA(실시간 앱)** : 이러한 웹앱은 사용자(또는 소프트웨어)가 정기적으로 업데이트가 있는지 소스를 확인하도록 요구하는 대신, 작성자가 게시하는 즉시 정보를 받을 수 있도록 하는 웹앱입니다. RTA의 예에는 인스턴트 메시징 앱 또는 대화방, 브라우저에서 재생할 수 있는 온라인 멀티플레이 게임, 온라인 공동 작업 문서, 커뮤니티 스토리지, 화상 회의 앱 등이 포함됩니다.
- **데이터 스트리밍 앱**: 연결을 열어 두어 필요에 따라 추가 데이터, 콘텐츠 또는 구성 요소를 계속 다운로드하면서 도착하는(또는 생성된) 데이터/콘텐츠를 전송하는 앱(또는 서비스)입니다. 몇 가지 예로는 비디오 및 오디오 스트리밍 앱이 있습니다.
- **REST API**: 이러한 인터페이스는 다른 사람의 웹앱이 상호 작용할 데이터를 제공하는 인터페이스입니다. 예를 들어 일정 API 서비스는 다른 사용자의 로컬 이벤트 웹 사이트에서 사용할 수 있는 콘서트 장소에 대한 날짜 및 시간을 제공할 수 있습니다.
- **SSRP(서버 쪽 렌더링 앱)** : 이러한 웹앱은 클라이언트(브라우저/프런트 엔드) 및 서버(백 엔드) 둘 다에서 실행될 수 있으므로 동적 페이지에서 알려진 콘텐츠는 표시(HTML 생성)하고 알려지지 않은 콘텐츠는 사용할 수 있게 될 때 가져오도록 할 수 있습니다. 이러한 애플리케이션을 종종 "동일 구조" 또는 "유니버설" 애플리케이션이라고 합니다. SSR은 사용할 때마다 다시 로드 하지 않아도 되는 SPA 방법을 활용합니다. 그러나 SSR는 사이트의 콘텐츠를 Google 검색 결과에 표시하고, 앱 링크가 Twitter 또는 Facebook과 같은 소셜 미디어에서 공유될 때 미리 보기 이미지를 제공하는 경우처럼 사용자에게 중요하거나 중요하지 않을 수 있는 몇 가지 이점을 제공합니다. 잠재적인 단점은 Node.js 서버를 지속적으로 실행해야 한다는 것입니다. 예를 들어, 사용자가 검색 결과 및 소셜 미디어에 표시하려고 하는 이벤트를 지원하는 소셜 네트워킹 앱은 SSR에서 혜택을 받을 수 있지만, 메일 앱은 SPA로도 충분할 수 있습니다. WordPress 블로그와 비슷할 수 있는 서버에서 렌더링된 SPA 없는 앱을 실행할 수도 있습니다. 알 수 있는 것처럼 복잡성이 늘어나서 중요한 사항만 결정해야 할 수도 있습니다.
- **명령줄 도구**: 이러한 작업을 통해 반복적인 작업을 자동화하고 방대한 Node.js 에코시스템에 도구를 배포할 수 있습니다. 명령줄 도구의 예로는 클라이언트 URL을 나타내는 cURL이 있습니다. 이 도구는 인터넷 URL에서 콘텐츠를 다운로드하는 데 사용됩니다. cURL은 Node.js 또는 Node.js 버전 관리자와 같은 항목을 설치하는 데 종종 사용됩니다.
- **하드웨어 프로그래밍**: 웹앱으로 널리 사용되지는 않지만, Node.js는 센서, 비콘, 송신기, 모터 또는 대량의 데이터를 생성하는 장치에서 데이터를 수집하는 것과 같은 IoT 용도로는 널리 사용됩니다. Node.js는 데이터 수집, 해당 데이터를 분석, 디바이스와 서버 간 통신, 분석에 따른 작업 수행 등을 지원할 수 있습니다. NPM에는 Arduino 컨트롤러, raspberry pi, Intel IoT Edison, 다양한 센서 및 Bluetooth 디바이스를 위한 80가지 이상의 패키지가 포함되어 있습니다.

## <a name="try-using-nodejs-in-vs-code"></a>VS Code에서 Node.js 사용해 보기

1. 명령줄(명령 프롬프트, PowerShell 또는 원하는 프로그램)을 열고 새 디렉터리를 만든 다음(`mkdir HelloNode`) 디렉터리를 입력합니다(`cd HelloNode`).

2. 내부에 "msg"라는 변수를 사용하여 "app.js"라는 JavaScript 파일을 만듭니다(`echo var msg > app.js`).

3. VS Code에서 디렉터리와 app.js 파일을 엽니다(`code .`).

4. 간단한 문자열 변수("Hello World")를 추가한 다음, "app.js" 파일에 다음을 입력하여 문자열의 내용을 콘솔로 보냅니다.

    ```js
    var msg = 'Hello World';
    console.log(msg);
    ```

5. Node.js를 사용하여 "app.js" 파일을 실행하려면 **보기** > **터미널**을 선택하여(또는 Ctrl+'를 선택하고, 억음 문자 사용) VS Code 내에서 바로 터미널을 엽니다. 기본 터미널을 변경해야 하는 경우 드롭다운 메뉴를 선택하고 **기본 셸 선택**을 선택합니다.

6. 터미널에서 `node app.js`를 입력합니다. 다음 메시지가 표시됩니다. "Hello World".

> [!NOTE]
> 'app.js' 파일에 `console`을 입력하면 IntelliSense를 사용하여 선택할 수 있도록 [`console`](https://developer.mozilla.org/docs/Web/API/Console) 개체와 관련된 지원 옵션이 VS Code에 표시됩니다. 다른 [JavaScript 개체](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects)를 사용하여 Intellisense를 시험해 보세요.

> [!TIP]
> 여러 명령줄(Ubuntu, PowerShell, Windows 명령 프롬프트 등)을 사용하려는 경우 또는 텍스트, 배경색, 키 바인딩, 다중 창 등을 비롯한 [터미널을 사용자 지정](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md)하려는 경우 새 [Windows 터미널](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md)을 사용해 보세요.

## <a name="set-up-a-basic-web-app-framework-by-using-express"></a>Express를 사용하여 기본 웹앱 프레임워크 설정

Express는 최소화되고 유연하며 간소화된 Node.js 프레임워크로, Express를 사용하면 GET, PUT, POST, DELETE 등 여러 유형의 요청을 처리할 수 있는 웹앱을 더 쉽게 개발할 수 있습니다. Express에는 앱의 파일 아키텍처를 자동으로 만드는 애플리케이션 생성기가 포함되어 있습니다.

Express.js를 사용하여 프로젝트를 만들려면

1. 명령줄(명령 프롬프트, Powershell 또는 원하는 프로그램)을 엽니다.
2. 새 프로젝트 폴더를 만들고(`mkdir ExpressProjects`) 해당 디렉터리를 입력합니다(`cd ExpressProjects`).
3. Express를 사용하여 HelloWorld 프로젝트 템플릿을 만듭니다(`npx express-generator HelloWorld --view=pug`).

>[!NOTE]
> 여기에서 `npx` 명령을 사용하여 실제로 설치하지 않고(또는 원하는 방식에 따라 임시로 설치) Express.js Node 패키지를 실행합니다. `express` 명령을 사용하거나 `express --version`을 사용하여 설치된 Express의 버전을 확인하려는 경우에는 Express를 찾을 수 없다는 응답을 받게 됩니다. 반복적으로 다시 사용하기 위해 전역적으로 Express를 설치하려면 `npm install -g express-generator`를 사용합니다. `npm list`를 사용하여 npm에 의해 설치된 패키지 목록을 볼 수 있습니다. 패키지는 수준(중첩된 디렉터리 수)별로 나열됩니다. 직접 설치한 패키지는 수준 0이 됩니다. 해당 패키지의 종속성은 수준 1이 되고, 추가 종속성은 수준 2 등이 됩니다. 자세한 내용은 Stackoverflow에서 [npx와 npm 간 차이점](https://stackoverflow.com/questions/50605219/difference-between-npx-and-npm)을 참조하세요.

4. VS Code에서 프로젝트를 열고 `code .`를 입력하여 Express가 포함한 파일 및 폴더를 확인합니다.

   Express에서 생성되는 파일은 처음에는 약간 어려워 보이는 아키텍처를 사용하는 웹앱을 만듭니다. VS Code **탐색기** 창(보려는 경우 Ctrl+Shift+E)에서 다음 파일 및 폴더가 생성되었음을 알 수 있습니다.

   - `bin`. 앱을 시작하는 실행 파일을 포함합니다. 대안을 제공하지 않은 경우 포트 3000에서 서버를 실행하고 기본 오류 처리를 설정합니다. 
   - `public`. JavaScript 파일, CSS 스타일시트, 글꼴 파일, 이미지 및 사용자가 웹 사이트에 연결할 때 필요한 기타 자산을 포함하여 공개적으로 액세스되는 모든 파일을 포함합니다.
   - `routes`. 애플리케이션에 대한 모든 경로 처리기를 포함합니다. 2개의 파일 `index.js`와 `users.js`가 이 폴더에 자동으로 생성되어 애플리케이션의 경로 구성을 분리하는 방법의 예를 제공합니다.
   - `views`. 템플릿 엔진에서 사용되는 파일을 포함합니다. Express는 렌더링 메서드가 호출될 때 여기서 일치하는 뷰를 찾도록 구성되어 있습니다. 기본 템플릿 엔진은 Jade이지만, Jade는 사용되지 않고 Pug로 대체되었으므로 `--view` 플래그를 사용하여 뷰(템플릿) 엔진을 변경했습니다. `express --help`를 사용하면 `--view` 플래그 옵션 등을 확인할 수 있습니다.
   - `app.js`. 앱의 시작 지점입니다. 모든 항목을 로드하고 사용자 요청을 처리하기 시작합니다. 기본적으로 모든 부분을 결합하는 접착제 역할을 합니다.
   - `package.json`. 프로젝트 설명, 스크립트 관리자 및 앱 매니페스트를 포함합니다. 주로 앱의 종속성과 해당 버전을 추적하는 데 사용됩니다.

5. 이제 HelloWorld Express 앱(`package.json` 파일에 정의된 대로 서버 실행과 같은 작업에 사용되는 패키지)을 빌드 및 실행하기 위해 Express에서 사용하는 종속성을 설치해야 합니다. VS Code 내에서 **보기** > **터미널**을 선택하여(또는 억음 악센트 문자를 사용하여 Ctrl+' 선택) 터미널을 열고 아직 'HelloWorld' 프로젝트 디렉터리에 있는지 확인합니다. 다음을 사용하여 Express 패키지 종속성을 설치합니다.

```bash
npm install
```

6. 이제 다양한 API와 HTTP 유틸리티 메서드 및 미들웨어에 액세스할 수 있는 다중 페이지 웹앱에 대한 프레임워크가 설정되었으며 강력한 API를 더 쉽게 만들 수 있습니다. 다음을 입력하여 가상 서버에서 Express 앱을 시작합니다.

```bash
npx cross-env DEBUG=HelloWorld:* npm start
```

> [!TIP]
> 위 명령의 `DEBUG=myapp:*` 부분에서는 디버깅을 위해 로깅을 설정할 것을 Node.js에 지시합니다. 'myapp'을 앱 이름으로 바꾸어야 합니다. `package.json` 파일의 "name" 속성 아래에서 앱 이름을 찾을 수 있습니다. `npx cross-env`를 사용하면 터미널에서 `DEBUG` 환경 변수가 설정되지만 터미널에서 특정 방식으로 설정할 수도 있습니다. `npm start` 명령은 `package.json` 파일에서 스크립트를 실행하도록 npm에 지시합니다.

7. 이제 웹 브라우저를 열고 **localhost:3000**으로 이동하여 실행 중인 앱을 볼 수 있습니다.

   ![브라우저에서 실행 중인 Express 앱의 스크린샷](../images/express-app.png)

8. 이제 HelloWorld Express 앱이 브라우저에서 로컬로 실행되고 있으므로 프로젝트 디렉터리에서 'views' 폴더를 열고 'index.pug' 파일을 선택하여 변경을 시도합니다. 열린 후에는 `h1= title`을 `h1= "Hello World!"`로 변경하고 **저장**(Ctrl+S)을 선택합니다. 웹 브라우저에서 **localhost:3000** URL을 새로 고쳐 변경 내용을 확인합니다.

9. Express 앱 실행을 중지하려면 터미널에서 다음을 입력합니다. **Ctrl+C**

## <a name="try-using-a-nodejs-module"></a>Node.js 모듈 사용

Node.js에는 서버 쪽 웹앱 개발에 유용한 여러 도구가 있습니다. 일부는 기본적으로 제공되며 npm을 통해 사용할 수 있는 도구가 훨씬 더 많습니다. 이러한 모듈은 다음과 같은 다양한 작업에 유용할 수 있습니다.

|도구               |사용 목적                                                                                                  |
|:----------------- |:---------------------------------------------------------------------------------------------------------|
|gm, sharp          |JavaScript 코드에서 직접 편집, 크기 조정, 압축 등을 비롯한 이미지 조작 수행 |
|PDFKit             |PDF 생성                                                                                            |
|validator.js       |문자열 유효성 검사                                                                                         |
|imagemin, UglifyJS2|축소                                                                                              |
|spritesmith        |Sprite sheet 생성                                                                                   |
|Winston            |로깅                                                                                                  |
|commander.js       |명령줄 애플리케이션 만들기                                                                       |

컴퓨터의 운영 체제에 대한 일부 정보를 얻기 위해 기본 제공 OS 모듈을 사용해 보겠습니다.

1) 명령줄에서 Node.js CLI를 엽니다. `node`를 입력하면 `>` 프롬프트에 Node.js를 사용하고 있다고 표시됩니다.

2) 현재 사용 중인 운영 체제를 확인하려면(Windows를 사용 중임을 나타내는 응답 반환) `os.platform()`을 입력합니다.

3) CPU의 아키텍처를 확인하려면 `os.arch()`를 입력합니다.

4) 시스템에서 사용할 수 있는 CPU를 보려면 `os.cpus()`를 입력합니다.

5) `.exit`를 입력하거나 Ctrl+C를 두 번 선택하여 Node.js CLI에서 나갑니다.

   > [!TIP]
   > Node.js OS 모듈을 사용하여 플랫폼을 확인하고 플랫폼별 변수를 반환하는 등의 작업을 수행할 수 있습니다. Windows 개발용 Win32/.bat, Mac/unix, Linux, SunOS 등을 위한 darwin/.sh(예: `var isWin = process.platform === "win32";`)

## <a name="next-steps"></a>다음 단계

이 가이드에서는 Node.js를 사용하여 수행할 수 있는 작업에 대한 몇 가지 기본적인 사항을 배우고, VS Code에서 Node.js 명령줄을 사용해 보고, Express.js를 사용하여 간단한 웹앱을 만든 후 웹 브라우저에서 로컬로 실행하고, 몇 가지 기본 제공 Node.js 모듈을 사용해 보았습니다. 널리 사용되는 Node.js 웹 프레임워크를 설치 및 사용하는 방법에 대한 자세한 내용은 Next.js(React를 기준으로 하는 서버 렌더링 웹 프레임워크), Nuxt.js(Vue를 기준으로 하는 서버 렌더링 웹 프레임워크) 및 Gatsby(React를 기준으로 하는 정적 렌더링 웹 프레임워크)를 다루는 다음 가이드를 계속 진행합니다. MongoDB 또는 PostgreSQL 데이터베이스나 Docker 컨테이너로 작업하는 방법을 학습하는 과정으로 건너뛸 수도 있습니다.

- [Windows에서 Node.js 웹 프레임워크 시작](./web-frameworks.md)
- [Node.js 앱을 데이터베이스에 연결](/windows/wsl/tutorials/wsl-database)
- [Node.js에서 Docker 컨테이너 사용 시작](./containers.md)