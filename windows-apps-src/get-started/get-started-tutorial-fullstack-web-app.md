---
author: libbymc
title: REST API 백 엔드를 사용하는 단일 페이지 생성
description: 인기 있는 웹 기술을 사용하여 Microsoft Store용으로 호스트된 웹앱 빌드
keywords: 호스트된 웹앱, HWA, REST API, 단일 페이지 앱, SPA
ms.author: libbymc
ms.date: 05/10/2017
ms.topic: article
ms.prod: Microsoft Edge, Azure, Visual Studio Code
ms.technology: web
ms.localizationpriority: medium
ms.openlocfilehash: 42f11cbdd749a44c4ba0d8bc1a0397a4f2882257
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/19/2018
ms.locfileid: "4054179"
---
# <a name="create-a-single-page-web-app-with-rest-api-backend"></a>REST API 백 엔드를 사용하는 단일 페이지 생성

**인기 있는 전체 스택 웹 기술을 사용하여 Microsoft Store용으로 호스트된 웹앱 빌드**

![단일 페이지 웹앱인 간단한 기억력 게임](images/fullstack.png)

두 부분으로 구성된 이 자습서에서는 브라우저와 Microsoft Store용으로 호스트된 웹앱 모두에서 작동하는 간단한 기억력 게임을 빌드하는 과정을 통해 최신 전체 스택 웹 개발을 간략히 살펴봅니다. 1부에서는 게임의 백 엔드를 위한 간단한 REST API 서비스를 작성합니다. 게임 논리를 API 서비스로 클라우드에서 호스팅함으로써, 게임 상태를 유지하여 사용자가 다른 디바이스에서도 동일한 게임 인스턴스를 플레이할 수 있도록 합니다. 2부에서는 프런트 엔드 UI를 반응형 레이아웃을 가진 단일 페이지 웹앱으로 제작합니다.

몇 가지 가장 인기 있는 웹 기술을 사용할 것입니다. 여기에는 [Node.js](https://nodejs.org/en/) 런타임, 서버 쪽 개발을 위한 [Express](http://expressjs.com/), [부트스크랩](http://getbootstrap.com/) UI 프레임워크, [Pug](https://www.npmjs.com/package/pug) 템플릿 엔진, RESTful API 작성을 위한 [Swagger](http://swagger.io/tools/)가 포함됩니다. 클라우드 호스팅을 위한 [Azure Portal](https://ms.portal.azure.com/) 사용과 [Visual Studio Code](https://code.visualstudio.com/) 편집기 사용 경험도 쌓을 것입니다.

## <a name="prerequisites"></a>필수 구성 요소

컴퓨터에 이러한 리소스가 아직 없다면, 아래 다운로드 링크를 따라 다운로드하십시오.

 - [Node.js](https://nodejs.org/en/download/) - 경로에 노드를 추가하는 옵션을 선택하세요.

 - [Express generator](http://expressjs.com/en/starter/generator.html)- 노드를 설치한 후 다음 코드를 실행하여 Express를 설치합니다. `npm install express-generator -g`

 - [Visual Studio Code](https://code.visualstudio.com/)

Microsoft Azure에서 API 서비스와 기억력 게임 앱을 호스팅하는 최종 단계를 완료하려면, 아직 계정이 없는 경우 [무료 Azure 계정 만들기](https://azure.microsoft.com/en-us/free/)가 필요합니다.

Azure 부분을 생략하거나 연기하려면, Azure 호스팅과 Microsoft Store를 위한 앱 패키징을 다루는 1부와 2부의 마지막 섹션을 건너뛰면 됩니다. 제작할 API 서비스와 웹앱은 컴퓨터에서 로컬(각각 `http://localhost:8000`과 `http://localhost:3000`)로 실행됩니다.

## <a name="part-i-build-a-rest-api-backend"></a>1부: REST API 백 엔드 제작

먼저 기억력 게임 웹앱의 중심이 될 간단한 기억력 게임 API를 만듭니다. [Swagger](http://swagger.io/)를 사용하여 API를 정의하고, 기반 코드를 만들고, 수동 테스팅을 위한 웹 UI를 만듭니다.

이 부분을 건너뛰고 [2부: 단일 페이지 웹 응용 프로그램 제작](#part-ii-build-a-single-page-web-appl)으로 바로 이동하려면 여기 [1부에 대한 완료된 코드](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/backend)를 사용하면 됩니다. *추가 정보* 지침을 따라 코드를 로컬로 실행하거나, *5. Azure에서 API 서비스를 호스트하고 CORS 활성화*를 참조하여 Azure에서 실행합니다.

### <a name="game-overview"></a>게임 개요

*기억력 게임*([*집중*](https://en.wikipedia.org/wiki/Concentration_(game)) 및 [*펠맨식 기억 게임*](https://en.wikipedia.org/wiki/Pelmanism_(system))이라고도 알려짐)은 카드 쌍 몇 벌로 구성된 간단한 게임입니다. 카드는 뒤집어 테이블에 배치되며, 플레이어는 한 번에 두 개의 카드 값을 검사하여 일치하는 카드 쌍을 찾습니다. 한 번의 턴이 끝날 때마다 카드는 다시 뒤집히며, 일치하는 짝을 찾으면 그대로 유지됩니다. 이 경우 이러한 두 카드는 게임에서 클리어한 것입니다. 게임의 목적은 모든 카드 쌍을 가장 적은 턴 내에 찾는 것입니다.

설명을 위한 게임이므로 구조는 매우 간단하게 하나의 게임, 한 명의 플레이어로 합니다. 그러나 게임 논리는 게임 상태를 유지하기 위해 서버 쪽(클라이언트가 아님)에서 처리되므로, 같은 게임을 다른 디바이스에서 계속 플레이할 수 있습니다.

기억력 게임의 데이터 구조는 간단한 JavaScript 개체의 배열로 구성되며, 각 배열은 카드 ID에 해당하는 하나의 카드를 나타냅니다. 서버에서 각 카드는 값과 **cleared** 플래그를 가집니다. 예를 들어, 2개의 짝(4개 카드)으로 구성된 보드는 임의로 생성되어 다음과 같이 일련화됩니다.

```json
[
    { "cleared":"false",
        "value":"0",
    },
    { "cleared":"false",
        "value":"1",
    },
    { "cleared":"false",
        "value":"1",
    },
    { "cleared":"false",
        "value":"0",
    }
]
```

보드 배열이 클라이언트에 전달되면 부정 행위(예: F12 브라우저 도구를 사용하여 HTTP 응답 본문 검사)를 막기 위해 배열에서 값 키가 제거됩니다. 따라서 클라이언트가 **GET /game** REST 끝점을 호출했을 때 동일한 새 게임의 코드는 다음과 같습니다.

```json
[{ "cleared":"false"},{ "cleared":"false"},{ "cleared":"false"},{ "cleared":"false"}]
```

끝점에 경우, 기억력 게임 서비스는 세 개의 REST API로 구성됩니다.

#### <a name="post-new"></a>POST /new
특정 크기(일치하는 카드 짝의 수)의 새 게임 보드를 초기화합니다.

| 매개 변수 | 설명 |
|-----------|-------------|
| int *size* |게임 "보드"에 섞여 배치되는 카드 짝의 수입니다. 예: `http://memorygameapisample/new?size=2`|

| 응답 | 설명 |
|----------|-------------|
| 200 OK | 요청된 크기의 새 기억력 게임이 준비되었습니다.|
| 400 BAD REQUEST| 요청한 크기가 허용할 수 있는 범위를 초과합니다.|


#### <a name="get-game"></a>GET /game
기억력 게임 보드의 현재 상태를 가져옵니다.

*매개 변수 없음*

| 응답 | 설명 |
|----------|-------------|
| 200 OK | 카드 개체의 JSON 배열을 반환합니다. 각 카드에는 일치하는 짝을 이미 찾았는지 나타내는 **cleared** 속성이 있습니다. 일치하는 카드는 자신의 **value**도 보고합니다. 예: `[{"cleared":"false"},{"cleared":"false"},{"cleared":"true","value":1},{"cleared":"true","value":1}]`|

#### <a name="put-guess"></a>PUT /guess
펼칠 카드를 지정하고 이전에 펼친 카드와 일치하는지 확인합니다.

| 매개 변수 | 설명 |
|-----------|-------------|
| int *card* | 펼칠 카드의 카드 ID(게임 보드 배열의 인덱스)입니다. 두 개의 카드로 구성된 완성된 각 "추측"(예: 유효하고 고유한 *카드* 값을 가진 두 개의 **/guess** 호출)입니다. 예: `http://memorygameapisample/guess?card=0`|

| 응답 | 설명 |
|----------|-------------|
| 200 OK | 지정한 카드의 **id**와 **value**를 가진 JSON을 반환합니다. 예: `[{"id":0,"value":1}]`|
| 400 BAD REQUEST |  지정한 카드에 오류가 있습니다. 자세한 내용은 HTTP 응답 본문을 참조하세요.|

### <a name="1-spec-out-the-api-and-generate-code-stubs"></a>1. API 사양을 확인하고 코드 스텁 생성

기억력 게임 API를 작동하는 Node.js 서버 코드로 변환하기 위해 [Swagger](http://swagger.io/)를 사용할 것입니다. [기억력 게임 Swagger 메타데이터](https://github.com/Microsoft/Windows-tutorials-web/blob/master/Single-Page-App-with-REST-API/backend/api.json)를 정의하는 방법은 다음과 같습니다. 이를 사용하여 서버 코드 스텁을 생성할 것입니다.

1. 새 폴더(예를 들어 로컬 *GitHub* 디렉터리에)를 만들고, 기억력 게임 API 정의를 포함한 [**api.json**](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/api.json?token=ACEfklXAHTeLkHYaI5plV20QCGuqC31cks5ZFhVIwA%3D%3D) 파일을 다운로드합니다. 폴더 이름에 공백이 없는지 확인합니다.

2. 해당 폴더에 대해 즐겨찾는 셸([또는 Visual Studio Code의 통합된 터미널 사용](https://code.visualstudio.com/docs/editor/integrated-terminal))을 열고, [Yeoman](http://yeoman.io/)(yo) 코드 기반 도구와 글로벌(**-g**) 노드 환경을 위한 Swagger 생성기를 설치하기 위해 다음 NPM(노드 패키지 관리자) 명령을 실행합니다.

    ```
    npm install -g yo
    npm install -g generator-swaggerize
    ```

3. 이제 Swagger를 사용하여 서버 기반 코드를 생성할 수 있습니다.

    ```
    yo swaggerize
    ```

4. **swaggerize** 명령은 몇 가지 질문을 합니다.
    - swagger 문서 **api.json**에 대한 경로(또는 URL)
    - 프레임워크: **express**
    - 이 프로젝트를 부를 이름(YourFolderNameHere): **[입력]**

    NPM 패키지를 코드로 배포할 수 있도록 자신의 연락처 정보를 포함하여 *package.json* 파일을 제공하는 정보 등 필요한 다른 정보에도 대답합니다.

5. 마지막으로, 새 프로젝트와 [Swagger UI](http://swagger.io/swagger-ui/) 지원을 위한 모든 종속 기능(*package.json*에 나열)을 설치합니다.

    ```
    npm install
    npm install swaggerize-ui
    ```

    이제 VS Code를 시작하고 **파일** > **폴더 열기...** 를 선택한 다음 MemoryGameAPI 디렉터리로 이동합니다. 이는 여러분이 만든 Node.js API 서버입니다. 이는 인기 있는 [ExpressJS](http://expressjs.com/en/4x/api.html) 웹 응용 프로그램 프레임워크를 사용하여 프로젝트를 구성하고 실행합니다.

### <a name="2-customize-the-server-code-and-setup-debugging"></a>2. 서버 코드 사용자 지정 및 설정 디버깅

프로젝트 루트의 *server.js* 파일은 서버의 "주" 기능 역할을 합니다. VS 코드에서 이를 열고 다음 코드를 복사합니다. 생성된 코드에서 수정된 줄은 설명과 함께 주석이 추가되어 있습니다.

```javascript
'use strict';

var port = process.env.PORT || 8000; // Better flexibility than hardcoding the port

var Http = require('http');
var Express = require('express');
var BodyParser = require('body-parser');
var Swaggerize = require('swaggerize-express');
var SwaggerUi = require('swaggerize-ui'); // Provides UI for testing our API
var Path = require('path');

var App = Express();
var Server = Http.createServer(App);

App.use(function(req, res, next) {  // Enable cross origin resource sharing (for app frontend)
    res.header('Access-Control-Allow-Origin', '*');
    res.header('Access-Control-Allow-Methods', 'GET,PUT,POST,OPTIONS');
    res.header('Access-Control-Allow-Headers', 'Content-Type, Authorization, Content-Length, X-Requested-With');

    // Prevents CORS preflight request (for PUT game_guess) from redirecting
    if ('OPTIONS' == req.method) {
      res.send(200);
    }
    else {
      next(); // Passes control to next (Swagger) handler
    }
});

App.use(BodyParser.json());
App.use(BodyParser.urlencoded({
    extended: true
}));

App.use(Swaggerize({
    api: Path.resolve('./config/swagger.json'),
    handlers: Path.resolve('./handlers'),
    docspath: '/swagger'   //  Hooks up the testing UI
}));

App.use('/', SwaggerUi({    // Serves the testing UI from our base URL
  docs: '/swagger'          //
}));

Server.listen(port, function () {  // Starts server with our modfied port settings
 });
```

이제 이 코드로 서버를 실행할 시간입니다. 노드 디버깅을 위한 Visual Studio Code를 설정해 보겠습니다. **디버그** 패널 아이콘(Ctrl + Shift + D)을 선택하고 톱니바퀴 아이콘(launch.json 열기)을 선택한 다음, "configurations"을 다음과 같이 수정합니다.

```json
"configurations": [
    {
        "type": "node",
        "request": "launch",
        "name": "Launch Program",
        "program": "${workspaceRoot}/server.js"
    }
]
```

이제 F5를 누르고 브라우저에서 [http://localhost:8000](http://localhost:8000)을 엽니다. 이 페이지는 기억력 게임 API를 위한 Swagger UI를 엽니다. 여기에서 각 메서드에 대한 세부 정보와 입력 필드를 확장할 수 있습니다. API 호출 시도도 할 수 있지만, 응답에는 [Swagmock](https://www.npmjs.com/package/swagmock) 모듈에서 제공하는 실험 데이터만 포함됩니다. 이제 이러한 API를 실제 API로 만들기 위해 게임 논리를 추가할 시간입니다.

### <a name="3-set-up-your-route-handlers"></a>3. 경로 처리기 설정

Swagger 파일(config\swagger.json)은 처리기 파일(\handlers 내)로 정의되는 각 URL 경로 및 해당 경로에 대해 정의되는 각 메서드(예: **GET**, **POST**)를 처리기 파일 내의 `operationId`(함수)로 매핑하여 다양한 클라이언트 HTTP 요청을 처리하는 방법을 서버에 지시합니다.

다양한 클라이언트 요청을 데이터 모델에 전달하기 전에 프로그램의 이 계층에서 일부 입력 확인 기능을 추가할 것입니다. 다음을 다운로드(또는 복사 및 붙여넣기)합니다.

 - 이 [game.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/handlers/game.js?token=ACEfkvhw6BUnkeSsZgnzVe086T5WLwjfks5ZFhW5wA%3D%3D) 코드를 **handlers\game.js** 파일에
 - 이 [guess.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/handlers/guess.js?token=ACEfkswel02rHVr0e61bVsNdpv_i1Rtuks5ZFhXPwA%3D%3D) 코드를 **handlers\guess.js** 파일에
 - 이 [new.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/handlers/new.js?token=ACEfkgk2QXJeRc0aaLzY5ulFsAR4Juidks5ZFhXawA%3D%3D) 코드를 **handlers\new.js** 파일에

 이러한 파일의 주석을 통해 변경에 대한 자세한 내용을 간단히 볼 수 있지만, 기본적으로 이러한 파일은 기본 입력 오류(예를 들어 클라이언트가 하나 미만의 카드 쌍을 가진 요청을 보내는 경우)를 검사하고 필요한 경우 설명이 포함된 오류 메시지를 보냅니다. 또한 처리기는 후속 처리를 위해 해당 데이터 파일(\data 내)을 통해 유효한 클라이언트 요청을 보냅니다. 다음 과정에서 이를 사용해 보겠습니다.

### <a name="4-set-up-your-data-model"></a>4. 데이터 모델 설정

이제 자리 표시자 데이터 실험 서비스를 기억력 게임 보드의 실제 데이터 모델로 교체할 시기입니다.

프로그램의 이 계층은 기억력 카드 자체를 나타내며, 새 게임을 위한 카드의 "순서 섞기", 일치하는 카드 쌍 파악, 게임 상태의 지속적인 추적을 포함한 여러 게임 논리를 제공합니다. 다음을 복사하여 붙여넣습니다.

 - 이 [game.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/data/game.js?token=ACEfksAceJNQmhF82aHjQTx78jILYKfCks5ZFhX4wA%3D%3D) 코드를 **data\game.js** 파일에
 - 이 [guess.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/data/guess.js?token=ACEfkvY69Zr1AZQ4iXgfCgDxeinT21bBks5ZFhYBwA%3D%3D) 코드를 **data\guess.js** 파일에
 - 이 [new.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/data/new.js?token=ACEfkiqeDN0HjZ4-gIKRh3wfVZPSlEmgks5ZFhYPwA%3D%3D) 코드를 **data\new.js** 파일에

간소화를 위해, 게임 보드를 노드 서버의 글로벌 변수(`global.board`)에 저장합니다. 하지만 실제로는 Google [클라우드 데이터 저장소](https://cloud.google.com/datastore/) 또는 Azure [DocumentDB](https://azure.microsoft.com/en-us/services/documentdb/) 같은 클라우드 저장소를 사용하여 이 변수를 여러 게임과 플레이어를 동시에 지원하는 기억력 게임 API 서비스로 만들 것입니다.

VS 코드에 모든 변경 사항이 저장되었는지 확인하고 서버를 다시 시작(VS 코드에서 F5 또는 셸에서 `npm start`를 입력하고 [http://localhost:8000](http://localhost:8000) 검색)하여 게임 API를 테스트합니다.

**Try it out!** 단추를 **/game**, **/guess** 또는 **/new** 작업에서 누를 때마다 모든 작업이 기대한 대로 작동하는지 확인하는 아래의 **Response Body** 및 **Response Code** 결과 확인이 수행됩니다.

시도해 보세요. 

1. 새로운 `size=2` 게임을 만듭니다.

    ![Swagger UI에서 새 기억력 게임을 시작합니다.](images/swagger_new.png)

2. 몇 가지 값을 추측합니다.

    ![Swagger UI에서 카드를 추측합니다.](images/swagger_guess.png)

3. 게임 보드에서 게임 진행 상황을 확인합니다.

    ![Swagger UI에서 게임 상태를 확인합니다.](images/swagger_game.png)

모두 잘 작동한다면, API 서비스를 Azure에서 호스트할 준비가 된 것입니다! 문제에 발생한다면 \data\game.js의 다음 줄에 주석을 추가하세요.

```javascript
for (var i=0; i < board.length; i++){
    var card = {};
    card.cleared = board[i].cleared;

    // Only reveal cleared card values
    //if ("true" == card.cleared) {        // To debug the board, comment out this line
        card.value = board[i].value;
    //}                                    // And this line
    currentBoardState.push(card);
}
```

이 변경으로 인해 **GET /game** 메서드는 아직 클리어하지 않은 것을 포함하여 모든 카드 값을 반환합니다. 이는 기억력 게임을 위한 프런트 엔드를 제작할 때 유용한 디버그 방식입니다.

### <a name="5-optional-host-your-api-service-on-azure-and-enable-cors"></a>5. (선택 사항) Azure에서 API 서비스를 호스트하고 CORS 활성화

Azure 문서는 다음 내용을 설명합니다.

 - [Azure Portal에 새 *API 앱* 등록](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-rest-api#createapiapp)
 - [API 앱을 위한 Git 배포 설정](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-rest-api#deploy-the-api-with-git)
 - [Azure에 API 앱 코드 배포](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-rest-api#deploy-the-api-with-git)

앱을 등록할 때 *앱 이름*을 차별화하세요(*http://memorygameapi.azurewebsites.net* URL에서 다른 사용자의 변형 요청으로 인해 명명 충돌이 발생하지 않도록 하기 위해).

지금까지 진행했다면 Azure가 이제 swagger UI를 서비스하게 됩니다. 기억력 게임 백 엔드까지는 한 단계가 남았습니다. [Azure Portal](https://portal.azure.com)에서 새로 만든 *앱 서비스*를 선택하고 **CORS**(원본 간 리소스 공유) 옵션을 선택하거나 찾습니다. **허용된 원본** 아래에서 별표(`*`)를 추가하고 **저장**을 클릭합니다. 이렇게 하면 로컬 컴퓨터에서 개발하는 동안 기억력 게임 프런트 엔드의 API 서비스에서 원본 간 호출을 할 수 있습니다. 기억력 게임 프런트 엔드를 마무리하고 Azure에 배포하면, 이 항목을 웹앱의 특정 URL로 대체할 수 있습니다.

### <a name="going-further"></a>더 나아가기

기억력 게임 API를 프로덕션 앱을 위한 실행 가능한 백 엔드 서비스로 만들려면, 여러 플레이어와 게임을 지원하도록 코드를 확장해야 합니다. 이를 위해서는 [인증](http://swagger.io/docs/specification/authentication/)(플레이어 ID 관리용), [NoSQL 데이터베이스](https://docs.microsoft.com/en-us/azure/documentdb/)(게임과 플레이어 추적용) 및 API에 대한 몇 가지 기본 [단위 테스트](https://apigee.com/about/blog/developer/swagger-test-templates-test-your-apis)에 연결해야 합니다.

더 발전시키기 위한 유용한 몇 가지 리소스는 다음과 같습니다.

 - [Visual Studio Code를 사용한 고급 Node.js 디버깅](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)

 - [Azure 웹 + 모바일 문서](https://docs.microsoft.com/en-us/azure/#pivot=services&panel=web)

 - [Azure DocumentDB 문서](https://docs.microsoft.com/en-us/azure/documentdb/index)

## <a name="part-ii-build-a-single-page-web-application"></a>2 부: 단일 페이지 웹 응용 프로그램을 빌드

이제 1부에서 [REST API 백 엔드](#part-i-build-a-rest-api-backend)를 구축(또는 [다운로드](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/backend))했으므로, [노드](https://nodejs.org/en/), [Express](http://expressjs.com/) 및 [부트스크랩](http://getbootstrap.com/)으로 단일 페이지 기억력 게임 프런트 엔드를 만들 준비가 되었습니다.

이 자습서의 2부에서는 다음을 사용해 볼 것입니다. 

* [Node.js](https://nodejs.org/en/): 게임을 호스팅하는 서버 생성
* [jQuery](http://jquery.com/): JavaScript 라이브러리
* [Express](http://expressjs.com/): 웹 응용 프로그램 프레임워크용
* [Pug](https://pugjs.org/): (공식적으로 Jade) 템플릿 엔진용
* [부트스크랩](http://getbootstrap.com/): 반응형 레이아웃용
* [Visual Studio Code ](https://code.visualstudio.com/): 코드 작성, 마크다운 보기 및 디버깅용

### <a name="1-create-a-nodejs-application-by-using-express"></a>1. Express를 사용하여 Node.js 응용 프로그램 만들기

Express를 사용하여 Node.js 프로젝트 만들기를 시작합니다.

1. 명령 프롬프트를 열고 게임을 저장하려는 디렉터리로 이동합니다. 
2. Express generator에서 템플릿 엔진 *Pug*를 사용하여 *memory*라는 새 응용 프로그램을 만듭니다.

    ```
    express --view=pug memory
    ```

3. memory 디렉터리에서 package.json 파일에 나열된 종속 기능을 설치합니다. package.json 파일은 프로젝트의 루트에 만들어집니다. 이 파일에는 Node.js 앱에 필요한 모듈이 포함되어 있습니다.  

    ```
    cd memory
    npm install
    ```

    이 명령을 실행한 후에는 이 앱에 필요한 모든 모듈이 포함된 node_modules라는 폴더를 볼 수 있습니다. 

4. 이제 응용 프로그램을 실행합니다.

    ```
    npm start
    ```

5. [http://localhost:3000/](http://localhost:3000/)으로 이동하여 응용 프로그램을 확인합니다.

    ![http://localhost:3000/의 스크린샷](./images/express.png)

6. memory\routes 디렉터리의 index.js를 편집하여 웹앱의 기본 제목을 변경합니다. 줄 아래의 `Express`를 `Memory Game`(또는 선택한 다른 제목)으로 변경합니다.

    ``` javascript
    res.render('index', { title: 'Express' });
    ```

7. 앱을 새로 고치고 새 제목을 보려면, 명령 프롬프트에서 **Crtl + C**, **Y**를 눌러 앱을 중지하고 `npm start`로 다시 시작합니다.

### <a name="2-add-client-side-game-logic-code"></a>2. 클라이언트 쪽 게임 논리 코드 추가
자습서의 이 부분에 필요한 파일은 [Start](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/frontend/Start) 폴더에서 찾을 수 있습니다. 코드를 잃어버린 경우, [Final](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/frontend/Final) 폴더에서 완료된 코드를 찾을 수 있습니다. 

1. [Start](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/frontend/Start) 폴더 내의 scripts.js 파일을 복사하여 memory\public\javascripts에 붙여넣습니다. 이 파일은 다음을 포함하여 게임을 실행하는 데 필요한 모든 게임 논리가 포함되어 있습니다.

    * 새 게임을 만들고/시작합니다.
    * 서버에 저장된 게임을 복원합니다.
    * 사용자의 선택을 기반으로 게임 보드와 카드를 그립니다.
    * 카드를 뒤집습니다.

2. script.js에서 `newGame()` 함수를 수정하는 것으로 시작합니다. 이 함수는 다음 기능을 합니다.

    * 사용자의 게임 크기 선택을 처리합니다.
    * 서버에서 [게임 보드 배열](#part-i-build-a-rest-api-backend)을 가져옵니다.
    * 게임 보드를 화면에 배치하는 `drawGameBoard()` 함수를 호출합니다.

    `newGame()` 내의 `// Add code from Part 2.2 here` 주석 뒤에 다음 코드를 추가합니다.

    ``` javascript
    // extract game size selection from user
    var size = $("#selectGameSize").val();

    // parse game size value
    size = parseInt(size, 10);
    ```

    이 코드는 나중에 만들 `id="selectGameSize"` 드롭다운 메뉴에서 값을 가져와 변수 `size`에 저장합니다.  드롭다운의 문자열 값을 구문 분석하기 위해 [`parseInt()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt) 함수를 사용하여 정수를 반환하며, 요청된 게임 보드의 `size`를 서버로 전달합니다. 

    이 자습서의 1부에서 만든 [`/new`](#part-i-build-a-rest-api-backend) 메서드를 사용하여 선택한 크기의 게임 보드의 서버를 게시합니다. 이 메서드는 카드의 한 JSON 배열과 카드가 일치하는지 나타내는 `true/false` 값을 반환합니다. 

3. 다음은 마지막 플레이한 게임을 복원하는 `restoreGame()` 함수를 채웁니다. 간단한 진행을 위해 앱은 항상 마지막 플레이한 게임을 로드합니다. 서버에 저장된 게임이 없으면 드롭다운 메뉴를 사용하여 새 게임을 시작합니다. 

    이 코드를 복사하여 `restoreGame()`에 붙여넣습니다.

   ``` javascript 
   // reset the game
   gameBoardSize = 0;
   cardsFlipped = 0;

   // fetch the game state from the server 
   $.get("http://localhost:8000/game", function (response) {
       // store game board size
       gameBoardSize = response.length;

       // draw the game board
       drawGameBoard(response);
   });
   ```

    이제 게임은 서버에서 게임 상태를 가져오게 됩니다. 이 단계에서 사용 중인 [`/game`](#part-i-build-a-rest-api-backend) 메서드에 대한 자세한 내용은 이 자습서의 파트 1을 참조하세요. Azure(또는 다른 서비스)를 사용하여 백 엔드 API를 호스팅하고 있는 경우에는 위의 *localhost* 주소를 프로덕션 URL로 바꿉니다.

4. 이제 `drawGameBoard()` 함수를 만들 수 있습니다.  이 함수는 다음 기능을 합니다.

    * 게임의 크기를 감지하고 특정 CSS 스타일을 적용합니다.
    * HTML에서 카드를 생성합니다.
    * 페이지에 카드를 추가합니다.

    이 코드를 복사하여 *scripts.js*의 `drawGameBoard()` 함수에 붙여넣습니다.

    ``` javascript
    // create output
    var output = "";
    // detect board size CSS class
    var css = "";
    switch (board.length / 4) {
        case 1:
            css = "rows1";
            break;
        case 2:
            css = "rows2";
            break;
        case 3:
            css = "rows3";
            break;
        case 4:
            css = "rows4";
            break;   
    }
    // generate HTML for each card and append to the output
    for (var i = 0; i < board.length; i++) {
        if (board[i].cleared == "true") {
            // if the card has been cleared apply the .flip class
            output += "<div class=\"flipContainer col-xs-3 " + css + "\"><div class=\"cards flip matched\" id=\"" + i + "\" onClick=\"flipCard(this)\">\
                <div class=\"front\"><span class=\"glyphicon glyphicon-question-sign\"></span></div>\
                <div class=\"back\">" + lookUpGlyphicon(board[i].value) + "</div>\
                </div></div>";
        } else {
            output += "<div class=\"flipContainer col-xs-3 " + css + "\"><div class=\"cards\" id=\"" + i + "\" onClick=\"flipCard(this)\">\
                <div class=\"front\"><span class=\"glyphicon glyphicon-question-sign\"></span></div>\
                <div class=\"back\"></div>\
                </div></div>";
        }
    }
    // place the output on the page
    $("#game-board").html(output);
    ```

5. 다음은 `flipCard()` 함수를 완료해야 합니다.  이 함수는 이 자습서의 1부에서 개발한 [`/guess`](#part-i-build-a-rest-api-backend) 메서드를 사용하여 서버에서 선택된 카드의 값을 가져오는 것을 포함하는 게임 논리의 대부분을 처리합니다. REST API 백 엔드를 클라우드에서 호스트할 경우 *localhost* 주소를 프로덕션 URL로 바꾸는 것을 잊지 마세요.

    `flipCard()` 함수에서, 이 코드의 주석 처리를 제거합니다.

    ``` javascript
    // post this guess to the server and get this card's value
    $.ajax({
        url: "http://localhost:8000/guess?card=" + selectedCards[0],
        type: 'PUT',
        success: function (response) {
            // display first card value
            $("#" + selectedCards[0] + " .back").html(lookUpGlyphicon(response[0].value));

            // store the first card value
            selectedCardsValues.push(response[0].value);
        }
    });
    ```

> [!TIP] 
> Visual Studio Code를 사용하는 경우, 주석 처리를 제거하려는 코드의 모든 줄을 선택하고 Crtl + K, U를 누릅니다.

여기서 1부에서 만든 [`jQuery.ajax()`](http://api.jquery.com/jQuery.ajax/) 및 **PUT**[`/guess`](#part-i-build-a-rest-api-backend) 메서드를 사용합니다. 

이 코드는 다음 순서로 실행됩니다.

* 사용자가 선택한 첫 카드의 `id`가 selectedCards[] 배열의 첫 번째 값으로 추가됩니다. `selectedCards[0]` 
* `selectedCards[0]`의 값(`id`)이 [`/guess`](#part-i-build-a-rest-api-backend) 메서드를 사용하여 서버에 게시됩니다.
* 서버가 카드의 `value`(정수)로 응답합니다.
* [부트스트랩 문자 모양 아이콘](http://getbootstrap.com/components/)이 카드 뒷면에 추가되며 `id`: `selectedCards[0]`
* 첫 번째 카드의 `value`(서버에서 반환)가 `selectedCardsValues[]` 배열인 `selectedCardsValues[0]`에 저장됩니다. 

사용자의 두 번째 추측은 다음 논리를 따릅니다. 사용자가 선택한 카드의 ID가 동일한 경우(예: `selectedCards[0] == selectedCards[1]`) 카드가 일치하는 것입니다! CSS 클래스 `.matched`가 일치하는 카드(녹색으로 전환)에 추가되고 카드가 뒤집힌 채로 유지됩니다.

이제 사용자가 일치하는 모든 카드를 찾아 게임을 성공했는지 확인하는 논리를 추가합니다. `flipCard()` 함수 내 `//check if the user won the game` 주석 아래 다음 코드 줄을 추가합니다. 

``` javascript
if (cardsFlipped == gameBoardSize) {
    setTimeout(function () {
        output = "<div id=\"playAgain\"><p>You Win!</p><input type=\"button\" onClick=\"location.reload()\" value=\"Play Again\" class=\"btn\" /></div>";
        $("#game-board").html(output);
    }, 1000);
}   
```

뒤집힌 카드의 수가 게임 보드의 크기와 일치하는 경우(예를 들어, `cardsFlipped == gameBoardSize`), 뒤집을 카드가 없으며 사용자가 게임을 성공한 것입니다. `id="game-board"`인 간단한 몇 가지 HTML을 `div`에 추가하여 사용자가 게임을 완료했으며 다시 플레이할 수 있음을 알립니다.  

### <a name="3-create-the-user-interface"></a>3. 사용자 인터페이스 만들기 
이제 사용자 인터페이스를 만들어 이 모든 코드가 작동하는지 살펴보겠습니다. 이 자습서에서는 템플릿 엔진으로 [Pug](https://pugjs.org/)(공식적으로 Jade)를 사용합니다.  *Pug*는 HTML을 작성하기 위한 깔끔한, 공백을 구분하는 구문입니다. 예를 들면 다음과 같습니다. 

```
body
    h1 Memory Game
    #container
        p We love tutorials!
```

이 코드는 다음으로 변경됩니다.

``` html
<body>
    <h1>Memory Game</h1>
    <div id="container">
        <p>We love tutorials!</p>
    </div>
</body>
```


1. Start 폴더의 layout.pug 파일로 memory\views의 layout.pug 파일을 바꿉니다. layout.pug 내에 다음에 대한 링크가 표시됩니다.

    * 부트스크랩
    * jQuery
    * 사용자 지정 CSS 파일
    * 수정을 마친 JavaScript 파일

2. memory\views 디렉터리의 index.pug라는 이름의 파일을 엽니다.
이 파일은 게임을 렌더링하는 layout.pug 파일을 확장합니다. layout.pug 내에 다음 코드 줄을 붙여넣습니다.

    ```
    extends layout
    block content  
        div
            form(method="GET")
            select(id="selectGameSize" class="form-control" onchange="newGame()")
                option(value="0") New Game
                option(value="2") 2 Matches
                option(value="4") 4 Matches
                option(value="6") 6 Matches
                option(value="8") 8 Matches         
            #game-board
                script restoreGame();
    ```

> [!TIP] 
> Pug는 공백을 구분합니다. 모든 들여쓰기가 정확한지 확인하십시오!

### <a name="4-use-bootstraps-grid-system-to-create-a-responsive-layout"></a>4. 부트스크랩의 그리드 시스템을 사용하여 반응형 레이아웃 만들기
부트스크랩의 [그리드 시스템](http://getbootstrap.com/css/#grid)은 디바이스의 뷰포트 변경에 따라 그리드의 크기를 변경하는 유연한 그리드 시스템입니다. 이 게임의 카드는 다음을 포함하여 레이아웃에 부트스트랩의 미리 정의된 그리드 시스템 클래스를 사용합니다.
* `.container-fluid`: 그리드에 대한 유동 컨테이너 지정
* `.row-fluid`: 유동 행 지정
* `.col-xs-3`: 열 수 지정

부트스트랩의 그리드 시스템을 사용하면 그리드 시스템을 하나의 세로 열로 축소할 수 있습니다. 이는 모바일 디바이스의 탐색 메뉴에서 볼 수 있는 것과 같습니다.  하지만 게임에 항상 열이 있기를 원하므로, 그리드를 항상 가로로 유지하는 사전 정의된 클래스 `.col-xs-3`를 사용합니다. 

그리드 시스템은 최대 12개의 열을 허용합니다. 게임에 열을 4개만 사용할 것이므로 `.col-xs-3` 클래스를 사용합니다. 이 클래스는 각 열의 범위를 앞에서 언급한 12개 열 중 3개 열로 지정합니다. 이 이미지는 이 게임에 사용한 12열 그리드와 4열 그리드를 보여줍니다.

![12열, 4열로 구성된 부트스트랩 그리드](./images/grid.png)

1. scripts.js를 열고 `drawGameBoard()` 함수를 찾습니다.  각 카드에 대한 HTML을 생성하는 코드 블록에서 `class="col-xs-3"`인 `div` 요소를 찾을 수 있습니까? 

2. index.pug 내에 유동 레이아웃을 만들기 위해 앞서 언급한 미리 정의된 부트스트랩 클래스를 추가해 보겠습니다.  index.pug를 다음으로 변경합니다.

    ```
    extends layout

    block content   

        .container-fluid
            form(method="GET")
            select(id="selectGameSize" class="form-control" onchange="newGame()")
                option(value="0") New Game
                option(value="2") 2 Matches
                option(value="4") 4 Matches
                option(value="6") 6 Matches
                option(value="8") 8 Matches         
            #game-board.row-fluid 
                script restoreGame();
    ```

### <a name="5-add-a-card-flip-animation-with-css-transforms"></a>5. CSS 변환을 사용하여 카드 뒤집기 애니메이션 추가
memory\public\stylesheets의 style.css 파일을 Start 폴더의 style.css 파일로 바꿉니다.

카드가 진짜처럼 보이게 만드는 3D 뒤집기 동작을 표현하는 [CSS 변환](https://docs.microsoft.com/en-us/microsoft-edge/dev-guide/css/transforms)을 사용하여 뒤집기 동작을 추가합니다. 게임의 카드는 다음 HTML 구조를 사용하여 만들어지며, 프로그래밍 방식으로 게임 보드에 추가(앞서 본 `drawGameBoard()` 기능 내에)됩니다.

``` html
<div class="flipContainer">
    <div class="cards">
        <div class="front"></div>
        <div class="back"></div>
    </div>
</div>
```

1. 시작하려면 [perspective](https://developer.mozilla.org/en-US/docs/Web/CSS/transform)를 애니메이션의 부모 컨테이너(`.flipContainer`)에 제공합니다.  이렇게 하면 자식 요소의 깊이가 있다는 착시 현상이 생깁니다. 값이 더 높으면 사용자에게서 더 멀리 있는 것으로 보입니다. style.css의 `.flipContainer` 클래스에 다음 perspective를 추가합니다.

    ``` css
    perspective: 1000px; 
    ```

2. 이제 다음 속성을 style.css의 `.cards` 클래스에 추가합니다. `.cards` `div`는 카드의 앞면 또는 뒷면을 표시하는 뒤집기 애니메이션을 실제로 수행하는 요소입니다. 

    ``` css
    transform-style: preserve-3d;
    transition-duration: 1s;
    ```

    [`transform-style`](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-style) 속성은 3D 렌더링 컨텍스트와 `.cards` 클래스(`.front`와 `.back`는 3D 공간의 멤버임)를 구축합니다. [`transition-duration`](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-duration) 속성을 추가하면 애니메이션을 완료할 시간(초)를 지정합니다. 

3.  [`transform`](https://developer.mozilla.org/en-US/docs/Web/CSS/transform) 속성을 사용하여 카드를 Y축으로 회전할 수 있습니다.  다음 CSS를 `cards.flip`에 추가합니다.

    ``` css
    transform: rotateY(180deg);
    ```

    `cards.flip`에 정의된 스타일은 [`.toggleClass()`](http://api.jquery.com/toggleClass/)를 사용하여 `flipCard` 함수에서 켜지고 꺼집니다. 

    ``` javascript
    $(card).toggleClass("flip");
    ```

    이제 사용자가 카드를 클릭하면 카드가 180도 회전됩니다.

### <a name="6-test-and-play"></a>6. 테스트 및 플레이
축하합니다! 웹앱을 만들기가 완료되었습니다! 테스트해 보겠습니다. 

1. memory 디렉터리에서 명령 프롬프트를 열고 다음 명령을 입력합니다. `npm start`

2. 브라우저에서 [http://localhost:3000/](http://localhost:3000/)으로 이동하여 게임을 플레이합니다!

3. 오류가 발생할 경우, 키보드에서 F5를 누르고 `Node.js`를 입력하여 Visual Studio Code의 Node.js 디버깅 도구를 사용할 수 있습니다. Visual Studio Code의 디버깅에 대한 자세한 내용은 이 [문서](http://code.visualstudio.com/docs/editor/debugging#_launch-configurations)를 확인하세요. 

    작성한 코드를 최종 폴더의 코드와 비교할 수도 있습니다.

4. 게임을 중지하려면 명령 프롬프트에서 **Ctrl + C**, **Y**를 입력합니다. 

### <a name="going-further"></a>더 나아가기

이제 모바일, 태블릿, 데스크톱과 같은 서로 다른 디바이스 폼 팩터에서 테스팅하기 위해 앱을 Azure(또는 다른 클라우드 호스팅 서비스)에 배포할 수 있습니다. (다른 브라우저에서도 테스트하는 것을 잊지 마세요!) 앱이 프로덕션 준비가 되면 손쉽게 *유니버설 Windows 플랫폼*(UWP)을 위한 *호스트된 웹앱*(HWA)으로 패키징하여 Microsoft Store에서 배포할 수 있습니다.

Microsoft Store에 게시하는 기본 단계는 다음과 같습니다.

 1. [Windows 개발자](https://developer.microsoft.com/en-us/store/register) 계정을 만듭니다.
 2. 앱 제출 [검사 목록](https://docs.microsoft.com/en-us/windows/uwp/publish/app-submissions)을 사용합니다.
 3. [인증](https://msdn.microsoft.com/windows/uwp/publish/the-app-certification-process)을 위해 앱을 제출합니다.

더 발전시키기 위한 유용한 몇 가지 리소스는 다음과 같습니다.

 - [응용 프로그램 개발 프로젝트를 Azure 웹 사이트에 배포](https://docs.microsoft.com/azure/cosmos-db/documentdb-nodejs-application#_Toc395783182)

 - [UWP(유니버설 Windows 플랫폼) 앱으로 웹 응용 프로그램 변환](https://docs.microsoft.com/en-us/windows/uwp/porting/hwa-create-windows)

 - [Windows 앱 게시](https://developer.microsoft.com/en-us/store/publish-apps)
