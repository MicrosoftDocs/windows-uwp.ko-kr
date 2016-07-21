---
author: msatranjr
title: "지도 인증 키 요청"
description: "Windows.Services.Maps 네임스페이스에서 MapControl 및 지도 서비스를 사용하려면 먼저 유니버설 Windows 앱을 인증해야 합니다."
ms.assetid: 13B400D7-E13F-4F07-ACC3-9C34087F0F73
translationtype: Human Translation
ms.sourcegitcommit: 92285ce32548bd6035c105e35c2b152432f8575a
ms.openlocfilehash: b7c981e071f70ab0a76d73333a94580b3c497b0e

---

# 지도 인증 키 요청


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


[**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 네임스페이스에서 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 및 지도 서비스를 사용하려면 먼저 [유니버설 Windows 앱](https://msdn.microsoft.com/library/windows/apps/dn894631)을 인증해야 합니다. 앱을 인증하려면 지도 인증 키를 지정해야 합니다. 이 항목에서는 [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)에서 지도 인증 키를 요청하고 이를 앱에 추가하는 방법을 설명합니다.

**팁** 앱에서 지도를 사용하는 방법을 알아보려면 GitHub의 [Windows-universal-samples 리포지토리](http://go.microsoft.com/fwlink/p/?LinkId=619979)에서 다음 샘플을 다운로드하세요.

-   [UWP(유니버설 Windows 플랫폼) 지도 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619977)

## 키 가져오기


[Bing 지도 개발자 센터](https://www.bingmapsportal.com/)를 사용하여 유니버설 Windows 앱을 위한 지도 인증 키를 만들고 관리합니다.

새 키를 만들려면

1.  브라우저에서 Bing 지도 개발자 센터([https://www.bingmapsportal.com](https://www.bingmapsportal.com/))로 이동합니다.

2.  로그인하라는 메시지가 표시되면 Microsoft 계정을 입력하고 **로그인**을 클릭합니다.

3.  Bing 지도 계정과 연결할 계정을 선택합니다. Microsoft 계정을 사용하려는 경우 **예**를 클릭합니다. 그러지 않은 경우 **다른 계정으로 로그인**을 클릭합니다.

4.  Bing 지도 계정이 아직 없는 경우 새 Bing 지도 계정을 만듭니다. **계정 이름**, **연락처 이름**, **회사 이름**, **메일 주소** 및 **전화 번호**를 입력합니다. 사용 약관을 수락한 후 **만들기**를 클릭합니다.

5.  **내 계정** 메뉴에서 **키 만들기 또는 보기**를 클릭합니다.

6.  링크를 클릭하여 **새 키를 만듭니다**.

7.  **키 만들기** 양식을 작성한 다음 **만들기**를 클릭합니다.

    -   **응용 프로그램 이름:** 응용 프로그램의 이름입니다.
    -   **응용 프로그램 URL(옵션):** 응용 프로그램의 URL입니다.
    -   **키 유형:** **기본**또는 **엔터프라이즈**를 선택합니다.
    -   **응용 프로그램 종류:** 유니버설 Windows 앱에서 사용할 **유니버설 Windows 앱**을 선택합니다.

    다음은 양식이 표시되는 모양의 예입니다.

    ![키 만들기 양식의 예입니다.](images/createkeydialog.png)

8.  **만들기**를 클릭한 후 **키 만들기** 양식 아래에 새 키가 나타납니다. 안전한 장소에 복사하거나 다음 단계에서 설명한 대로 즉시 앱에 추가합니다.

## 앱에 키 추가


유니버설 Windows 앱에서 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 및 지도 서비스([**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979))를 사용하려면 지도 인증 키가 필요합니다. 지도 컨트롤에 인증 키를 추가하고 해당하는 경우 서비스 개체를 매핑합니다.

### 지도 컨트롤에 키를 추가하려면

[**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)을 인증하려면 [**MapServiceToken**](https://msdn.microsoft.com/library/windows/apps/dn637036) 속성을 인증 키 값으로 설정합니다. 기본 설정에 따라 코드 또는 XAML 태그에서 이 속성을 설정할 수 있습니다. **MapControl** 사용에 대한 자세한 내용은 [2D, 3D 및 Streetside 뷰로 지도 표시](display-maps.md)를 참조하세요.

-   이 예제는 **MapServiceToken**을 코드의 인증 키 값으로 설정합니다.

    ```cs
    MapControl1.MapServiceToken = "abcdef-abcdefghijklmno";
    ```

-   이 예제는 **MapServiceToken**을 XAML 태그의 인증 키 값으로 설정합니다.

    ```xml
    <Maps:MapControl x:Name="MapControl1" MapServiceToken="abcdef-abcdefghijklmno"/>
    ```

### 지도 서비스에 키를 추가하려면

[**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 네임스페이스의 서비스를 사용하려면 [**ServiceToken**](https://msdn.microsoft.com/library/windows/apps/dn636977) 속성을 인증 키 값으로 설정합니다. 지도 서비스 사용에 대한 자세한 내용은 [경로 및 길 찾기 표시](routes-and-directions.md) 및 [지오코딩 및 리버스 지오코딩 수행](geocoding.md)을 참조하세요.

-   이 예제는 **ServiceToken**을 코드의 인증 키 값으로 설정합니다.

    ```cs
    MapService.ServiceToken = "abcdef-abcdefghijklmno";
    ```

## 관련 항목

* [Bing 지도 개발자 센터](https://www.bingmapsportal.com/)
* [UWP 지도 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [지도에 대한 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [빌드 2015 동영상: Windows 앱에서 휴대폰, 태블릿 및 PC 간에 지도 및 위치 활용](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 교통 앱 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619982)





<!--HONumber=Jun16_HO5-->


