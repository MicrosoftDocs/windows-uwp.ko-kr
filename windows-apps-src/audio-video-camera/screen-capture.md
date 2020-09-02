---
title: 화면 캡처
description: Windows.Graphics.Capture 네임스페이스는 디스플레이 또는 애플리케이션 창에서 프레임을 획득하고 비디오 스트림 또는 스냅샷을 만들어 협업 및 대화형 환경을 빌드하기 위해 API를 제공합니다.
ms.assetid: 349C959D-9C74-44E7-B5F6-EBDB5CA87B9F
ms.date: 06/14/2019
ms.topic: article
dev_langs:
- csharp
- vb
keywords: windows 10, uwp, 화면 캡처
ms.localizationpriority: medium
ms.openlocfilehash: b57be844e5ee10d384046aac651ab4f198f37d9e
ms.sourcegitcommit: 14c0b1ea2447a81ddf31982b40e19a74ecc6d59e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89310074"
---
# <a name="screen-capture"></a>화면 캡처

Windows 10 버전 1803부터, [Windows Graphics. Capture](/uwp/api/windows.graphics.capture) 네임 스페이스는 표시 또는 응용 프로그램 창에서 프레임을 획득 하는 api를 제공 하 여 비디오 스트림이나 스냅숏을 만들어 공동 작업 및 대화형 환경을 구축 합니다.

화면 캡처를 사용 하면 개발자는 최종 사용자를 위해 보안 시스템 UI를 호출 하 여 캡처할 표시 또는 응용 프로그램 창을 선택 하 고 시스템에서 능동적으로 캡처한 항목 주위에 노란색 알림 테두리를 그립니다. 여러 동시 캡처 세션의 경우 캡처된 각 항목 주위에 노란색 테두리가 그려집니다.

> [!NOTE]
> 화면 캡처 Api는 데스크톱 및 Windows Mixed Reality 몰입 형 헤드셋 에서만 지원 됩니다.

이 문서에서는 표시 또는 응용 프로그램 창에서 단일 이미지를 캡처하는 방법을 설명 합니다. 화면에서 비디오 파일로 캡처된 프레임을 인코딩하는 방법에 대 한 자세한 내용은 [비디오에 대 한 화면 캡처](screen-capture-video.md) 를 참조 하세요.

## <a name="add-the-screen-capture-capability"></a>화면 캡처 기능 추가

**Windows Graphics. Capture** 네임 스페이스에 있는 api는 응용 프로그램의 매니페스트에서 일반적인 기능을 선언 해야 합니다.

1. **솔루션 탐색기**에서 **appxmanifest.xml** 을 엽니다.
2. **기능** 탭을 선택합니다.
3. **그래픽 캡처**를 확인 합니다.

![그래픽 캡처](images/screen-capture-1.png)

## <a name="launch-the-system-ui-to-start-screen-capture"></a>시스템 UI를 시작 하 여 화면 캡처 시작

시스템 UI를 시작 하기 전에 응용 프로그램에서 현재 화면 캡처를 수행할 수 있는지 확인할 수 있습니다. 응용 프로그램에서 화면 캡처를 사용 하지 못할 수 있는 이유에는 여러 가지가 있습니다. 예를 들어 장치가 하드웨어 요구 사항을 충족 하지 않거나 캡처 대상 응용 프로그램이 화면 캡처를 대상으로 하는 경우를 포함 합니다. [GraphicsCaptureSession](/uwp/api/windows.graphics.capture.graphicscapturesession) 클래스의 **issupported** 메서드를 사용 하 여 UWP 화면 캡처가 지원 되는지 확인 합니다.

```csharp
// This runs when the application starts.
public void OnInitialization()
{
    if (!GraphicsCaptureSession.IsSupported())
    {
        // Hide the capture UI if screen capture is not supported.
        CaptureButton.Visibility = Visibility.Collapsed;
    }
}
```

```vb
Public Sub OnInitialization()
    If Not GraphicsCaptureSession.IsSupported Then
        CaptureButton.Visibility = Visibility.Collapsed
    End If
End Sub
```

화면 캡처가 지원 되는지 확인 한 후에는 [GraphicsCapturePicker](/uwp/api/windows.graphics.capture.graphicscapturepicker) 클래스를 사용 하 여 시스템 선택 UI를 호출 합니다. 최종 사용자는이 UI를 사용 하 여 화면 캡처를 수행할 표시 또는 응용 프로그램 창을 선택 합니다. 선택은 **GraphicsCaptureSession**를 만드는 데 사용 되는 [GraphicsCaptureItem](/uwp/api/windows.graphics.capture.graphicscaptureitem) 를 반환 합니다.

```csharp
public async Task StartCaptureAsync()
{
    // The GraphicsCapturePicker follows the same pattern the
    // file pickers do.
    var picker = new GraphicsCapturePicker();
    GraphicsCaptureItem item = await picker.PickSingleItemAsync();

    // The item may be null if the user dismissed the
    // control without making a selection or hit Cancel.
    if (item != null)
    {
        // We'll define this method later in the document.
        StartCaptureInternal(item);
    }
}
```

```vb
Public Async Function StartCaptureAsync() As Task
    ' The GraphicsCapturePicker follows the same pattern the
    ' file pickers do.
    Dim picker As New GraphicsCapturePicker
    Dim item As GraphicsCaptureItem = Await picker.PickSingleItemAsync()

    ' The item may be null if the user dismissed the
    ' control without making a selection or hit Cancel.
    If item IsNot Nothing Then
        StartCaptureInternal(item)
    End If
End Function
```

Ui 코드 이므로 UI 스레드에서 호출 해야 합니다. 응용 프로그램의 페이지에 대 한 코드 숨김으로 호출 하는 경우 (예: **MainPage.xaml.cs**)이 작업은 자동으로 수행 되지만 그렇지 않은 경우 다음 코드를 사용 하 여 UI 스레드에서 강제로 실행 되도록 할 수 있습니다.

```csharp
CoreWindow window = CoreApplication.MainView.CoreWindow;

await window.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, async () =>
{
    await StartCaptureAsync();
});
```

```vb
Dim window As CoreWindow = CoreApplication.MainView.CoreWindow
Await window.Dispatcher.RunAsync(CoreDispatcherPriority.Normal,
                                 Async Sub() Await StartCaptureAsync())
```

## <a name="create-a-capture-frame-pool-and-capture-session"></a>캡처 프레임 풀 및 캡처 세션 만들기

**GraphicsCaptureItem**를 사용 하 여 D3D 장치, 지원 되는 픽셀 형식 (**DXGI \_ 형식 \_ B8G8R8A8 \_ unorm**), 원하는 프레임 수 (임의 정수) 및 프레임 크기를 사용 하 여 [Direct3D11CaptureFramePool](/uwp/api/windows.graphics.capture.direct3d11captureframepool) 를 만듭니다. **GraphicsCaptureItem** 클래스의 **contentsize** 속성은 프레임 크기로 사용할 수 있습니다.

```csharp
private GraphicsCaptureItem _item;
private Direct3D11CaptureFramePool _framePool;
private CanvasDevice _canvasDevice;
private GraphicsCaptureSession _session;

public void StartCaptureInternal(GraphicsCaptureItem item)
{
    _item = item;

    _framePool = Direct3D11CaptureFramePool.Create(
        _canvasDevice, // D3D device
        DirectXPixelFormat.B8G8R8A8UIntNormalized, // Pixel format
        2, // Number of frames
        _item.Size); // Size of the buffers
}
```

```vb
WithEvents CaptureItem As GraphicsCaptureItem
WithEvents FramePool As Direct3D11CaptureFramePool
Private _canvasDevice As CanvasDevice
Private _session As GraphicsCaptureSession

Private Sub StartCaptureInternal(item As GraphicsCaptureItem)
    CaptureItem = item

    FramePool = Direct3D11CaptureFramePool.Create(
        _canvasDevice, ' D3D device
        DirectXPixelFormat.B8G8R8A8UIntNormalized, ' Pixel format
        2, '  Number of frames
        CaptureItem.Size) ' Size of the buffers
End Sub
```

다음으로 **CreateCaptureSession** 메서드에 **GraphicsCaptureItem** 를 전달 하 여 **Direct3D11CaptureFramePool** 에 대 한 **GraphicsCaptureSession** 클래스의 인스턴스를 가져옵니다.

```csharp
_session = _framePool.CreateCaptureSession(_item);
```

```vb
_session = FramePool.CreateCaptureSession(CaptureItem)
```

사용자가 응용 프로그램 창 캡처 또는 시스템 UI에 표시에 대 한 동의를 명시적으로 지정 하면 **GraphicsCaptureItem** 를 여러 **CaptureSession** 개체에 연결할 수 있습니다. 이러한 방식으로 응용 프로그램은 다양 한 환경에 대해 동일한 항목을 캡처하도록 선택할 수 있습니다.

여러 항목을 동시에 캡처하려면 응용 프로그램에서 캡처할 각 항목에 대 한 캡처 세션을 만들어야 합니다. 그러면 캡처할 각 항목에 대해 선택 UI를 호출 해야 합니다.

## <a name="acquire-capture-frames"></a>캡처 프레임 획득

프레임 풀과 캡처 세션이 만들어진 상태에서 **GraphicsCaptureSession** 인스턴스의 **startcapture** 메서드를 호출 하 여 시스템에 캡처 프레임을 보내기 시작 하도록 시스템에 알립니다.

```csharp
_session.StartCapture();
```

```vb
_session.StartCapture()
```

이러한 캡처 프레임 ( [Direct3D11CaptureFrame](/uwp/api/windows.graphics.capture.direct3d11captureframe) 개체)을 얻기 위해 **FrameArrived** 이벤트를 사용할 수 있습니다.

```csharp
_framePool.FrameArrived += (s, a) =>
{
    // The FrameArrived event fires for every frame on the thread that
    // created the Direct3D11CaptureFramePool. This means we don't have to
    // do a null-check here, as we know we're the only one  
    // dequeueing frames in our application.  

    // NOTE: Disposing the frame retires it and returns  
    // the buffer to the pool.
    using (var frame = _framePool.TryGetNextFrame())
    {
        // We'll define this method later in the document.
        ProcessFrame(frame);
    }  
};
```

```vb
Private Sub FramePool_FrameArrived(sender As Direct3D11CaptureFramePool, args As Object) Handles FramePool.FrameArrived
    ' The FrameArrived event is raised for every frame on the thread
    ' that created the Direct3D11CaptureFramePool. This means we
    ' don't have to do a null-check here, as we know we're the only
    ' one dequeueing frames in our application.  

    ' NOTE Disposing the frame retires it And returns  
    ' the buffer to the pool.

    Using frame = FramePool.TryGetNextFrame()
        ProcessFrame(frame)
    End Using
End Sub
```

**FrameArrived**에 가능 하면 UI 스레드를 사용 하지 않는 것이 좋습니다 .이 이벤트는 새 프레임을 사용할 수 있을 때마다 발생 하며이는 자주 발생 합니다. UI 스레드에 대해 **FrameArrived** 를 수신 하도록 선택 하는 경우 이벤트가 발생할 때마다 수행 하는 작업의 양을 염두에 둘 수 있습니다.

또는 필요한 모든 프레임을 가져올 때까지 **Direct3D11CaptureFramePool** 메서드를 사용 하 여 프레임을 수동으로 끌어올 수 있습니다.

**Direct3D11CaptureFrame** 개체에는 **contentsize**, **Surface**및 **SystemRelativeTime**속성이 포함 됩니다. **SystemRelativeTime** 은 다른 미디어 요소를 동기화 하는 데 사용할 수 있는 qpc ([queryperformancecounter](/windows/desktop/api/profileapi/nf-profileapi-queryperformancecounter)) 시간입니다.

## <a name="process-capture-frames"></a>프로세스 캡처 프레임

**TryGetNextFrame**를 호출할 때 **Direct3D11CaptureFramePool** 의 각 프레임을 체크 아웃 하 고 **Direct3D11CaptureFrame** 개체의 수명에 따라 다시 체크 인 합니다. 네이티브 응용 프로그램의 경우 **Direct3D11CaptureFrame** 개체를 해제 하면 프레임 풀에 프레임을 다시 체크 인할 수 있습니다. 관리 되는 응용 프로그램의 경우 **Direct3D11CaptureFrame** (c + +에서**닫기** ) 메서드를 사용 하는 것이 좋습니다. **Direct3D11CaptureFrame** 는 c # 호출자의 [IDisposable](/dotnet/api/system.idisposable) 으로 프로젝션 된 [은 windows.foundation.iclosable](/uwp/api/Windows.Foundation.IClosable) 인터페이스를 구현 합니다.

응용 프로그램은 **Direct3D11CaptureFrame** 개체에 대 한 참조를 저장 해서는 안 되며, 프레임이 다시 체크 인 된 후에도 기본 Direct3D 화면에 대 한 참조를 저장 하지 않아야 합니다.

프레임을 처리 하는 동안 응용 프로그램에서 **Direct3D11CaptureFramePool** 개체와 연결 된 동일한 장치에 대해 [ID3D11Multithread](/windows/desktop/api/d3d11_4/nn-d3d11_4-id3d11multithread) 잠금을 사용 하는 것이 좋습니다.

기본 Direct3D 표면은 **Direct3D11CaptureFramePool**를 만들거나 다시 만들 때 항상 지정 된 크기가 됩니다. 내용이 프레임 보다 큰 경우 내용이 프레임 크기로 잘립니다. 내용이 프레임 보다 작으면 프레임의 나머지 부분에는 정의 되지 않은 데이터가 포함 됩니다. 응용 프로그램은 정의 되지 않은 콘텐츠가 표시 되지 않도록 **Direct3D11CaptureFrame** 의 **contentsize** 속성을 사용 하 여 하위 사각형을 복사 하는 것이 좋습니다.

## <a name="take-a-screenshot"></a>스크린샷을 찍습니다.

이 예제에서는 각 **Direct3D11CaptureFrame** 를 [Win2D Api](https://microsoft.github.io/Win2D/html/Introduction.htm)의 일부인 [CanvasBitmap](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_CanvasBitmap.htm)로 변환 합니다.

```csharp
// Convert our D3D11 surface into a Win2D object.
CanvasBitmap canvasBitmap = CanvasBitmap.CreateFromDirect3D11Surface(
    _canvasDevice,
    frame.Surface);
```

**CanvasBitmap**을 만든 후에는 이미지 파일로 저장할 수 있습니다. 다음 예제에서는 사용자의 **저장 된 그림** 폴더에 PNG 파일로 저장 합니다.

```csharp
StorageFolder pictureFolder = KnownFolders.SavedPictures;
StorageFile file = await pictureFolder.CreateFileAsync("test.png", CreationCollisionOption.ReplaceExisting);

using (var fileStream = await file.OpenAsync(FileAccessMode.ReadWrite))
{
    await canvasBitmap.SaveAsync(fileStream, CanvasBitmapFileFormat.Png, 1f);
}
```

## <a name="react-to-capture-item-resizing-or-device-lost"></a>캡처 항목 크기 조정 또는 장치 손실에 대응

캡처 프로세스를 진행 하는 동안 응용 프로그램은 **Direct3D11CaptureFramePool**의 측면을 변경할 수 있습니다. 여기에는 새 Direct3D 장치를 제공 하거나, 프레임 버퍼의 크기를 변경 하거나, 풀 내의 버퍼 수를 변경 하는 작업이 포함 됩니다. 이러한 각 시나리오에서 **Direct3D11CaptureFramePool** 개체에 대 한 **다시 만들기** 메서드는 권장 도구입니다.

**다시 만들기** 가 호출 되 면 기존 프레임은 모두 삭제 됩니다. 이는 기본 Direct3D 서피스가 응용 프로그램에서 더 이상 액세스할 수 없는 장치에 속한 프레임을 처리 하지 않도록 하기 위한 것입니다. 따라서 **다시 만들기**를 호출 하기 전에 보류 중인 모든 프레임을 처리 하는 것이 좋을 수 있습니다.

## <a name="putting-it-all-together"></a>모든 항목 요약

다음 코드 조각은 UWP 응용 프로그램에서 화면 캡처를 구현 하는 방법의 종단 간 예제입니다. 이 샘플에는 프런트 엔드에 두 개의 단추가 있습니다. 하나는 **Button_ClickAsync**호출 하 고 다른 하나는 **ScreenshotButton_ClickAsync**를 호출 합니다.

> [!NOTE]
> 이 코드 조각은 2D 그래픽용 라이브러리인 [Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm)를 사용 합니다. 프로젝트에 맞게 설정 하는 방법에 대 한 자세한 내용은 해당 설명서를 참조 하세요.

```csharp
using Microsoft.Graphics.Canvas;
using Microsoft.Graphics.Canvas.UI.Composition;
using System;
using System.Numerics;
using System.Threading.Tasks;
using Windows.Foundation;
using Windows.Graphics;
using Windows.Graphics.Capture;
using Windows.Graphics.DirectX;
using Windows.Storage;
using Windows.UI;
using Windows.UI.Composition;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Hosting;

namespace ScreenCaptureTest
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        // Capture API objects.
        private SizeInt32 _lastSize;
        private GraphicsCaptureItem _item;
        private Direct3D11CaptureFramePool _framePool;
        private GraphicsCaptureSession _session;

        // Non-API related members.
        private CanvasDevice _canvasDevice;
        private CompositionGraphicsDevice _compositionGraphicsDevice;
        private Compositor _compositor;
        private CompositionDrawingSurface _surface;
        private CanvasBitmap _currentFrame;
        private string _screenshotFilename = "test.png";

        public MainPage()
        {
            this.InitializeComponent();
            Setup();
        }

        private void Setup()
        {
            _canvasDevice = new CanvasDevice();

            _compositionGraphicsDevice = CanvasComposition.CreateCompositionGraphicsDevice(
                Window.Current.Compositor,
                _canvasDevice);

            _compositor = Window.Current.Compositor;

            _surface = _compositionGraphicsDevice.CreateDrawingSurface(
                new Size(400, 400),
                DirectXPixelFormat.B8G8R8A8UIntNormalized,
                DirectXAlphaMode.Premultiplied);    // This is the only value that currently works with
                                                    // the composition APIs.

            var visual = _compositor.CreateSpriteVisual();
            visual.RelativeSizeAdjustment = Vector2.One;
            var brush = _compositor.CreateSurfaceBrush(_surface);
            brush.HorizontalAlignmentRatio = 0.5f;
            brush.VerticalAlignmentRatio = 0.5f;
            brush.Stretch = CompositionStretch.Uniform;
            visual.Brush = brush;
            ElementCompositionPreview.SetElementChildVisual(this, visual);
        }

        public async Task StartCaptureAsync()
        {
            // The GraphicsCapturePicker follows the same pattern the
            // file pickers do.
            var picker = new GraphicsCapturePicker();
            GraphicsCaptureItem item = await picker.PickSingleItemAsync();

            // The item may be null if the user dismissed the
            // control without making a selection or hit Cancel.
            if (item != null)
            {
                StartCaptureInternal(item);
            }
        }

        private void StartCaptureInternal(GraphicsCaptureItem item)
        {
            // Stop the previous capture if we had one.
            StopCapture();

            _item = item;
            _lastSize = _item.Size;

            _framePool = Direct3D11CaptureFramePool.Create(
               _canvasDevice, // D3D device
               DirectXPixelFormat.B8G8R8A8UIntNormalized, // Pixel format
               2, // Number of frames
               _item.Size); // Size of the buffers

            _framePool.FrameArrived += (s, a) =>
            {
                // The FrameArrived event is raised for every frame on the thread
                // that created the Direct3D11CaptureFramePool. This means we
                // don't have to do a null-check here, as we know we're the only
                // one dequeueing frames in our application.  

                // NOTE: Disposing the frame retires it and returns  
                // the buffer to the pool.

                using (var frame = _framePool.TryGetNextFrame())
                {
                    ProcessFrame(frame);
                }
            };

            _item.Closed += (s, a) =>
            {
                StopCapture();
            };

            _session = _framePool.CreateCaptureSession(_item);
            _session.StartCapture();
        }

        public void StopCapture()
        {
            _session?.Dispose();
            _framePool?.Dispose();
            _item = null;
            _session = null;
            _framePool = null;
        }

        private void ProcessFrame(Direct3D11CaptureFrame frame)
        {
            // Resize and device-lost leverage the same function on the
            // Direct3D11CaptureFramePool. Refactoring it this way avoids
            // throwing in the catch block below (device creation could always
            // fail) along with ensuring that resize completes successfully and
            // isn’t vulnerable to device-lost.
            bool needsReset = false;
            bool recreateDevice = false;

            if ((frame.ContentSize.Width != _lastSize.Width) ||
                (frame.ContentSize.Height != _lastSize.Height))
            {
                needsReset = true;
                _lastSize = frame.ContentSize;
            }

            try
            {
                // Take the D3D11 surface and draw it into a  
                // Composition surface.

                // Convert our D3D11 surface into a Win2D object.
                CanvasBitmap canvasBitmap = CanvasBitmap.CreateFromDirect3D11Surface(
                    _canvasDevice,
                    frame.Surface);

                _currentFrame = canvasBitmap;

                // Helper that handles the drawing for us.
                FillSurfaceWithBitmap(canvasBitmap);
            }

            // This is the device-lost convention for Win2D.
            catch (Exception e) when (_canvasDevice.IsDeviceLost(e.HResult))
            {
                // We lost our graphics device. Recreate it and reset
                // our Direct3D11CaptureFramePool.  
                needsReset = true;
                recreateDevice = true;
            }

            if (needsReset)
            {
                ResetFramePool(frame.ContentSize, recreateDevice);
            }
        }

        private void FillSurfaceWithBitmap(CanvasBitmap canvasBitmap)
        {
            CanvasComposition.Resize(_surface, canvasBitmap.Size);

            using (var session = CanvasComposition.CreateDrawingSession(_surface))
            {
                session.Clear(Colors.Transparent);
                session.DrawImage(canvasBitmap);
            }
        }

        private void ResetFramePool(SizeInt32 size, bool recreateDevice)
        {
            do
            {
                try
                {
                    if (recreateDevice)
                    {
                        _canvasDevice = new CanvasDevice();
                    }

                    _framePool.Recreate(
                        _canvasDevice,
                        DirectXPixelFormat.B8G8R8A8UIntNormalized,
                        2,
                        size);
                }
                // This is the device-lost convention for Win2D.
                catch (Exception e) when (_canvasDevice.IsDeviceLost(e.HResult))
                {
                    _canvasDevice = null;
                    recreateDevice = true;
                }
            } while (_canvasDevice == null);
        }

        private async void Button_ClickAsync(object sender, RoutedEventArgs e)
        {
            await StartCaptureAsync();
        }

        private async void ScreenshotButton_ClickAsync(object sender, RoutedEventArgs e)
        {
            await SaveImageAsync(_screenshotFilename, _currentFrame);
        }

        private async Task SaveImageAsync(string filename, CanvasBitmap frame)
        {
            StorageFolder pictureFolder = KnownFolders.SavedPictures;

            StorageFile file = await pictureFolder.CreateFileAsync(
                filename,
                CreationCollisionOption.ReplaceExisting);

            using (var fileStream = await file.OpenAsync(FileAccessMode.ReadWrite))
            {
                await frame.SaveAsync(fileStream, CanvasBitmapFileFormat.Png, 1f);
            }
        }
    }
}
```

```vb
Imports System.Numerics
Imports Microsoft.Graphics.Canvas
Imports Microsoft.Graphics.Canvas.UI.Composition
Imports Windows.Graphics
Imports Windows.Graphics.Capture
Imports Windows.Graphics.DirectX
Imports Windows.UI
Imports Windows.UI.Composition
Imports Windows.UI.Xaml.Hosting

Partial Public NotInheritable Class MainPage
    Inherits Page

    ' Capture API objects.
    WithEvents CaptureItem As GraphicsCaptureItem
    WithEvents FramePool As Direct3D11CaptureFramePool

    Private _lastSize As SizeInt32
    Private _session As GraphicsCaptureSession

    ' Non-API related members.
    Private _canvasDevice As CanvasDevice
    Private _compositionGraphicsDevice As CompositionGraphicsDevice
    Private _compositor As Compositor
    Private _surface As CompositionDrawingSurface

    Sub New()
        InitializeComponent()
        Setup()
    End Sub

    Private Sub Setup()
        _canvasDevice = New CanvasDevice()
        _compositionGraphicsDevice = CanvasComposition.CreateCompositionGraphicsDevice(Window.Current.Compositor, _canvasDevice)
        _compositor = Window.Current.Compositor
        _surface = _compositionGraphicsDevice.CreateDrawingSurface(
            New Size(400, 400), DirectXPixelFormat.B8G8R8A8UIntNormalized, DirectXAlphaMode.Premultiplied)
        Dim visual = _compositor.CreateSpriteVisual()
        visual.RelativeSizeAdjustment = Vector2.One
        Dim brush = _compositor.CreateSurfaceBrush(_surface)
        brush.HorizontalAlignmentRatio = 0.5F
        brush.VerticalAlignmentRatio = 0.5F
        brush.Stretch = CompositionStretch.Uniform
        visual.Brush = brush
        ElementCompositionPreview.SetElementChildVisual(Me, visual)
    End Sub

    Public Async Function StartCaptureAsync() As Task
        ' The GraphicsCapturePicker follows the same pattern the
        ' file pickers do.
        Dim picker As New GraphicsCapturePicker
        Dim item As GraphicsCaptureItem = Await picker.PickSingleItemAsync()

        ' The item may be null if the user dismissed the
        ' control without making a selection or hit Cancel.
        If item IsNot Nothing Then
            StartCaptureInternal(item)
        End If
    End Function

    Private Sub StartCaptureInternal(item As GraphicsCaptureItem)
        ' Stop the previous capture if we had one.
        StopCapture()

        CaptureItem = item
        _lastSize = CaptureItem.Size

        FramePool = Direct3D11CaptureFramePool.Create(
            _canvasDevice, ' D3D device
            DirectXPixelFormat.B8G8R8A8UIntNormalized, ' Pixel format
            2, '  Number of frames
            CaptureItem.Size) ' Size of the buffers

        _session = FramePool.CreateCaptureSession(CaptureItem)
        _session.StartCapture()
    End Sub

    Private Sub FramePool_FrameArrived(sender As Direct3D11CaptureFramePool, args As Object) Handles FramePool.FrameArrived
        ' The FrameArrived event is raised for every frame on the thread
        ' that created the Direct3D11CaptureFramePool. This means we
        ' don't have to do a null-check here, as we know we're the only
        ' one dequeueing frames in our application.  

        ' NOTE Disposing the frame retires it And returns  
        ' the buffer to the pool.

        Using frame = FramePool.TryGetNextFrame()
            ProcessFrame(frame)
        End Using
    End Sub

    Private Sub CaptureItem_Closed(sender As GraphicsCaptureItem, args As Object) Handles CaptureItem.Closed
        StopCapture()
    End Sub

    Public Sub StopCapture()
        _session?.Dispose()
        FramePool?.Dispose()
        CaptureItem = Nothing
        _session = Nothing
        FramePool = Nothing
    End Sub

    Private Sub ProcessFrame(frame As Direct3D11CaptureFrame)
        ' Resize and device-lost leverage the same function on the
        ' Direct3D11CaptureFramePool. Refactoring it this way avoids
        ' throwing in the catch block below (device creation could always
        ' fail) along with ensuring that resize completes successfully And
        ' isn't vulnerable to device-lost.

        Dim needsReset As Boolean = False
        Dim recreateDevice As Boolean = False

        If (frame.ContentSize.Width <> _lastSize.Width) OrElse
            (frame.ContentSize.Height <> _lastSize.Height) Then
            needsReset = True
            _lastSize = frame.ContentSize
        End If

        Try
            ' Take the D3D11 surface and draw it into a  
            ' Composition surface.

            ' Convert our D3D11 surface into a Win2D object.
            Dim bitmap = CanvasBitmap.CreateFromDirect3D11Surface(
                _canvasDevice,
                frame.Surface)

            ' Helper that handles the drawing for us.
            FillSurfaceWithBitmap(bitmap)
            ' This is the device-lost convention for Win2D.
        Catch e As Exception When _canvasDevice.IsDeviceLost(e.HResult)
            ' We lost our graphics device. Recreate it and reset
            ' our Direct3D11CaptureFramePool.  
            needsReset = True
            recreateDevice = True
        End Try

        If needsReset Then
            ResetFramePool(frame.ContentSize, recreateDevice)
        End If
    End Sub

    Private Sub FillSurfaceWithBitmap(canvasBitmap As CanvasBitmap)
        CanvasComposition.Resize(_surface, canvasBitmap.Size)

        Using session = CanvasComposition.CreateDrawingSession(_surface)
            session.Clear(Colors.Transparent)
            session.DrawImage(canvasBitmap)
        End Using
    End Sub

    Private Sub ResetFramePool(size As SizeInt32, recreateDevice As Boolean)
        Do
            Try
                If recreateDevice Then
                    _canvasDevice = New CanvasDevice()
                End If
                FramePool.Recreate(_canvasDevice, DirectXPixelFormat.B8G8R8A8UIntNormalized, 2, size)
                ' This is the device-lost convention for Win2D.
            Catch e As Exception When _canvasDevice.IsDeviceLost(e.HResult)
                _canvasDevice = Nothing
                recreateDevice = True
            End Try
        Loop While _canvasDevice Is Nothing
    End Sub

    Private Async Sub Button_ClickAsync(sender As Object, e As RoutedEventArgs) Handles CaptureButton.Click
        Await StartCaptureAsync()
    End Sub

End Class
```

## <a name="record-a-video"></a>비디오 기록

응용 프로그램의 비디오를 기록 하려는 경우 [비디오에 대 한 화면 캡처](screen-capture-video.md)문서에 나와 있는 연습을 수행할 수 있습니다. 또는 Windows. a p a. p. a p a [네임 스페이스](/uwp/api/windows.media.apprecording)를 사용할 수 있습니다. 데스크톱 확장 SDK의 일부 이므로 데스크톱 에서만 작동 하므로 프로젝트에서이에 대 한 참조를 추가 해야 합니다. 자세한 내용은 [장치 패밀리 개요](/uwp/extension-sdks/device-families-overview) 를 참조 하세요.

## <a name="see-also"></a>추가 정보

* [Windows. Graphics. Capture 네임 스페이스](https://docs.microsoft.com/uwp/api/windows.graphics.capture)
* [비디오에 화면 캡처](screen-capture-video.md)