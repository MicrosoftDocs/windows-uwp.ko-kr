---
title: 화면 캡처
description: Windows.Graphics.Capture 네임스페이스는 디스플레이 또는 응용 프로그램 창에서 프레임을 획득하고 비디오 스트림 또는 스냅숏을 만들어 공동 작업 및 대화형 환경을 빌드하기 위해 API를 제공합니다.
ms.assetid: 349C959D-9C74-44E7-B5F6-EBDB5CA87B9F
ms.date: 06/14/2019
ms.topic: article
dev_langs:
- csharp
- vb
keywords: windows 10, uwp, 화면 캡처
ms.localizationpriority: medium
ms.openlocfilehash: ad9a6bbc4055258b5f89b07d8670f3147eafc86d
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339766"
---
# <a name="screen-capture"></a>화면 캡처

Windows 10 버전 1803부터 [Windows.Graphics.Capture](https://docs.microsoft.com/uwp/api/windows.graphics.capture) 네임스페이스는 디스플레이 또는 응용 프로그램 창에서 프레임을 획득하고 비디오 스트림 또는 스냅숏을 만들어 공동 작업 및 대화형 환경을 빌드하기 위해 API를 제공합니다.

화면 캡처를 통해 개발자는 최종 사용자가 캡처할 디스플레이 또는 응용 프로그램 창을 선택하기 위해 보안 시스템 UI를 호출하고 적극적으로 캡처된 항목 주위에 있는 시스템을 통해 노란색 알림 테두리가 표시됩니다. 여러 개의 동시 캡처 세션의 경우 캡처 중인 각 항목 주위에 노란색 테두리가 그려집니다.

> [!NOTE]
> 화면 캡처 Api는 데스크톱 및 Windows Mixed Reality 몰입 형 헤드셋 에서만 지원 됩니다.

## <a name="add-the-screen-capture-capability"></a>화면 캡처 기능 추가

**Windows Graphics. Capture** 네임 스페이스에 있는 api는 응용 프로그램의 매니페스트에서 일반적인 기능을 선언 해야 합니다.

1. **솔루션 탐색기**에서 **appxmanifest.xml** 을 엽니다.
2. **접근 권한 값** 탭을 선택합니다.
3. **그래픽 캡처**를 확인 합니다.

![그래픽 캡처](images/screen-capture-1.png)

## <a name="launch-the-system-ui-to-start-screen-capture"></a>시스템 UI를 실행하여 화면 캡처 시작

시스템 UI를 시작하기 전에 현재 응용 프로그램이 현재 화면 캡처 지원하는지 확인할 수 있습니다. 디바이스가 하드웨어 요구 사항을 충족하지 못하는 경우나 캡처 대상 응용 프로그램이 화면 캡처를 차단하는 경우를 포함하여 응용 프로그램이 화면 캡처를 사용할 수 없는 몇 가지 이유가 있습니다. [GraphicsCaptureSession](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscapturesession) 클래스에서 **IsSupported** 메서드를 사용하여 UWP 화면 캡처 지원 여부를 결정합니다.

```cs
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

화면 캡처가 지원되는지 확인한 후 [GraphicsCapturePicker](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscapturepicker) 클래스를 사용하여 시스템 선택기 UI를 호출합니다. 최종 사용자는 이 UI를 사용하여 화면 캡처를 실행할 디스플레이 또는 응용 프로그램 창을 선택합니다. 선택기는 다음과 같이 **GraphicsCaptureSession**을 만드는 데 사용할 [GraphicsCaptureItem](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscaptureitem)을 반환합니다.

```cs
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

```cs
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

**GraphicsCaptureItem**를 사용 하 여 D3D 장치로 [Direct3D11CaptureFramePool](https://docs.microsoft.com/uwp/api/windows.graphics.capture.direct3d11captureframepool) , 지원 되는 픽셀 형식 (**DXGI @ no__t 형식 @ no__t-4B8G8R8A8 @ no__t-5orm**), 원하는 프레임 수 (정수 일 수 있음)를 만듭니다. ) 및 프레임 크기를 조정 합니다. **GraphicsCaptureItem** 클래스의 **ContentSize** 속성을 프레임의 크기로 사용할 수 있습니다.

```cs
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

그 다음 **GraphicsCaptureItem**를 **CreateCaptureSession** 메서드로 전달하여 **Direct3D11CaptureFramePool**에 대한 **GraphicsCaptureSession** 클래스의 인스턴스를 가져옵니다.

```cs
_session = _framePool.CreateCaptureSession(_item);
```

```vb
_session = FramePool.CreateCaptureSession(CaptureItem)
```

사용자가 응용 프로그램 창 캡처 또는 시스템 UI에서의 표시에 명시적으로 동의하면 **GraphicsCaptureItem**을 여러 개의 **CaptureSession** 개체에 연결할 수 있습니다. 이렇게 하면 응용 프로그램에서 다양한 환경에 대해 동일한 항목을 캡처하도록 선택할 수 있습니다.

동시에 여러 항목을 캡처하려면 응용 프로그램에서 캡처할 각 항목에 대한 캡처 세션을 만들어야합니다. 캡처할 각 항목에 대한 선택기 UI를 호출해야 합니다.

## <a name="acquire-capture-frames"></a>캡처 프레임 획득

프레임 풀 및 캡처 세션이 생성되면 **GraphicsCaptureSession** 인스턴스에서 **StartCapture** 메서드를 호출하고 시스템에 알려 캡처 프레임을 앱에 보냅니다.

```cs
_session.StartCapture();
```

```vb
_session.StartCapture()
```

[Direct3D11CaptureFrame](https://docs.microsoft.com/uwp/api/windows.graphics.capture.direct3d11captureframe) 개체인 이 캡처 프레임을 획득하기 위해 **Direct3D11CaptureFramePool.FrameArrived** 이벤트를 사용할 수 있습니다.

```cs
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

가능하면 **FrameArrived**의 경우 UI 스레드를 사용하지 않는 것이 좋습니다. 새 프레임을 사용할 수 있게 될 때마다 이 이벤트가 자주 발생하기 때문입니다. UI 스레드에서 **FrameArrived**를 수신하기로 선택할 경우 이벤트가 발생할 때마다 얼마나 많은 작업을 수행하고 있는지에 주의해야 합니다.

또는 필요한 모든 프레임을 가져올 때까지 **Direct3D11CaptureFramePool.TryGetNextFrame** 메서드로 프레임을 수동으로 가져올 수 있습니다.

**Direct3D11CaptureFrame** 개체는 **ContentSize**, **Surface**, **SystemRelativeTime** 속성을 포함합니다. **SystemRelativeTime**은 다른 미디어 요소를 동기화하는 데 사용할 수 있는 QPC([QueryPerformanceCounter](https://docs.microsoft.com/windows/desktop/api/profileapi/nf-profileapi-queryperformancecounter)) 시간입니다.

## <a name="process-capture-frames"></a>프로세스 캡처 프레임

**Direct3D11CaptureFramePool**의 각 프레임은 **TryGetNextFrame**를 호출할 때 확인되고 **Direct3D11CaptureFrame** 객체의 수명에 따라 다시 확인됩니다. 기본 응용 프로그램의 경우 **Direct3D11CaptureFrame** 개체를 해제하면 프레임 풀에서 프레임을 다시 충분히 확인할 수 있습니다. 관리 응용 프로그램의 경우 **Direct3D11CaptureFrame.Dispose**(C++에서 **Close**) 메서드를 사용하는 것이 좋습니다. **Direct3D11CaptureFrame**은 [IClosable](https://docs.microsoft.com/uwp/api/Windows.Foundation.IClosable) 인터페이스를 구현하고, 이것은 C# 호출자에 대해 [IDisposable](https://docs.microsoft.com/dotnet/api/system.idisposable)로서 프로젝션됩니다.

응용 프로그램은 **Direct3D11CaptureFrame** 개체에 대한 참조를 저장하지 말아야 하며 프레임을 다시 확인한 후에 기본 Direct3D 표면에 대한 참조를 저장해서도 안 됩니다.

프레임을 처리하는 동안 응용 프로그램은 **Direct3D11CaptureFramePool** 개체와 연결된 동일한 디바이스에서 [ID3D11Multithread](https://docs.microsoft.com/windows/desktop/api/d3d11_4/nn-d3d11_4-id3d11multithread) 잠금을 가져오는 것이 좋습니다.

기본 Direct3D 표면은 **Direct3D11CaptureFramePool**을 만들거나 다시 만들 때 항상 지정된 크기가 됩니다. 콘텐츠가 프레임보다 크면 콘텐츠가 프레임 크기로 잘립니다. 콘텐츠가 프레임보다 작은 경우 나머지 프레임에는 정의되지 않은 데이터를 포함합니다. 정의되지 않은 콘텐츠가 표시되지 않도록 응용 프로그램에서 이 **Direct3D11CaptureFrame**에 대한 **ContentSize** 속성을 사용하여 하위 리소스를 복사하는 것이 좋습니다.

## <a name="take-a-screenshot"></a>스크린샷을 찍습니다.

이 예제에서는 각 **Direct3D11CaptureFrame** 를 [Win2D Api](https://microsoft.github.io/Win2D/html/Introduction.htm)의 일부인 [CanvasBitmap](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_CanvasBitmap.htm)로 변환 합니다.

```cs
// Convert our D3D11 surface into a Win2D object.
CanvasBitmap canvasBitmap = CanvasBitmap.CreateFromDirect3D11Surface(
    _canvasDevice,
    frame.Surface);
```

**CanvasBitmap**을 만든 후에는 이미지 파일로 저장할 수 있습니다. 다음 예제에서는 사용자의 **저장 된 그림** 폴더에 PNG 파일로 저장 합니다.

```cs
StorageFolder pictureFolder = KnownFolders.SavedPictures;
StorageFile file = await pictureFolder.CreateFileAsync("test.png", CreationCollisionOption.ReplaceExisting);

using (var fileStream = await file.OpenAsync(FileAccessMode.ReadWrite))
{
    await canvasBitmap.SaveAsync(fileStream, CanvasBitmapFileFormat.Png, 1f);
}
```

## <a name="react-to-capture-item-resizing-or-device-lost"></a>캡처 항목 크기 조정 또는 디바이스 분실에 대응

캡처 프로세스 중에 응용 프로그램이 **Direct3D11CaptureFramePool**의 여러 측면을 변경하려고 할 수 있습니다. 여기에는 새로운 Direct3D 디바이스를 제공하거나, 프레임 버퍼의 크기를 변경하거나, 풀 내의 버퍼 수를 변경하는 등의 작업이 포함됩니다. 이 시나리오 각각에서 **Direct3D11CaptureFramePool** 개체의 **Recreate** 메서드가 권장되는 도구입니다.

**Recreate**를 호출하면 모든 기존 프레임이 삭제됩니다. 이것은 프레임을 전달하지 못하도록 하기 위한 것이며, 이 프레임의 기본 Direct3D 표면은 응용 프로그램이 더 이상 액세스 할 수 없는 장치에 속합니다. 이러한 이유로 **Recreate**를 호출하기 전에 보류 중인 모든 프레임을 처리하는 것이 좋습니다.

## <a name="putting-it-all-together"></a>요약

다음 코드 조각은 UWP 응용 프로그램에서 화면 캡처를 구현 하는 방법의 종단 간 예제입니다. 이 샘플에는 프런트 엔드에 두 개의 단추가 있습니다. 하나는 **Button_ClickAsync**를 호출 하 고 다른 하나는 **ScreenshotButton_ClickAsync**를 호출 합니다.

> [!NOTE]
> 이 코드 조각은 2D 그래픽용 라이브러리인 [Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm)를 사용 합니다. 프로젝트에 맞게 설정 하는 방법에 대 한 자세한 내용은 해당 설명서를 참조 하세요.

```cs
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

응용 프로그램의 비디오를 기록 하려는 경우에는 [Windows.](https://docs.microsoft.com/uwp/api/windows.media.apprecording)p a d. 데스크톱 확장 SDK의 일부 이므로 데스크톱 에서만 작동 하므로 프로젝트에서이에 대 한 참조를 추가 해야 합니다. 자세한 내용은 [장치 패밀리 개요](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview) 를 참조 하세요.

## <a name="see-also"></a>참조

* [Windows. Graphics. Capture 네임 스페이스](https://docs.microsoft.com/uwp/api/windows.graphics.capture)
