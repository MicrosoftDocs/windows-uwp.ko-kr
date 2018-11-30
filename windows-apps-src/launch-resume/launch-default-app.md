---
title: URI에 대한 기본 앱 실행
description: URI(Uniform Resource Identifier)에 대한 기본 앱 시작 방법을 알아봅니다. URI를 사용하면 다른 앱을 실행하여 특정 작업을 수행할 수 있습니다. 이 항목에서는 Windows에 기본 제공되는 다양한 URI 스키마에 대한 개요도 제공합니다.
ms.assetid: 7B0D0AF5-D89E-4DB0-9B79-90201D79974F
ms.date: 06/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 143aa8310cdfe9dd5f0be29bf07f03c23293a647
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8216044"
---
# <a name="launch-the-default-app-for-a-uri"></a>URI에 대한 기본 앱 실행


**중요 API**

- [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
- [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
- [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

URI(Uniform Resource Identifier)에 대한 기본 앱 시작 방법을 알아봅니다. URI를 사용하면 다른 앱을 실행하여 특정 작업을 수행할 수 있습니다. 이 항목에서는 Windows에 기본 제공되는 다양한 URI 스키마에 대한 개요도 제공합니다. 사용자 지정 URI를 실행할 수도 있습니다. 사용자 지정 URI 스키마를 등록하고 URI 활성화를 처리하는 방법에 대한 자세한 내용은 [URI 활성화 처리](handle-uri-activation.md)를 참조하세요.

URI 스키마를 사용하면 하이퍼링크를 클릭하여 앱을 열 수 있습니다. **mailto:** 를 사용하여 새 메일을 시작하는 것처럼 **http:** 를 사용하여 기본 웹 브라우저를 열 수 있습니다.

이 항목에서는 Windows에 기본 제공되는 다음 URI 스키마를 설명합니다.

| URI 스키마 | 시작 |
| ----------:|----------|
|[bingmaps:, ms-drive-to: 및 ms-walk-to: ](#maps-app-uri-schemes) | 지도 앱 |
|[http:](#http-uri-scheme) | 기본 웹 브라우저 |
|[mailto:](#email-uri-scheme) | 기본 메일 앱 |
|[ms-call:](#call-app-uri-scheme) |  통화 앱 |
|[ms-chat:](#messaging-app-uri-scheme) | 메시지 앱 |
|[ms-people:](#people-app-uri-scheme) | 피플 앱 |
|[ms-photos:](#photos-app-uri-scheme) | 사진 앱 |
|[ms-settings:](#settings-app-uri-scheme) | 설정 앱 |
|[ms-store:](#store-app-uri-scheme)  | 스토어 앱 |
|[ms-tonepicker:](#tone-picker-uri-scheme) | 톤 선택기 |
|[ms-yellowpage:](#nearby-numbers-app-uri-scheme) | 근처 전화 번호 앱 |
|[msnweather:](#weather-app-uri-scheme) | 날씨 앱 |

<br>
예를 들어 다음 URI는 기본 브라우저를 열고 Bing 웹 사이트를 표시합니다.

`http://bing.com`

사용자 지정 URI 스키마를 실행할 수도 있습니다. 해당 URI를 처리하는 앱이 설치되지 않은 경우 사용자가 설치할 앱을 권장할 수 있습니다. 자세한 내용은 [URI를 처리할 수 없는 경우 앱 권장](#recommend-an-app-if-one-is-not-available-to-handle-the-uri)을 참조하세요.

일반적으로 개발자 앱에서는 실행된 앱을 선택할 수 없습니다. 사용자가 어떤 앱이 실행되고 있는지 확인합니다. 두 개 이상의 앱을 등록하여 동일한 URI 스키마를 처리할 수 있습니다. 예약된 URI 스키마는 예외입니다. 예약된 URI 스키마의 등록은 무시됩니다. 예약된 URI 스키마의 전체 목록은 [URI 활성화 처리](handle-uri-activation.md)를 참조하세요. 둘 이상의 앱에서 동일한 URI 스키마를 등록할 수 있는 경우 앱에서 실행할 특정 앱을 권장할 수 있습니다. 자세한 내용은 [URI를 처리할 수 없는 경우 앱 권장](#recommend-an-app-if-one-is-not-available-to-handle-the-uri)을 참조하세요.

### <a name="call-launchuriasync-to-launch-a-uri"></a>LaunchUriAsync를 호출하여 URI 실행

[**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 메서드를 사용하여 URI를 실행합니다. 이 메서드를 호출할 때 앱은 포그라운드 앱이어야 합니다. 즉, 사용자에게 표시되어야 합니다. 이 요구 사항은 사용자가 제어권을 갖도록 하는 데 도움이 됩니다. 이 요구 사항을 충족하려면 모든 URI 실행을 앱의 UI에 직접 연결해야 합니다. 사용자는 항상 URI 실행을 시작하기 위해 일부 작업을 수행해야 합니다. URI를 실행하려는 경우 앱이 포그라운드에 없으면 URI가 실행되지 않으며 오류 콜백이 호출됩니다.

먼저 URI를 나타내는 [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/system.uri.aspx) 개체를 만든 다음 이 개체를 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 메서드로 전달합니다. 반환되는 결과를 사용하여 다음 예제에 표시된 대로 호출에 성공했는지 확인합니다.

```cs
private async void launchURI_Click(object sender, RoutedEventArgs e)
{
   // The URI to launch
   var uriBing = new Uri(@"http://www.bing.com");

   // Launch the URI
   var success = await Windows.System.Launcher.LaunchUriAsync(uriBing);

   if (success)
   {
      // URI launched
   }
   else
   {
      // URI launch failed
   }
}
```

경우에 따라 운영 체제에서 사용자에게 실제로 앱을 전환할 것인지 묻는 메시지를 표시합니다.

![회색으로 표시된 앱 배경에 오버레이된 경고 대화 상자. 이 대화 상자에서는 앱을 전환할지 여부를 사용자에게 질문하며 오른쪽 아래에는 '예'와 '아니요' 단추가 있고 ‘아니요' 단추가 강조 표시되어 있습니다.](images/warningdialog.png)

이 메시지가 항상 발생하도록 하려면 [**Windows.System.LauncherOptions.TreatAsUntrusted**](https://msdn.microsoft.com/library/windows/apps/hh701442) 속성을 사용하여 운영 체제에 경고를 표시하라고 지시합니다.

```cs
// The URI to launch
var uriBing = new Uri(@"http://www.bing.com");

// Set the option to show a warning
var promptOptions = new Windows.System.LauncherOptions();
promptOptions.TreatAsUntrusted = true;

// Launch the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriBing, promptOptions);
```

### <a name="recommend-an-app-if-one-is-not-available-to-handle-the-uri"></a>URI를 처리할 수 없는 경우 앱 권장

경우에 따라 사용자는 시작할 URI를 처리하는 앱을 설치하지 않으려 할 수 있습니다. 기본적으로 운영 체제는 스토어에서 적절한 앱을 검색할 수 있는 링크를 사용자에게 제공하여 이러한 경우를 처리합니다. 이러한 경우 사용자에게 필요한 앱에 대한 특정 권장 지침을 제공하려면 시작할 URI와 함께 해당 권장 지침을 전달할 수 있습니다.

권장 사항은 둘 이상의 앱이 URI 스키마를 처리하도록 등록된 경우에도 유용합니다. 특정 앱을 권장하면 Windows에서 해당 앱(이미 설치된 경우)을 엽니다.

권장하려면 [**LauncherOptions.preferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)을 스토어에서 권장하려는 앱의 패키지 패밀리 이름으로 설정하여 [**Windows.System.Launcher.LaunchUriAsync(Uri, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701484) 메서드를 호출합니다. 운영 체제에서는 이 정보를 사용하여 스토어에서 앱을 검색하는 일반적인 옵션을 스토어에서 권장 앱을 다운로드하는 특정 옵션으로 바꿉니다.

```cs
// Set the recommended app
var options = new Windows.System.LauncherOptions();
options.PreferredApplicationPackageFamilyName = "Contoso.URIApp_8wknc82po1e";
options.PreferredApplicationDisplayName = "Contoso URI Ap";

// Launch the URI and pass in the recommended app
// in case the user has no apps installed to handle the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriContoso, options);
```

### <a name="set-remaining-view-preference"></a>나머지 보기 기본 설정 지정

[**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)를 호출하는 원본 앱은 URI가 시작된 후 화면에 유지되도록 요청할 수 있습니다. 기본적으로 Windows는 URI를 처리하는 대상 앱과 원본 앱 사이에 모든 사용 가능한 공간을 동일하게 공유하려고 합니다. 원본 앱은 [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314) 속성을 사용하여 앱 창이 거의 모든 사용 가능한 공간을 사용하려고 한다는 것을 운영 체제에 나타냅니다. **DesiredRemainingView**를 사용하여 URI가 시작된 후 원본 앱이 화면에서 유지될 필요가 없고 대상 앱으로 완전히 대체될 수 있다는 것을 나타낼 수도 있습니다. 이 속성은 호출 앱의 기본 창 크기만 지정합니다. 화면에 동시에 나타날 수도 있는 다른 앱의 동작은 지정하지 않습니다.

**참고**Windows 고려 같은 여러 가지 요소 예를 들어 원본 앱의 최종 창 크기를 결정할 때 원본 앱의 기본 설정, 화면, 화면 방향 및 등에서 앱의 수입니다. [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)를 설정해도 원본 앱에 대한 특정 창 관리 동작이 보장되지 않습니다.

```cs
// Set the desired remaining view.
var options = new Windows.System.LauncherOptions();
options.DesiredRemainingView = Windows.UI.ViewManagement.ViewSizePreference.UseLess;

// Launch the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriContoso, options);
```

## <a name="uri-schemes"></a>URI 스키마 ##

다음은 다양한 URI 스키마에 대한 설명입니다.

### <a name="call-app-uri-scheme"></a>통화 앱 URI 스키마

**ms-call:** URI 스키마를 사용하여 통화 앱을 실행할 수 있습니다.

| URI 스키마       | 결과                   |
|------------------|--------------------------|
| ms-call:settings | 호출 앱 설정 페이지입니다. |

### <a name="email-uri-scheme"></a>메일 URI 스키마

**mailto:** URI 스키마를 사용하여 기본 메일 앱을 실행할 수 있습니다.

| URI 스키마 |결과                          |
|------------|---------------------------------|
| mailto:    | 기본 메일 앱을 실행합니다. |
| mailto:\[email address\] | 메일 앱을 실행하고 받는 사람 줄에 메일 주소를 지정한 새 메서드를 작성합니다. 메일은 사용자가 보내기를 탭해야 전송됩니다. |

### <a name="http-uri-scheme"></a>HTTP URI 스키마

**http:** URI 스키마를 사용하여 기본 웹 브라우저를 실행할 수 있습니다.

| URI 스키마 | 결과                           |
|------------|-----------------------------------|
| http:      | 기본 웹 브라우저를 실행합니다. |

### <a name="maps-app-uri-schemes"></a>지도 앱 URI 스키마

**bingmaps:**, **ms-drive-to:** 및 **ms-walk-to:** URI 스키마로 [Windows 지도 앱을 실행](launch-maps-app.md)하여 특정 지도, 길 찾기 및 검색 결과를 표시할 수 있습니다. 예를 들어 다음 URI는 Windows 지도 앱을 열고 뉴욕시를 중심으로 지도를 표시합니다.

`bingmaps:?cp=40.726966~-74.006076`

![Windows 지도 앱의 예](images/mapnyc.png)

자세한 내용은 [Windows 지도 앱 실행](launch-maps-app.md)을 참조하세요. 사용자 고유의 앱에서 지도 컨트롤을 사용하려면 [2D, 3D 및 Streetside 뷰로 지도 표시](https://msdn.microsoft.com/library/windows/apps/mt219695)를 참조하세요.

### <a name="messaging-app-uri-scheme"></a>메시징 앱 URI 스키마

**ms-chat:** URI 스키마를 사용하여 Windows 메시지 앱을 실행할 수 있습니다.

| URI 스키마 |결과 |
|------------|--------|
| ms-chat:   | 메시지 앱을 실행합니다. |
| ms-chat:?ContactID={contacted}  |  메시지 응용 프로그램이 특정 연락처의 정보를 사용하여 실행되도록 허용합니다.   |
| ms-chat:?Body={body} | 메시지 응용 프로그램이 메시지 내용으로 사용할 문자열을 사용하여 실행되도록 허용합니다.|
| ms-chat:?Addresses={address}&Body={body} | 메시지 응용 프로그램이 특정 주소 정보 및 메시지 내용으로 사용할 문자열을 사용하여 실행되도록 허용합니다. 참고: 주소를 연결할 수 있습니다. |
| ms-chat:?TransportId={transportId}  | 메시지 응용 프로그램이 특정 전송 ID를 사용하여 실행되도록 허용합니다. |

### <a name="tone-picker-uri-scheme"></a>톤 선택기 URI 스키마

**ms-tonepicker:** URI 스키마를 사용하여 벨소리, 알람 및 시스템 톤을 선택할 수 있습니다. 새 벨소리를 저장하고 톤의 표시 이름을 가져올 수도 있습니다.

| URI 스키마 | 결과 |
|------------|---------|
| ms-tonepicker: | 벨소리, 알람 및 시스템 톤을 선택합니다. |

매개 변수는 [ValueSet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset.aspx)를 통해 LaunchURI API로 전달됩니다. 자세한 내용은 [ms-tonepicker URI 스키마를 사용하여 신호음 선택 및 저장](launch-ringtone-picker.md)을 참조하세요.

### <a name="nearby-numbers-app-uri-scheme"></a>근처 전화 번호 앱 URI 스키마

**ms-yellowpage:** URI 스키마를 사용하여 근처 전화 번호 앱을 실행할 수 있습니다.

| URI 스키마 | 결과 |
|------------|---------|
| ms-yellowpage:?input=\[keyword\]&amp;method=\[문자열 또는 T9\] | 근처 전화 번호 앱을 시작합니다.<br>`input` 검색하려는 키워드를 나타냅니다.<br>`method` 검색의 유형(문자열 또는 T9 검색)을 나타냅니다.<br>`method`가 `T9`(키보드의 일종)인 경우 `keyword`는 검색할 T9 키보드 문자에 매핑되는 숫자 문자열이어야 합니다.<br>`method`가 `String`인 경우 `keyword`는 검색할 키워드입니다. |

### <a name="people-app-uri-scheme"></a>피플 앱 URI 스키마

**ms-people:** URI 스키마를 사용하여 피플 앱을 실행할 수 있습니다.
자세한 내용은 [피플 앱 실행](launch-people-apps.md)을 참조하세요.

### <a name="photos-app-uri-scheme"></a>사진 앱 URI 스키마

**ms-photos:** URI 스키마로 사진 앱을 실행하여 이미지를 보거나 비디오를 편집할 수 있습니다. 예를 들어,  
이미지를 보려면 다음을 수행합니다. `ms-photos:viewer?fileName=c:\users\userName\Pictures\image.jpg`  
또는 비디오를 편집하려면 다음을 수행합니다. `ms-photos:videoedit?InputToken=123abc&Action=Trim&StartTime=01:02:03`  

> [!NOTE]
> 비디오를 편집하거나 이미지를 표시하는 URI는 데스크톱에서만 사용할 수 있습니다.

| URI 스키마 |결과 |
|------------|--------|
| ms-photos:viewer?fileName={filename} | 사진 앱을 실행하여 지정된 이미지를 봅니다. 여기서, {filename}은 정규화된 경로 이름입니다. 예를 들어, `c:\users\userName\Pictures\ImageToView.jpg` |
| ms-photos:videoedit?InputToken={input token} | 파일 토큰에 의해 표시되는 파일을 위해 사진 앱을 비디오 편집 모드로 실행합니다. **InputToken**은 필수입니다. [SharedStorageAccessManager](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager)를 사용하여 파일의 토큰을 가져옵니다. |
| ms-photos:videoedit?Action={action} | 지정된 비디오 편집 모드에서 사진 앱을 여는 선택적 매개 변수입니다. 여기서, {action}은 **SlowMotion**, **FrameExtraction**, **Trim**, **View**, **Ink** 중 하나입니다. 이를 지정하지 않는 경우 기본값은 **View**입니다. |
| ms-photos:videoedit?StartTime={timespan} | 비디오 재생을 시작할 위치를 지정하는 선택적 매개 변수입니다. `{timespan}` `"hh:mm:ss.ffff"` 형식을 사용해야 합니다. 이를 지정하지 않는 경우 기본값은 다음과 같습니다. `00:00:00.0000` |

### <a name="settings-app-uri-scheme"></a>설정 앱 URI 스키마

**ms-settings:** URI 스키마를 사용하여 [Windows 설정 앱을 실행](launch-settings-app.md)할 수 있습니다. 설정 앱 실행은 개인 정보 인식 앱 작성의 중요한 부분입니다. 앱에서 중요한 리소스에 액세스할 수 없는 경우 사용자에게 해당 리소스의 개인 정보 설정에 대한 편리한 링크를 제공하는 것이 좋습니다. 예를 들어 다음 URI는 설정 앱을 열고 카메라 개인 정보 설정을 표시합니다.

`ms-settings:privacy-webcam`

![카메라 개인 정보 설정](images/privacyawarenesssettingsapp.png)

자세한 내용은 [Windows 설정 앱 실행](launch-settings-app.md) 및 [개인 정보 인식 앱에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh768223)을 참조하세요.

### <a name="store-app-uri-scheme"></a>스토어 앱 URI 스키마

**ms-windows-store:** URI 스키마를 사용하여 [UWP 앱을 실행](launch-store-app.md)할 수 있습니다. 제품 세부 정보 페이지, 제품 리뷰 페이지, 검색 페이지 등을 엽니다. 예를 들어 다음 URI는 UWP 앱을 열고 Microsoft Store의 홈페이지를 시작합니다.

`ms-windows-store://home/`

자세한 내용은 [UWP 앱 실행](launch-store-app.md)을 참조하세요.

### <a name="weather-app-uri-scheme"></a>날씨 앱 URI 스키마

사용 합니다 **msnweather:** URI 스키마를 날씨 응용 프로그램을 실행 합니다.

| URI 스키마 | 결과 |
|------------|---------|
| msnweather://forecast?la= \[latitude\] 및 lo = \ [longitude\] | 위치 지리적 좌표를 기반으로 예측 페이지에서 날씨 앱을 실행 합니다.<br>`latitude` latitude 위치를 나타냅니다.<br> `longitude` 경도 위치를 나타냅니다.<br> |
