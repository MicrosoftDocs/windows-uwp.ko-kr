---
author: TylerMSFT
title: "URI에 대한 기본 앱 실행"
description: "URI(Uniform Resource Identifier)에 대한 기본 앱 시작 방법을 알아봅니다. URI를 사용하면 다른 앱을 실행하여 특정 작업을 수행할 수 있습니다. 이 항목에서는 Windows에 기본 제공되는 다양한 URI 스키마에 대한 개요도 제공합니다."
ms.assetid: 7B0D0AF5-D89E-4DB0-9B79-90201D79974F
ms.sourcegitcommit: 9011d2e2e1e51edc89851e815d31e13390c24f96
ms.openlocfilehash: d454317d135e2b2b952c16fb00685e34b489865c

---

# URI에 대한 기본 앱 실행


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

URI(Uniform Resource Identifier)에 대한 기본 앱 시작 방법을 알아봅니다. URI를 사용하면 다른 앱을 실행하여 특정 작업을 수행할 수 있습니다. 이 항목에서는 Windows에 기본 제공되는 다양한 URI 스키마에 대한 개요도 제공합니다. 사용자 지정 URI를 실행할 수도 있습니다. 사용자 지정 URI 스키마를 등록하고 URI 활성화를 처리하는 방법에 대한 자세한 내용은 [URI 활성화 처리](handle-uri-activation.md)를 참조하세요.

## URI를 실행하는 방법


URI 스키마를 사용하면 하이퍼링크를 클릭하여 앱을 열 수 있습니다. **mailto:**를 사용하여 새 메일을 시작하는 것처럼 **http:**를 사용하여 기본 웹 브라우저를 열 수 있습니다. 이 항목에서는 Windows에 기본 제공되는 URI 스키마 중 일부를 설명합니다.

-   [ms-settings: URI 스키마](#settings)는 Windows 설정 앱을 실행합니다.
-   [ms-store: URI 스키마](#store)는 Windows 스토어 앱을 실행합니다.
-   [http: URI 스키마](#browser)는 기본 웹 브라우저를 실행합니다.
-   [mailto: URI 스키마](#email)는 기본 메일 앱을 실행합니다.
-   [bingmaps:, ms-drive-to: 및 ms-walk-to: URI 스키마](#maps)는 Windows 지도 앱을 실행합니다.

예를 들어 다음 URI는 기본 브라우저를 열고 Bing 웹 사이트를 표시합니다.

`http://bing.com`

사용자 지정 URI 스키마를 실행할 수도 있습니다. 해당 URI를 처리하는 앱이 설치되지 않은 경우 사용자가 설치할 앱을 권장할 수 있습니다. 자세한 내용은 [앱 권장](#recommend)을 참조하세요.

일반적으로 개발자 앱에서는 실행된 앱을 선택할 수 없습니다. 사용자가 어떤 앱이 실행되고 있는지 확인합니다. 두 개 이상의 앱을 등록하여 동일한 URI 스키마를 처리할 수 있습니다. 예약된 URI 스키마는 예외입니다. 예약된 URI 스키마의 등록은 무시됩니다. 예약된 URI 스키마의 전체 목록은 [URI 활성화 처리](handle-uri-activation.md)를 참조하세요. 둘 이상의 앱에서 동일한 URI 스키마를 등록할 수 있는 경우 앱에서 실행할 특정 앱을 권장할 수 있습니다. 자세한 내용은 [앱 권장](#recommend)을 참조하세요.

### LaunchUriAsync 호출

[
            **LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 메서드를 사용하여 URI를 실행합니다. 이 메서드를 호출할 때 앱은 포그라운드 앱이어야 합니다. 즉, 사용자에게 표시되어야 합니다. 이 요구 사항은 사용자가 제어권을 갖도록 하는 데 도움이 됩니다. 이 요구 사항을 충족하려면 모든 URI 실행을 앱의 UI에 직접 연결해야 합니다. 사용자는 항상 URI 실행을 시작하기 위해 일부 작업을 수행해야 합니다. URI를 실행하려는 경우 앱이 포그라운드에 없으면 URI가 실행되지 않으며 오류 콜백이 호출됩니다.

먼저 URI를 나타내는 [**System.Uri**](https://msdn.microsoft.com/en-us/library/windows/apps/system.uri.aspx) 개체를 만든 다음 이 개체를 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 메서드로 전달합니다. 반환되는 결과를 사용하여 다음 예제에 표시된 대로 호출에 성공했는지 확인합니다.

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

경우에 따라 운영 체제에서 사용자에게 실제로 앱을 전환할지 묻는 메시지를 표시합니다.

![회색으로 표시된 앱 배경에 오버레이된 경고 대화 상자. 이 대화 상자에서는 앱을 전환할지 여부를 사용자에게 질문하며 오른쪽 아래에는 '예'와 '아니요' 단추가 있고 ‘아니요' 단추가 강조 표시되어 있습니다.](images/warningdialog.png)

이 메시지가 항상 발생하도록 하려면 [**Windows.System.LauncherOptions.TreatAsUntrusted**](https://msdn.microsoft.com/library/windows/apps/hh701442) 속성을 사용하여 운영 체제에 경고가 표시됨을 나타냅니다.

```cs
// The URI to launch
var uriBing = new Uri(@"http://www.bing.com");

// Set the option to show a warning
var promptOptions = new Windows.System.LauncherOptions();
promptOptions.TreatAsUntrusted = true;

// Launch the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriBing, promptOptions);
```

### 앱 권장

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

### 나머지 보기 기본 설정 지정

[
            **LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)를 호출하는 원본 앱은 URI가 시작된 후 화면에 유지되도록 요청할 수 있습니다. 기본적으로 Windows는 URI를 처리하는 대상 앱과 원본 앱 사이에 모든 사용 가능한 공간을 동일하게 공유하려고 합니다. 원본 앱은 [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314) 속성을 사용하여 앱 창이 거의 모든 사용 가능한 공간을 사용하려고 한다는 것을 운영 체제에 나타냅니다. **DesiredRemainingView**를 사용하여 URI가 시작된 후 원본 앱이 화면에서 유지될 필요가 없고 대상 앱으로 완전히 대체될 수 있다는 것을 나타낼 수도 있습니다. 이 속성은 호출 앱의 기본 창 크기만 지정합니다. 화면에 동시에 나타날 수도 있는 다른 앱의 동작은 지정하지 않습니다.

**참고** Windows는 원본 앱의 최종 창 크기를 결정할 때 원본 앱의 기본 설정, 화면의 앱 수, 화면 방향 같은 여러 가지 요소를 고려합니다. [
            **DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)를 설정해도 원본 앱에 대한 특정 창 관리 동작이 보장되지 않습니다.

 

```cs
// Set the desired remaining view.
var options = new Windows.System.LauncherOptions();
options.DesiredRemainingView = Windows.UI.ViewManagement.ViewSizePreference.UseLess;

// Launch the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriContoso, options);
```

## Windows 지도 앱 URI 스키마


앱에서 **bingmaps:**, **ms-drive-to:** 및 **ms-walk-to:** URI 스키마로 [Windows 지도 앱을 실행](launch-maps-app.md)하여 특정 지도, 길 찾기 및 검색 결과를 표시할 수 있습니다. 예를 들어 다음 URI는 Windows 지도 앱을 열고 뉴욕시를 중심으로 지도를 표시합니다.

`bingmaps:?cp=40.726966~-74.006076`

![Windows 지도 앱의 예](images/mapnyc.png)

자세한 내용은 [Windows 지도 앱 실행](launch-maps-app.md)을 참조하세요. 사용자 고유의 앱에서 지도 컨트롤을 사용하려면 [2D, 3D 및 Streetside 뷰로 지도 표시](https://msdn.microsoft.com/library/windows/apps/mt219695)를 참조하세요.

## Windows 설정 앱 URI 스키마


앱에서 **ms-settings:** URI 스키마를 사용하여 [Windows 설정 앱을 실행](launch-settings-app.md)할 수 있습니다. 설정 앱 실행은 개인 정보 인식 앱 작성의 중요한 부분입니다. 앱에서 중요한 리소스에 액세스할 수 없는 경우 사용자에게 해당 리소스의 개인 정보 설정에 대한 편리한 링크를 제공하는 것이 좋습니다. 예를 들어 다음 URI는 설정 앱을 열고 카메라 개인 정보 설정을 표시합니다.

`ms-settings:privacy-webcam`

![카메라 개인 정보 설정](images/privacyawarenesssettingsapp.png)

자세한 내용은 [Windows 설정 앱 실행](launch-settings-app.md) 및 [개인 정보 인식 앱에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh768223)을 참조하세요.

## Windows 스토어 앱 URI 스키마


앱에서 **ms-windows-store:** URI 스키마를 사용하여 [Windows 스토어 앱을 실행](launch-store-app.md)할 수 있습니다. 제품 세부 정보 페이지, 제품 리뷰 페이지, 검색 페이지 등을 엽니다. 예를 들어 다음 URI는 Windows 스토어 앱을 열고 스토어의 홈 페이지를 시작합니다.

`ms-windows-store://home/`

자세한 내용은 [Windows 스토어 앱 실행](launch-store-app.md)을 참조하세요.

## 통화 앱 URI 스키마


앱에서 **ms-call:** URI 스키마를 사용하여 통화 앱을 실행할 수 있습니다.

| URI 스키마       | 결과                               |
|------------------|---------------------------------------|
| ms-call:settings | 통화 앱 설정 페이지를 시작합니다. |

 

## 채팅 앱 URI 스키마


앱에서 **ms-chat:** URI 스키마를 사용하여 메시지 앱을 실행할 수 있습니다.

| URI 스키마                               | 결과                                                                                                                                                                                |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ms-chat:                                 | 메시징 앱을 시작합니다.                                                                                                                                                            |
| ms-chat:?ContactID={contacted}           | 메시징 응용 프로그램이 특정 연락처의 정보를 사용하여 실행되도록 허용합니다.                                                                                               |
| ms-chat:?Body={body}                     | 메시징 응용 프로그램이 문자열을 메시지 내용으로 사용하여 실행되도록 허용합니다.                                                                                    |
| ms-chat:?Addresses={address}&amp;Body={body} | 메시징 응용 프로그램이 문자열을 메시지 내용으로 사용하고 특정 주소의 정보를 사용하여 실행되도록 허용합니다. 참고: 주소를 연결할 수 있습니다. |
| ms-chat:?TransportId={transportId}       | 메시징 응용 프로그램이 특정 전송 ID를 사용하여 실행되도록 허용합니다.                                                                                                        |

 

## 메일 URI 스키마


앱에서 **mailto:** URI 스키마를 사용하여 기본 메일 앱을 실행할 수 있습니다.

| URI 스키마               | 결과                                                                                                                                                     |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| mailto:                  | 기본 메일 앱을 실행합니다.                                                                                                                             |
| mailto:\[email address\] | 메일 앱을 실행하고 받는 사람 줄에 메일 주소를 지정한 새 메서드를 작성합니다. 메일은 사용자가 보내기를 탭해야 전송됩니다. |

 

## HTTP URI 스키마


앱에서 **http:** URI 스키마를 사용하여 기본 웹 브라우저를 실행할 수 있습니다.

| URI 스키마 | 결과                           |
|------------|-----------------------------------|
| http:      | 기본 웹 브라우저를 실행합니다. |

 

## 근처 전화 번호 앱 URI 스키마


앱에서 **ms-yellowpage:** URI 스키마를 사용하여 근처 전화 번호 앱을 실행할 수 있습니다.

| URI 스키마                                            | 결과                                                                               |
|-------------------------------------------------------|---------------------------------------------------------------------------------------|
| ms-yellowpage:?input=\[keyword\]&amp;method=\[String|T9\] | 이 새 URI를 지원하는 설치된 POI(안내 표시) 검색 앱을 실행합니다. |

 

## 피플 앱 URI 스키마


앱에서 **ms-people:** URI 스키마를 사용하여 피플 앱을 실행할 수 있습니다.

자세한 내용은 [피플 앱 실행](launch-people-apps.md)을 참조하세요.

 

 



<!--HONumber=Jun16_HO4-->


