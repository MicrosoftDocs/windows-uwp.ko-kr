---
title: 시작 화면 추가
description: Microsoft Visual Studio를 사용 하 여 앱의 시작 화면 이미지 및 배경색을 설정 합니다.
ms.assetid: 41F53046-8AB7-4782-9E90-964D744B7D66
ms.date: 05/08/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 06f9d412976ab9ccd5af5c8108bd5632792e4112
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167917"
---
# <a name="add-a-splash-screen"></a>시작 화면 추가

Microsoft Visual Studio를 사용 하 여 앱의 시작 화면 이미지 및 배경색을 설정 합니다.

## <a name="set-the-splash-screen-image-and-background-color-in-visual-studio"></a>Visual Studio에서 시작 화면 이미지 및 배경색 설정

Visual Studio 템플릿을 사용 하 여 앱을 만드는 경우 기본 이미지가 프로젝트에 추가 되 고 시작 화면 이미지로 설정 됩니다. 시작 화면의 배경색은 연한 회색으로 표시 됩니다. 앱 시작 화면의 기본 이미지 또는 색을 변경 하려면 다음 단계를 수행 합니다.

1. Visual Studio에서 UWP (기존 유니버설 Windows 플랫폼) 앱 프로젝트를 엽니다.
2. **솔루션 탐색기**에서 "appxmanifest.xml" 파일을 엽니다. **프로젝트** &gt; **저장소** &gt; **편집 응용 프로그램 매니페스트**를 선택 하 여 메뉴 모음에서이 파일을 열 수도 있습니다.
3. **시각적 자산** 탭을 열고 "appxmanifest.xml" 창의 왼쪽에 있는 **모든 시각적 자산** 창에서 **시작 화면** 을 선택 합니다. 처음으로 시작 화면을 변경 하는 경우 \\ **시작 화면** 필드에 "자산SplashScreen.png" 경로가 표시 됩니다.

    다음 스크린샷은 Visual Studio의 "appxmanifest.xml" 창을 보여 줍니다. 프로젝트 형식에 따라 시각적 자산의 약간 다른 집합이 표시 됩니다.

    ![Visual Studio 2019의 "appxmanifest.xml" 창 스크린샷](images/appmanifest.png)

    텍스트 편집기에서 "appxmanifest.xml"를 열면 [**SplashScreen 요소가**](/uwp/schemas/appxpackage/appxmanifestschema/element-splashscreen) [**visualelements 요소의**](/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements)자식으로 표시 됩니다. 매니페스트 파일의 기본 시작 화면 태그는 텍스트 편집기에서 다음과 같이 표시 됩니다.

    ```xml
    <uap:SplashScreen Image="Assets\SplashScreen.png" />
    ```

4. UWP 앱에 대 한 새 시작 화면 이미지를 선택 하려면 **크기 조정 된 자산**아래 **1240 x 600 px** 레이블 옆에 나타나는 줄임표를 사용 하 여 단추를 누릅니다. 시작 화면 이미지에 사용할 1240 x 600 픽셀 이미지 (.png, .jpg 또는 .jpeg)를 선택 합니다.

    **중요**    선택 하는 시작 화면 이미지는 1x 배율 인수를 사용 하 여 620 x 300 픽셀 이어야 합니다. 또한 시작 화면을 디자인 하는 경우 화면 보다 작고 가운데 맞춤 됩니다. Windows Phone 스토어 앱의 시작 화면과 같은 화면을 채우지 않습니다.

5. Windows Phone 스토어 앱에 대 한 새 시작 화면 이미지를 선택 하려면 **크기 조정 된 자산**아래 **1152 x 1920 px** 레이블 옆에 나타나는 줄임표를 사용 하 여 단추를 누릅니다. 시작 화면 이미지에 사용할 1152 x 1920 픽셀 이미지 (.png, .jpg 또는 .jpeg)를 선택 합니다.

    **중요**    선택 하는 시작 화면 이미지는 2.4 x 배율 인수에 대 한 올바른 크기인 1152 x 1920 픽셀 이어야 합니다. 이 자산이 유일 하 게 제공 되는 경우 1.4 x 및 1x 배율 요소에 대해 축소 됩니다.

6. **시작 화면** 섹션의 **배경색** 필드에서 시작 화면 이미지와 함께 표시 되는 배경색을 설정 합니다. 색의 이름 또는 ' '를 입력 하거나 16 진수 값을 입력할 수 있습니다 \# . 사용 가능한 색의 이름 목록은 [**SplashScreen 요소**](/uwp/schemas/appxpackage/appxmanifestschema/element-splashscreen)를 참조 하세요. 시작 화면의 배경색 설정은 선택 사항입니다. UWP 앱에 대 한 색을 지정 하지 않으면 시작 화면 배경색의 기본값은 연한 회색 (16 진수 값 \# 464646)입니다. 이 색은 기본 **타일** 배경색과 동일한 색입니다 ( **시각적 자산** 탭에서 **타일 이미지 및 로고** 섹션의 **배경색** 필드 참조). Windows Phone에 대 한 색을 지정 하지 않거나 "투명"으로 설정 하는 경우 시작 화면 배경색은 투명 합니다.

## <a name="summary-and-next-steps"></a>요약 및 다음 단계

앱이 로드 하는 데 시간이 오래 걸리면 확장 된 시작 화면을 추가 하는 것이 좋습니다. 단계별 지침은 [사용자 지정 된 시작 화면 만들기](create-a-customized-splash-screen.md)를 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [사용자 지정 된 시작 화면 만들기](create-a-customized-splash-screen.md)
* [패키지 매니페스트 스키마 참조: SplashScreen 요소](/uwp/schemas/appxpackage/appxmanifestschema/element-splashscreen)
* [SplashScreen 클래스입니다.](/uwp/api/Windows.ApplicationModel.Activation.SplashScreen)