---
Description: 타일은 시작 메뉴에 표시되는 앱의 표시를 말합니다. 모든 앱에는 타일이 있습니다. Microsoft Visual Studio에서 새 Windows 앱 앱 프로젝트를 만들 때 앱의 이름과 로고를 표시 하는 기본 타일이 포함 됩니다.
title: Windows 앱의 타일
ms.assetid: 09C7E1B1-F78D-4659-8086-2E428E797653
label: Tiles
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0882ac67766bc2ce037133cf8a39b5393f616e13
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970998"
---
# <a name="tiles-for-windows-apps"></a>Windows 앱의 타일

 

*타일* 은 시작 메뉴의 앱 표현입니다. 모든 앱에는 타일이 있습니다. Microsoft Visual Studio에서 새 Windows 앱 앱 프로젝트를 만들 때 앱의 이름과 로고를 표시 하는 기본 타일이 포함 됩니다.앱을 처음 설치할 때 Windows에서이 타일을 표시 합니다. 앱이 설치 된 후 알림을 통해 타일의 콘텐츠를 변경할 수 있습니다. 예를 들어 타일을 변경 하 여 새 정보 (예: 뉴스 헤드라인) 또는 가장 최근의 읽지 않은 메시지의 제목을 사용자에 게 전달할 수 있습니다.

## <a name="configure-the-default-tile"></a>기본 타일 구성


Visual Studio에서 새 프로젝트를 만들면 앱의 이름과 로고가 표시 되는 간단한 기본 타일이 생성 됩니다.

타일을 편집 하려면 주 UWP 프로젝트에서 **appxmanifest.xml** 파일을 두 번 클릭 하 여 디자이너를 열거나, 파일을 마우스 오른쪽 단추로 클릭 하 고 코드 보기를 선택 합니다.

```XML
  <Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="ExampleApp.App">
      <uap:VisualElements
        DisplayName="ExampleApp"
        Square150x150Logo="Assets\Square150x150Logo.png"
        Square44x44Logo="Assets\Square44x44Logo.png"
        Description="ExampleApp"
        BackgroundColor="#464646">
        <uap:SplashScreen Image="Assets\SplashScreen.png" />
      </uap:VisualElements>
    </Application>
  </Applications>
```

업데이트 해야 할 몇 가지 항목이 있습니다.

-   DisplayName:이 값을 타일에 표시 하려는 이름으로 바꿉니다.
-   짧은 이름: 타일에 맞게 표시 이름에 대해 공간이 제한 되어 있기 때문에 앱의 이름이 잘리지 않도록 짧은 이름도 지정 하는 것이 좋습니다.
-   로고 이미지:

    이러한 이미지를 고유한 이미지로 바꾸어야 합니다. 다양 한 시각적 눈금에 대 한 이미지를 제공할 수 있지만 모두 제공 해야 하는 것은 아닙니다. 앱이 장치 범위에서 제대로 표시 되도록 하려면 각 이미지의 100%, 200% 및 400% scale 버전을 제공 하는 것이 좋습니다. 이러한 자산을 생성 하는 방법에 대해 자세히 알아보려면 [앱 아이콘 및 로고](/windows/uwp/design/style/app-icons-and-logos) 를 참조 하세요.

    크기 조정 된 이미지는 다음과 같은 명명 규칙을 따릅니다.
    
    *&lt;&gt;* *이미지 이름&gt;. 배율 배율 인수 &lt;* 입니다. *이미지 파일 확장명&gt; &lt;* 

    예: SplashScreen. scale-100

    이미지를 참조 하는 경우 이미지 * &lt;이름&gt;* 으로 참조 합니다. *이미지 파일 확장명&gt; (이 예에서는 "SplashScreen")입니다. &lt;* 시스템은 사용자가 제공한 이미지에서 장치의 적절 한 크기 조정 이미지를 자동으로 선택 합니다.

-   필요 하지는 않지만, 사용자가 앱의 타일 크기를 해당 크기로 조정할 수 있도록 광범위 하 고 큰 타일 크기에 로고를 제공 하는 것이 좋습니다. 이러한 추가 이미지를 제공 하려면 **Defaulttile** 요소를 만들고 **Wide310x150Logo** 및 **Square310x310Logo** 특성을 사용 하 여 추가 이미지를 지정 합니다.
```    XML
  <Applications>
        <Application Id="App"
          Executable="$targetnametoken$.exe"
          EntryPoint="ExampleApp.App">
          <uap:VisualElements
            DisplayName="ExampleApp"
            Square150x150Logo="Assets\Square150x150Logo.png"
            Square44x44Logo="Assets\Square44x44Logo.png"
            Description="ExampleApp"
            BackgroundColor="#464646">
            <uap:DefaultTile
              Wide310x150Logo="Assets\Wide310x150Logo.png"
              Square310x310Logo="Assets\Square310x310Logo.png">
            </uap:DefaultTile>
            <uap:SplashScreen Image="Assets\SplashScreen.png" />
          </uap:VisualElements>
        </Application>
      </Applications>
```

## <a name="use-notifications-to-customize-your-tile"></a>알림을 사용 하 여 타일 사용자 지정


앱을 설치한 후에는 알림을 사용 하 여 타일을 사용자 지정할 수 있습니다. 앱을 처음 시작할 때 또는 푸시 알림과 같은 이벤트에 대 한 응답으로이 작업을 수행할 수 있습니다.

타일 알림을 보내는 방법을 알아보려면 [로컬 타일 알림 보내기](sending-a-local-tile-notification.md)를 참조 하세요.
