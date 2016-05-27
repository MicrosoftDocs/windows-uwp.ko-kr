---
author: mcleanbyron
Description: 다음은 광고 조정과 관련된 일반적인 여러 개발 문제를 해결하는 방법입니다.
title: 광고 조정 문제 해결
ms.assetid: 8728DE4F-E050-4217-93D3-588DD3280A3A
---

# 광고 조정 문제 해결


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

다음은 광고 조정과 관련된 일반적인 여러 개발 문제를 해결하는 방법입니다.

**디자인 화면에 AdMediatorControl을 추가할 수 없습니다.**  
C# 또는 Visual Basic과 XAML을 사용하여 UWP(유니버설 Windows 플랫폼), Windows 8.1 또는 Windows Phone 8.1 프로젝트에서 처음으로 디자이너에 **AdMediatorControl** 컨트롤을 끌어오는 경우, Visual Studio에서 필수 광고 조정자 어셈블리 참조가 프로젝트에 추가되지만 컨트롤은 디자이너에 아직 추가되지 않습니다. 컨트롤을 추가하려면 Visual Studio에서 표시하는 메시지에서 확인을 클릭하고 디자이너가 새로 고쳐질 때까지 몇 초간 대기한 후 컨트롤을 다시 디자이너로 끌어옵니다.

그래도 여전히 디자이너에 컨트롤을 추가할 수 없으면 프로젝트가 **모든 CPU**보다는 앱에 적용 가능한 프로세서 아키텍처(예: **x86**)를 대상으로 하도록 합니다. 프로젝트가 빌드 플랫폼에 대해 **모든 CPU**를 대상으로 하는 경우 컨트롤을 디자이너에 추가할 수 없습니다.

*
            *Microsoft의 광고를 제공할 때 AdMediatorControl이 런타임에 "&lt;*width* &gt; x &lt;*height*&gt; 지원되지 않음" 오류를 표시합니다. **Microsoft Advertising에서는 [IAB(Interactive Advertising Bureau)에서 권장하는 특정 광고 크기](add-and-use-the-ad-mediator-control.md#supported-ad-sizes-for-microsoft-advertising)만 지원합니다. 디자이너 또는 XAML에서 광고 조정자 컨트롤의 높이 및 너비를 이렇게 지원되는 광고 크기 중 하나로 설정한 경우에도 상황에 따라 크기 조정 및 반올림 문제 때문에 광고 조정 프레임워크에서 광고를 제공하지 못할 수 있습니다. 이 문제를 방지하려면 코드에서 Microsoft Advertising의 선택적 **너비** 및** 높이 매개 변수를 지원되는 광고 크기 중 하나로 할당합니다.

다음 코드 예제는 Microsoft Advertising의 선택적 **너비** 및 **높이** 매개 변수를 728 x 90으로 할당하는 방법을 보여 줍니다.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Width"] = 728;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Height"] = 90;
```

**광고 조정에는 광고 네트워크의 위치(위도/경도)가 포함되지 않음**  
앱에서 위치 기능을 사용하면 광고 조정자 컨트롤이 자동으로 위도/경도 좌표를 가져와 좌표를 지원하는 광고 네트워크에 제공합니다.

**Smaato 광고 컨트롤이 올바르게 정렬되지 않음**  
선택적 매개 변수를 사용하여 SDK 컨트롤에 값을 설정합니다.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Margin"] = new Thickness(0, -20, 0, 0);
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Width"] = 50d;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Height"] = 320d;
```

**AdDuplex 광고 컨트롤이 올바른 크기로 표시되지 않습니다(250×250으로 표시됨).**  
광고 조정에 크기의 값이 설정되지 않았으므로 선택적 **Size** 매개 변수를 사용하여 변경해야 합니다. 예를 들면 다음과 같습니다.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.AdDuplex]["Size"] = "160x600";
```

**“광고 컨트롤이 가려져 있음”이라는 오류 발생**  
앱에서 어떤 식으로든 광고가 가려져 있는 경우 Ad Duplex에서 항상 오류를 표시합니다. 이 오류에 대한 [해결 방법을 읽으세요](http://blog.adduplex.com/2014/01/solving-something-is-covering-ad.mdl).

**"두 파일이 서로 충돌함" 오류 발생**  
앱의 다른 위치에서 Microsoft Advertising 어셈블리를 참조했습니다. 광고 조정은 앱에서 독점적으로 작동하도록 설계되었으며 Microsoft Advertising에 대한 다른 참조가 사용되는 경우 작동하지 않습니다. Microsoft Advertising 참조를 수동으로 제거하고 Microsoft 스토어 참여 및 수익 창출 SDK를 다시 설치하여 오류를 해결하세요.

**AdMediator.config 파일에서 RefreshRate 값을 변경한 후 예기치 않은 동작이 발생**

Visual Studio 프로젝트의 **연결된 서비스 추가** 대화 상자에서 **광고 중재자** 구성 요소를 실행하여 광고 네트워크를 구성하고 난 후 기본 구성 정보는 프로젝트의 AdMediator.config 파일에 저장됩니다. 이 파일은 직접 수정할 수 없습니다. 대신 Windows 개발자 센터 대시보드에 앱 패키지를 업로드한 후 [앱에 대한 광고 조정 설정을 구성할 때](submit-your-app-and-configure-ad-mediation.md) 이 정보를 수정할 수 있습니다(새 광고에 대한 새로 고침 빈도 포함).

AdMediator.config 파일에서 **RefreshRate** 값을 수정하는 경우 이 값은 새로 고침 빈도를 초 단위로 나타내는 30과 120 사이의 정수를 포함해야 합니다. 이 값을 30보다 낮거나 120보다 큰 정수로 설정하면 광고 조정 프레임워크에서는 자동으로 새로 고침 빈도를 60초로 사용합니다.

## 관련 항목

* [광고 네트워크 선택 및 관리](select-and-manage-your-ad-networks.md)
* [광고 조정 컨트롤 추가 및 사용](add-and-use-the-ad-mediator-control.md)
* [광고 조정 구현 테스트](test-your-ad-mediation-implementation.md)
* [앱 제출 및 광고 조정 구성](submit-your-app-and-configure-ad-mediation.md)
 

 


<!--HONumber=May16_HO2-->


