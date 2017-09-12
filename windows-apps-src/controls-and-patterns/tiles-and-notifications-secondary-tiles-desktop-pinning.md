---
author: vladimp
Description: "Windows 데스크톱 응용 프로그램은 데스크톱 브리지 덕분에 보조 타일을 고정할 수 있습니다!"
title: "데스크톱 응용 프로그램에서 보조 타일 고정"
label: Pin secondary tiles from desktop application
template: detail.hbs
ms.author: wdg-dev-content
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, 데스크톱 브리지, 보조 타일, 고정, 고정하기, 빠른 시작, 코드 샘플, 예, 보조타일, 데스크톱 응용 프로그램, win32, winforms, wpf"
ms.openlocfilehash: dea17368a21983b9ca800aac2efa6410dab06958
ms.sourcegitcommit: 6396a69aab081f5c7a9a59739c83538616d3b1c7
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/30/2017
---
# <a name="pin-secondary-tiles-from-desktop-application"></a>데스크톱 응용 프로그램에서 보조 타일 고정
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Win32, Windows Forms, WPF 등의 Windows 데스크톱 응용 프로그램은 [데스크톱 브리지](https://developer.microsoft.com/en-us/windows/bridges/desktop) 덕분에 보조 타일을 고정할 수 있습니다!

> [!IMPORTANT]
> **시험판 | 가을 크리에이터 업데이트 필요**: 데스크톱 응용 프로그램에서 보조 타일을 사용하려면 [참가자 빌드 16199](https://blogs.windows.com/windowsexperience/2017/05/17/announcing-windows-10-insider-preview-build-16199-pc-build-15215-mobile/#bDqf2Ah3Gd7FM66g.97) 이상을 실행 중이어야 합니다.

![보조 타일의 스크린샷](images/secondarytiles.png)

WPF 또는 WinForms 응용 프로그램에서 보조 타일을 추가하는 방법은 순수 UWP 앱과 매우 유사합니다. 유일한 차이점은 기본 창 핸들(HWND)을 지정해야 한다는 점입니다. 이러한 이유로 타일을 고정할 때 Windows는 타일을 고정할 것인지 사용자에게 확인을 요청하는 모달 대화 상자를 표시합니다. 데스크톱 응용 프로그램이 소유자 창을 통해 SecondaryTile 개체를 구성하지 않으면 Windows는 어디에 대화 상자를 그려야 할지 알 수 없고 따라서 작업이 실패합니다.


## <a name="package-your-app-with-desktop-bridge"></a>데스크톱 브리지를 사용하여 앱 패키징

아직 데스크톱 브리지를 사용하여 앱을 패키징하지 않은 경우 [패키징해야만](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-root) UWP API를 사용할 수 있습니다.


## <a name="enable-access-to-iinitializewithwindow-interface"></a>IInitializeWithWindow 인터페이스에 대한 액세스 사용

응용 프로그램이 C# 또는 Visual Basic 등 관리되는 언어로 작성된 경우 앱 코드의 IInitializeWithWindow 인터페이스를 다음 C# 예제에 나타나는 [ComImport](https://msdn.microsoft.com/library/system.runtime.interopservices.comimportattribute.aspx) 및 Guid 특성을 사용하여 선언합니다. 이 예제에서는 System.Runtime.InteropServices 네임스페이스에 대한 코드 파일에 using 문이 있다고 가정합니다.

```csharp
[ComImport]
[Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IInitializeWithWindow
{
    void Initialize(IntPtr hwnd);
}
```

또는 C++를 사용하는 경우 코드에 **shobjidl.h** 헤더 파일에 대한 참조를 추가합니다. 이 헤더 파일에는 *IInitializeWithWindow* 인터페이스의 선언이 포함되어 있습니다.


## <a name="initialize-the-secondary-tile"></a>보조 타일 초기화

일반 UWP 앱에서 하는 방법과 똑같은 방법으로 새 보조 타일 개체를 초기화합니다. 보조 타일을 만들고 고정하는 방법에 대한 자세한 내용은 [보조 타일 고정](tiles-and-notifications-secondary-tiles-pinning.md)을 참조하세요.

```csharp
// Initialize the tile with required arguments
SecondaryTile tile = new SecondaryTile(
    "myTileId5391",
    "Display name",
    "myActivationArgs",
    new Uri("ms-appx:///Assets/Square150x150Logo.png"),
    TileSize.Default);
```


## <a name="assign-the-window-handle"></a>창 핸들 할당

데스크톱 응용 프로그램의 핵심 단계입니다. 개체를 [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx) 개체로 캐스트합니다. 그런 다음 [IInitializeWithWindow.Initialize](https://msdn.microsoft.com/library/windows/desktop/hh706982.aspx) 메서드를 호출하고, 모달 대화 상자의 소유자가 되려는 창의 핸들을 전달합니다. 다음 C# 예제에서는 앱 주 창의 핸들을 메서드에 전달하는 방법을 보여 줍니다.

```csharp
// Assign the window handle
IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)tile;
initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
```


## <a name="pin-the-tile"></a>타일 고정

마지막으로, 일반 UWP 앱과 같은 방법으로 타일 고정을 요청합니다.

```csharp
// Pin the tile
bool isPinned = await tile.RequestCreateAsync();

// TODO: Update UI to reflect whether user can now either unpin or pin
```


## <a name="resources"></a>리소스

* [전체 코드 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [보조 타일 개요](tiles-and-notifications-secondary-tiles.md)
* [보조 타일 고정(UWP)](tiles-and-notifications-secondary-tiles-pinning.md)
* [데스크톱 브리지](https://developer.microsoft.com/en-us/windows/bridges/desktop)
* [데스크톱 브리지 코드 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)