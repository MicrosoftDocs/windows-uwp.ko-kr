---
author: mcleanbyron
ms.assetid: 3C03FDD8-FA61-4E7B-BDCA-3C29DFEA20E4
description: "Microsoft 스토어 참여 및 수익 창출 SDK를 설치한 후 이 항목의 지침을 따라 앱에서 Ad Mediator 컨트롤을 사용하세요."
title: "Ad Mediator 컨트롤 추가 및 사용"
translationtype: Human Translation
ms.sourcegitcommit: 8c3f1997427a7c3d4f4b4b7acc876a2a091e4553
ms.openlocfilehash: a0d73b50207d251c079714265845a816f4ac23da

---

# Ad Mediator 컨트롤 추가 및 사용


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


[Microsoft 스토어 참여 및 수익 창출 SDK를 설치](http://aka.ms/store-em-sdk)한 후 이 항목의 지침을 따라 앱에서 Ad Mediator 컨트롤을 사용하세요. 광고 조정에서 현재 지원되는 광고 네트워크 및 프로젝트 유형의 목록은 [광고 네트워크 선택 및 관리](select-and-manage-your-ad-networks.md)를 참조하세요.

## 프로젝트에 광고 조정자 컨트롤 추가


프로젝트에 광고 조정자 컨트롤의 인스턴스를 추가하려면

1.  Visual Studio에서 프로젝트를 엽니다.
2.  광고 조정에서 지원되는 [광고 네트워크](select-and-manage-your-ad-networks.md)를 통해 이미 수익을 내고 있는 앱에 광고 조정을 추가할 경우 계속하기 전에 기존 광고 구현과 해당하는 모든 참조를 제거해야 합니다.
3.  **솔루션 탐색기**서 광고를 표시할 앱의 페이지를 찾은 다음 페이지를 두 번 클릭하여 디자이너에서 엽니다.
4.  **도구 상자**에서 새 **AdMediatorControl**을 디자이너에 끌어옵니다. XAML 코드가 아니라 디자이너에 컨트롤을 끌어와야 합니다. 광고를 표시하려는 위치에 컨트롤을 놓습니다. 앱의 여러 영역에 광고를 표시하려면 컨트롤을 여러 개 추가할 수 있습니다.

    **AdMediatorControl**은 다음 **도구 상자** 위치에 있습니다.

    -   UWP(유니버설 Windows 플랫폼) 프로젝트에서, **AdMediator 유니버설** 섹션에 있는 **AdMediatorControl**을 사용합니다.
    -   C# 또는 Visual Basic과 XAML을 사용하여 Windows 8.1 또는 Windows Phone 8.1 프로젝트에서, **AdMediator** 섹션에 있는 **AdMediatorControl**을 사용합니다.
    -   Windows Phone Silverlight 프로젝트에서, **모든 Windows Phone 컨트롤** 섹션에 있는 **AdMediatorControl**을 사용합니다.

    > **참고** C# 또는 Visual Basic과 XAML을 사용하여 UWP, Windows 8.1 또는 Windows Phone 8.1 프로젝트에서 처음으로 **AdMediatorControl** 컨트롤을 디자이너로 끌면 Visual Studio에서 프로젝트에 필요한 Ad Mediator 어셈블리 참조를 추가하지만 컨트롤은 디자이너에 추가되지 않습니다. 컨트롤을 추가하려면 Visual Studio에서 표시하는 메시지에서 확인을 클릭하고 디자이너가 새로 고쳐질 때까지 몇 초간 대기한 후 컨트롤을 다시 디자이너로 끌어옵니다. 그래도 여전히 디자이너에 컨트롤을 추가할 수 없으면 프로젝트가 **모든 CPU**보다는 앱에 적용 가능한 프로세서 아키텍처(예: **x86**)를 대상으로 하도록 합니다. 프로젝트가 빌드 플랫폼에 대해 **모든 CPU**를 대상으로 하는 경우 컨트롤을 디자이너에 추가할 수 없습니다.

5.  Visual Studio는 프로젝트에 광고 조정자 어셈블리 참조를 추가하고 광고 조정자 컨트롤의 고유 ID 및 컨트롤을 비롯해 컨트롤의 XAML을 현재 페이지에 삽입합니다. 어셈블리 참조 및 XAML은 대상 플랫폼에 따라 다릅니다. 예를 들어 UWP(유니버설 Windows 플랫폼) 앱의 경우 어셈블리 이름은 **Microsoft.AdMediator.Universal**이며 생성되는 XAML은 다음 예제와 같습니다.

    ```xml
    // Code that gets added to the XAML page header
    xmlns:Universal="using:Microsoft.AdMediator.Universal"

    // Code that gets added for the ad mediator control
    <Universal:AdMediatorControl x:Name="AdMediator_3D4884"
      Grid.ColumnSpan="2" HorizontalAlignment="Left" Height="250"
      Id="AdMediator-Id-D1FDFDA7-EABB-474C-940C-ECA7FBCFF143" Margin="121,175,0,0"
      VerticalAlignment="Top" Width="300"/>
    ```

    **Name** 요소는 광고 조정을 구성할 때 앱에서 특정 컨트롤을 식별하는 데 도움이 됩니다. 이 요소를 원하는 대로 변경할 수 있지만 **Id** 요소는 변경하거나 복제하지 마세요. 이 **Id**는 앱 내에서 각 컨트롤에 대해 고유해야 합니다.

6.  필요에 따라 컨트롤의 크기와 위치를 조정합니다. 자세한 내용은 [크기 및 위치 조정](#adjust-size-and-position)을 참조하세요.

## 광고 네트워크 구성

원하는 컨트롤을 모두 추가한 후 연결된 서비스를 통해 광고 네트워크를 구성할 수 있습니다.

> **중요** 나중에 다른 AdMediatorControl을 추가할 경우 연결된 서비스를 통해 컨트롤을 다시 구성해야 합니다. 그렇지 않으면 새 컨트롤에서 광고 조정을 사용할 수 없습니다.

광고 네트워크를 구성하려면 다음을 수행합니다.

1.  솔루션 탐색기에서 프로젝트의 이름을 마우스 오른쪽 단추로 클릭하고 **추가**를 클릭한 다음 **연결된 서비스...**를 클릭하여 **연결된 서비스 추가** 창(Visual Studio 2015) 또는 **서비스 관리자** 창(Visual Studio 2013)을 시작합니다.
2.  Visual Studio 2015를 사용 중인 경우 **Ad Mediator**를 클릭한 다음 **구성**을 클릭하여 **Ad Mediator** 창을 엽니다. Visual Studio 2013을 사용 중인 경우 **서비스 관리자**의 왼쪽 창에 있는 **광고 중재자**를 클릭하기만 하면 됩니다.

    프로젝트에 AdMediator.config 파일이 추가됩니다. 초기 광고 네트워크 구성 설정이 프로젝트에서 로컬로 이 파일에 저장됩니다.

3.  **광고 중재자**(Visual Studio 2015) 또는 **서비스 관리자**(Visual Studio 2013) 창에서 **광고 네트워크 선택**을 클릭하고 사용할 광고 네트워크를 선택한 다음 **광고 네트워크 선택** 창에서 **확인**을 클릭합니다.

    > **팁** 지금 당장에는 앱에서 모두 사용하지 않더라도 계정이 있는 네트워크를 모두 추가하는 것이 좋습니다. 앱이 게시된 후 코드를 변경하고 앱을 다시 제출할 필요 없이 개발자 센터에서 각 네트워크를 사용할 빈도를 구성하거나 이전에는 사용되지 않던 네트워크를 사용할 수 있습니다.

    Visual Studio는 선택한 광고 네트워크에 필요한 어셈블리를 가져오고 해당 어셈블리 참조를 프로젝트에 추가합니다. 이 프로세스가 완료되면 **가져오기 상태** 대화 상자에서 **확인**을 클릭합니다.

4.  **광고 중재자**(Visual Studio 2015) 또는 **서비스 관리자**(Visual Studio 2013) 창에서 선택적으로 각 네트워크를 선택하고 **구성**을 클릭하여 앱을 테스트하는 동안 사용할 각 네트워크의 구성 정보를 입력합니다. 이 정보는 프로젝트의 AdMediator.config 파일에 저장됩니다. Windows 개발자 센터 대시보드에서 광고 네트워크 동작을 구성할 때 이 정보를 수정할 수 있습니다. 자세한 내용은 [앱 제출 및 광고 조정 구성](submit-your-app-and-configure-ad-mediation.md)을 참조하세요.
    > **참고** 이 단계 중에 구성 정보를 입력하지 않으면 개발 컴퓨터(UWP 및 Windows 8.1 XAML 앱) 또는 에뮬레이터나 디바이스(Windows Phone 앱)에서 앱을 실행할 때 광고 조정에서 테스트 구성 값을 자동으로 사용합니다.

5.  **광고 조정자**(Visual Studio 2015) 또는 **서비스 관리자**(Visual Studio 2013) 창에서 선택한 각 광고 네트워크가 **가져옴**을 표시하는지 확인합니다. **확인**을 클릭하여 프로젝트의 변경 사항을 제출합니다.

> **참고** 나중에 Microsoft 스토어 참여 및 수익 창출 SDK의 최신 버전으로 업그레이드할 경우 **연결된 서비스**를 다시 시작하여 자동으로 가져온 광고 네트워크 DLL이 올바르게 업데이트되었는지 확인해야 합니다.

### 필요한 기능 선언

각 광고 네트워크에 특정 앱 기능이 필요할 수 있습니다. 이러한 기능은 각 제공자가 **광고 조정자**(Visual Studio 2015용) 또는 **서비스 관리자**(Visual Studio 2013용) 창에 표시됩니다. 광고가 올바르게 표시되도록 앱 매니페스트에서 필요한 기능을 모두 선언해야 합니다.

다음 스크린샷은 Windows 8.1 또는 Windows Phone 8.1 XAML 앱에서 여러 광고 네트워크에 필요한 기능을 보여 줍니다.

![가져온 모든 참조를 보여 주는 서비스 관리자](images/ad-med-8.jpg)

다음 스크린샷은 Windows Phone 8.1 Silverlight 앱에서 여러 광고 네트워크에 필요한 기능을 보여 줍니다.

![가져온 모든 참조를 보여 주는 서비스 관리자](images/ad-med-6.jpg)
### 수동으로 광고 네트워크 DLL 가져오기

경우에 따라 특정 DLL을 가져오지 못할 수도 있습니다. 이 경우 해당 DLL을 수동으로 추가해야 합니다. 링크를 통해 개별 어셈블리를 다운로드하려면 [광고 네트워크 선택 및 관리](select-and-manage-your-ad-networks.md)(영문)를 참조하세요.

> **참고** DLL을 수동으로 추가할 경우 "상위 버전 또는 호환되지 않는 어셈블리에 대한 참조는 프로젝트에 추가할 수 없습니다."라는 오류 메시지가 표시될 수 있습니다. 이 오류를 해결하려면 탐색기에서 DLL을 마우스 오른쪽 단추로 클릭한 다음 **속성**을 선택합니다. 보안 섹션에서 **차단 해제**를 클릭합니다.

![오류 메시지 해결을 위한 차단 해제 단추](images/ad-med-4.png)
## 크기 및 위치 조정

디자이너 또는 XAML 코드에서 광고 조정자 컨트롤의 크기와 위치를 구성할 수 있습니다. 이 크기가 광고 네트워크에서 표시하려는 모든 광고를 표시하는 데 충분한지 확인합니다. 일부 광고 네트워크에서는 canvas가 작아서 전체 광고를 표시할 수 없을 경우 광고를 제공하지 않습니다. 광고 단위가 기본 크기보다 클 경우 canvas 크기를 가장 큰 광고에 맞게 조정할 수 있습니다.

디자이너에 컨트롤을 끌어올 때 기본 컨트롤 크기는 다음과 같습니다.

-   UWP 및 Windows 8.1 XAML: 300 너비 x 250 높이.
-   Windows Phone 8.1 XAML: 400 너비 x 67 높이.
-   Windows Phone 8 및 Windows Phone 8.1 Silverlight: 480 너비 x 80 높이.

아래에 표시된 대로 **너비** 및 **높이** 선택적 매개 변수를 사용하여 기본 광고 크기를 재정의할 수 있습니다.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Width"] = 400;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Height"] = 80;
```

앱의 요구 사항에 따라 다양한 크기와 위치에 맞게 각 컨트롤을 배치하는 방법을 지정할 수도 있습니다. 일부 광고 네트워크의 경우 선택적 매개 변수를 사용하여 조정할 수 있습니다. 예를 들어 Microsoft Advertising의 광고를 왼쪽 아래에 정렬하려면

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["HorizontalAlignment"] = HorizontalAlignment.Left;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["VerticalAlignment"] = VerticalAlignment.Bottom;
```

### Microsoft Advertising에 지원되는 광고 크기

Microsoft Advertising은 다음과 같은 플랫폼에서 실행되는 앱에 대해 IAB(Interactive Advertising Bureau)에서 권장하는 다음과 같은 표준 크기로 된 광고만 지원합니다.

-   Windows 10 및 Windows 8.1의 경우
    -   160 x 600
    -   300 x 250
    -   300 x 600
    -   728 x 90
-   Windows 10 Mobile, Windows Phone 8.1 및 Windows Phone 8:
    -   300 x 50
    -   320 x 50
    -   480 x 80 (이 크기는 Windows Phone Silverlight에서만 지원됨)
    -   640 x 100

Microsoft Advertising에서 지원하는 광고 크기에 맞지 않는 Ad Mediator 컨트롤을 지정하기를 원할 수 있습니다. 예를 들어 다른 크기가 앱의 UI에 더 적합하거나 다른 광고 크기를 지원하는 다른 광고 네트워크를 대상으로 하는 경우가 해당될 수 있습니다. 이 작업을 수행하려면 디자이너나 XAML 코드에서 원하는 컨트롤 크기를 정확하게 지정한 다음 Microsoft Advertising의 **Width** 및 **Height** 선택적 매개 변수를 최대한 컨트롤 범위에 속하며 지원되는 크기에 맞게 지정합니다. 디자이너에서는 컨트롤이 사용자가 지정하는 크기로 정확하게 표시되지만, Microsoft Advertising에서는 **Width** 및 **Height** 선택적 매개 변수를 사용하여 지정하는 크기에 맞는 광고를 제공합니다.

예를 들어 UWP 앱이 있고 Ad Mediator 컨트롤을 300 x 300으로 표시하려면 디자이너나 XAML 코드에서 컨트롤을 300 x 300으로 설정합니다. 그런 다음에 다음 코드에 표시된 대로 Microsoft Advertising에 대해 **Width** 선택적 매개 변수를 300으로 할당하고 **Height** 선택적 매개 변수를 250으로 할당합니다.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Width"] = 300;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Height"] = 250;
```

크기 설정이 Microsoft Advertising과 호환되는지 확인하려면 [광고 조정 구현을 테스트](test-your-ad-mediation-implementation.md)하고 Microsoft Advertising의 테스트 광고가 표시되는지 확인합니다.

## 광고 조정 일시 중지, 다시 시작 및 사용 안 함

앱 런타임 중에 지정된 시간 동안 광고 조정을 일시 중지하려면 AdMediatorControl.Pause() 메서드를 사용합니다. 이렇게 하면 AdMediatorControl.Pause() 메서드를 호출하여 중재를 다시 시작할 때까지 최근 중재가 계속 표시됩니다.

광고 중재자 완전히 사용하지 않으려면 AdMediatorControl.Disable() 메서드를 사용합니다. 그러면 표시 중인 모든 광고가 제거되고 중재자의 메모리 공간이 최소화됩니다. AdMediatorControl.Resume()을 호출하여 조정을 다시 시작할 수 있지만, 광고 조정을 사용하지 않도록 설정하면 평상시보다 시작 시간이 느려집니다.

## 시간 제한 설정

광고 네트워크에서 광고를 요청한 후 해당 요청을 중단하고 다른 네트워크에 요청할 때까지 광고 조정이 대기해야 하는 시간(2-60초)을 지정할 수 있습니다. 기본적으로 모든 광고 네트워크에 대한 제한 시간은 15초입니다.

아래 코드는 Microsoft Advertising에 대한 시간 제한 기간을 지정하는 방법을 보여 줍니다. 필요에 따라 기간과 네트워크를 수정할 수 있습니다.

```CSharp
myAdMediatorControl.AdSdkTimeouts[AdSdkNames.MicrosoftAdvertising] = TimeSpan.FromSeconds(10);
```

> **참고** 개발자 센터 대시보드의 **광고로 수익 창출** 페이지에서 시간 제한 값을 설정할 수도 있습니다. 코드와 대시보드에서 시간 제한을 설정하는 경우 코드에 설정한 값이 대시보드 값을 재정의합니다.

## 이벤트 처리

이벤트를 기록하고 광고 조정 오류를 캡처하는 코드를 추가하면 문제를 해결하는 데 도움이 될 수 있습니다. 아래 코드 샘플은 컨트롤의 특정 이벤트에 대한 이벤트 처리기를 추가합니다.

```CSharp
// add this during initialization of your app

    AdMediator_Bottom.AdSdkError += AdMediator_Bottom_AdError;
    AdMediator_Bottom.AdMediatorFilled += AdMediator_Bottom_AdFilled;
    AdMediator_Bottom.AdMediatorError += AdMediator_Bottom_AdMediatorError;
    AdMediator_Bottom.AdSdkEvent += AdMediator_Bottom_AdSdkEvent;

// and then add these functions

void AdMediator_Bottom_AdSdkEvent(object sender, Microsoft.AdMediator.Core.Events.AdSdkEventArgs e)
{
    Debug.WriteLine("AdSdk event {0} by {1}", e.EventName, e.Name);}

void AdMediator_Bottom_AdMediatorError(object sender, Microsoft.AdMediator.Core.Events.AdMediatorFailedEventArgs e)
{
    Debug.WriteLine("AdMediatorError:" + e.Error + " " + e.ErrorCode );
    // if (e.ErrorCode == AdMediatorErrorCode.NoAdAvailable)
    // AdMediator will not show an ad for this mediation cycle
}

void AdMediator_Bottom_AdFilled(object sender, Microsoft.AdMediator.Core.Events.AdSdkEventArgs e)
{
    Debug.WriteLine("AdFilled:" + e.Name);
}

void AdMediator_Bottom_AdError(object sender, Microsoft.AdMediator.Core.Events.AdFailedEventArgs e)
{
    Debug.WriteLine("AdSdkError by {0} ErrorCode: {1} ErrorDescription: {2} Error: {3}", e.Name, e.ErrorCode, e.ErrorDescription, e.Error);
}
```

## 광고 네트워크에서 처리되지 않은 예외 처리

> **참고** 테스트 과정에서 앱 충돌을 방지하기 위해 앱 내에서 처리되어야 하지만 특정 광고 네트워크에서 처리되지 않은 많은 예외를 식별했습니다. 아래 코드 샘플을 복사하여 App.xaml.cs 파일에 붙여넣는 것이 좋습니다.

C# 및 XAML을 사용하여 UWP, Windows 8.1 또는 Windows Phone 앱에 사용할 코드

```CSharp
// In App.xaml.cs file, register with the UnhandledException event handler.
UnhandledException += App_UnhandledException;

void App_UnhandledException(object sender, UnhandledExceptionEventArgs e)
   {
      if (e != null)
      {
         Exception exception = e.Exception;
         if (exception is NullReferenceException && exception.ToString().ToUpper().Contains("SOMA"))
         {
            Debug.WriteLine("Handled Smaato null reference exception {0}", exception);
            e.Handled = true;
            return;
         }
      }
// APP SPECIFIC HANDLING HERE

   if (Debugger.IsAttached)
      {
         // An unhandled exception has occurred; break into the debugger
         Debugger.Break();
      }
   }
```

Windows Phone Silverlight 앱에 사용할 코드

```CSharp
// In App.xaml.cs file, register with the UnhandledException event handler.
UnhandledException += Application_UnhandledException;

// Code to execute on unhandled exceptions
private void Application_UnhandledException(object sender, ApplicationUnhandledExceptionEventArgs e)
{
    if (e != null)
   {
       Exception exception = e.ExceptionObject;
       if ((exception is XmlException || exception is NullReferenceException) && exception.ToString().ToUpper().Contains("INNERACTIVE"))
       {
           Debug.WriteLine("Handled Inneractive exception {0}", exception);
           e.Handled = true;
           return;
       }
       else if (exception is NullReferenceException && exception.ToString().ToUpper().Contains("SOMA"))
       {
           Debug.WriteLine("Handled Smaato null reference exception {0}", exception);
           e.Handled = true;
           return;
       }
       else if ((exception is System.IO.IOException || exception is NullReferenceException) && exception.ToString().ToUpper().Contains("GOOGLE"))
      {
          Debug.WriteLine("Handled Google exception {0}", exception);
          e.Handled = true;
          return;
       }
       else if ((exception is NullReferenceException || exception is XamlParseException) && exception.ToString().ToUpper().Contains("MICROSOFT.ADVERTISING"))
       {
           Debug.WriteLine("Handled Microsoft.Advertising exception {0}", exception);
           e.Handled = true;
           return;
       }

   }
// APP SPECIFIC HANDLING HERE

if (Debugger.IsAttached)
   {
       // An unhandled exception has occurred; break into the debugger
       Debugger.Break();
   }
   //e.Handled = true;
}
```

## 관련 항목

* [광고 네트워크 선택 및 관리](select-and-manage-your-ad-networks.md)
* [광고 조정 구현 테스트](test-your-ad-mediation-implementation.md)
* [앱 제출 및 광고 조정 구성](submit-your-app-and-configure-ad-mediation.md)
* [광고 조정 문제 해결](troubleshoot-ad-mediation.md)
 

 



<!--HONumber=Jun16_HO4-->


