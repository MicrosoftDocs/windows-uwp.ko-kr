---
Description: 타일은 시작 메뉴에 표시되는 앱의 표시를 말합니다. 모든 앱에 타일이 있습니다. Microsoft Visual Studio에서 새 UWP(유니버설 Windows 플랫폼) 앱 프로젝트를 만들면 앱 이름 및 로고를 표시하는 기본 타일이 포함됩니다.
title: 타일
ms.assetid: 09C7E1B1-F78D-4659-8086-2E428E797653
label: 타일
template: detail.hbs
---

# UWP 앱의 타일


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


*타일*은 시작 메뉴에 표시되는 앱의 표시를 말합니다. 모든 앱에 타일이 있습니다. Microsoft Visual Studio에서 새 UWP(유니버설 Windows 플랫폼) 앱 프로젝트를 만들면 앱 이름 및 로고를 표시하는 기본 타일이 포함됩니다. Windows에서는 앱이 처음 설치될 때 이 타일을 표시합니다. 앱이 설치된 후 알림을 통해 타일 콘텐츠를 변경할 수 있습니다. 예를 들어 뉴스 헤드라인 또는 읽지 않은 가장 최근 메시지의 제목과 같은 새 정보를 사용자에게 전달하기 위해 타일을 변경할 수 있습니다.

## <span id="Configure_the_default_tile"> </span> <span id="configure_the_default_tile"> </span> <span id="CONFIGURE_THE_DEFAULT_TILE"> </span>기본 타일 구성


Visual Studio에서 새 프로젝트를 만든 경우 앱의 이름 및 로고를 표시하는 간단한 기본 타일이 만들어집니다.

```XML
  <Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="ExampleApp.App">
      <uap:VisualElements
        DisplayName="ExampleApp"
        Square150x150Logo="Assets\Logo.png"
        Square44x44Logo="Assets\SmallLogo.png"
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

    이러한 이미지를 고유한 이미지로 바꿔야 합니다. 다양한 표시 배율로 이미지를 제공할 수도 있지만 이미지를 모두 제공할 필요는 없습니다. 앱이 다양한 장치에서 올바르게 표시되도록 하려면 각 이미지의 100%, 200% 및 400% 배율 버전을 제공하는 것이 좋습니다.

    크기 조정된 이미지는 다음 명명 규칙을 따릅니다.
    
    *&lt;image name&gt;*.scale-*&lt;scale factor&gt;*.*&lt;image file extension&gt;* 


     

    For example: SmallLogo.scale-100.png

    When you refer to the image, you refer to it as *&lt;image name&gt;*.*&lt;image file extension&gt;* ("SmallLogo.png" in this example). The system will automatically select the appropriate scaled image for the device from the images you've provided.

-   사용자가 앱 타일을 해당 크기로 조정할 수 있도록 넓고 큰 타일 크기에 대한 로고를 제공하는 것이 좋습니다. 이러한 추가 이미지를 제공하려면 `DefaultTile` 요소를 만들고 `Wide310x150Logo` 및 `Square310x310Logo` 특성을 사용하여 추가 이미지를 지정합니다.
```    XML
  <Applications>
        <Application Id="App"
          Executable="$targetnametoken$.exe"
          EntryPoint="ExampleApp.App">
          <uap:VisualElements
            DisplayName="ExampleApp"
            Square150x150Logo="Assets\Logo.png"
            Square44x44Logo="Assets\SmallLogo.png"
            Description="ExampleApp"
            BackgroundColor="#464646">
            <uap:DefaultTile
              Wide310x150Logo="Assets\WideLogo.png"
              Square310x310Logo="Assets\LargeLogo.png">
            </uap:DefaultTile>
            <uap:SplashScreen Image="Assets\SplashScreen.png" />
          </uap:VisualElements>
        </Application>
      </Applications>
```

## <span id="Use_notifications_to_customize_your_tile"> </span> <span id="use_notifications_to_customize_your_tile"> </span> <span id="USE_NOTIFICATIONS_TO_CUSTOMIZE_YOUR_TILE"> </span>알림을 사용하여 타일 사용자 지정


앱이 설치된 후 알림을 사용하여 타일을 사용자 지정할 수 있습니다. 앱이 처음 실행되거나 푸시 알림과 같은 일부 이벤트에 응답할 때 이렇게 할 수 있습니다.

1.  타일을 설명하는 XML 페이로드([**Windows.Data.Xml.Dom.XmlDocument**](https://msdn.microsoft.com/library/windows/apps/br206173) 형식)를 만듭니다.

    -   Windows 10에는 새로운 적응형 타일 스키마가 도입되었습니다. 자세한 내용은 [적응형 타일](tiles-and-notifications-create-adaptive-tiles.md)을 참조하세요. 이 스키마에 대한 자세한 내용은 [적응형 타일 스키마](tiles-and-notifications-adaptive-tiles-schema.md)를 참조하세요. 

    -   Windows 8.1 타일 템플릿을 사용하여 타일을 정의할 수 있습니다. 자세한 내용은 [타일 및 배지 만들기(Windows 8.1)](https://msdn.microsoft.com/library/windows/apps/xaml/hh868260)를 참조하세요.

2.  타일 알림 개체를 만들어 만든 [**XmlDocument**](https://msdn.microsoft.com/library/windows/apps/br206173)로 전달합니다. 여러 유형의 알림 개체가 있습니다.
    -   [
            **Windows.UI.NotificationsTileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616) 개체는 타일을 즉시 업데이트합니다.
    -   [
            **Windows.UI.Notifications.ScheduledTileNotification**](https://msdn.microsoft.com/library/windows/apps/hh701637) 개체는 타일을 나중에 업데이트합니다.

3.  [
            **Windows.UI.Notifications.TileUpdateManager.CreateTileUpdaterForApplication**](https://msdn.microsoft.com/library/windows/apps/br208623)을 사용하여 [**TileUpdater**](https://msdn.microsoft.com/library/windows/apps/br208628) 개체를 만듭니다.
4.  [
            **TileUpdater.Update**](https://msdn.microsoft.com/library/windows/apps/br208632) 메서드를 호출하여 2단계에서 만든 타일 알림 개체로 전달합니다.

 

 






<!--HONumber=Mar16_HO1-->


