---
title: URI에 대한 기본 앱 실행
description: URI(Uniform Resource Identifier)에 대한 기본 앱 시작 방법을 알아봅니다. URI를 사용하면 다른 앱을 실행하여 특정 작업을 수행할 수 있습니다. 이 항목에서는 Windows에 기본 제공되는 다양한 URI 스키마에 대한 개요도 제공합니다.
ms.assetid: 7B0D0AF5-D89E-4DB0-9B79-90201D79974F
ms.date: 06/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ad25d4ba5d8dfe638d3de3e210f69ea204c48a14
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220016"
---
# <a name="launch-the-default-app-for-a-uri"></a>URI에 대한 기본 앱 실행


**중요 API**

- [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync)
- [**PreferredApplicationPackageFamilyName**](/uwp/api/windows.system.launcheroptions.preferredapplicationpackagefamilyname)
- [**DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview)

URI(Uniform Resource Identifier)에 대한 기본 앱 시작 방법을 알아봅니다. URI를 사용하면 다른 앱을 실행하여 특정 작업을 수행할 수 있습니다. 이 항목에서는 Windows에 기본 제공되는 다양한 URI 스키마에 대한 개요도 제공합니다. 사용자 지정 Uri를 시작할 수도 있습니다. 사용자 지정 URI 체계를 등록 하 고 URI 활성화를 처리 하는 방법에 대 한 자세한 내용은 [uri 활성화 처리](handle-uri-activation.md)를 참조 하세요.

URI 체계를 사용 하면 하이퍼링크를 클릭 하 여 앱을 열 수 있습니다. **Mailto:** 를 사용 하 여 새 전자 메일을 시작할 수 있는 것 처럼 http를 사용 하 여 기본 웹 브라우저를 열 수 있습니다 **.**

이 항목에서는 Windows에 기본 제공 되는 다음 URI 체계에 대해 설명 합니다.

| URI 체계 | 됩니다 |
| ----------:|----------|
|[bingmaps:, ms-to: 및 ms 연습: ](#maps-app-uri-schemes) | Maps 앱 |
|[http](#http-uri-scheme) | 기본 웹 브라우저 |
|[mailto](#email-uri-scheme) | 기본 메일 앱 |
|[ms 호출:](#call-app-uri-scheme) |  앱 호출 |
|[ms 채팅:](#messaging-app-uri-scheme) | 메시징 앱 |
|[ms-사람:](#people-app-uri-scheme) | 피플 앱 |
|[ms-사진:](#photos-app-uri-scheme) | 사진 앱 |
|[ms-설정:](#settings-app-uri-scheme) | 설정 앱 |
|[ms 스토어:](#store-app-uri-scheme)  | 스토어 앱 |
|[ms-tonepicker:](#tone-picker-uri-scheme) | 톤 선택기 |
|[ms-yellowpage:](#nearby-numbers-app-uri-scheme) | 가까운 숫자 앱 |
|[msnweather:](#weather-app-uri-scheme) | 날씨 앱 |

<br>
예를 들어 다음 URI는 기본 브라우저를 열고 Bing 웹 사이트를 표시합니다.

`https://bing.com`

사용자 지정 URI 스키마를 시작할 수도 있습니다. 해당 URI를 처리 하는 앱이 설치 되어 있지 않으면 사용자가 설치할 앱을 추천할 수 있습니다. 자세한 내용은 [앱이 URI를 처리 하는 데 사용할 수 없는 경우 권장](#recommend-an-app-if-one-is-not-available-to-handle-the-uri)합니다 .를 참조 하세요.

일반적으로 앱은 시작 된 앱을 선택할 수 없습니다. 사용자는 시작 되는 앱을 결정 합니다. 둘 이상의 앱이 동일한 URI 체계를 처리 하도록 등록할 수 있습니다. 이에 대 한 예외는 예약 된 URI 체계에 대 한 것입니다. 예약 된 URI 체계의 등록은 무시 됩니다. 예약 된 URI 체계의 전체 목록은 [uri 활성화 처리](handle-uri-activation.md)를 참조 하세요. 둘 이상의 앱이 동일한 URI 체계를 등록할 수 있는 경우 앱은 특정 앱을 시작할 수 있습니다. 자세한 내용은 [앱이 URI를 처리 하는 데 사용할 수 없는 경우 권장](#recommend-an-app-if-one-is-not-available-to-handle-the-uri)합니다 .를 참조 하세요.

### <a name="call-launchuriasync-to-launch-a-uri"></a>LaunchUriAsync를 호출 하 여 URI를 시작 합니다.

[**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync) 메서드를 사용 하 여 URI를 시작 합니다. 이 메서드를 호출할 때 앱은 전경 앱 이어야 합니다. 즉, 사용자에 게 표시 되어야 합니다. 이 요구 사항은 사용자가 컨트롤에 유지 되도록 하는 데 도움이 됩니다. 이 요구 사항을 충족 하려면 모든 URI 시작을 앱의 UI에 직접 연결 해야 합니다. 사용자는 항상 URI 시작을 시작 하기 위해 몇 가지 작업을 수행 해야 합니다. URI를 시작 하려고 하지만 응용 프로그램이 전경에 있지 않으면 시작이 실패 하 고 오류 콜백이 호출 됩니다.

먼저 URI를 나타내는 system.uri 개체를 만든 다음 [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync) 메서드에 [**전달 합니다.**](/dotnet/api/system.uri) 다음 예제와 같이 반환 결과를 사용 하 여 호출이 성공 했는지 확인 합니다.

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

경우에 따라 운영 체제에서 사용자에 게 앱을 실제로 전환 하는지 여부를 묻는 메시지를 표시 합니다.

![경고 대화 상자는 앱의 회색으로 표시 된 배경에 오버레이할. 사용자에 게 앱을 전환 하 고 오른쪽 아래에 ' 예 ' 및 ' 아니요 ' 단추가 있는지 여부를 묻는 대화 상자가 표시 됩니다. ' 아니요 ' 단추가 강조 표시 됩니다.](images/warningdialog.png)

이 메시지가 항상 표시 되도록 하려면 [**Windows.System를 사용 합니다. TreatAsUntrusted**](/uwp/api/windows.system.launcheroptions.treatasuntrusted) 속성을 통해 운영 체제에 경고를 표시 하도록 지시할 수 있습니다.

```cs
// The URI to launch
var uriBing = new Uri(@"http://www.bing.com");

// Set the option to show a warning
var promptOptions = new Windows.System.LauncherOptions();
promptOptions.TreatAsUntrusted = true;

// Launch the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriBing, promptOptions);
```

### <a name="recommend-an-app-if-one-is-not-available-to-handle-the-uri"></a>URI를 처리 하는 데 사용할 수 없는 앱을 권장 합니다.

사용자에 게 시작 하는 URI를 처리 하는 앱이 설치 되어 있지 않은 경우도 있습니다. 기본적으로 운영 체제는 저장소에서 적절 한 앱을 검색 하기 위한 링크를 사용자에 게 제공 하 여 이러한 사례를 처리 합니다. 사용자에 게이 시나리오에서 가져올 앱에 대 한 특정 권장 사항을 제공 하려는 경우 시작 하는 URI와 함께 권장 사항을 전달 하면 됩니다.

권장 사항은 URI 스키마를 처리 하기 위해 둘 이상의 앱을 등록 한 경우에도 유용 합니다. 특정 앱을 권장 하 여 Windows가 이미 설치 되어 있는 경우 해당 앱을 엽니다.

권장 사항을 만들려면Windows.System를 호출 [**합니다. LaunchUriAsync (Uri, LauncherOptions) 메서드 (preferredApplicationPackageFamilyName)**](/uwp/api/windows.system.launcher.launchuriasync#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_Windows_System_LauncherOptions_) 를 사용 하 여 권장 되는 스토어에 있는 앱의 패키지 패밀리 이름으로 설정 [**합니다.**](/uwp/api/windows.system.launcheroptions.preferredapplicationpackagefamilyname) 운영 체제는이 정보를 사용 하 여 저장소에서 권장 앱을 얻기 위한 특정 옵션을 사용 하 여 스토어에서 앱을 검색 하는 일반 옵션을 대체 합니다.

```cs
// Set the recommended app
var options = new Windows.System.LauncherOptions();
options.PreferredApplicationPackageFamilyName = "Contoso.URIApp_8wknc82po1e";
options.PreferredApplicationDisplayName = "Contoso URI Ap";

// Launch the URI and pass in the recommended app
// in case the user has no apps installed to handle the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriContoso, options);
```

### <a name="set-remaining-view-preference"></a>남은 보기 기본 설정 지정

[**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync) 를 호출 하는 소스 앱은 URI 시작 후 화면에 유지 되도록 요청할 수 있습니다. 기본적으로 Windows는 소스 앱과 URI를 처리 하는 대상 앱 간에 사용 가능한 모든 공간을 동일 하 게 공유 하려고 시도 합니다. 원본 앱은 [**DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview) 속성을 사용 하 여 사용 가능한 공간을 늘리거나 줄일 수 있도록 응용 프로그램 창을 선호 하는 운영 체제를 나타낼 수 있습니다. **DesiredRemainingView** 를 사용 하 여 URI가 시작 된 후 소스 앱이 화면에 남아 있을 필요가 없으며 대상 앱으로 완전히 바뀔 수도 있음을 나타낼 수도 있습니다. 이 속성은 호출 하는 앱의 기본 창 크기만 지정 합니다. 동시에 화면에 있을 수도 있는 다른 앱의 동작을 지정 하지 않습니다.

**참고**    Windows는 원본 앱의 최종 창 크기 (예: 원본 앱의 기본 설정, 화면에 있는 앱의 수, 화면 방향 등)를 결정할 때 여러 가지 요인을 고려 합니다. [**DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview)를 설정 하 여 원본 앱에 대 한 특정 창 지정 동작을 보장할 수 없습니다.

```cs
// Set the desired remaining view.
var options = new Windows.System.LauncherOptions();
options.DesiredRemainingView = Windows.UI.ViewManagement.ViewSizePreference.UseLess;

// Launch the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriContoso, options);
```

## <a name="uri-schemes"></a>URI 체계 ##

다양 한 URI 스키마는 아래에 설명 되어 있습니다.

### <a name="call-app-uri-scheme"></a>호출 앱 URI 체계

**Ms call:** URI 체계를 사용 하 여 호출 앱을 시작 합니다.

| URI 체계       | 결과                   |
|------------------|--------------------------|
| ms 호출: 설정 | 앱 설정 페이지를 호출 합니다. |

### <a name="email-uri-scheme"></a>전자 메일 URI 체계

**Mailto:** URI 체계를 사용 하 여 기본 메일 앱을 시작 합니다.

| URI 체계 |결과                          |
|------------|---------------------------------|
| mailto:    | 기본 전자 메일 앱을 시작 합니다. |
| mailto: \[ 전자 메일 주소\] | 전자 메일 앱을 시작 하 고 대상 줄에 지정 된 전자 메일 주소를 사용 하 여 새 메시지를 만듭니다. 사용자가 보내기를 누를 때까지 전자 메일이 전송 되지 않습니다. |

### <a name="http-uri-scheme"></a>HTTP URI 체계

**Http:** URI 체계를 사용 하 여 기본 웹 브라우저를 시작 합니다.

| URI 체계 | 결과                           |
|------------|-----------------------------------|
| http:      | 기본 웹 브라우저를 시작 합니다. |

### <a name="maps-app-uri-schemes"></a>Maps 앱 URI 체계

**Bingmaps:**, **ms-drive to:** 및 **ms 단계별:** URI 체계를 사용 하 여 [Windows maps 앱](launch-maps-app.md) 을 특정 지도, 방향 및 검색 결과에 시작 합니다. 예를 들어 다음 URI는 Windows Maps 앱을 열고 뉴욕 도시를 중심으로 지도를 표시 합니다.

`bingmaps:?cp=40.726966~-74.006076`

![Windows 지도 앱의 예](images/mapnyc.png)

자세한 내용은 [Windows 지도 앱 실행](launch-maps-app.md)을 참조하세요. 사용자 고유의 앱에서 지도 컨트롤을 사용 하려면 [2d, 3d 및 Streetside 뷰가 포함 된 지도 표시](../maps-and-location/display-maps.md)를 참조 하세요.

### <a name="messaging-app-uri-scheme"></a>메시징 앱 URI 체계

**Ms chat:** URI 체계를 사용 하 여 Windows 메시징 앱을 시작 합니다.

| URI 체계 |결과 |
|------------|--------|
| ms 채팅:   | 메시징 앱을 시작 합니다. |
| ms 채팅:? ContactID = {연락}  |  특정 연락처의 정보를 사용 하 여 메시징 응용 프로그램을 시작할 수 있습니다.   |
| ms 채팅:? 본문 = {body} | 메시지 콘텐츠로 사용할 문자열을 사용 하 여 메시징 응용 프로그램을 시작할 수 있습니다.|
| ms 채팅:? 주소 = {address} &본문 = {body} | 메시징 응용 프로그램을 특정 주소 정보를 사용 하 여 시작 하 고 문자열을 사용 하 여 메시지 내용으로 사용할 수 있습니다. 참고: 주소를 연결할 수 있습니다. |
| ms 채팅:? TransportId = {transportId}  | 특정 전송 ID를 사용 하 여 메시징 응용 프로그램을 시작할 수 있습니다. |

### <a name="tone-picker-uri-scheme"></a>톤 선택기 URI 체계

**Tonepicker:** URI 체계를 사용 하 여 벨 소리, 경보 및 시스템 톤을 선택 합니다. 새 벨 소리를 저장 하 고 톤의 표시 이름을 가져올 수도 있습니다.

| URI 체계 | 결과 |
|------------|---------|
| ms-tonepicker: | 벨 소리, 경보 및 시스템 톤을 선택 합니다. |

매개 변수는 [Valueset](/uwp/api/windows.foundation.collections.valueset) 를 통해 LAUNCHURI API로 전달 됩니다. 자세한 내용은 [TONEPICKER URI 체계를 사용 하 여 톤 선택 및 저장](launch-ringtone-picker.md) 을 참조 하세요.

### <a name="nearby-numbers-app-uri-scheme"></a>가까운 숫자 앱 URI 체계

**Yellowpage:** URI 체계를 사용 하 여 가까운 숫자 앱을 시작 합니다.

| URI 체계 | 결과 |
|------------|---------|
| yellowpage:? input = \[ keyword \]&method = \[ String 또는 T9\] | 가까운 숫자 앱을 시작 합니다.<br>`input` 검색 하려는 키워드를 참조 합니다.<br>`method` 검색 형식 (문자열 또는 T9 검색)을 참조 합니다.<br>`method`가 `T9` (키보드 유형) 인 경우는 `keyword` 검색할 T9 키보드 문자에 매핑되는 숫자 문자열 이어야 합니다.<br>`method`가 이면 `String` 는 `keyword` 검색할 키워드입니다. |

### <a name="people-app-uri-scheme"></a>피플 앱 URI 체계

**Ms 사용자:** URI 체계를 사용 하 여 피플 앱을 시작 합니다.
자세한 내용은 [사용자 앱 시작](launch-people-apps.md)을 참조 하세요.

### <a name="photos-app-uri-scheme"></a>사진 앱 URI 체계

**Ms 사진:** URI 체계를 사용 하 여 사진 앱을 시작 하 고 이미지를 보거나 비디오를 편집 합니다. 예를 들어:  
이미지를 보려면 다음을 수행 합니다. `ms-photos:viewer?fileName=c:\users\userName\Pictures\image.jpg`  
비디오를 편집 하려면 다음을 수행 합니다. `ms-photos:videoedit?InputToken=123abc&Action=Trim&StartTime=01:02:03`  

> [!NOTE]
> 비디오를 편집 하거나 이미지를 표시 하는 Uri는 바탕 화면 에서만 사용할 수 있습니다.

| URI 체계 |결과 |
|------------|--------|
| ms-사진: 뷰어? 파일 이름 = {filename} | 지정 된 이미지를 보려면 사진 앱을 시작 합니다. 여기서 {filename}은 정규화 된 경로 이름입니다. 예: `c:\users\userName\Pictures\ImageToView.jpg` |
| ms-사진: videoedit? InputToken = {입력 토큰} | 파일 토큰으로 표시 되는 파일에 대 한 비디오 편집 모드로 사진 앱을 시작 합니다. **Inputtoken** 이 필요 합니다. [SharedStorageAccessManager](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) 를 사용 하 여 파일에 대 한 토큰을 가져옵니다. |
| ms-사진: videoedit? Action = {action} | 사진 앱을 여는 데 사용할 비디오 편집 모드를 나타내는 매개 변수입니다. 여기서 {action}은 **SlowMotion**, **프레임 추출을**, **Trim**, **View**, **Ink**중 하나입니다. **조치가** 필요 합니다. |
| ms-사진: videoedit? StartTime = {timespan} | 비디오 재생을 시작할 위치를 지정 하는 선택적 매개 변수입니다. `{timespan}` 형식 이어야 합니다 `"hh:mm:ss.ffff"` . 지정 하지 않으면 기본값은입니다. `00:00:00.0000` |

### <a name="settings-app-uri-scheme"></a>설정 앱 URI 체계

[Windows 설정 앱을 시작](launch-settings-app.md)하려면 **ms 설정:** URI 체계를 사용 합니다. 설정 앱을 시작 하는 것은 개인 정보 인식 앱을 작성 하는 데 중요 한 부분입니다. 앱에서 중요 한 리소스에 액세스할 수 없는 경우 해당 리소스에 대 한 개인 정보 설정에 대 한 편리한 링크를 사용자에 게 제공 하는 것이 좋습니다. 예를 들어 다음 URI는 설정 앱을 열고 카메라 개인 정보 설정을 표시 합니다.

`ms-settings:privacy-webcam`

![카메라 개인 정보 설정.](images/privacyawarenesssettingsapp.png)

자세한 내용은 [Windows 설정 앱 시작](launch-settings-app.md) 및 [개인 정보 인식 앱에 대 한 지침](../security/index.md)을 참조 하세요.

### <a name="store-app-uri-scheme"></a>스토어 응용 프로그램 URI 체계

**Microsoft azure-store:** URI 체계를 사용 하 여 [UWP 앱을 시작](launch-store-app.md)합니다. 제품 정보 페이지, 제품 검토 페이지 및 검색 페이지 등을 엽니다. 예를 들어 다음 URI는 UWP 앱을 열고 상점의 홈 페이지를 시작 합니다.

`ms-windows-store://home/`

자세한 내용은 [UWP 앱 시작](launch-store-app.md)을 참조 하세요.

### <a name="weather-app-uri-scheme"></a>날씨 앱 URI 체계

**Msnweather:** URI 체계를 사용 하 여 날씨 앱을 시작 합니다.

| URI 체계 | 결과 |
|------------|---------|
| msnweather://예측? la = \[ 위도 \]&하 = \[ 경도\] | 위치 지리 좌표를 기준으로 예측 페이지에서 날씨 앱을 시작 합니다.<br>`latitude` 위치의 위도를 참조 합니다.<br> `longitude` 위치의 경도를 참조 합니다.<br> |