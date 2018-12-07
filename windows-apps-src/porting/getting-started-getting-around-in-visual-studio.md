---
description: Visual Studio 둘러보기
title: Visual Studio 둘러보기
ms.assetid: 7FBB50A2-6D22-4082-B333-5153DADDDE9A
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 82cb45dae1a4b9b1a9db8fabc044edf8157f1eb1
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8800179"
---
# <a name="getting-started-getting-around-in-visual-studio"></a>시작: Visual Studio 둘러보기


## <a name="getting-around-in-microsoft-visual-studio"></a>Microsoft Visual Studio 둘러보기

이제 앞에서 만든 프로젝트로 돌아가서 Microsoft Visual Studio IDE(통합 개발 환경)를 탐색하는 방법에 대해 알아보겠습니다.

Xcode 개발자는 아래의 기본 보기에 대해 잘 알고 있어야 합니다. 이 그림에서 왼쪽 창에는 소스 파일이, 가운데 창에는 편집기(UI 또는 소스 코드)가, 오른쪽 창에는 컨트롤 및 속성이 있습니다.

![Xcode 개발 환경](images/ios-to-uwp/xcode-ide.png)

Microsoft Visual Studio도 매우 유사하지만 기본 보기에서 왼쪽의 **도구 상자**에 컨트롤이 있습니다. 소스 파일은 오른쪽의 **솔루션 탐색기**에 있고 속성은 **솔루션 탐색기** 창의 **속성**에 있습니다.

![Visual Studio 개발 환경](images/ios-to-uwp/vs-ide.png)

익숙하지 않은 경우 Visual Studio에서 창을 다시 정렬하여 화면 왼쪽에 소스 파일을 두고, 오른쪽에 도구 상자를 둘 수 있습니다. 실제로 창의 제목 표시줄을 클릭하고 끌어서 위치를 변경할 수 있으며, 이 경우 Visual Studio에서 마우스를 놓으면 고정되는 위치를 알려 주는 음영 상자를 표시합니다. 많은 창에는 해당 제목 표시줄에 작은 그리기 고정 아이콘도 있습니다. 이를 통해 패널을 현재 위치에 잠가, 있는 그대로 고정할 수 있습니다. 창의 고정을 해제하고 축소하여 공간을 절약할 수 있습니다. 이는 모니터가 좁은 쪽에 있는 경우에 유용합니다. 작업을 망친 경우 **창** 메뉴에서 **창 레이아웃 다시 설정**을 선택하여 순서를 복원합니다.

## <a name="adding-controls-setting-their-properties-and-responding-to-events"></a>컨트롤 추가, 속성 설정 및 이벤트에 응답

이제 프로젝트에 몇 가지 컨트롤을 추가해 보겠습니다. 그런 다음 몇 가지 속성을 변경하고 컨트롤 이벤트 중 하나에 응답하도록 코드를 작성합니다.

Xcode에서 컨트롤을 추가하려면 원하는 .xib 파일 또는 스토리보드를 열고 아래 표시된 대로 **모서리가 둥근 직사각형 단추** 또는 **레이블**과 같은 컨트롤을 끌어 놓습니다.

![Xcode에서 UI 디자인](images/ios-to-uwp/xcode-add-button-label.png)

Visual Studio에서도 비슷한 작업을 수행할 수 있습니다. **도구 상자**에서 **단추** 컨트롤을 MainPage.xaml 파일의 디자인 화면으로 끌어 놓습니다.

**TextBlock** 컨트롤도 같은 방법으로 작업합니다.

![Visual Studio에서 UI 디자인](images/ios-to-uwp/vs-add-button-label.png)

레이아웃 및 바인딩 정보를 .xib 또는 스토리보드 파일에 숨기는 Xcode와 달리, Visual Studio에서는 이러한 세부 정보를 풍부하고 편집 가능하며 선언적인 XML 같은 언어로 저장하는 데 사용되는 XAML 파일을 편집하는 것이 좋습니다. XAML(Extensible Application Markup Language)에 대한 자세한 내용은 [XAML 개요](https://msdn.microsoft.com/library/windows/apps/mt185595)를 참조하세요. **디자인** 창에 표시되는 모든 항목은 **XAML** 창에서 정의됩니다. 필요한 경우 **XAML** 창에서 세부적으로 제어할 수 있으며 자세히 알고 있으면 사용자 인터페이스 코드를 수동으로 신속하게 개발할 수 있습니다. 하지만 여기서는 **디자인** 창과 **속성** 창을 중심으로 살펴보겠습니다.

단추의 세부 정보를 변경해 보겠습니다. Xcode에서 단추의 이름을 변경하려면 해당 속성 패널에서 **제목** 필드 값을 변경합니다.

Visual Studio에서도 매우 유사하게 작업을 수행합니다. **디자인** 창에서 단추를 탭하여 포커스를 설정합니다. 그런 다음 **속성** 창에서 **콘텐츠** 값을 "Button"에서 "Press Me"로 변경합니다. 이제 **이름** 값을 그림과 같이 “&lt;No Name&gt;”에서 “myButton”으로 변경하여 단추 컨트롤의 이름을 업데이트합니다.

![Visual Studio의 단추 속성 창](images/ios-to-uwp/vs-button-properties.png)

이제 **TextBlock** 컨트롤의 내용을 "Hello, World!"로 변경하도록 일부 코드를 작성해 보겠습니다.

Xcode에서 코드를 작성한 다음 컨트롤에 연결하거나, 종종 그림과 같이 Ctrl 키를 누른 상태에서 단추를 소스 코드로 끌어 이벤트를 컨트롤과 연결합니다.

![Xcode에서 이벤트에 단추 연결](images/ios-to-uwp/xcode-add-button-event.png)

```swift
// Swift implementation.

@IBAction func buttonPressed(sender: UIButton) {
    
}
```

Visual Studio도 유사합니다. **속성**의 오른쪽 위에 번개 모양의 단추가 있습니다. 여기에 선택한 컨트롤과 연결된 이벤트가 나열됩니다.

![Visual Studio의 단추 이벤트 목록](images/ios-to-uwp/vs-button-event.png)

단추의 클릭 이벤트에 대한 코드를 추가하려면 먼저 **디자인** 창에서 단추를 선택합니다. 그런 다음 번개 모양 단추를 클릭하고 **클릭**이라는 이름 옆의 빈 상자를 클릭합니다. Visual Studio에서 다음과 같이 "myButton\_Click" 이벤트를 **클릭** 상자에 추가하고 MainPage.xaml.cs 파일에 해당 이벤트 처리기를 추가한 다음 표시합니다.

```csharp
private void myButton_Click(object sender, RoutedEventArgs e)
{

}
```

이제 **TextBlock** 컨트롤을 연결해 보겠습니다. Xcode에서는 다음과 같이 Ctrl 키를 누른 상태에서 단추를 소스 코드 파일로 끌어 컨트롤을 해당 정의와 연결합니다.

![Xcode에서 레이블에 정의 연결](images/ios-to-uwp/xcode-add-button-reference.png)

```swift
// Swift implentation.

@IBOutlet weak var myLabel : UILabel
```

Visual Studio에서는 컨트롤을 연결할 필요가 없습니다. 항상 자동으로 연결됩니다. 하지만 일부 속성을 변경해 보겠습니다.

1.  MainPage.xaml 파일 탭을 탭합니다.
2.  **디자인** 창에서 **TextBlock** 컨트롤을 탭합니다.
3.  **속성** 창에서 렌치 단추를 탭하여 해당 속성을 표시합니다.
4.  **이름** 상자에서 “&lt;No Name&gt;”을 “myLabel”로 변경합니다.

![Visual Studio의 레이블 속성 창](images/ios-to-uwp/vs-label-properties.png)

이제 단추의 클릭 이벤트에 일부 코드를 추가합니다. 이렇게 하려면 MainPage.xaml.cs 파일을 탭하고 다음 코드를 myButton\_Click 이벤트 처리기에 추가합니다.

```csharp
private void myButton_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.    
    myLabel.Text = "Hello, World!";
}
```

이는 Swift에서 작성한 것과 유사합니다.

```swift
@IBAction func buttonPressed(sender: UIButton) {
    myLabel.text = "Hello, World!"
}
```

마지막으로, 앱을 실행하려면 **디버그** 메뉴를 선택한 다음 **디버깅 시작**을 선택합니다(또는 F5 키를 누름). 앱이 시작되면 "Press Me" 단추를 클릭합니다. 그러면 레이블의 내용이 "TextBlock"에서 "Hello, World!"로 변경됩니다.

![첫 번째 연습 실행 결과: Hello, World!](images/ios-to-uwp/vs-hello-world.png)

앱을 종료하려면 Visual Studio로 돌아가서 **디버그** 메뉴를 탭한 후 **디버깅 중지**를 탭하거나 Shitf+F5를 누릅니다. Visual Studio에서는 여러 장치에서 앱을 시험하여 각 장치에서 제대로 실행되는지 확인할 수 있습니다.

## <a name="next-step"></a>다음 단계

[시작: 공용 컨트롤](getting-started-common-controls.md)

