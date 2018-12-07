---
Description: A tile is an app's representation on the Start menu. Every app has a tile. When you create a new Universal Windows Platform (UWP) app project in Microsoft Visual Studio, it includes a default tile that displays your app's name and logo.
title: 타일
ms.assetid: 09C7E1B1-F78D-4659-8086-2E428E797653
label: Tiles
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10 uwp
ms.localizationpriority: medium
ms.openlocfilehash: e46d73c91f54b1bb74a70990a238f13ccd47645d
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8796704"
---
# <a name="tiles-for-uwp-apps"></a>UWP 앱의 타일

 

*타일*은 시작 메뉴에 표시되는 앱의 표시를 말합니다. 모든 앱에 타일이 있습니다. Microsoft Visual Studio에서 새 UWP(유니버설 Windows 플랫폼) 앱 프로젝트를 만들면 앱 이름 및 로고를 표시하는 기본 타일이 포함됩니다.Windows에서는 앱이 처음 설치될 때 이 타일을 표시합니다. 앱이 설치된 후 알림을 통해 타일 콘텐츠를 변경할 수 있습니다. 예를 들어 뉴스 헤드라인 또는 읽지 않은 가장 최근 메시지의 제목과 같은 새 정보를 사용자에게 전달하기 위해 타일을 변경할 수 있습니다.

## <a name="configure-the-default-tile"></a>기본 타일 구성


Visual Studio에서 새 프로젝트를 만든 경우 앱의 이름 및 로고를 표시하는 간단한 기본 타일이 만들어집니다.

타일을 편집하려면 주 UWP 프로젝트에서 **Package.appxmanifest** 파일을 두 번 클릭하여 디자이너를 엽니다(또는 해당 파일을 마우스 오른쪽 단추로 클릭하고 코드 보기 선택).

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

업데이트해야 하는 몇 가지 항목이 있습니다.

-   DisplayName: 이 값을 타일에 표시하려는 이름으로 바꿉니다.
-   ShortName: 표시 이름을 타일에 맞추기 위해서는 공간이 제한되어 있으므로 앱 이름이 잘리지 않도록 ShortName도 지정하는 것이 좋습니다.
-   로고 이미지:

    이러한 이미지를 고유한 이미지로 바꿔야 합니다. 다양한 표시 배율로 이미지를 제공할 수도 있지만 이미지를 모두 제공할 필요는 없습니다. 앱이 다양한 디바이스에서 올바르게 표시되도록 하려면 각 이미지의 100%, 200% 및 400% 배율 버전을 제공하는 것이 좋습니다. 이러한 자산 생성에 대해 자세히 알아보려면 [타일 및 아이콘 자산](app-assets.md)을 참조하세요.

    크기 조정된 이미지는 다음 명명 규칙을 따릅니다.
    
    *&lt;이미지 이름&gt;*.scale-*&lt;배율 인수&gt;*.*&lt;이미지 파일 확장명&gt;* 

    예: SplashScreen.scale-100.png

    이미지를 참조할 때 *&lt;image name&gt;*.*&lt;image file extension&gt;*(이 예제의 경우 "SplashScreen.png")으로 참조합니다. 제공한 이미지 중에서 디바이스에 적절한 크기의 이미지가 자동으로 선택됩니다.

-   사용자가 앱 타일을 해당 크기로 조정할 수 있도록 넓고 큰 타일 크기에 대한 로고를 제공하는 것이 좋습니다. 이러한 추가 이미지를 제공하려면 **DefaultTile** 요소를 만들고 **Wide310x150Logo** 및 **Square310x310Logo** 특성을 사용하여 추가 이미지를 지정합니다.
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

## <a name="use-notifications-to-customize-your-tile"></a>알림을 사용하여 타일 사용자 지정


앱이 설치된 후 알림을 사용하여 타일을 사용자 지정할 수 있습니다. 앱이 처음으로 실행될 때 또는 푸시 알림과 같은 이벤트에 응답할 때 이렇게 할 수 있습니다.

타일 알림을 전송하는 방법을 알아보려면 [로컬 타일 알림 보내기](sending-a-local-tile-notification.md)를 참조하세요.
