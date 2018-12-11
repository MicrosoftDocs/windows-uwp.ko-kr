---
title: 3D Babylon.js 게임에 WebVR 지원 추가
description: 기존 3D Babylon.js 게임에 WebVR 지원을 추가 하는 방법을 알아봅니다.
ms.date: 11/29/2017
ms.topic: article
keywords: webvr, edge, 웹 개발, babylon, babylonjs, babylon.js, javascript
ms.localizationpriority: medium
ms.openlocfilehash: 3e2081f0dbe163dcbcf35d83ea111caf573dacfb
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8874682"
---
# <a name="adding-webvr-support-to-a-3d-babylonjs-game"></a>3D Babylon.js 게임에 WebVR 지원 추가

Babylon.js를 사용 하 여 3D 게임을 만든 가상 현실 (VR)에서 멋진 비슷합니다 것 이라고 생각 한 경우이 자습서는 현실 나타나도록 하려면 간단한 단계를 따릅니다.

여기에 표시 된 게임에 WebVR 지원을 추가 됩니다. 계속 하려면를 시험해 보는 방법에 Xbox 컨트롤러를 연결!


<iframe height='300' scrolling='no' title='Babylon.GUI를 사용 하 여 Babylon.js dino 게임' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/wrOvoj/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>펜 <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/wrOvoj/'>Babylon.js dino 게임 Babylon.GUI를 사용 하 여</a> Microsoft Edge 문서를 참조 하세요 (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) <a href='https://codepen.io'>CodePen</a>에 있습니다.
</iframe>

VR에 대 한 하지만 평면에서 잘 작동 하는 3D 게임입니다.
이 자습서에서는 살펴봅니다 몇 가지 단계 것이 준비를 사용 하 고 WebVR 사용 하 여 실행 합니다. Microsoft Edge에서 WebVR에 대 한 지원이 추가 되었습니다을 활용할 수 있는 [Windows Mixed Reality](https://developer.microsoft.com/en-us/windows/mixed-reality) 헤드셋을 사용 됩니다. 게임에 이러한 변경 내용을 적용, WebVR을 지 원하는 다른 브라우저/헤드셋 조합에서 작동 하도록도 기대할 수 있습니다.



## <a name="prerequisites"></a>사전 요구 사항

- 텍스트 편집기 (예: [Visual Studio Code](https://code.visualstudio.com/download))
- 컴퓨터에 연결 되어 있는 Xbox 컨트롤러
- Windows 10 크리에이터스 업데이트
- [Windows Mixed Reality를 실행 하려면 필수 최소 사양](https://developer.microsoft.com/en-us/windows/mixed-reality/immersive_headset_setup) 포함 된 컴퓨터
- Windows 혼합 현실 장치 (선택 사항) 



## <a name="getting-started"></a>시작

시작 하기 위한 가장 간단한 방법은 방문 [Windows 자습서 웹 GitHub 리포지토리](https://github.com/Microsoft/Windows-tutorials-web)눌러 녹색 **복제 또는 다운로드** 단추를 클릭 한 **Visual Studio에서 열기**를 선택 합니다.

![복제 또는 다운로드 단추](images/3dclone.png)

프로젝트를 복제하는 대신 zip 파일로 다운로드할 수도 있습니다.
다음 [두 폴더, 하기 전과 [후](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/after)](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) 해야 합니다. "이전" 폴더 모든 VR 기능이 추가 되 고 "후" 폴더 VR 지원 사용 하 여 완성 된 게임은 게임입니다.

전과 후 폴더의이 파일 포함 합니다.
-   **텍스처 /** -게임에 사용 되는 이미지가 들어 있는 폴더.
-   **css /** -CSS 게임에 들어 있는 폴더.
-   **js /** -JavaScript 파일이 들어 있는 폴더. Main.js 파일은 게임, 및 기타 파일은 사용 하는 라이브러리입니다.
-   **모델 /** -3D 모델이 들어 있는 폴더. 이 게임에는 공룡 모델 하나만 모델을 했습니다.
-   **index.html** -게임의 렌더러를 호스트 하는 웹 페이지. Microsoft Edge에서이 페이지를 열고 게임을 실행 합니다.

Microsoft Edge에서 자신의 해당 index.html 파일을 열어 게임의 두 버전 모두를 테스트할 수 있습니다.



## <a name="the-mixed-reality-portal"></a>Mixed Reality 포털

Windows Mixed Reality 잘 모르는 호환 그래픽 카드를 사용 하 여 컴퓨터에 설치 된 Windows 10 크리에이터 스 업데이트가 있는 경우 Windows 10에서 시작 메뉴에서 **Mixed Reality 포털** 앱을 열어 보십시오.

![Mixed Reality 포털 검색](images/mixed-reality-portal.png)

모든 요구 사항을 충족 하는 경우 다음 개발자 기능을 컴퓨터에 연결 하는 Windows Mixed Reality 헤드셋을 시뮬레이트합니다. 근처에 있는 실제는 헤드셋을 충분히 하더라도 인 경우 연결 하 고 설치 프로그램을 실행 합니다.

> [!IMPORTANT]
> 이 자습서는 동안 항상 Mixed Reality 포털에서 열린 이어야 합니다.

이제 Microsoft Edge로 WebVR 경험을 준비가 되었습니다.

## <a name="2d-ui-in-a-virtual-world"></a>가상 세계에서 2D UI

>[!NOTE]
> 시작 샘플을 다운로드 [**하기 전에**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) 폴더를 가져옵니다.

[Babylon.GUI](https://doc.babylonjs.com/how_to/gui) VR 친화적인 라이브러리 이면 간단한을 만들려면 대화형 사용자 인터페이스 VR 원활 하 게 동작 하는 수 있도록 및 비-VR 표시 됩니다.
Babylon.js, 확장에는 `GUI` 라이브러리를 사용 하는 throuhout 2D 요소를 만드는 예제입니다.


2D 텍스트 `GUI` 요소 들을 얼마나 많은 특성에 따라 몇 줄을 사용 하 여 만들 수 있습니다.
다음 코드 조각은 이미에 [**하기 전에**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) 샘플 보겠습니다 하지만 과정과 연습입니다.
먼저 확인 하는 [`AdvancedDynamicTexture`](https://doc.babylonjs.com/how_to/gui#advanceddynamictexture) 살펴보겠습니다 GUI를 설정 하는 개체입니다. 샘플 설정 `CreateFullScreenUI()`, 전체 화면을 사용 하는 UI를 의미 합니다. 사용 하 여 `AdvancedDynamicTexture` 를 만든 다음 양식이 사용 하 여 게임 시작 시 표시 되는 2D 텍스트 상자가 `GUI.Rectanlge()` 및 `GUI.TextBlock()`합니다.


이 코드는 [**main.js**](https://github.com/Microsoft/Windows-tutorials-web/blob/master/BabylonJS-game-with-WebVR/before/js/main.js#L157-L168)내에서 추가 됩니다.
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


이 UI가 사용 하 여 켜거나 끌 수 있지만 생성 되 면 표시 `isVisible` 게임의 상황을 따라 합니다.
```javascript
startUI.isVisible = false;
```



## <a name="detecting-headsets"></a>헤드셋 감지

여러 시나리오를 지원할 수 있도록 두 가지 유형의 카메라 있어야 VR 응용 프로그램에 대 한 것이 좋습니다. 이 게임에서는 헤드셋을 사용 하는 작업 헤드셋을 연결 하 고 다른 필요로 하는 하나의 카메라를 지원 됩니다. 대상을 게임 써서를 먼저 확인 해야 합니다 헤드셋 검색 되었는지 여부를 확인 합니다. 이렇게 하려면 사용 [`navigator.getVRDisplays()`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getVRDisplays).


위의이 코드 추가 `window.addEventListener('DOMContentLoaded')` **main.js**에 있습니다.
```javascript
var headset;
// If a VR headset is connected, get its info
navigator.getVRDisplays().then(function (displays) {
    if (displays[0]) {
        headset = displays[0];
    }
});
```

에 저장 된 정보를 사용 하 여는 `headset` 유동적에서는 이제 수를 사용자에 게 가장 적합 카메라를 선택할 수 있습니다.


## <a name="creating-and-selecting-the-initial-camera"></a>만들고 초기 카메라를 선택 합니다.

Babylon.js를 사용 하 여 WebVR 추가할 수 신속 하 게 사용 하 여는 [`WebVRFreeCamera`](http://doc.babylonjs.com/classes/3.1/webvrfreecamera). 이 카메라 키보드 입력을 취할 수 및 "머리" 회전을 제어 VR 헤드셋을 사용할 수 있습니다.


### <a name="step-1-checking-for-headsets"></a>1 단계: 헤드셋 확인

대체는 카메라를 사용 합니다 [`UniversalCamera`](https://doc.babylonjs.com/classes/3.1/universalcamera) 현재 원래 게임에 사용 되는 합니다.

확인 하는 `headset` 에서는 사용할 수 있는지 여부를 결정 하는 변수는 `WebVRFreeCamera` 카메라입니다.

대체 `camera = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);` 다음 코드를 사용 합니다.
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


### <a name="step-2-activating-the-webvrfreecamera"></a>2 단계: 활성화는 WebVRFreeCamera
대부분의 브라우저에서이 카메라를 활성화 하려면 사용자는 가상 경험을 요청 하는 몇 가지 상호 작용을 수행 해야 합니다.
이 기능은 마우스 클릭 최대 해보겠습니다.


내에서 코드를 붙여 `createScene()` 후 `camera.applyGravity = true;` .
```javascript
        scene.onPointerDown = function () {
            scene.onPointerDown = undefined
            camera.attachControl(canvas, true);
        }
```

게임에서 클릭 이제 다음과 같은 메시지를 표시 하거나 표시 게임 헤드셋에 바로 이면 사용자는 전에 프롬프트를 수락 합니다.

![몰입 형 프롬프트](images/immersiveview.png)

코드를 표시 하는 부분을 추가할 수 있습니다는 `UniversalCamera` 전에로 전환 하는 `WebVRFreeCamera`, 수 있으므로 사용자는 파란색 창이 대신 게임에 대해 합니다. 

다음 추가 `engine.runRenderLoop(function () {`.
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

### <a name="step-3-adding-gamepad-support"></a>3 단계: 게임 패드 지원 추가

이후는 `WebVRFreeCamera` 게임 패드를 지원 하지 않는 처음 키보드 화살표 키에는 게임 패드 단추 매핑 됩니다에서는 합니다. 분석 하 여이 작업을 수행 합니다 하는 `inputs` 카메라의 속성입니다. 왼쪽된 아날로그 스틱에 대 한 해당 코드를 추가 위, 아래, 왼쪽 및 오른쪽 화살표 키를 사용 하 여 일치 하는 게임 패드 작업에서 다시입니다.


아래에 다음이 코드를 추가 합니다 `scene.onPointerDown = function() {...}` 를 호출 합니다.
``` javascript
    // Custom input, adding Xbox controller support for left analog stick to map to keyboard arrows
    camera.inputs.attached.keyboard.keysUp.push(211);    // Left analog up
    camera.inputs.attached.keyboard.keysDown.push(212);  // Left analog down
    camera.inputs.attached.keyboard.keysLeft.push(214);  // Left analog left
    camera.inputs.attached.keyboard.keysRight.push(213); // Left analog right
```


### <a name="step-4-give-it-a-try"></a>4 단계: 시도해!

**Index.html** 는 헤드셋을 사용 하 여 엽니다 게임 컨트롤러 연결 하는 경우 파란색 게임 창의 왼쪽된 클릭은 게임 VR 모드로 전환 됩니다! 계속 진행 하 고 결과 확인 하도록 헤드셋에 배치 합니다. 


<iframe height='300' scrolling='no' title='Babylon.GUI-WebVR을 사용 하 여 Babylon.js dino 게임' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/RjgpJd/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>펜 <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/RjgpJd/'>Babylon.js dino 게임 Babylon.GUI-WebVR을 사용 하 여</a> Microsoft Edge 문서를 참조 하세요 (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) <a href='https://codepen.io'>CodePen</a>에 있습니다.
</iframe>


## <a name="conclusion"></a>결론

축하합니다! 전체 Babylon.js 게임에 WebVR 지원 생겼습니다. 여기서에서는 훨씬 더 나은 게임을 제작 또는이 이와 오프 빌드 배운 취할 수 있습니다.
