---
title: JavaScript로 UWP 게임 만들기
description: 간단한 UWP JavaScript 및 createjs로 작성 된 Microsoft Store에 대 한 게임
author: GrantMeStrength
ms.author: jken
ms.date: 02/09/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 01af8254-b073-445e-af4c-e474528f8aa3
ms.localizationpriority: medium
ms.openlocfilehash: 597451826958c355dad9f9380dbdc1264bc87883
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7306796"
---
# <a name="create-a-uwp-game-in-javascript"></a>JavaScript로 UWP 게임 만들기

## <a name="a-simple-2d-uwp-game-for-the-microsoft-store-written-in-javascript-and-createjs"></a>JavaScript 및 CreateJS로 작성된 간단한 Microsoft Store용 2D UWP 게임


![Walking Dino 스프라이트 시트](images/JS2D_1.png)


## <a name="introduction"></a>소개


Microsoft Store에 앱을 게시 공유 (하거나 판매할!) 수백만 명의 사람들과 각종 디바이스.  

Microsoft Store에 앱을 게시 하려면 UWP (유니버설 Windows 플랫폼) 앱으로 작성 해야 합니다. 한편 UWP는 매우 유연하며 다양한 언어와 프레임워크를 지원합니다. 이를 입증하듯, 이 샘플은 JavaScript로 작성된 간단한 게임으로 여러 CreateJS 라이브러리를 사용하며, 스프라이트를 그리고 게임 루프를 만들고 키보드와 마우스를 지원하고 다양한 화면 크기에 맞게 조정하는 방법을 보여줍니다.

이 프로젝트는 Visual Studio를 사용하여 JavaScript로 빌드되었습니다. 몇 가지 사소한 변경 작업을 수행하면 웹 사이트에 호스트하거나 다른 플랫폼에 맞게 조정할 수도 있습니다. 

**참고:** 이 완전 한 (또는 뛰어난) 게임이 아니며, JavaScript 및 제 3 자 라이브러리를 사용 하 여 Microsoft Store에 게시할 수 있는 앱을 만드는 방법을 보여주기 위해 설계 되었습니다.


## <a name="requirements"></a>요구 사항

이 프로젝트를 플레이하려면 다음이 필요합니다.

* Windows 10 최신 버전을 실행하는 Windows 컴퓨터(또는 가상 컴퓨터).
* Visual Studio 복사본. 무료 Visual Studio Community Edition은 [Visual Studio 홈 페이지](http://visualstudio.com)에서 다운로드할 수 있습니다.

이 프로젝트는 CreateJS JavaScript 프레임워크를 사용합니다. CreateJS는 MIT 라이선스를 기반으로 출시된 도구 모음으로 스프라이트 기반 게임을 간편하게 만들 수 있도록 설계되었습니다. CreateJS 라이브러리는 이미 프로젝트에 있습니다(솔루션 탐색기 보기에서 *js/easeljs-0.8.2.min.js* 및 *js/preloadjs-0.6.2.min.js* 검색). CreateJS에 대한 자세한 내용은 [CreateJS 홈 페이지](http://www.createjs.com)에서 확인할 수 있습니다.


## <a name="getting-started"></a>시작

앱의 전체 소스 코드는 [GitHub](https://github.com/Microsoft/Windows-appsample-get-started-js2d)에 저장되어 있습니다.

시작하는 가장 간단한 방법은 GitHub를 방문하여 **복제 또는 다운로드** 단추를 클릭하고 **Visual Studio에서 열기**를 선택하는 것입니다. 

![리포지토리 복제](images/JS2D_2.png)

프로젝트를 zip 파일로 다운로드하거나 다른 표준 방법으로 [GitHub 프로젝트](https://msdn.microsoft.com/en-us/windows/uwp/get-started/get-uwp-app-samples)를 작업할 수도 있습니다.

솔루션이 Visual Studio에 로드되면 다음을 포함한 여러 파일이 표시됩니다.

* 이미지/ - UWP 앱에 필요한 여러 아이콘과 게임의 SpriteSheet 및 기타 비트맵이 들어 있는 폴더
* Js/ - JavaScript 파일이 들어 있는 폴더. main.js 파일은 게임이고, 기타 파일은 EaselJS 및 PreloadJS입니다.
* Index.html - 게임 그래픽을 호스트하는 캔버스 개체가 들어 있는 웹 페이지.

이제 게임을 실행할 수 있습니다!

**F5** 키를 눌러 앱을 실행합니다. 창이 열리고 목가적인 () 경우 가로에 낯익은 공룡이 서 보일 것입니다. 이제 계속 진행하면서 앱을 검사하고, 몇 가지 중요한 부분에 대해 설명하고, 나머지 기능의 잠금을 해제합니다.

![닌자 고양이를 등에 업고 있는 평범한 공룡](images/JS2D_3.png)

**참고:** 문제가 있나요? 웹 지원과 함께 Visual Studio가 설치되어 있어야 합니다. 새 프로젝트를 만들어서 검사할 수 있습니다. JavaScript가 지원되지 않는 경우 Visual Studio를 다시 설치하고 *Microsoft Web 개발자 도구* 상자를 검사해야 합니다.

## <a name="walkthough"></a>연습

F5 키로 게임을 시작했으면 이제 어떻게 될지 궁금하실 것입니다. 및 많은 코드는 현재 주석으로 "별로 없습니다", 됩니다. 지금까지 공룡 그리고 스페이스바를 눌러야 글자만 무성의 모두 표시 됩니다. 

### <a name="1-setting-the-stage"></a>1 스테이지 설정

**index.html**을 열어서 살펴보면 거의 비어 있습니다. 이 파일은 앱이 들어 있는 기본 웹 페이지이며, 두 가지 중요한 역할만 수행합니다. 첫째, **EaselJS** 및 **PreloadJS** CreateJS 라이브러리의 JavaScript 소스 코드와 **main.js**(이 예제의 소스 코드 파일)를 포함합니다.
둘째, 모든 그래픽이 표시되는 &lt;canvas&gt; 태그를 정의합니다. &lt;canvas&gt;는 표준 HTML5 문서 구성 요소입니다. **main.js**로 작성된 코드에서 참조할 수 있도록 gameCanvas라는 이름을 붙이겠습니다. JavaScript 게임을 처음부터 새로 만드는 경우에는 **EaselJS** 및 **PreloadJS** 파일을 솔루션으로 복제한 다음 캔버스 개체를 만들어야 합니다.

EaselJS는 *스테이지*라고 하는 새로운 개체를 제공합니다. 스테이지는 캔버스에 연결되고 이미지와 텍스트를 표시하는 데 사용됩니다. 다음과 같이 스테이지에 표시하려는 개체를 스테이지의 자식으로 추가해야 합니다.

```
    stage.addChild(myObject);
```

이 코드 줄이 **main.js**에 여러 차례 나타날 것입니다.

말이 나온 김에 지금 **main.js**를 열도록 하겠습니다.

### <a name="2-loading-the-bitmaps"></a>2. 비트맵 로드

EaselJS는 여러 종류의 그래픽 개체를 제공합니다. 하늘에 사용되는 파란색 사각형 같은 간단한 셰이프, 잠시 후 우리가 추가할 구름 같은 비트맵, 텍스트 개체 그리고 스프라이트를 만들 수 있습니다. 스프라이트 사용 (SpriteSheet) [http://createjs.com/docs/easeljs/classes/SpriteSheet.html]: 여러 이미지가 들어 있는 단일 비트맵인 합니다. 예를 들어 이 SpriteSheet를 사용하여 공룡 애니메이션의 여러 프레임을 저장합니다.

![Walking Dino 스프라이트 시트](images/JS2D_4.png)

이 코드에서 여러 프레임과 프레임 애니메이션 속도를 정의하여 공룡이 걷는 동작을 구현합니다.

```
    // Define the animated dino walk using a spritesheet of images,
    // and also a standing-still state, and a knocked-over state.
    var data = {
        images: [loader.getResult("dino")],
        frames: { width: 373, height: 256 },
        animations: {
            stand: 0,
            lying: { 
                frames: [0, 1],
                speed: 0.1
            },
            walk: {
                frames: [0, 1, 2, 3, 2, 1],
                speed: 0.4
            }
        }
    }

    var spriteSheet = new createjs.SpriteSheet(data);
    dino_walk = new createjs.Sprite(spriteSheet, "walk");
    dino_stand = new createjs.Sprite(spriteSheet, "stand");
    dino_lying = new createjs.Sprite(spriteSheet, "lying");

```

이제 스테이지에 솜털 같은 작은 구름을 추가하겠습니다. 게임이 실행되면 구름이 화면을 떠 다닙니다. 구름 이미지는 이미 솔루션의 *images* 폴더에 있습니다.

**init()** 함수를 찾을 때까지 **main.js**를 찾아봅니다. 이 함수는 게임이 시작될 때 호출되며 모든 그래픽 개체를 여기서 설정합니다.

다음 코드를 찾아서 구름 이미지를 참조하는 줄에서 주석(\\)을 제거합니다.

```
 manifest = [
        { src: "walkingDino-SpriteSheet.png", id: "dino" },
        { src: "barrel.png", id: "barrel" },
        { src: "fluffy-cloud-small.png", id: "cloud" },
    ];
```

JavaScript는 이미지 같은 리소스를 로드할 때 약간의 도움이 필요합니다. 따라서 이미지를 미리 로드할 수 있는 CreateJS 라이브러리의 기능인 [LoadQueue](http://www.createjs.com/docs/preloadjs/classes/LoadQueue.html)를 사용하겠습니다. 이미지가 로드될 때까지 시간이 얼마나 걸릴지 알 수 없기 때문에 LoadQueue를 사용하여 처리하겠습니다. 이미지를 사용할 수 있게 되면 큐가 그 사실을 알려줄 것입니다. 이렇게 만들기 위해 먼저 모든 이미지를 나열하는 새 개체를 만든 후 LoadQueue 개체를 만듭니다. 아래 코드를 보시면 모든 준비가 완료되면 **loadingComplete()** 함수를 호출하도록 설정하는 방법을 알 수 있습니다.

```
    // Now we create a special queue, and finally a handler that is
    // called when they are loaded. The queue object is provided by preloadjs.

    loader = new createjs.LoadQueue(false);
    loader.addEventListener("complete", loadingComplete);
    loader.loadManifest(manifest, true, "../images/");
```    

**loadingComplete()** 함수가 호출되면 이미지가 로드되어 사용할 수 있게 됩니다. 구름을 만드는 주석 처리된 섹션이 보이고, 이제 그 비트맵을 사용할 수 있습니다. 주석을 제거하면 다음과 같이 표시됩니다.

```
    // Create some clouds to drift by..
    for (var i = 0; i < 3; i++) {
        cloud[i] = new createjs.Bitmap(loader.getResult("cloud"));
        cloud[i].x = Math.random()*1024; // Random start location
        cloud[i].y = 64 + i * 48;
        stage.addChild(cloud[i]);
    }
```
이 코드는 각각 미리 로드된 이미지를 사용하는 세 개의 구름 개체를 만들고, 위치를 정의한 다음 스테이지에 추가합니다.

F5 키를 눌러 앱을 다시 실행하면 구름이 표시된 것을 볼 수 있습니다.

### <a name="3-moving-the-clouds"></a>3. 구름 이동

이번에는 구름을 움직여 보겠습니다. 구름을 포함하여 어떤 물체를 움직이는 방법은 1초에 여러 차례 반복해서 호출되는 [ticker](http://www.createjs.com/docs/easeljs/classes/Ticker.html) 함수입니다. 이 함수는 호출될 때마다 약간 다른 위치에 그래픽을 다시 그립니다.

<p data-height="500" data-theme-id="23761" data-slug-hash="vxZVRK" data-default-tab="result" data-user="MicrosoftEdgeDocumentation" data-embed-version="2" data-pen-title="CreateJS - Animating clouds" data-preview="true" data-editable="true" class="codepen"><a href="http://codepen.io">CodePen</a>에서 Pen <a href="https://codepen.io/MicrosoftEdgeDocumentation/pen/vxZVRK/">CreateJS - Animating clouds</a> by Microsoft Edge Docs(<a href="http://codepen.io/MicrosoftEdgeDocumentation">@MicrosoftEdgeDocumentation</a>)를 참조하세요.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>
 
이 작업을 수행하는 코드는 EaselJS이며 CreateJS 라이브러리에서 제공하는 **main.js** 파일에 이미 있습니다. 다음과 같이 표시됩니다.

```
    // Set up the game loop and keyboard handler.
    // The keyword 'tick' is required to automatically animated the sprite.
    createjs.Ticker.timingMode = createjs.Ticker.RAF;
    createjs.Ticker.addEventListener("tick", gameLoop);
```

이 코드는 초당 30~60프레임 사이에서 **gameLoop()** 함수를 호출합니다. 정확한 속도는 컴퓨터 속도에 따라 다릅니다.

**gameLoop()** 함수를 찾아서 끝까지 내리면 **animateClouds()** 함수가 보일 것입니다. 주석 처리되지 않도록 이 함수를 편집합니다.

```
    // Move clouds
    animateClouds();
```

이 함수의 정의를 보면 이 함수가 어떻게 각 구름을 순서대로 이동하고 구름의 x 좌표를 변경하는지 알 수 있습니다. x 좌표가 화면 밖에 있으면 맨 오른쪽으로 다시 설정됩니다. 또한 각 구름은 약간씩 다른 속도로 이동합니다.

```
function animate_clouds()
{
    // Move the cloud sprites across the sky. If they get to the left edge, 
    // move them over to the right.

    for (var i = 0; i < 3; i++) {    
        cloud[i].x = cloud[i].x - (i+1);
        if (cloud[i].x < -128)
            cloud[i].x = width + 128;
    }
}
```

이제 앱을 실행하면 구름이 움직이는 것을 볼 수 있습니다. 드디어 동작을 구현했습니다.

### <a name="4-adding-keyboard-and-mouse-input"></a>4. 키보드 및 마우스 입력 추가

사용자가 조작할 수 없는 게임은 게임이 아닙니다. 플레이어가 키보드나 마우스로 무언가를 할 수 있게 만들어 보겠습니다. **loadingComplete()** 함수로 돌아가면 다음과 같이 표시될 것입니다. 주석을 제거합니다.

```
    // This code will call the method 'keyboardPressed' is the user presses a key.
    this.document.onkeydown = keyboardPressed;

    // Add support for mouse clicks
    stage.on("stagemousedown", mouseClicked);
```

이제 플레이어가 키를 누르거나 마우스를 클릭할 때마다 두 개의 함수가 호출됩니다. 두 이벤트 모두 **userDidSomething()** 함수를 호출합니다. 이 함수는 gamestate 변수를 살펴보고 현재 게임 상황을 파악한 다음 그 이후에 이어져야 하는 일을 결정하는 함수입니다.

Gamestate는 게임에 일반적으로 사용되는 디자인 패턴입니다. 발생하는 모든 일은 티커 타이머가 호출하는 **gameLoop()** 함수에서 발생합니다. gameLoop() 함수는 게임이 진행 중인지, "game over state"인지, "ready-to-play state"인지 또는 작성자가 변수를 사용하여 정의한 그 밖의 상태인지를 추적합니다. 이 상태 변수는 switch 문에서 테스트되며, 테스트 결과에 따라 호출할 다른 함수가 정의됩니다. 상태가 "playing"으로 설정되면 공룡을 점프하게 하고 술통을 움직이는 함수가 호출됩니다. 공룡이 어떤 이유로 사망하면 gamestate 변수가 "game over state"로 설정되고 "Game over!" 메시지가 대신 표시됩니다. 게임 디자인 패턴에 관심이 있는 분들은 [게임 프로그래밍 패턴](http://gameprogrammingpatterns.com/)이라는 책을 읽어보시기 바랍니다. 유용한 내용이 아주 많습니다.

앱을 다시 실행하면 드디어 게임을 플레이할 수 있습니다. 스페이스바를 누르면(또는 마우스를 클릭하거나 화면을 탭하면) 게임이 시작됩니다. 

술통이 굴러 오는 것을 볼 수 있습니다. 적절한 타이밍에 스페이스바를 누르거나 마우스를 다시 클릭하면 공룡이 점프합니다. 타이밍을 잘못 맞추면 게임이 끝납니다.

술통은 구름과 똑같은 방식으로 애니메이션되며(매번 점점 빨라지기는 하지만), 공룡과 술통이 충돌하지 않도록 공룡과 술통의 위치를 확인해야 합니다.

```
 // Very simple check for collision between dino and barrel
                if ((barrel.x > 220 && barrel.x < 380)
                    &&
                    (!jumping))
                {
                    barrel.x = 380;
                    GameState = GameStateEnum.GameOver;
                }
```

만약 공룡이 점프를 하지 않았고 술통이 가까이에 있으면 코드가 상태 변수를 *GameOver* 상태로 변경합니다. 짐작하시겠지만 *GameOver*는 게임을 종료합니다.

이렇게 이 게임의 주요 메커니즘이 완성되었습니다.

### <a name="5-resizing-support"></a>5. 크기 조정 지원

게임이 거의 완성되었습니다! 하지만 마지막으로 처리할 까다로운 문제가 하나 남았습니다. 게임을 실행하고 창 크기를 조정해 보세요. 개체가 있어야 할 위치를 벗어나면서 게임이 순식간에 엉망이 됩니다. 플레이어가 창 크기를 조정할 때 또는 디바이스를 가로 방향에서 세로 방향으로 회전할 때 생성되는 창 크기 조정 이벤트 처리기를 만들어서 이 문제를 해결할 수 있습니다.

이 작업을 수행하는 코드가 이미 있습니다(사실, 게임이 처음으로 시작될 때 기본 창 크기가 정상적으로 작동하도록 보장하기 위해 이 코드가 호출됩니다. 왜냐하면 UWP 앱이 실행될 때 창 크기가 어떻게 될지 확신할 수 없기 때문입니다).

화면 크기 이벤트가 발생하면 함수를 호출하도록 이 줄의 주석을 제거합니다.

```
    // This code makes the app call the method 'resizeGameWindow' if the user resizes the current window.
     window.addEventListener('resize', resizeGameWindow);
```

이제 앱을 다시 실행하면 창 크기를 조절할 수 있으며 보다 나아진 결과를 얻을 수 있습니다.

## <a name="publishing-to-the-microsoft-store"></a>Microsoft Store에 게시

(먼저 개선 가정!) Microsoft Store에 게시할 수는 UWP 앱을 만들었으니 이제 

몇 가지 단계를 처리해야 합니다.

1. Windows 개발자로 [등록](https://developer.microsoft.com/en-us/store/register)해야 합니다.
2. 앱 제출 [검사 목록](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)을 사용해야 합니다.
3. 앱을 제출하여 [인증](https://msdn.microsoft.com/windows/uwp/publish/the-app-certification-process)을 받아야 합니다.

자세한 내용은 [게시 하는 UWP 앱을](https://developer.microsoft.com/en-us/store/publish-apps)참조 하세요.

## <a name="suggestions-for-other-features"></a>다른 기능에 대한 제안.

그 다음에 무엇을 해야 합니까? 여러분의 앱에 다음과 같은 기능을 추가하면 더욱 좋을 것 같습니다.

1. 사운드 효과. CreateJS 라이브러리는 [SoundJS](http://www.createjs.com/soundjs)라고 하는 라이브러리와 함께 사운드 지원을 포함하고 있습니다.
2. 게임 패드 지원. [API](https://gamedevelopment.tutsplus.com/tutorials/using-the-html5-gamepad-api-to-add-controller-support-to-browser-games--cms-21345)가 제공됩니다.
3. 좀 더 멋진 게임을 만들어보세요! 이 부분은 개발자에게 달렸지만, 온라인에 수많은 리소스가 제공됩니다. 

## <a name="other-links"></a>기타 링크

* [JavaScript로 간단한 Windows 게임 만들기](https://www.sitepoint.com/creating-a-simple-windows-8-game-with-javascript-game-basics-createjseaseljs/)
* [HTML/JS 게임 엔진 선택](https://html5gameengine.com/)
* [JS 기반 게임에 CreateJS 사용](https://blogs.msdn.microsoft.com/cbowen/2012/09/19/using-createjs-in-your-javascript-based-windows-8-game/)
* [LinkedIn Learning의 게임 개발 과정](https://www.linkedin.com/learning/topics/game-development)
