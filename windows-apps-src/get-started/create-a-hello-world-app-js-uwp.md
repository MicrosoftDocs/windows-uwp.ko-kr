---
ms.assetid: 3a17e682-40be-41b4-8bd3-fbf0b15259d6
title: "\"Hello, world\" 앱 만들기(JS)"
description: 이 자습서에서는 JavaScript 및 HTML을 사용하여 Windows 10의 UWP(유니버설 Windows 플랫폼)를 대상으로 하는 간단한 &\#0034;Hello, world&\#0034; 앱을 만드는 방법을 설명합니다.
ms.date: 09/12/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a161a5e26bd647a37b06a270ef6714d54bc16d80
ms.sourcegitcommit: ac9fb37fe58cc728df2fa0495e36ee4d7b811a23
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74467414"
---
# <a name="create-a-hello-world-app-js"></a>"Hello, world" 앱 만들기(JS)

이 자습서에서는 JavaScript 및 HTML을 사용하여 Windows 10의 UWP(유니버설 Windows 플랫폼)를 대상으로 하는 간단한 Hello, world 앱을 만드는 방법을 설명합니다. Microsoft Visual Studio의 단일 프로젝트를 사용하여 Windows 10 디바이스에서 실행되는 앱을 빌드할 수 있습니다.

> [!NOTE]
> 이 자습서에서는 Visual Studio Community 2017을 사용합니다. 다른 버전의 Visual Studio를 사용하는 경우 약간 다르게 보일 수 있습니다.

> [!WARNING]
> Javascript UWP 앱 개발은 Visual Studio 2019에서 지원되지 않습니다. Javascript UWP 앱을 개발하려면 Visual Studio 2017을 사용해야 합니다.

이 문서에서는 다음 작업을 수행하는 방법을 알아봅니다.

-   **Windows 10** 및 **UWP**를 대상으로 하는 새 **Visual Studio 2017** 프로젝트를 만듭니다.
-   HTML 및 JavaScript 콘텐츠를 추가합니다.
-   Visual Studio의 로컬 데스크톱에서 프로젝트를 실행합니다.

## <a name="before-you-start"></a>시작하기 전 확인 사항

-   [UWP 앱이란 무엇인가요?](universal-application-platform-guide.md)
-   이 자습서를 완료하려면 Windows 10 및 Visual Studio가 필요합니다. [설정 방법](get-set-up.md)
-   또한, 여기에서는 Visual Studio의 기본 창 레이아웃을 사용한다고 가정합니다. 기본 레이아웃이 변경된 경우 **창** 메뉴에서 **창 레이아웃 다시 설정** 명령을 사용하여 다시 설정할 수 있습니다.

## <a name="step-1-create-a-new-project-in-visual-studio"></a>1단계: Visual Studio에서 새 프로젝트 만들기

1.  Visual Studio 2017을 시작합니다.

2.  **파일** 메뉴에서 **새로 만들기 > 프로젝트**를 선택하여 **새 프로젝트 만들기** 대화 상자를 엽니다.

3.  **비어 있는 앱(유니버설 Windows) JavaScript**를 선택한 후 **다음**을 선택합니다.

    (유니버설 템플릿이 보이지 않으면 UWP 앱을 만들기 위한 구성 요소가 누락된 것일 수 있습니다. 설치 과정을 다시 실행하고 **새 프로젝트 만들기** 대화 상자에서 **Visual Studio 설치 관리자 열기**를 선택하여 UWP 지원을 추가할 수 있습니다. [설정 방법](get-set-up.md)을 참조하세요.

4.  **새 프로젝트 구성** 대화 상자에서 **프로젝트 이름**으로 **HelloWorld**를 입력한 다음, **만들기**를 선택합니다.

> [!NOTE]
> Visual Studio를 처음 사용하는 경우 **개발자 모드**를 사용하도록 설정하라는 설정 대화 상자가 표시될 수 있습니다. 개발자 모드는 앱을 Store에서만 실행하는 것이 아니라 직접 실행할 수 있는 권한처럼 특정 기능을 활성화하는 특수 설정입니다. 자세한 내용은 [디바이스를 개발에 사용하도록 설정](enable-your-device-for-development.md)을 읽어보세요. 이 가이드를 계속하려면 **개발자 모드**를 선택하고 **예**를 선택한 다음, 대화 상자를 닫습니다.

 ![개발자 모드 활성화 대화 상자](images/win10-cs-00.png)

5.  대상 버전/최소 버전 대화 상자가 나타납니다. 이 자습서에는 기본 설정이면 충분하므로 **확인**을 선택하여 프로젝트를 만듭니다.

    ![솔루션 탐색기 창](images/win10-cs-02.png)

6.  새 프로젝트가 열리면 해당 파일이 오른쪽의 **솔루션 탐색기** 창에 표시됩니다. 파일을 보려면 **속성** 탭 대신 **솔루션 탐색기** 탭을 선택해야 할 수 있습니다.

    ![솔루션 탐색기 창](images/win10-js-02.png)

**새 앱**(유니버설 Window)은 최소한의 템플릿이지만 많은 파일이 포함되어 있습니다. 이러한 파일은 JavaScript를 사용하는 모든 UWP 앱에 필수적입니다. Visual Studio에서 만든 모든 프로젝트에는 해당 파일이 포함됩니다.


### <a name="whats-in-the-files"></a>파일에 포함된 항목

프로젝트의 파일을 보고 편집하려면 **솔루션 탐색기**에서 해당 파일을 두 번 클릭합니다.

*default.css*

-  앱에서 사용하는 기본 스타일시트입니다.

*main.js*

- 기본 JavaScript 파일입니다. index.html 파일에서 참조됩니다.

*index.html*

- 앱의 웹 페이지로, 앱이 실행될 때 로드되어 표시됩니다.

*로고 이미지 세트*
-   Assets/Square150x150Logo.scale-200.png는 **시작** 메뉴에서 앱을 나타냅니다.
-   Assets/StoreLogo.png는 Microsoft Store에 앱을 나타냅니다.
-   Assets/SplashScreen.scale-200.png는 앱 시작 시 표시되는 시작 화면입니다.

## <a name="step-2-adding-a-button"></a>2단계: 단추 추가

**index.html**을 선택하여 편집기에서 선택하고 읽기 위해 포함된 HTML을 다음과 같이 변경합니다.

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8" />
    <title>Hello World</title>
    <script src="js/main.js"></script>
    <link href="css/default.css" rel="stylesheet" />
</head>

<body>
    <p>Click the button..</p>
    <button id="button">Hello world!</button>
</body>

</html>
```

다음과 같이 표시되어야 합니다.

 ![프로젝트의 HTML](images/win10-js-03.png)

이 HTML은 JavaScript를 포함할 *main.js*를 참조한 다음, 웹 페이지 본문에 텍스트 한 줄과 단추 하나를 추가합니다. JavaScript가 참조할 수 있도록 이 단추에 *ID*가 부여됩니다.


## <a name="step-3-adding-some-javascript"></a>3단계: 일부 JavaScript 추가

이제 JavaScript를 추가하겠습니다. **main.js**를 선택하여 선택한 후, 다음 항목을 추가합니다.

```javascript
// Your code here!

window.onload = function () {
    document.getElementById("button").onclick = function (evt) {
        sayHello()
    }
}


function sayHello() {
    var messageDialog = new Windows.UI.Popups.MessageDialog("Hello, world!", "Alert");
    messageDialog.showAsync();
}

```

다음과 같이 표시되어야 합니다.

 ![프로젝트의 JavaScript](images/win10-js-04.png)

이 JavaScript는 두 함수를 선언합니다. *index.html*이 표시되면 *window.onload* 함수가 자동으로 호출됩니다. 이 함수는 우리가 앞에서 선언한 ID를 사용하여 단추를 찾은 다음, onclick 처리기를 추가합니다. 이 처리기는 단추 클릭 시 호출되는 메서드입니다.

두 번째 함수인 *sayHello()* 는 대화 상자를 만들어서 표시합니다. 이전 JavaScript 개발에서 보셨을 수도 있는 *Alert()* 함수와 매우 유사합니다.


## <a name="step-4-run-the-app"></a>4단계: 앱을 실행합니다.

이제 F5 키를 눌러 앱을 실행할 수 있습니다. 앱이 로드되고 웹 페이지가 표시됩니다. 단추를 선택하면 메시지 대화 상자가 팝업됩니다.

 ![프로젝트 실행](images/win10-js-05.png)



## <a name="summary"></a>요약


축하합니다. Windows 10 및 UWP용 JavaScript 앱을 만드셨습니다. 아주 간단한 예제였지만 이제 여러분은 원하는 JavaScript 라이브러리와 프레임워크를 추가하여 여러분만의 앱을 만들 수 있을 것입니다. 그리고 이 앱은 UWP 앱이므로 Store에 게시할 수 있습니다. 타사 프레임워크를 추가하는 방법은 다음 프로젝트를 참조하세요.

* [JavaScript 및 CreateJS로 작성된 간단한 Microsoft Store용 2D UWP 게임](get-started-tutorial-game-js2d.md)
* [JavaScript 및 threeJS로 작성된 Microsoft Store용 3D UWP 게임](get-started-tutorial-game-js3d.md)
