---
description: Visual Studio에서 살펴보기
title: Visual Studio에서 살펴보기
ms.assetid: 7FBB50A2-6D22-4082-B333-5153DADDDE9A
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 02a0b3c631d3ee85353eb0516d5c6d1aa511f77e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174927"
---
# <a name="getting-started-getting-around-in-visual-studio"></a>시작: Visual Studio 둘러보기


## <a name="getting-around-in-microsoft-visual-studio"></a>Microsoft Visual Studio 살펴보기

이제 이전에 만든 프로젝트로 돌아가 Microsoft Visual Studio IDE (통합 개발 환경)를 어떻게 찾을 수 있는지 살펴보겠습니다.

Xcode 개발자 인 경우 왼쪽 창의 소스 파일, 가운데 창에 있는 편집기 (UI 또는 소스 코드), 오른쪽 창에서 컨트롤 및 해당 속성을 사용 하 여 아래 기본 뷰를 잘 알고 있어야 합니다.

![xcode 개발 환경](images/ios-to-uwp/xcode-ide.png)

기본 뷰는 **도구 상자**의 왼쪽에 컨트롤을 포함 하지만 Microsoft Visual Studio 매우 유사 합니다. 소스 파일은 오른쪽에 **솔루션 탐색기** 있으며 속성은 다음과 같이 **솔루션 탐색기** 창의 **속성** 에 있습니다.

![visual studio 개발 환경](images/ios-to-uwp/vs-ide.png)

이렇게 하는 것이 거의 외계인 않는 경우 Visual Studio에서 창을 다시 정렬 하 여 화면 왼쪽에 소스 파일을 배치 하 고 도구 상자를 오른쪽에 배치할 수 있다는 것을 알게 될 것입니다. 실제로 창의 제목 표시줄을 클릭 하 고 끌어서 위치를 조정할 수 있으며, Visual Studio는 사용자가 파일을 놓을 때 도킹할 위치를 알려 주는 회색 상자를 표시 합니다. 많은 창에도 제목 표시줄에 작은 그리기 핀 아이콘이 있습니다. 그러면 패널을 있는 그대로 고정 하 여 제자리에서 잠글 수 있습니다. 창을 고정 해제 하 고 공간 절약을 위해 축소할 수 있습니다. 모니터가 작은 쪽에 있으면 유용 합니다. 모든 작업을 완료 한 경우 (걱정할 필요가 없음) **창** 메뉴에서 **창 레이아웃 다시 설정** 을 선택 하 여 순서를 복원 합니다.

## <a name="adding-controls-setting-their-properties-and-responding-to-events"></a>컨트롤을 추가 하 고, 속성을 설정 하 고, 이벤트에 응답

이제 프로젝트에 일부 컨트롤을 추가 해 보겠습니다. 그런 다음 해당 속성 중 일부를 변경 하 고 컨트롤의 이벤트 중 하나에 응답 하는 코드를 작성 합니다.

Xcode에 컨트롤을 추가 하려면 아래와 같이 xib 파일이 나 스토리 보드를 연 다음**라운드 Rect 단추** 또는 **레이블과**같은 컨트롤을 끌어서 놓습니다.

![xcode에서 ui 디자인](images/ios-to-uwp/xcode-add-button-label.png)

Visual Studio에서 유사한 작업을 수행해 보겠습니다. **도구 상자**에서 **Button** 컨트롤을 끌어 mainpage 파일의 디자인 화면에 놓습니다.

**TextBlock** 컨트롤과 동일한 작업을 수행 하므로 다음과 같이 표시 됩니다.

![visual studio에서 ui 디자인](images/ios-to-uwp/vs-add-button-label.png)

레이아웃 및 바인딩 정보를 .xib 또는 스토리보드 파일에 숨기는 Xcode와 달리, Visual Studio에서는 이러한 세부 정보를 풍부하고 편집 가능하며 선언적인 XML 같은 언어로 저장하는 데 사용되는 XAML 파일을 편집하는 것이 좋습니다. XAML (Extensible Application Markup Language)에 대 한 자세한 내용은 [xaml 개요](../xaml-platform/xaml-overview.md)를 참조 하세요. 이제 **디자인** 창에 표시 되는 모든 항목이 **XAML** 창에 정의 되어 있습니다. **XAML** 창에서는 필요한 경우를 세밀 하 게 제어할 수 있으며, 자세한 정보를 통해 사용자 인터페이스 코드를 수동으로 빠르게 개발할 수 있습니다. 그러나 지금은 **디자인** 및 **속성** 창에만 집중 하겠습니다.

단추의 세부 정보를 변경해 보겠습니다. 아시다시피 Xcode에서 단추의 이름을 변경 하려면 해당 속성 패널에서 **제목** 필드의 값을 변경 합니다.

Visual Studio를 사용 하는 경우 매우 유사한 작업을 수행할 수 있습니다. **디자인** 창에서 단추를 탭 하 여 포커스를 제공 합니다. 그런 다음 **속성** 창에서 **콘텐츠** 값을 "Button"에서 "Press Me"로 변경 합니다. 다음으로, 다음과 같이 **이름** 값을 " &lt; No name &gt; "에서 "myButton"으로 변경 하 여 단추 컨트롤의 이름을 업데이트 합니다.

![visual studio의 단추 속성 창](images/ios-to-uwp/vs-button-properties.png)

이제 **TextBlock** 컨트롤의 내용을 "Hello, 세계!"로 변경 하는 코드를 작성해 보겠습니다. 작성해 보겠습니다.

Xcode에서 코드를 작성한 다음 컨트롤에 연결하거나, 종종 그림과 같이 Ctrl 키를 누른 상태에서 단추를 소스 코드로 끌어 이벤트를 컨트롤과 연결합니다.

![xcode에서 이벤트에 단추 배선](images/ios-to-uwp/xcode-add-button-event.png)

```swift
// Swift implementation.

@IBAction func buttonPressed(sender: UIButton) {
    
}
```

Visual Studio도 유사 합니다. **속성** 의 오른쪽 위에는 번개 모양의 단추가 있습니다. 다음과 같이 선택 된 컨트롤과 연결 된 가능한 이벤트가 나열 됩니다.

![visual studio의 단추 이벤트 목록](images/ios-to-uwp/vs-button-event.png)

단추의 click 이벤트에 대 한 코드를 추가 하려면 먼저 **디자인** 창에서 단추를 선택 합니다. 이제 번개 모양의 단추를 클릭 하 고 이름 옆에 있는 빈 상자를 두 **번 클릭 합니다**. 그러면 Visual Studio에서 \_ **클릭** 상자에 "myButton 클릭" 이벤트를 추가한 다음 MainPage.xaml.cs 파일에 다음과 같이 해당 이벤트 처리기를 추가 하 고 표시 합니다.

```csharp
private void myButton_Click(object sender, RoutedEventArgs e)
{

}
```

이제 **TextBlock** 컨트롤을 연결 하겠습니다. Xcode에서는 단추를 소스 코드 파일로 컨트롤을 끌어와 같이 컨트롤과 해당 정의를 연결 합니다.

![xcode의 해당 정의에 레이블 연결](images/ios-to-uwp/xcode-add-button-reference.png)

```swift
// Swift implentation.

@IBOutlet weak var myLabel : UILabel
```

Visual Studio에서는이 작업이 항상 수행 되기 때문에 컨트롤을 연결 하지 않아도 됩니다. 일부 속성을 변경 하겠습니다.

1.  MainPage .xaml 파일 탭을 탭 합니다.
2.  **디자인** 창에서 **TextBlock** 컨트롤을 누릅니다.
3.  **속성** 창에서 렌치 단추를 탭 하 여 해당 속성을 표시 합니다.
4.  **이름** 상자에서 " &lt; 이름 없음 &gt; "을 "myLabel"으로 변경 합니다.

![visual studio의 레이블 속성 창](images/ios-to-uwp/vs-label-properties.png)

이제 단추의 click 이벤트에 몇 가지 코드를 추가 해 보겠습니다. 이렇게 하려면 MainPage.xaml.cs 파일을 탭 하 고, myButton click 이벤트 처리기에 다음 코드를 추가 합니다 \_ .

```csharp
private void myButton_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.    
    myLabel.Text = "Hello, World!";
}
```

이는 Swift에서 작성 하는 것과 유사 합니다.

```swift
@IBAction func buttonPressed(sender: UIButton) {
    myLabel.text = "Hello, World!"
}
```

마지막으로, 응용 프로그램을 실행 하려면 **디버그** 메뉴를 선택한 다음 **디버깅 시작** 을 선택 하거나 F5 키를 누릅니다. 앱이 시작 된 후에는 다음 그림과 같이 "누르기" 단추를 클릭 하 고 레이블의 내용이 "TextBlock"에서 "Hello, 세계!"로 변경 된 것을 확인 합니다.

![첫 번째 연습을 실행 한 결과: hello, 세계!](images/ios-to-uwp/vs-hello-world.png)

앱을 종료 하려면 Visual Studio로 돌아가서 **디버그** 메뉴를 탭 한 다음 **디버깅 중지** 를 탭 하거나 SHIFT + f5 키를 누릅니다. Visual Studio를 사용 하면 다양 한 장치에서 앱을 시도 하 여 각 장치에서 수행 되는 방식을 확인할 수 있습니다.

## <a name="next-step"></a>다음 단계

[시작: 공용 컨트롤](getting-started-common-controls.md)