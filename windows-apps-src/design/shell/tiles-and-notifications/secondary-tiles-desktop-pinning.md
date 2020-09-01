---
Description: Windows 데스크톱 응용 프로그램은 데스크톱 브리지 덕분에 보조 타일을 고정할 수 있습니다.
title: 데스크톱 응용 프로그램에서 보조 타일 고정
label: Pin secondary tiles from desktop application
template: detail.hbs
ms.date: 05/25/2017
ms.topic: article
keywords: windows 10, 데스크톱 브리지, 보조 타일, pin, 고정, 빠른 시작, 코드 샘플, 예제, secondarytile, 데스크톱 응용 프로그램, win32, winforms, wpf
ms.localizationpriority: medium
ms.openlocfilehash: 111d66e69ddb9cff56f36a26bd8094429fe808ef
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172377"
---
# <a name="pin-secondary-tiles-from-desktop-application"></a>데스크톱 응용 프로그램에서 보조 타일 고정


[데스크톱 브리징](https://developer.microsoft.com/windows/bridges/desktop)덕분에 Windows 데스크톱 응용 프로그램 (예: Win32, WINDOWS FORMS, WPF)은 보조 타일을 고정할 수 있습니다.

![보조 타일의 스크린샷](images/secondarytiles.png)

> [!IMPORTANT]
> **낙하 작성자 업데이트 필요**: 데스크톱 브리지 앱에서 보조 타일을 고정 하려면 SDK 16299을 대상으로 하 고 빌드 16299 이상을 실행 해야 합니다.

WPF 또는 WinForms 응용 프로그램에서 보조 타일을 추가 하는 것은 순수 UWP 앱과 매우 유사 합니다. 유일한 차이점은 기본 창 핸들 (HWND)을 지정 해야 한다는 것입니다. 이는 타일을 고정 하는 경우 사용자가 타일을 고정할 지 여부를 확인 하는 모달 대화 상자를 표시 하기 때문입니다. 데스크톱 응용 프로그램에서 소유자 창을 사용 하 여 SecondaryTile 개체를 구성 하지 않는 경우 Windows에서는 대화를 그릴 위치를 알지 못하며 작업이 실패 합니다.


## <a name="package-your-app-with-desktop-bridge"></a>데스크톱 브리지를 사용 하 여 앱 패키지

데스크톱 브리지를 사용 하 여 앱을 패키지 하지 않은 경우 Windows 런타임 Api를 사용 하기 전에 [먼저이 작업을 수행 해야 합니다](/windows/msix/desktop/source-code-overview) .


## <a name="enable-access-to-iinitializewithwindow-interface"></a>IInitializeWithWindow 인터페이스에 대 한 액세스 사용

응용 프로그램이 c # 또는 Visual Basic와 같은 관리 되는 언어로 작성 된 경우 다음 c # 예제와 같이 [ComImport](/dotnet/api/system.runtime.interopservices.comimportattribute) 및 Guid 특성을 사용 하 여 앱의 코드에서 IInitializeWithWindow 인터페이스를 선언 합니다. 이 예제에서는 코드 파일에 T e m 네임 스페이스에 대 한 using 문이 있다고 가정 합니다.

```csharp
[ComImport]
[Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IInitializeWithWindow
{
    void Initialize(IntPtr hwnd);
}
```

또는 c + +를 사용 하는 경우 코드에 **shobjidl. h** 헤더 파일에 대 한 참조를 추가 합니다. 이 헤더 파일에는 *Iinitializewithwindow* 인터페이스의 선언이 포함 되어 있습니다.


## <a name="initialize-the-secondary-tile"></a>보조 타일 초기화

정상적인 UWP 앱과 똑같이 새 보조 타일 개체를 초기화 합니다. 보조 타일을 만들고 고정 하는 방법에 대 한 자세한 내용은 [보조 타일 고정](secondary-tiles-pinning.md)을 참조 하세요.

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

데스크톱 응용 프로그램에 대 한 주요 단계입니다. 개체를 [Iinitializewithwindow](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iinitializewithwindow) 개체로 캐스팅 합니다. 그런 다음 [IInitializeWithWindow.Initialize](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-iinitializewithwindow-initialize) 메서드를 호출 하 고 원하는 창의 핸들을 모달 대화 상자의 소유자로 전달 합니다. 다음 c # 예제에서는 응용 프로그램의 주 창 핸들을 메서드에 전달 하는 방법을 보여 줍니다.

```csharp
// Assign the window handle
IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)tile;
initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
```


## <a name="pin-the-tile"></a>타일 고정

마지막으로 일반적인 UWP 앱과 마찬가지로 타일 고정을 요청 합니다.

```csharp
// Pin the tile
bool isPinned = await tile.RequestCreateAsync();

// TODO: Update UI to reflect whether user can now either unpin or pin
```


## <a name="send-tile-notifications"></a>타일 알림 보내기

> [!IMPORTANT]
> **4 월 2018 버전 17134.81 이상이 필요**합니다. 데스크톱 브리지 앱에서 보조 타일에 타일 또는 배지 알림을 보내려면 빌드 17134.81 이상을 실행 해야 합니다. 이. 81 업데이트를 설치 하기 전에 0x80070490 *요소를 찾을 수 없습니다* . 예외는 데스크톱 브리지 앱에서 보조 타일에 타일 또는 배지 알림을 보낼 때 발생 합니다.

타일 또는 배지 알림은 UWP 앱과 동일 합니다. 시작 하려면 [로컬 타일 알림 보내기](sending-a-local-tile-notification.md) 를 참조 하세요.


## <a name="resources"></a>리소스

* [전체 코드 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [보조 타일 개요](secondary-tiles.md)
* [보조 타일 고정 (UWP)](secondary-tiles-pinning.md)
* [데스크톱 브리지](https://developer.microsoft.com/windows/bridges/desktop)
* [데스크톱 브리지 코드 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)