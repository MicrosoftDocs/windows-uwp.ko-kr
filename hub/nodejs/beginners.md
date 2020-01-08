---
title: 초보자를 위한 Windows에서 NodeJS 사용 시작
description: Windows에서 node.js 개발을 시작 하는 데 도움이 되는 가이드입니다.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, node.js, windows 10, microsoft, 학습 NodeJS, windows의 노드, 초보자를 위한 windows의 노드, windows에서 노드를 사용 하 여 개발, windows에서 NodeJS를 사용 하는 개발자
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 433eb5701696f590f10d8b3276481098b9ec073d
ms.sourcegitcommit: 8f9cea69f33b06166fec22677eaa43466352c14d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/04/2020
ms.locfileid: "75657085"
---
# <a name="get-started-using-nodejs-on-windows-for-beginners"></a>초보자를 위한 Windows에서 node.js를 사용 하 여 시작 하기

Node.js를 처음 사용 하는 경우이 가이드를 통해 몇 가지 기본 사항을 시작 하는 데 도움을 얻을 수 있습니다.

## <a name="prerequisites"></a>전제 조건

이 가이드에서는 다음을 포함 하 여 [기본 창에서 node.js 개발 환경을 설정](./setup-on-windows.md)하는 단계를 이미 완료 했다고 가정 합니다.

- Node.js 버전 관리자를 설치 합니다.
- Visual Studio Code를 설치 합니다.

Windows에서 직접 node.js를 설치 하는 것은 최소한의 설정으로 기본 node.js 작업을 수행 하기 시작 하는 가장 간단한 방법입니다.

Node.js를 사용 하 여 프로덕션 응용 프로그램을 개발할 준비가 되 면 (일반적으로 Linux 서버에 배포 하는 경우) [WSL2를 사용 하 여 node.js 개발 환경을 설정](./setup-on-wsl2.md)하는 것이 좋습니다. Windows server에 웹 앱을 배포할 수 있지만 [Linux 서버를 사용 하 여 node.js 앱을 호스트](https://azure.microsoft.com/develop/nodejs/)하는 것이 훨씬 더 일반적입니다.

## <a name="types-of-nodejs-applications"></a>Node.js 애플리케이션 유형

Node.js는 웹 응용 프로그램을 만드는 데 주로 사용 되는 JavaScript 런타임입니다. 또 다른 방법으로 응용 프로그램의 백 엔드를 작성 하는 데 사용 되는 JavaScript의 서버 쪽 구현입니다. 많은 node.js 프레임 워크는 프런트 엔드를 처리할 수도 있습니다. Node.js를 사용 하 여 만들 수 있는 작업의 몇 가지 예는 다음과 같습니다.

- **단일 페이지 앱 (SPAs)** : 브라우저 내에서 작동 하 고 새 데이터를 가져오는 데 사용할 때마다 페이지를 다시 로드 하지 않아도 되는 웹 앱입니다. 몇 가지 예로는 소셜 네트워킹 앱, 메일 또는 맵 앱, 온라인 텍스트 또는 그리기 도구 등이 있습니다.
- **Rta (실시간 앱)** : 사용자 (또는 소프트웨어)가 정기적으로 업데이트를 위해 소스를 확인 하도록 요구 하는 대신 작성자가 게시 하는 즉시 정보를 받을 수 있도록 하는 웹 앱입니다. Rta 예에는 인스턴트 메시징 앱 또는 대화방, 브라우저에서 재생할 수 있는 온라인 멀티 플레이 게임, 온라인 공동 작업 문서, 커뮤니티 저장소, 비디오 회의 앱 등이 포함 됩니다.
- **데이터 스트리밍 앱**: 필요에 따라 추가 데이터, 콘텐츠 또는 구성 요소를 계속 해 서 다운로드 하기 위해 연결을 열어 둔 상태에서 데이터/콘텐츠를 전송 하는 앱 (또는 서비스)입니다. 몇 가지 예로는 비디오 및 오디오 스트리밍 앱이 있습니다.
- **REST api**: 다른 사람의 웹 앱이 상호 작용할 수 있는 데이터를 제공 하는 인터페이스입니다. 예를 들어 일정 API 서비스는 다른 사용자의 로컬 이벤트 웹 사이트에서 사용할 수 있는 공동 장소에 대 한 날짜 및 시간을 제공할 수 있습니다.
- **서버 쪽에서 렌더링 한 앱 (SSRs)** : 이러한 웹 앱은 클라이언트 (브라우저/프런트 엔드) 및 서버 (백 엔드)에서 실행 될 수 있으며, 알려진 콘텐츠를 표시 하 고 (HTML 생성), 사용 가능한 것으로 알려지지 않은 콘텐츠를 신속 하 게 가져올 수 있도록 합니다. 이러한 응용 프로그램을 종종 "isomorphic" 또는 "유니버설" 응용 프로그램 이라고 합니다. SSRs는 사용할 때마다 다시 로드 하지 않아도 되는 SPA 방법을 활용 합니다. 그러나 SSRs는 사용자에 게 중요 하거나 중요 하지 않을 수 있는 몇 가지 이점을 제공 합니다. 예를 들어 사이트의 콘텐츠를 Google 검색 결과에 표시 하 고, 앱에 대 한 링크가 Twitter 또는 Facebook과 같은 소셜 미디어에서 공유 되는 경우 미리 보기 이미지를 제공 하는 것과 같습니다. 잠재적인 단점은 node.js 서버를 지속적으로 실행 해야 한다는 것입니다. 예를 들어, 사용자가 검색 결과 및 소셜 미디어에 표시 하고자 하는 이벤트를 지 원하는 소셜 네트워킹 앱은 SSR에서 혜택을 받을 수 있으며, 전자 메일 앱은 SPA로도 충분 합니다. 서버에서 렌더링 된 SPA 없는 앱을 실행할 수도 있습니다 .이 앱은 WordPress 블로그와 같습니다. 표시 되는 것 처럼 복잡 해질 수 있는 것은 중요 한 사항을 결정 해야 하는 것입니다.
- **명령줄 도구**: 이러한 작업을 통해 반복적인 작업을 자동화 하 고 방대한 node.js 에코 시스템에 도구를 배포할 수 있습니다. 명령줄 도구의 예는 클라이언트 URL을 위한 것 이며 인터넷 URL에서 콘텐츠를 다운로드 하는 데 사용 되는 말아 넘기기입니다. 말아는 node.js 또는 node.js 버전 관리자와 같은 항목을 설치 하는 데 종종 사용 됩니다.
- **하드웨어 프로그래밍**: 웹 앱으로 널리 사용 되지 않는 반면, node.js는 센서, 탐지 장치, 송신기, 모터 또는 많은 양의 데이터를 생성 하는 모든 항목에서 데이터를 수집 하는 것과 같은 IoT 용도로 널리 사용 됩니다. Node.js는 데이터 수집을 사용 하도록 설정 하 고, 해당 데이터를 분석 하 고, 장치와 서버 간에 통신 하 고, 분석에 따라 조치를 취하는 데 사용할 수 있습니다. NPM에는 Arduino 컨트롤러, raspberry pi, Intel IoT Edison, 다양 한 센서 및 Bluetooth 장치에 대 한 패키지 80 개 이상이 포함 되어 있습니다.

## <a name="try-using-nodejs-in-vs-code"></a>VS Code에서 node.js를 사용해 보세요.

1. 명령줄 (명령 프롬프트, PowerShell 또는 원하는 모든 항목)을 열고 새 디렉터리 `mkdir HelloNode`만든 다음 디렉터리를 입력 합니다. `cd HelloNode`

2. 내에서 "msg" 라는 변수를 사용 하 여 "node.js" 라는 JavaScript 파일을 만듭니다. `echo var msg > app.js`

3. VS Code:: `code .`에서 디렉터리와 node.js 파일을 엽니다.

4. 단순 문자열 변수 ("Hello World")를 추가한 다음 "node.js" 파일에 입력 하 여 문자열의 내용을 콘솔로 보냅니다.

    ```js
    var msg = 'Hello World';
    console.log(msg);
    ```

5. Node.js를 사용 하 여 "node.js" 파일을 실행 합니다.  > **터미널** **보기** (또는 Ctrl + '를 선택 하 고, 억음 문자 사용)를 선택 하 여 VS Code 내부에서 바로 터미널을 엽니다. 기본 터미널을 변경 해야 하는 경우 드롭다운 메뉴를 선택 하 고 **기본 셸 선택**을 선택 합니다.

6. 터미널에서 `node app.js`를 입력 합니다. "Hello World" 출력이 표시 됩니다.

> [!NOTE]
> ' Node.js ' 파일에 `console`를 입력 하면 IntelliSense 사용에서 선택할 수 있도록 [`console`](https://developer.mozilla.org/docs/Web/API/Console) 개체와 관련 하 여 지원 되는 옵션이 VS Code 표시 됩니다. 다른 [JavaScript 개체](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects)를 사용 하 여 Intellisense를 시험해 보세요.

> [!TIP]
> 여러 명령줄 (Ubuntu, PowerShell, Windows 명령 프롬프트 등)을 사용 하거나 텍스트, 배경색, 키 바인딩, 여러 창 창 등을 포함 하 여 [터미널을 사용자 지정](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md)하려는 경우 새 [Windows 터미널](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md) 을 사용해 보세요.

## <a name="set-up-a-basic-web-app-framework-by-using-express"></a>Express를 사용하여 기본 웹앱 프레임워크 설정

Express는 최소화되고 유연하며 간소화된 Node.js 프레임워크로, Express를 사용하면 GET, PUT, POST, DELETE 등 여러 유형의 요청을 처리할 수 있는 웹앱을 더 쉽게 개발할 수 있습니다. Express에는 앱의 파일 아키텍처를 자동으로 만드는 애플리케이션 생성기가 포함되어 있습니다.

Express .js를 사용 하 여 프로젝트를 만들려면

1. 명령줄 (명령 프롬프트, Powershell 또는 원하는 모든 항목)을 엽니다.
2. 새 프로젝트 폴더 만들기: `mkdir ExpressProjects` 하 고 해당 디렉터리를 입력 합니다. `cd ExpressProjects`
3. Express를 사용 하 여 HelloWorld 프로젝트 템플릿 만들기: `npx express-generator HelloWorld --view=pug`

>[!NOTE]
> 여기에서 `npx` 명령을 사용 하 여 실제로 설치 하지 않고도 Express .js 노드 패키지를 실행 하거나, 원하는 방법에 따라 임시로 설치 합니다. `express` 명령을 사용 하거나를 사용 하 `express --version`여 설치 된 Express의 버전을 확인 하는 경우에는 Express를 찾을 수 없다는 응답을 받게 됩니다. 전체적으로 Express를 사용 하도록 Express를 전역으로 설치 하려면 `npm install -g express-generator`을 사용 합니다. `npm list`를 사용 하 여 npm에 의해 설치 된 패키지 목록을 볼 수 있습니다. 패키지는 수준(중첩된 디렉터리 수)별로 나열됩니다. 직접 설치한 패키지는 수준 0이 됩니다. 해당 패키지의 종속성은 수준 1이 되고, 추가 종속성은 수준 2 등이 됩니다. 자세한 내용은 Stackoverflow에서 [npx와 npm?의 차이점](https://stackoverflow.com/questions/50605219/difference-between-npx-and-npm) 을 참조 하세요.

4. VS Code에서 프로젝트를 열고 다음을 사용 하 여 포함 된 파일 및 폴더를 검사 합니다. `code .`

   Express에서 생성되는 파일은 처음에는 약간 어려워 보이는 아키텍처를 사용하는 웹앱을 만듭니다. 다음 파일 및 폴더가 생성 되었는지 VS Code **탐색기** 창 (Ctrl + Shift + E)에서 볼 수 있습니다.

   - `bin`을 차례로 클릭합니다. 앱을 시작하는 실행 파일을 포함합니다. 대안을 제공하지 않은 경우 포트 3000에서 서버를 실행하고 기본 오류 처리를 설정합니다. 
   - `public`을 차례로 클릭합니다. JavaScript 파일, CSS 스타일시트, 글꼴 파일, 이미지 및 사용자가 웹 사이트에 연결할 때 필요한 기타 자산을 포함하여 공개적으로 액세스되는 모든 파일을 포함합니다.
   - `routes`을 차례로 클릭합니다. 애플리케이션에 대한 모든 경로 처리기를 포함합니다. 2개의 파일 `index.js`와 `users.js`가 이 폴더에 자동으로 생성되어 애플리케이션의 경로 구성을 분리하는 방법의 예를 제공합니다.
   - `views`을 차례로 클릭합니다. 템플릿 엔진에서 사용되는 파일을 포함합니다. Express는 렌더링 메서드가 호출될 때 여기서 일치하는 뷰를 찾도록 구성되어 있습니다. 기본 템플릿 엔진은 Jade이지만, Jade는 사용되지 않고 Pug로 대체되었으므로 `--view` 플래그를 사용하여 뷰(템플릿) 엔진을 변경했습니다. `express --help`를 사용하면 `--view` 플래그 옵션 등을 확인할 수 있습니다.
   - `app.js`을 차례로 클릭합니다. 앱의 시작 지점입니다. 모든 항목을 로드하고 사용자 요청을 처리하기 시작합니다. 기본적으로 모든 부분을 결합하는 접착제 역할을 합니다.
   - `package.json`을 차례로 클릭합니다. 프로젝트 설명, 스크립트 관리자 및 앱 매니페스트를 포함합니다. 주로 앱의 종속성과 해당 버전을 추적하는 데 사용됩니다.

5. 이제를 사용 하 여 HelloWorld Express 앱을 빌드 및 실행 하기 위해 Express에서 사용 하는 종속성을 설치 해야 합니다 (`package.json` 파일에 정의 된 대로 서버 실행과 같은 작업에 사용 되는 패키지). VS Code 내에서 **보기** > **터미널** 을 선택 하 여 터미널을 열거나 (또는 Ctrl + '를 선택 하 여 억음 문자 사용) 여전히 ' HelloWorld ' 프로젝트 디렉터리에 있는지 확인 합니다. 다음을 사용 하 여 Express 패키지 종속성을 설치 합니다.

```bash
npm install
```

6. 이제 다양한 API와 HTTP 유틸리티 메서드 및 미들웨어에 액세스할 수 있는 다중 페이지 웹앱에 대한 프레임워크가 설정되었으며 강력한 API를 더 쉽게 만들 수 있습니다. 다음을 입력 하 여 가상 서버에서 Express 앱을 시작 합니다.

```bash
npx cross-env DEBUG=HelloWorld:* npm start
```

> [!TIP]
> 위의 명령 `DEBUG=myapp:*` 부분에서는 디버깅을 위해 로깅을 설정 하려는 node.js를 지시 하는 것을 의미 합니다. ' Myapp '를 앱 이름으로 바꾸어야 합니다. `package.json` 파일의 "name" 속성 아래에서 앱 이름을 찾을 수 있습니다. `npx cross-env`를 사용 하 여 터미널에서 `DEBUG` 환경 변수를 설정 하지만 터미널 특정 방식으로 설정할 수도 있습니다. `npm start` 명령은 `package.json` 파일에서 스크립트를 실행 하도록 npm에 지시 합니다.

7. 이제 웹 브라우저를 열고 **localhost: 3000** 으로 이동 하 여 실행 중인 응용 프로그램을 볼 수 있습니다.

   ![브라우저에서 실행 중인 Express 앱의 스크린샷](../images/express-app.png)

8. 이제 HelloWorld Express 앱이 브라우저에서 로컬로 실행 되 고 있으므로 프로젝트 디렉터리에서 ' views ' 폴더를 열고 ' pug ' 파일을 선택 하 여 변경을 시도 합니다. 열린 후에는 `h1= title` `h1= "Hello World!"` 변경 하 고 **저장** (Ctrl + S)을 선택 합니다. 웹 브라우저에서 **localhost: 3000** URL을 새로 고쳐 변경 내용을 확인 합니다.

9. 터미널에서 Express 앱 실행을 중지 하려면 **Ctrl + C** 를 입력 합니다.

## <a name="try-using-a-nodejs-module"></a>Node.js 모듈을 사용해보기

Node.js에는 서버 쪽 웹앱 개발에 유용한 여러 도구가 있습니다. 일부는 기본적으로 제공되며 npm을 통해 사용할 수 있는 도구가 훨씬 더 많습니다. 이러한 모듈은 다음과 같은 다양한 작업에 유용할 수 있습니다.

|도구               |사용 대상                                                                                                  |
|:----------------- |:---------------------------------------------------------------------------------------------------------|
|gm, sharp          |JavaScript 코드에서 직접 편집, 크기 조정, 압축 등을 비롯한 이미지 조작 수행 |
|PDFKit             |PDF 생성                                                                                            |
|validator.js       |문자열 유효성 검사                                                                                         |
|imagemin, UglifyJS2|축소                                                                                              |
|spritesmith        |Sprite sheet 생성                                                                                   |
|Winston            |Logging(로깅)                                                                                                  |
|commander.js       |명령줄 애플리케이션 만들기                                                                       |

컴퓨터의 운영 체제에 대한 일부 정보를 얻기 위해 기본 제공 OS 모듈을 사용해 보겠습니다.

1) 명령줄에서 node.js CLI를 엽니다. 다음을 입력 한 후 node.js를 사용 하 고 있음을 알리는 `>` 프롬프트가 표시 됩니다. `node`

2) 현재 사용 중인 운영 체제를 확인 하려면 (Windows에 대 한 응답을 반환 해야 함) 다음을 입력 합니다. `os.platform()`

3) CPU의 아키텍처를 확인 하려면 다음을 입력 합니다. `os.arch()`

4) 시스템에서 사용할 수 있는 Cpu를 보려면 다음을 입력 `os.cpus()`

5) `.exit`를 입력하거나 Ctrl+C를 두 번 선택하여 Node.js CLI에서 나갑니다.

   > [!TIP]
   > Node.js OS 모듈을 사용 하 여 플랫폼을 확인 하 고 플랫폼별 변수를 반환 하는 것과 같은 작업을 수행할 수 있습니다. 예 `var isWin = process.platform === "win32";`를 들어, Windows 개발의 경우 Win32/.bat, Mac/unix, 다윈

## <a name="next-steps"></a>다음 단계

이 가이드에서는 node.js를 사용 하 여 수행할 수 있는 작업에 대 한 몇 가지 기본적인 사항을 배웠습니다. VS Code에서 node.js 명령줄을 사용 하 여, Express .js를 사용 하 여 간단한 웹 앱을 만든 다음 웹 브라우저에서 로컬로 실행 하 고 몇 가지 기본 제공 node.js 모듈을 사용 하 여 시도 했습니다. 널리 사용 되는 node.js 웹 프레임 워크를 설치 하 고 사용 하는 방법에 대 한 자세한 내용은 다음 가이드를 계속 진행 합니다 .이 가이드에서는 다음 .js (응답을 기반으로 하는 서버에서 렌더링 된 웹 프레임 워크), Nuxt (Vue 기반 서버 렌더링 웹 프레임 워크) 및 Gatsby (a 응답을 기반으로 정적으로 렌더링 된 웹 프레임 워크입니다. MongoDB 또는 PostgreSQL 데이터베이스 또는 Docker 컨테이너로 작업 하는 방법에 대 한 학습을 건너뛸 수도 있습니다.

- [Windows에서 node.js 웹 프레임 워크 시작](./web-frameworks.md)
- [Node.js 앱을 데이터베이스에 연결 하기 시작](./databases.md)
- [Node.js에서 Docker 컨테이너를 사용 하 여 시작](./containers.md)
