---
author: payzer
title: "마우스 모드를 사용하지 않도록 설정하는 방법"
description: 
area: Xbox
ms.sourcegitcommit: bdf7a32d2f0673ab6c176a775b805eff2b7cf437
ms.openlocfilehash: bd7780f105f86d7d292c5db2535ad40af07d6e4f

---

# 마우스 모드를 사용하지 않도록 설정하는 방법
마우스 모드는 기본적으로 모든 응용 프로그램에 대해 설정되어 있습니다. 즉, 옵트아웃(opt out)하지 않은 모든 응용 프로그램은 콘솔의 Edge 브라우저에 있는 것과 유사한 마우스 포인터를 받게 됩니다. 마우스 모드를 끄고 방향 컨트롤러 탐색에 최적화하는 것이 좋습니다.   
   
## HTML   
마우스 모드를 끄려면 응용 프로그램의 JavaScript 파일에 다음을 추가합니다.   
   
```code
navigator.gamepadInputEmulation = "keyboard";
```   

## XAML    
마우스 모드를 끄려면 응용 프로그램의 JavaScript 파일에 다음을 추가합니다.   
   
```code
public App() {
        this.InitializeComponent();
        this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
        this.Suspending += OnSuspending;
        }
```

## C++/DirectX   
C++/DirectX 앱을 작성할 경우에는 수행할 작업이 없습니다. 마우스 모드는 HTML 및 XAML 응용 프로그램에만 적용됩니다.



<!--HONumber=Jun16_HO4-->


