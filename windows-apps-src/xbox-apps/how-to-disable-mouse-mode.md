---
author: payzer
title: "마우스 모드를 사용하지 않도록 설정하는 방법"
description: "기본 마우스 모드를 사용하지 않도록 설정하는 지침입니다."
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: e57ee4e6-7807-4943-a933-c2b4dc80fc01
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 6848d1783df4489571fcf493a55447f67e5d6fd9
ms.lasthandoff: 02/08/2017

---

# <a name="how-to-disable-mouse-mode"></a>마우스 모드를 사용하지 않도록 설정하는 방법
모든 응용 프로그램은 기본적으로 마우스 모드를 사용합니다. 즉, 옵트아웃하지 않은 모든 응용 프로그램은 콘솔의 Edge 브라우저에 있는 것과 유사한 마우스 포인터를 받게 됩니다. 마우스 모드를 끄고 방향 컨트롤러 탐색에 최적화하는 것이 좋습니다.   
   
## <a name="html"></a>HTML   
JavaScript UWP(유니버설 Windows 플랫폼) 앱에서 방향 컨트롤러 탐색을 켜려면 [TVHelpers 방향 탐색](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation) JavaScript 라이브러리를 사용합니다. 앱 패키지에 방향 탐색 JavaScript 파일을 포함하고 방향 컨트롤러 탐색이 필요한 모든 HTML 페이지에서 다음과 같이 참조를 추가합니다.

```code
<script src="directionalnavigation-1.0.0.0.js"></script>
```
자세한 내용은 [방향 탐색 wiki](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation)를 참조하세요.

마우스 모드를 끄는 대신 DOM 또는 WinRT 게임 패드 API를 직접 사용하려는 경우 게임 패드가 필요한 모든 페이지에서 다음을 실행합니다. 
   
```code
navigator.gamepadInputEmulation = "gamepad";
```   

   이 속성은 기본적으로 마우스 모드를 사용할 수 있는 `mouse`로 설정됩니다. 이 속성을 `keyboard`로 설정하면 마우스 모드가 꺼지고 대신에, 게임 패드 입력이 DOM 키보드 이벤트를 생성합니다. 이 속성을 `gamepad`로 설정하면 마우스 모드가 꺼지고 DOM 키보드 이벤트가 생성되지 않으며, 사용자가 DOM 또는 WinRT 게임 패드 API를 사용할 수 있게 됩니다.

## <a name="xaml"></a>XAML    
마우스 모드를 끄려면 앱의 생성자에 다음을 추가합니다.   
   
```code
public App() {
        this.InitializeComponent();
        this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
        this.Suspending += OnSuspending;
}
```

## <a name="cdirectx"></a>C++/DirectX   
C++/DirectX 앱을 작성할 경우에는 수행할 작업이 없습니다. 마우스 모드는 HTML 및 XAML 응용 프로그램에만 적용됩니다.

## <a name="see-also"></a>참고 항목
- [Xbox에 적용할 수 있는 최선의 방법](tailoring-for-xbox.md)
- [Xbox One의 UWP](index.md)


