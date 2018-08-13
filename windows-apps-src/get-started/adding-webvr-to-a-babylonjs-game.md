---
title: 3 차원 Babylon.js 게임을 WebVR 지원 추가 (영문)
description: 기존 3D Babylon.js 게임에 WebVR 지원을 추가 하는 방법에 알아봅니다.
author: abbycar
ms.author: abigailc
ms.date: 11/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: webvr에 지, 웹 개발, babylon, babylonjs, babylon.js, javascript
ms.localizationpriority: medium
ms.openlocfilehash: 41665e8719493bb658f9926947061b1b5f81a139
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1018653"
---
# <a name="adding-webvr-support-to-a-3d-babylonjs-game"></a>3 차원 Babylon.js 게임을 WebVR 지원 추가 (영문)

Babylon.js를 사용 하 여 3 차원 게임을 만든 것 가상 현실 (VR)에서 뛰어난 같습니다 생각 했을 때 경우는 실제로 해야이 자습서에서는 간단한 단계를 수행 합니다.

여기에 표시 된 게임을 WebVR 지원을 추가 합니다. 계속 하려면를을 사용해 서는 Xbox 컨트롤러에 연결!


<iframe height='300' scrolling='no' title='Babylon.GUI를 사용 하 여 Babylon.js dino 게임' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/wrOvoj/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Microsoft에 지 문서 하 여 펜 <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/wrOvoj/'>Babylon.GUI를 사용 하 여 Babylon.js dino 게임</a> 을 참조 하십시오 (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) <a href='https://codepen.io'>CodePen</a>에 있습니다.
</iframe>

이 작업은 VR의 평면 화면을 하지만 어떤에서 제대로 작동 하는 방법에 대 한 3D 게임을?
이 자습서에서는 여기서 안내 몇 단계가 것이 준비를 사용 하 고 WebVR으로 실행 합니다. Microsoft에 지에 WebVR에 대 한 지원이 추가 되었습니다을 활용할 수 있는 [Windows 혼합 현실](https://developer.microsoft.com/en-us/windows/mixed-reality) 헤드셋을 사용 합니다. 이러한 변경 내용을 게임 적용할 것을 지 원하는 WebVR 다른 브라우저/헤드셋 조합에서 작동 하도록도 예상할 수 있습니다.



## <a name="prerequisites"></a>필수 구성 요소

- 텍스트 편집기 (예: [Visual Studio 코드](https://code.visualstudio.com/download))
- 사용자의 컴퓨터에 연결 되는 Xbox 컨트롤러
- Windows 10 크리에이터스 업데이트
- [혼합 현실 Windows를 실행 하려면 필요한 최소 사양](https://developer.microsoft.com/en-us/windows/mixed-reality/immersive_headset_setup) 을 사용 하는 컴퓨터
- Windows 혼합 실제 장치 (선택 사항) 



## <a name="getting-started"></a>시작

시작 하기 위한 가장 간단한 방법은 [Windows-자습서-웹 GitHub repo](https://github.com/Microsoft/Windows-tutorials-web)방문 하 여 눌러 녹색 **복제 또는 다운로드** 단추를 클릭 한 **Visual Studio에서 열기**를 선택 합니다.

![복제 또는 다운로드 단추](images/3dclone.png)

프로젝트를 복제하는 대신 zip 파일로 다운로드할 수도 있습니다.
다음 [두 종류의 폴더가, 하기 전과 [후](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/after)](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) 해야 합니다. "Before" 폴더는 모든 VR 기능을 추가 하 고 "후" 폴더는 VR 지원 완성 된 게임 전에 사용해 게임입니다.

이러한 파일을 포함 하는 폴더 후:
-   **질감 /** -게임에 사용 되는 모든 이미지를 포함 하는 폴더입니다.
-   **css /** -게임에 대 한 CSS 들어 있는 폴더입니다.
-   **js /** -JavaScript 파일이 들어 있는 폴더입니다. Main.js 파일은이 게임 및 다른 파일을 사용 하는 라이브러리입니다.
-   **모델 /** -3 차원 모델을 포함 하는 폴더입니다. 이 게임에 대 한 공룡에 대 한 하나의 모델을 했습니다.
-   **index.html** -게임의 렌더러를 호스팅하는 웹 페이지입니다. Microsoft에 지에서이 페이지를 열고 해당 게임을 시작 합니다.

Microsoft에 지에서 자신의 개별 index.html 파일을 열어 게임의 두 버전 모두를 테스트할 수 있습니다.



## <a name="the-mixed-reality-portal"></a>혼합된 현실 포털

Windows 혼합 현실 잘 모르는 호환 그래픽 카드를 사용 하는 컴퓨터에 설치 된 Windows 10 작성자 업데이트 하는 경우에 Windows 10의 시작 메뉴에서 **혼합 현실 포털** 응용 프로그램을 열어 보십시오.

![혼합된 현실 포털 검색](images/mixed-reality-portal.png)

요구 사항을 모두 충족 하는 경우 다음 개발자 기능 설정 및 사용자 컴퓨터에 연결 하는 Windows 혼합 현실 헤드셋을 시뮬레이션 수 있습니다. 근처에 있는 실제는 헤드셋을 충분히 하더라도 인 경우 다시 연결 하 고 사용 하 여 설치를 실행 합니다.

> [!IMPORTANT]
> 혼합 현실 포털은 항상 하는 동안이 자습서에서 열려 있어야 합니다.

이제 Microsoft 가장자리에 맞추어 WebVR 경험 준비가 되었습니다.

## <a name="2d-ui-in-a-virtual-world"></a>가상 세계에서 2D UI

>[!NOTE]
> 가져올 starter 샘플 [**하기 전에**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) 폴더를 가져옵니다.

[Babylon.GUI](https://doc.babylonjs.com/how_to/gui) 는 VR에 게 친숙 한 라이브러리, 간단한 만들려는 대화형 사용자 인터페이스 (영문) VR에 적합 한의 작동 및 비-VR를 표시 하는 수 있도록 합니다.
Babylon.js에 대 한 확장 프로그램은 `GUI` 라이브러리를 사용 하는 throuhout 2D 요소를 만드는 예제입니다.


2D 텍스트 `GUI` 조정 하려는 얼마나 많은 특성에 따라 몇 줄 요소를 만들 수 있습니다.
다음 코드 조각 이미를 사용해 [**하기 전에**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) 샘플 보겠습니다 하지만 소식 연습 합니다.
먼저 확인 한 [`AdvancedDynamicTexture`](https://doc.babylonjs.com/how_to/gui#advanceddynamictexture) 살펴보겠습니다 GUI를 설정 하는 개체입니다. 이 샘플 설정 하는이 `CreateFullScreenUI()`, 전체 화면을 사용 하는 UI를 의미 합니다. 함께 `AdvancedDynamicTexture` 을 만든 다음 결정을 사용 하 여 게임을 시작 하면 표시 되는 2D 텍스트 상자가 `GUI.Rectanlge()` 및 `GUI.TextBlock()`합니다.


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


이 UI 만들었지만 상태를 전환할 수 설정 또는 해제 된 후 표시 되는 `isVisible` 게임에서 수행 중인 작업에 따라 합니다.
```javascript
startUI.isVisible = false;
```



## <a name="detecting-headsets"></a>헤드셋 하 여 검색

VR 응용 프로그램 2 가지 유형의 카메라의 많은 시나리오를 지원할 수 있도록 하는 것이 좋습니다. 이 게임에 대 한에서는 헤드셋을 사용 하는 작업 헤드셋을 연결 하 고 다른 필요한 하나의 카메라를 지원 됩니다. 어떤 것을 확인 하려면 게임 사용 하 여, 먼저 확인 해야 헤드셋 검색 되었는지 여부를 참조 하십시오. 이렇게 하기 위해 사용 합니다 [`navigator.getVRDisplays()`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getVRDisplays)합니다.


위의이 코드를 추가 `window.addEventListener('DOMContentLoaded')` **main.js**에 있습니다.
```javascript
var headset;
// If a VR headset is connected, get its info
navigator.getVRDisplays().then(function (displays) {
    if (displays[0]) {
        headset = displays[0];
    }
});
```

에 저장 된 정보와 함께 `headset` 변수는 이제 있으므로 사용자에 게 가장 적합 카메라를 선택할 수 있도록 합니다.


## <a name="creating-and-selecting-the-initial-camera"></a>초기 카메라를 선택 하 고 만들기 (영문)

Babylon.js와 WebVR 추가할 수 신속 하 게 사용 하 여는 [`WebVRFreeCamera`](http://doc.babylonjs.com/classes/3.1/webvrfreecamera)합니다. 이 카메라 키보드 입력을 받아 수 및 VR 헤드셋을 사용 하 여 "헤드" 회전을 제어할 수 있습니다.


### <a name="step-1-checking-for-headsets"></a>1 단계: 헤드셋에 대 한 검사

이 대체 카메라에 대 한 사용할 것은 [`UniversalCamera`](https://doc.babylonjs.com/classes/3.1/universalcamera) 원래 게임 내에서 현재 사용 되는.

하는지를 검사 사용해 `headset` 변수는 사용할 수 있는지 여부를 결정 하는 `WebVRFreeCamera` 카메라 합니다.

바꾸기 `camera = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);` 다음 코드를 사용 합니다.
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


### <a name="step-2-activating-the-webvrfreecamera"></a>2 단계:는 WebVRFreeCamera 활성화
사용자는 대부분의 브라우저에서이 카메라를 활성화 하려면 가상 환경에서 요청 하는 일부 상호작용을 수행 해야 합니다.
이 기능은 마우스를 클릭 하는 최대 연결 하겠습니다.


내에서 코드를 붙여넣습니다 `createScene()` 발생 한 이후 작동 `camera.applyGravity = true;` 합니다.
```javascript
        scene.onPointerDown = function () {
            scene.onPointerDown = undefined
            camera.attachControl(canvas, true);
        }
```

이제는 다음과 같은 프롬프트를 만듭니다 또는 표시 하는 게임 헤드셋에 즉시 사용자가 전에 프롬프트를 허용 하는 경우 게임에서을 클릭 합니다.

![몰입 형 프롬프트](images/immersiveview.png)

표시 하는 코드 조각을 추가할 수도 있습니다는 `UniversalCamera` 것으로 전환 하기 전에 보려면 사용해 `WebVRFreeCamera`, 파란색 창 대신 게임을 살펴보는 사용자 수 있도록 설정 합니다. 

한 후 다음을 추가 `engine.runRenderLoop(function () {`합니다.
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

### <a name="step-3-adding-gamepad-support"></a>3 단계: 게임 패드 지원 추가 (영문)

이후에 `WebVRFreeCamera` 게임 패드를 지원 하지 않는 처음 키보드 화살표 키를 사용해 게임 패드 단추를 매핑할 수는 있습니다. 분석 하 여이 작업을 수행 하는 `inputs` 카메라의 속성입니다. 위쪽, 아래쪽, 왼쪽 및 오른쪽 화살표 키와 일치 시킵니다 왼쪽된 아날로그 연결에 대 한 해당 코드를 추가, 하 여이 게임 패드 작업 다시입니다.


아래에 다음이 코드를 추가 `scene.onPointerDown = function() {...}` 를 호출 합니다.
``` javascript
    // Custom input, adding Xbox controller support for left analog stick to map to keyboard arrows
    camera.inputs.attached.keyboard.keysUp.push(211);    // Left analog up
    camera.inputs.attached.keyboard.keysDown.push(212);  // Left analog down
    camera.inputs.attached.keyboard.keysLeft.push(214);  // Left analog left
    camera.inputs.attached.keyboard.keysRight.push(213); // Left analog right
```


### <a name="step-4-give-it-a-try"></a>4 단계: 시도해!

우리가 **index.html** 사용해 헤드셋와 열고 게임 컨트롤러 연결 하는 경우 파란색 게임 창의 왼쪽된 클릭을 사용 하 여이 게임 VR 모드에 전환 됩니다! 계속 진행 하 고 결과 체크아웃 하려면 헤드셋에 배치 합니다. 


<iframe height='300' scrolling='no' title='Babylon.GUI-WebVR를 사용 하 여 Babylon.js dino 게임' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/RjgpJd/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Microsoft에 지 문서 하 여 펜 <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/RjgpJd/'>Babylon.GUI-WebVR를 사용 하 여 Babylon.js dino 게임</a> 을 참조 하십시오 (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) <a href='https://codepen.io'>CodePen</a>에 있습니다.
</iframe>


## <a name="conclusion"></a>결론

축하합니다! 이제 WebVR 지원이 포함 된 완전 한 Babylon.js 게임을 해야합니다. 여기서에서는 더 나은 게임을 작성 하거나이 오프 빌드를 얻은 했을 때 취할 수 있습니다.
