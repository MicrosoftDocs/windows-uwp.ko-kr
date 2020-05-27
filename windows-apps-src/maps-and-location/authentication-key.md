---
title: 지도 인증 키 요청
description: 유니버설 Windows 앱은 없습니다 네임 스페이스에서 서비스를 사용 하 고 매핑하기 전에 인증 되어야 합니다.
ms.assetid: 13B400D7-E13F-4F07-ACC3-9C34087F0F73
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 지도 인증 키, 지도 컨트롤
ms.localizationpriority: medium
ms.openlocfilehash: ab0d1900398c313021600c18338ecc1201241410
ms.sourcegitcommit: f806d5f3b0c1e046c903d3388092c0e059d21858
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83791001"
---
# <a name="request-a-maps-authentication-key"></a>지도 인증 키 요청

[유니버설 windows 앱](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide) 은 [**없습니다**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) [**네임 스페이스**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) 에서 서비스를 사용 하 고 매핑하기 전에 인증 되어야 합니다. 앱을 인증하려면 지도 인증 키를 지정해야 합니다. 이 항목에서는 [Bing Maps 개발자 센터](https://www.bingmapsportal.com/) 에서 지도 인증 키를 요청 하 고 앱에 추가 하는 방법에 대해 설명 합니다.

**팁** 앱에서 지도를 사용 하는 방법에 대 한 자세한 내용은 GitHub의 [Windows 유니버설 샘플 리포지토리](https://github.com/Microsoft/Windows-universal-samples) 에서 다음 샘플을 다운로드 하세요.

-   [UWP(유니버설 Windows 플랫폼) 지도 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)

## <a name="get-a-key"></a>키 가져오기


[Bing Maps 개발자 센터](https://www.bingmapsportal.com/)를 사용 하 여 유니버설 Windows 앱에 대 한 맵 인증 키를 만들고 관리 합니다.

새 키를 만들려면

1.  브라우저에서 Bing Maps 개발자 센터 ()로 이동 [https://www.bingmapsportal.com](https://www.bingmapsportal.com/) 합니다.

2.  로그인 하 라는 메시지가 표시 되 면 Microsoft 계정 입력 하 고 **로그인**을 클릭 합니다.

3.  Bing Maps 계정에 연결할 계정을 선택 합니다. Microsoft 계정를 사용 하려면 **예**를 클릭 합니다. 그러지 않은 경우 **다른 계정으로 로그인**을 클릭합니다.

4.  Bing Maps 계정이 아직 없는 경우 새 Bing Maps 계정을 만듭니다. **계정 이름**, **연락처 이름**, **회사 이름**, **전자 메일 주소**및 **전화 번호**를 입력 합니다. 사용 약관에 동의한 후 **만들기**를 클릭 합니다.

5.  **내 계정** 메뉴에서 **내 키**를 클릭 합니다.

6.  이전에 키를 만든 경우 링크를 클릭 하 여 새 키를 만듭니다. 그렇지 않으면 Create Key 폼으로 이동 합니다.

7.  **만들기 키** 양식을 완료 하 고 **만들기**를 클릭 합니다.

    -   **응용 프로그램 이름:** 응용 프로그램의 이름입니다.
    -   **응용 프로그램 URL (옵션):** 응용 프로그램의 URL입니다.
    -   **키 유형:** **기본** 또는 **엔터프라이즈**를 선택 합니다.
    -   **응용 프로그램 유형:** 유니버설 Windows 앱에서 사용할 **Windows 응용 프로그램** 을 선택 합니다.

    다음은 폼의 모양에 대 한 예입니다.

    ![create key 폼의 예입니다.](images/createkeydialog.png)

8.  **만들기**를 클릭 하면 새 키가 **만들기 키** 양식 아래에 나타납니다. 다음 단계에 설명 된 것 처럼 안전한 장소에 복사 하거나 즉시 앱에 추가 합니다.

## <a name="add-the-key-to-your-app"></a>앱에 키 추가


유니버설 Windows 앱의 없습니다 및 map services ( [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) )를 사용 하려면 맵 인증 키가 필요[**합니다.**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) 해당 하는 경우 맵 컨트롤에 추가 하 고 서비스 개체를 매핑합니다.

### <a name="to-add-the-key-to-a-map-control"></a>지도 컨트롤에 키를 추가 하려면

[**없습니다**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)을 인증 하려면 [**MapServiceToken**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken) 속성을 인증 키 값으로 설정 합니다. 기본 설정에 따라 코드 또는 XAML 태그에서이 속성을 설정할 수 있습니다. **없습니다**사용에 대 한 자세한 내용은 [2d, 3d 및 Streetside 뷰가 포함 된 지도 표시](display-maps.md)를 참조 하세요.

-   이 예제에서는 **MapServiceToken** 를 코드의 인증 키 값으로 설정 합니다.

    ```cs
    MapControl1.MapServiceToken = "abcdef-abcdefghijklmno";
    ```

-   이 예제에서는 **MapServiceToken** 를 XAML 태그의 인증 키 값으로 설정 합니다.

    ```xml
    <Maps:MapControl x:Name="MapControl1" MapServiceToken="abcdef-abcdefghijklmno"/>
    ```

### <a name="to-add-the-key-to-map-services"></a>지도 서비스에 키를 추가 하려면

ServiceToken 네임 스페이스에서 서비스를 사용 하려면 인증 키 값으로 [**ServiceToken**](https://docs.microsoft.com/uwp/api/windows.services.maps.mapservice.servicetoken) 속성을 설정 [**합니다.**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) Map services를 사용 하는 방법에 대 한 자세한 내용은 [경로 및 방향 표시](routes-and-directions.md) 및 [지 오 코딩 및 역방향 지 오 코딩 수행](geocoding.md)을 참조 하세요.

-   이 예제에서는 **ServiceToken** 를 코드의 인증 키 값으로 설정 합니다.

    ```cs
    MapService.ServiceToken = "abcdef-abcdefghijklmno";
    ```

## <a name="related-topics"></a>관련 항목

* [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)
* [UWP 지도 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [지도에 대한 디자인 지침](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)
* [빌드 2015 비디오: Windows 앱의 휴대폰, 태블릿 및 PC에서 맵 및 위치 활용](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 교통 앱 샘플](https://github.com/Microsoft/Windows-appsample-trafficapp)
