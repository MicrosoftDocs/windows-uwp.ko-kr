---
ms.assetid: 3FD2AA71-EF67-47B2-9332-3FFA5D3703EA
description: 이 문서에서는 Bitmapdecoder에서 및 BitmapEncoder를 사용 하 여 이미지 파일을 로드 하 고 저장 하는 방법과, \Bitmap 개체를 사용 하 여 비트맵 이미지를 나타내는 방법을 설명 합니다.
title: 비트맵 이미지 만들기, 편집 및 저장
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d503e6849a1a7b17678f856649f6b8194dc16722
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157447"
---
# <a name="create-edit-and-save-bitmap-images"></a>비트맵 이미지 만들기, 편집 및 저장



이 문서에서는 [**bitmapdecoder에서**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) 및 [**BitmapEncoder**](/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) 를 사용 하 여 이미지 파일을 로드 하 고 저장 하는 방법과, [**\bitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 개체를 사용 하 여 비트맵 이미지를 나타내는 방법을 설명 합니다.

**\Bitmap** 클래스는 이미지 파일, [**WriteableBitmap**](/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap) 개체, Direct3D 표면 및 코드를 비롯 한 여러 소스에서 만들 수 있는 다양 한 API입니다. **\Bitmap** 을 사용 하면 서로 다른 픽셀 형식과 알파 모드 간에 쉽게 변환할 수 있으며 픽셀 데이터에 대 한 하위 수준 액세스를 사용할 수 있습니다. 또한 다음과 같은 Windows의 여러 기능에서 사용 **되는 공용 인터페이스입니다.**

-   [**CapturedFrame**](/uwp/api/Windows.Media.Capture.CapturedFrame) 를 사용 하 여 카메라에서 캡처한 프레임을 **\비트맵**으로 가져올 수 있습니다.

-   [**Videoframe**](/uwp/api/Windows.Media.VideoFrame) 을 사용 하면 **videoframe**의 기능 **비트맵** 표현을 가져올 수 있습니다.

-   [**FaceDetector**](/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) 를 사용 하면 기능 **비트맵**에서 얼굴을 검색할 수 있습니다.

이 문서의 샘플 코드에서는 다음 네임 스페이스의 Api를 사용 합니다.

[!code-cs[Namespaces](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetNamespaces)]

## <a name="create-a-softwarebitmap-from-an-image-file-with-bitmapdecoder"></a>Bitmapdecoder에서를 사용 하 여 이미지 파일에서 \비트맵 만들기

파일에서 [**\Bitmap 비트맵**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 을 만들려면 이미지 데이터를 포함 하는 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 의 인스턴스를 가져옵니다. 이 예제에서는 [**Fileopenpicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 를 사용 하 여 사용자가 이미지 파일을 선택할 수 있도록 합니다.

[!code-cs[PickInputFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetPickInputFile)]

**StorageFile** 개체의 [**openasync**](/uwp/api/windows.storage.istoragefile.openasync) 메서드를 호출 하 여 이미지 데이터를 포함 하는 임의 액세스 스트림을 가져옵니다. 정적 메서드 [**bitmapdecoder에서**](/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync) 를 호출 하 여 지정 된 스트림에 대 한 [**bitmapdecoder에서**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) 클래스의 인스턴스를 가져옵니다. [**GetSoftwareBitmapAsync**](/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync) 를 호출 하 여 이미지를 포함 하는 [**\bitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 개체를 가져옵니다.

[!code-cs[CreateSoftwareBitmapFromFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateSoftwareBitmapFromFile)]

## <a name="save-a-softwarebitmap-to-a-file-with-bitmapencoder"></a>BitmapEncoder를 사용 하 여 파일에 \Bitmap 저장

파일에 저장 **비트맵** 을 저장 하려면 이미지가 저장 될 **StorageFile** 의 인스턴스를 가져옵니다. 이 예제에서는 [**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker) 를 사용 하 여 사용자가 출력 파일을 선택할 수 있도록 합니다.

[!code-cs[PickOutputFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetPickOutputFile)]

**StorageFile** 개체의 [**openasync**](/uwp/api/windows.storage.istoragefile.openasync) 메서드를 호출 하 여 이미지가 기록 될 임의 액세스 스트림을 가져옵니다. 정적 메서드 [**BitmapEncoder**](/uwp/api/windows.graphics.imaging.bitmapencoder.createasync) 를 호출 하 여 지정 된 스트림에 대 한 [**BitmapEncoder**](/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) 클래스의 인스턴스를 가져옵니다. **Createasync** 에 대 한 첫 번째 매개 변수는 이미지를 인코딩하는 데 사용 해야 하는 코덱을 나타내는 GUID입니다. **BitmapEncoder** 클래스는 인코더가 지 원하는 각 코덱 (예: [**JpegEncoderId**](/uwp/api/windows.graphics.imaging.bitmapencoder.jpegencoderid))에 대 한 ID를 포함 하는 속성을 노출 합니다.

[**Setsoftwarebitmap**](/uwp/api/windows.graphics.imaging.bitmapencoder.setsoftwarebitmap) 메서드를 사용 하 여 인코딩할 이미지를 설정 합니다. [**BitmapTransform**](/uwp/api/Windows.Graphics.Imaging.BitmapTransform) 속성의 값을 설정 하 여 인코딩 중인 이미지에 기본 변환을 적용할 수 있습니다. [**IsThumbnailGenerated**](/uwp/api/windows.graphics.imaging.bitmapencoder.isthumbnailgenerated) 속성은 인코더가 미리 보기를 생성할지 여부를 결정 합니다. 모든 파일 형식이 미리 보기를 지 원하는 것은 아니므로이 기능을 사용 하는 경우에는 미리 보기가 지원 되지 않는 경우에 throw 되는 지원 되지 않는 작업 오류를 catch 해야 합니다.

[**FlushAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync) 를 호출 하 여 인코더가 이미지 데이터를 지정 된 파일에 쓰도록 합니다.

[!code-cs[SaveSoftwareBitmapToFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSaveSoftwareBitmapToFile)]

새 [**BitmapPropertySet**](/uwp/api/Windows.Graphics.Imaging.BitmapPropertySet) 개체를 만들고 인코더 설정을 나타내는 하나 이상의 [**BitmapTypedValue**](/uwp/api/Windows.Graphics.Imaging.BitmapTypedValue) 개체로 채워서 **BitmapEncoder** 를 만들 때 추가 인코딩 옵션을 지정할 수 있습니다. 지원 되는 인코더 옵션 목록은 [BitmapEncoder options 참조](bitmapencoder-options-reference.md)를 참조 하세요.

[!code-cs[UseEncodingOptions](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetUseEncodingOptions)]

## <a name="use-softwarebitmap-with-a-xaml-image-control"></a>XAML 이미지 컨트롤과 함께 사용 비트맵 사용

[**이미지**](/uwp/api/Windows.UI.Xaml.Controls.Image) 컨트롤을 사용 하 여 xaml 페이지 내에 이미지를 표시 하려면 먼저 xaml 페이지에서 **이미지** 컨트롤을 정의 합니다.

[!code-xml[ImageControl](./code/ImagingWin10/cs/MainPage.xaml#SnippetImageControl)]

현재 **이미지** 컨트롤은 BGRA8 encoding 및 미리 곱하기 또는 알파 채널을 사용 하지 않는 이미지만 지원 합니다. 이미지를 표시 하기 전에 테스트 하 여 올바른 형식이 있는지 확인 하 고, 그렇지 않은 경우에 **는 해당 이미지** 를 지원 되는 [**Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) 형식으로 변환 합니다.

새 [**SoftwareBitmapSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) 개체를 만듭니다. [**SetBitmapAsync**](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync)를 호출하고 **SoftwareBitmap**을 전달하여 원본 개체의 콘텐츠를 설정합니다. 그런 다음 **이미지** 컨트롤의 [**Source**](/uwp/api/windows.ui.xaml.controls.image.source) 속성을 새로 만든 **SoftwareBitmapSource**로 설정할 수 있습니다.

[!code-cs[SoftwareBitmapToWriteableBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmapToWriteableBitmap)]

**SoftwareBitmapSource** 를 사용 하 여 ImageBrush에 대 한 [**ImageSource**](/uwp/api/windows.ui.xaml.media.imagebrush.imagesource) 로는 [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) **비트맵** 을 설정할 수도 있습니다.

## <a name="create-a-softwarebitmap-from-a-writeablebitmap"></a>WriteableBitmap에서 \Bitmap 비트맵 만들기

[**CreateCopyFromBuffer**](/uwp/api/windows.graphics.imaging.softwarebitmap.createcopyfrombuffer) 를 호출 하 고 **WriteableBitmap** 의 **PixelBuffer** 속성을 제공 하 여 픽셀 데이터를 설정 하면 기존 **WriteableBitmap** 에서 데이터 **비트맵** 을 만들 수 있습니다. 두 번째 인수를 사용 하 여 새로 만든 **WriteableBitmap**에 대 한 픽셀 형식을 요청할 수 있습니다. **WriteableBitmap** 의 [**PixelWidth**](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.pixelwidth) 및 [**PixelHeight**](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.pixelheight) 속성을 사용 하 여 새 이미지의 크기를 지정할 수 있습니다.

[!code-cs[WriteableBitmapToSoftwareBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetWriteableBitmapToSoftwareBitmap)]

## <a name="create-or-edit-a-softwarebitmap-programmatically"></a>프로그래밍 방식으로 \Bitmap 비트맵 만들기 또는 편집

지금까지이 항목에서는 이미지 파일 작업에 대해 설명 했습니다. 코드에서 프로그래밍 방식으로 새 데이터 **비트맵** 을 만들고 동일한 기술을 사용 하 여 기능 **비트맵**의 픽셀 데이터에 액세스 하 고 수정할 수도 있습니다.

**\Bitmap** 은 COM interop를 사용 하 여 픽셀 데이터를 포함 하는 원시 버퍼를 노출 합니다.

COM interop를 사용 하려면 프로젝트에 **t e m** 네임 스페이스에 대 한 참조를 포함 해야 합니다.

[!code-cs[InteropNamespace](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetInteropNamespace)]

네임 스페이스 내에 다음 코드를 추가 하 여 [**IMemoryBufferByteAccess**](/previous-versions/mt297505(v=vs.85)) COM 인터페이스를 초기화 합니다.

[!code-cs[COMImport](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCOMImport)]

원하는 픽셀 형식 및 크기를 사용 하 여 새 고가 **비트맵** 을 만듭니다. 또는 픽셀 데이터를 편집 하려는 기존 데이터 **비트맵** 을 사용 합니다. 데이터 버퍼를 나타내는 [**BitmapBuffer**](/uwp/api/Windows.Graphics.Imaging.BitmapBuffer) 클래스의 인스턴스를 가져오려면 [**\Bitmap. lockbuffer**](/uwp/api/windows.graphics.imaging.softwarebitmap.lockbuffer) 를 호출 합니다. **BitmapBuffer** 을 **IMemoryBufferByteAccess** COM 인터페이스로 캐스팅 한 다음 [**IMemoryBufferByteAccess**](/windows/desktop/WinRT/imemorybufferbyteaccess-getbuffer) 를 호출 하 여 바이트 배열을 데이터로 채웁니다. [**BitmapBuffer**](/uwp/api/windows.graphics.imaging.bitmapbuffer.getplanedescription) 메서드를 사용 하 여 각 픽셀에 대 한 버퍼의 오프셋을 계산 하는 데 도움이 되는 [**BitmapPlaneDescription**](/uwp/api/Windows.Graphics.Imaging.BitmapPlaneDescription) 개체를 가져옵니다.

[!code-cs[CreateNewSoftwareBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateNewSoftwareBitmap)]

이 메서드는 Windows 런타임 형식을 기반으로 하는 원시 버퍼에 액세스 하므로 **unsafe** 키워드를 사용 하 여 선언 해야 합니다. 또한 프로젝트의 **속성** 페이지를 열고, **빌드** 속성 페이지를 클릭 하 고, **안전 하지 않은 코드 허용** 확인란을 선택 하 여 안전 하지 않은 코드를 컴파일할 수 있도록 Microsoft Visual Studio에서 프로젝트를 구성 해야 합니다.

## <a name="create-a-softwarebitmap-from-a-direct3d-surface"></a>Direct3D surface에서에는 \비트맵 만들기

Direct3D surface에서 **개체를 만들려면 프로젝트** 에 [**Direct3D11**](/uwp/api/Windows.Graphics.DirectX.Direct3D11) 네임 스페이스를 포함 해야 합니다.

[!code-cs[Direct3DNamespace](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetDirect3DNamespace)]

[**CreateCopyFromSurfaceAsync**](/uwp/api/windows.graphics.imaging.softwarebitmap.createcopyfromsurfaceasync) 를 호출 하 여 화면에서 새 고가 **비트맵** 을 만듭니다. 이름이 나타내는 것 처럼 새 데이터 **비트맵** 에는 이미지 데이터의 개별 복사본이 있습니다. **이 경우에는 Direct3D** 화면에 영향을 주지 않습니다.

[!code-cs[CreateSoftwareBitmapFromSurface](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateSoftwareBitmapFromSurface)]

## <a name="convert-a-softwarebitmap-to-a-different-pixel-format"></a>다른 픽셀 형식으로 \Bitmap 비트맵 변환

**\Bitmap** 클래스는 정적 메서드인 [**Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert)를 제공 합니다 .이를 통해 기존 모델 **비트맵**에서 지정 **하는 픽셀** 형식 및 알파 모드를 사용 하는 새 모델을 쉽게 만들 수 있습니다. 새로 만든 비트맵에는 이미지 데이터의 개별 복사본이 있습니다. 새 비트맵을 수정 해도 원본 비트맵에는 영향을 주지 않습니다.

[!code-cs[Convert](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetConvert)]

## <a name="transcode-an-image-file"></a>이미지 파일 트랜스 코딩

이미지 파일을 [**bitmapdecoder에서**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) 에서 [**BitmapEncoder**](/uwp/api/Windows.Graphics.Imaging.BitmapEncoder)로 직접 변환할 수 있습니다. 트랜스 코딩 될 파일에서 [**IRandomAccessStream**](/uwp/api/Windows.Storage.Streams.IRandomAccessStream) 를 만듭니다. 입력 스트림에서 새 **bitmapdecoder에서** 을 만듭니다. 인코더에서 쓸 새 [**InMemoryRandomAccessStream**](/uwp/api/Windows.Storage.Streams.InMemoryRandomAccessStream) 을 만들고 [**BitmapEncoder**](/uwp/api/windows.graphics.imaging.bitmapencoder.createfortranscodingasync)를 호출 하 여 메모리 내 스트림과 디코더 개체를 전달 합니다. 트랜스 코딩 시에는 인코딩 옵션이 지원 되지 않습니다. 대신 [**Createasync**](/uwp/api/windows.graphics.imaging.bitmapencoder.createasync)를 사용 해야 합니다. 인코더에서 특별히 설정 하지 않은 입력 이미지 파일의 모든 속성은 변경 되지 않은 상태로 출력 파일에 기록 됩니다. [**FlushAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync) 를 호출 하 여 인코더가 메모리 내 스트림으로 인코딩합니다. 마지막으로 파일 스트림과 메모리 내 스트림을 검색 하 여 시작 부분으로 이동 하 고 [**Copyasync**](/uwp/api/windows.storage.streams.randomaccessstream.copyasync) 를 호출 하 여 메모리 내 스트림을 파일 스트림에 씁니다.

[!code-cs[TranscodeImageFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetTranscodeImageFile)]

## <a name="related-topics"></a>관련 항목

* [BitmapEncoder 옵션 참조](bitmapencoder-options-reference.md)
* [이미지 메타 데이터](image-metadata.md)
 

 