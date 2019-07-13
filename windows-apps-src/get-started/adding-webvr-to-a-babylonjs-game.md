---
title: 3D Babylon.js 게임에 WebVR 지원 추가
description: 기존 3D Babylon.js 게임에 WebVR 지원을 추가하는 방법을 알아보세요.
ms.date: 11/29/2017
ms.topic: article
keywords: webvr, edge, 웹 개발, babylon, babylonjs, babylon.js, javascript
ms.localizationpriority: medium
ms.openlocfilehash: 5f212e4e06035134b0ac5b5ea69381ed0d985783
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321159"
---
# <a name="adding-webvr-support-to-a-3d-babylonjs-game"></a>3D Babylon.js 게임에 WebVR 지원 추가

Babylon.js로 3D 게임을 만들었고 가상 현실(VR)에서 멋지게 보이도록 만들려면, 이 자습서의 간단한 단계를 따라 현실성을 높이세요.

여기에 표시된 게임에 WebVR 지원을 추가할 것입니다. 이대로 진행하여 Xbox 컨트롤러에서 시도해 보세요!


<iframe height='300' scrolling='no' title='Babylon.GUI를 사용하는 Babylon.js dino 게임' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/wrOvoj/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen</a>에서 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/wrOvoj/'>Babylon.GUI를 사용하는 Babylon.js dino 게임</a> by Microsoft Edge Docs(<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>)를 참조하세요.
</iframe>

이 게임은 평면 화면에서 잘 작동하는 3D 게임입니다. 하지만 VR에 대해서는 어떨까요?
이 자습서에서는 이 게임이 WebVR에서 실행되기 위한 몇 가지 단계를 살펴보겠습니다. 이를 위해 Microsoft Edge에서 WebVR을 위한 지원을 추가하는 [Windows 혼합 현실](https://developer.microsoft.com/mixed-reality) 헤드셋을 활용할 것입니다. 게임에 이러한 변경 사항을 적용하면 WebVR을 지원하는 다른 브라우저/헤드셋 조합에서도 작동할 것으로 기대할 수 있습니다.



## <a name="prerequisites"></a>필수 구성 요소

- 텍스트 편집기(예: [Visual Studio Code](https://code.visualstudio.com/download))
- 컴퓨터에 연결되어 있는 Xbox 컨트롤러
- Windows 10 크리에이터스 업데이트
- [Windows Mixed Reality를 실행할 수 있는 최소 사양](https://developer.microsoft.com/en-us/windows/mixed-reality/immersive_headset_setup)을 갖춘 컴퓨터
- Windows Mixed Reality 디바이스(선택 사항) 



## <a name="getting-started"></a>시작

시작하는 가장 간단한 방법은 [Windows-tutorials-web GitHub repo](https://github.com/Microsoft/Windows-tutorials-web)를 방문하여 녹색 **복제 또는 다운로드** 단추를 누르고 **Visual Studio에서 열기**를 선택하는 것입니다.

![복제 또는 다운로드 단추](images/3dclone.png)

프로젝트를 복제하는 대신 zip 파일로 다운로드할 수도 있습니다.
이렇게 하면 [before](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before)와 [after](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/after)의 두 폴더가 생깁니다. "before" 폴더는 VR 기능을 추가하기 전의 게임이며, "after" 폴더는 VR 지원이 완료된 상태의 게임입니다.

before와 after 폴더는 이러한 파일을 포함합니다.
-   **textures/** - 게임에 사용되는 모든 이미지를 포함하는 폴더
-   **css/** - 게임의 CSS를 포함하는 폴더
-   **js/** - JavaScript 파일이 들어 있는 폴더 main.js 파일은 게임이고, 다른 파일은 사용되는 라이브러리입니다.
-   **models/** - 3D 모델을 포함하는 폴더. 이 게임의 경우 공룡 모델 하나만 있습니다.
-   **index.html** - 게임의 렌더러를 호스트하는 웹 페이지. Microsoft Edge에서 이 페이지를 열면 게임이 시작됩니다.

Microsoft Edge에서 각각의 index.html 파일을 열어 양쪽 버전의 게임을 테스트할 수 있습니다.



## <a name="the-mixed-reality-portal"></a>Mixed Reality 포털

Windows Mixed Reality에 친숙하지 않고 호환되는 그래픽 카드가 있는 Windows 10 크리에이터 업데이트가 설치된 컴퓨터가 있는 경우 Windows 10의 시작 메뉴에서 **Mixed Reality 포털** 앱을 열어 보세요.

![Mixed Reality 포털 검색](images/mixed-reality-portal.png)

모든 필수 요구 사항을 충족하는 경우, 개발자 기능을 켜고 컴퓨터에 연결된 Windows Mixed Reality 헤드셋을 시뮬레이트할 수 있습니다. 실제 헤드셋이 준비되어 있다면, 연결하고 설치 프로그램을 실행합니다.

> [!IMPORTANT]
> 이 자습서를 진행하는 동안 Mixed Reality 포털이 열려 있어야 합니다.

이제 Microsoft Edge를 사용한 WebVR을 체험할 준비가 되었습니다.

## <a name="2d-ui-in-a-virtual-world"></a>가상 세계의 2D UI

>[!NOTE]
> 시작 샘플을 다운로드하려면 [**before**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) 폴더를 다운로드합니다.

[Babylon.GUI](https://doc.babylonjs.com/how_to/gui)는 VR 친화적 라이브러리로, VR 및 비 VR 디스플레이에서 잘 작동하는 간단한 대화형 사용자 인터페이스를 사용할 수 있도록 지원합니다.
Babylon.js의 확장인 `GUI` 라이브러리는 샘플 전체에서 2D 요소를 만드는 데 사용합니다.


2D 텍스트 `GUI` 요소는 조정하려는 특성 수에 따라 줄 몇 개만으로도 만들 수 있습니다.
다음 코드 조각은 이미 [**before**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) 샘플에 들어 있지만 어떤 작업이 진행되는지 살펴보겠습니다.
먼저 GUI가 표시될 영역을 설정하는 [`AdvancedDynamicTexture`](https://doc.babylonjs.com/how_to/gui#advanceddynamictexture) 개체를 만들어 보겠습니다. 이 샘플은 이 개체를 `CreateFullScreenUI()`로 설정합니다. 이것은 UI가 전체 화면에 표시됨을 의미합니다. `AdvancedDynamicTexture`가 만들어지면 `GUI.Rectanlge()` 및 `GUI.TextBlock()`을 사용하여 게임을 시작할 때 표시되는 2D 입력란을 만듭니다.


이 코드는 [**main.js**](https://github.com/Microsoft/Windows-tutorials-web/blob/master/BabylonJS-game-with-WebVR/before/js/main.js#L157-L168) 내에 추가됩니다.
```javascript
// GUI
var advancedTexture = BABYLON.GUI.AdvancedDynamicTexture.CreateFullscreenUI("UI");

// Start UI
startUI = new BABYLON.GUI.Rectangle("start");
startUI.background = "black"
startUI.alpha = .8;
startUI.thickness = 0;
startUI.height = "60px";
startUI.width = "400px";
advancedTexture.addControl(startUI); 
var tex2 = new BABYLON.GUI.TextBlock();
tex2.text = "Stay away from the dinosaur! \n Plug in an Xbox controller and press A to start";
tex2.color = "white";
startUI.addControl(tex2); 
```


이 UI는 일단은 화면에 표시되도록 생성되지만 게임에서 진행되는 상황에 따라 `isVisible`을 사용하여 설정 또는 해제할 수 있습니다.
```javascript
startUI.isVisible = false;
```



## <a name="detecting-headsets"></a>헤드셋 감지

VR 애플리케이션에서는 두 가지 종류의 카메라를 준비하여 여러 시나리오를 지원하도록 하는 것이 좋은 방법입니다. 이 게임의 경우 작동하는 헤드셋이 필요한 하나의 카메라를 지원하고, 헤드셋을 사용하지 않는 다른 하나를 지원할 것입니다. 게임에서 어떤 것을 사용할지 판단하려면, 헤드셋이 감지되었는지 먼저 확인해야 합니다. 이 작업을 수행하기 위해 [`navigator.getVRDisplays()`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getVRDisplays)를 사용해 보겠습니다.


이 코드를 **main.js**에서 `window.addEventListener('DOMContentLoaded')` 위에 추가합니다.
```javascript
var headset;
// If a VR headset is connected, get its info
navigator.getVRDisplays().then(function (displays) {
    if (displays[0]) {
        headset = displays[0];
    }
});
```

`headset` 변수에 저장된 정보를 사용하여, 이제 사용자에게 맞는 카메라를 선택할 수 있습니다.


## <a name="creating-and-selecting-the-initial-camera"></a>초기 카메라 만들기 및 선택

Babylon.js를 통해, [`WebVRFreeCamera`](https://doc.babylonjs.com/api/classes/babylon.webvrfreecamera)를 사용하여 WebVR을 신속하게 추가할 수 있습니다. 이 카메라는 키보드 입력을 받을 수 있으며, VR 헤드셋으로 "머리"의 회전을 제어할 수 있습니다.


### <a name="step-1-checking-for-headsets"></a>1단계: 헤드셋 확인

대체 카메라를 위해, 본래 게임에 현재 사용되는 [`UniversalCamera`](https://doc.babylonjs.com/api/classes/babylon.universalcamera)를 사용합니다.

`WebVRFreeCamera` 카메라를 사용할 수 있는지 확인하기 위해 `headset` 변수를 확인할 것입니다.

다음 코드로 `camera = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);`을 교체합니다.
```javascript
        if(headset){
            // Create a WebVR camera with the trackPosition property set to false so that we can control movement with the gamepad
            camera = new BABYLON.WebVRFreeCamera("vrcamera", new BABYLON.Vector3(0, 14, 0), scene, true, { trackPosition: false });
            camera.deviceScaleFactor = 1;
        } else {
            // No headset, use universal camera
            camera = new BABYLON.UniversalCamera("camera", new BABYLON.Vector3(0, 18, -45), scene);
        }
```


### <a name="step-2-activating-the-webvrfreecamera"></a>2단계: WebVRFreeCamera 활성화
대부분의 브라우저에서 카메라를 활성화하려면 사용자가 가상 환경을 요청하는 몇 가지 상호 작용을 수행해야 합니다.
이 기능을 마우스 클릭에 연결하겠습니다.


코드를 `createScene()` 함수의 `camera.applyGravity = true;` 후에 붙여넣습니다.
```javascript
        scene.onPointerDown = function () {
            scene.onPointerDown = undefined
            camera.attachControl(canvas, true);
        }
```

이제 게임에서 마우스를 클릭하면 다음과 같은 메시지가 표시되거나, 사용자가 이미 메시지를 수락했다면 헤드셋에 게임을 바로 표시합니다.

![몰입형 메시지](images/immersiveview.png)

`WebVRFreeCamera`로 전환하기 전에 `UniversalCamera` 보기를 표시하는 코드 조각을 추가하여 사용자에게 파란 창 대신 게임이 표시되도록 할 수도 있습니다. 

`engine.runRenderLoop(function () {` 뒤에 다음을 추가합니다.
```javascript
            if (headset) {
                if (!(headset.isPresenting)) {
                    var camera2 = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);
                    scene.activeCamera = camera2;
                } else {
                    scene.activeCamera = camera;
                }
            }
```

### <a name="step-3-adding-gamepad-support"></a>3단계: 게임 패드 지원 추가

`WebVRFreeCamera`는 처음부터 게임 패드를 지원하지는 않기 때문에, 게임 패드 단추를 키보드 화살표에 매핑할 것입니다. 이는 카메라의 `inputs` 속성을 통해 수행됩니다. 왼쪽 아날로그 스틱을 위로, 아래로, 왼쪽으로, 오른쪽으로 움직이는 것을 화살표 키에 매핑하는 코드를 추가함으로써, 게임 패드가 작동하게 됩니다.


이 코드를 `scene.onPointerDown = function() {...}` 호출 아래에 추가합니다.
``` javascript
    // Custom input, adding Xbox controller support for left analog stick to map to keyboard arrows
    camera.inputs.attached.keyboard.keysUp.push(211);    // Left analog up
    camera.inputs.attached.keyboard.keysDown.push(212);  // Left analog down
    camera.inputs.attached.keyboard.keysLeft.push(214);  // Left analog left
    camera.inputs.attached.keyboard.keysRight.push(213); // Left analog right
```


### <a name="step-4-give-it-a-try"></a>4단계: 한 번 시도해 보세요.

헤드셋과 게임 컨트롤러를 연결할 상태에서 **index.html**을 열면, 파란 게임 창을 마우스 왼쪽 단추로 클릭하면 게임이 VR 모드로 전환됩니다. 계속 진행하고 헤드셋을 착용하여 결과를 확인합니다. 


<iframe height='300' scrolling='no' title='Babylon.GUI를 사용하는 Babylon.js dino 게임 - WebVR' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/RjgpJd/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen</a>에서 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/RjgpJd/'>Babylon.GUI를 사용하는 Babylon.js dino 게임 - WebVR</a> by Microsoft Edge Docs(<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>)를 참조하세요.
</iframe>


## <a name="conclusion"></a>결론

축하합니다. 이제 WebVR을 지원하는 완전한 Babylon.js 게임이 완성되었습니다. 여기에서 더 나은 게임 제작을 위해 배운 내용을 활용하거나, 그대로 빌드할 수 있습니다.
