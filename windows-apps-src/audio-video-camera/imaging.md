---
ms.assetid: 3FD2AA71-EF67-47B2-9332-3FFA5D3703EA
이 문서에서는 BitmapDecoder 및 BitmapEncoder를 사용하여 이미지 파일을 로드하고 저장하는 방법과 SoftwareBitmap 개체를 사용하여 비트맵 이미지를 나타내는 방법을 설명합니다.
이미징
---

# 이미징

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


이 문서에서는 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) 및 [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206)를 사용하여 이미지 파일을 로드하고 저장하는 방법과 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) 개체를 사용하여 비트맵 이미지를 나타내는 방법을 설명합니다.

**SoftwareBitmap** 클래스는 이미지 파일, [**WriteableBitmap**](https://msdn.microsoft.com/library/windows/apps/br243259) 개체, Direct3D 화면, 코드를 비롯한 다양한 소스에서 만들 수 있는 다용도 API입니다. **SoftwareBitmap**을 사용하면 서로 다른 픽셀 형식과 알파 모드를 손쉽게 변환할 수 있으며 픽셀 데이터에 낮은 수준으로 액세스할 수 있습니다. 또한 **SoftwareBitmap**은 다음과 같은 여러 Windows 기능에서 사용되는 공용 인터페이스입니다.

-   [
            **CapturedFrame**](https://msdn.microsoft.com/library/windows/apps/dn278725) - 카메라로 캡처된 프레임을 **SoftwareBitmap**으로 가져올 수 있습니다.

-   [
            **VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917) - **VideoFrame**의 **SoftwareBitmap** 표현을 가져올 수 있습니다.

-   [
            **FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) - **SoftwareBitmap**에서 얼굴을 검색할 수 있습니다.

이 문서의 샘플 코드는 다음 네임스페이스의 API를 사용합니다.

[!code-cs[Namespaces](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetNamespaces)]

## BitmapDecoder를 사용하여 이미지 파일에서 SoftwareBitmap 만들기

파일에서 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358)을 만들려면 이미지 데이터가 포함된 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 인스턴스를 가져옵니다. 이 예제에서는 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)를 사용하여 사용자가 이미지 파일을 선택할 수 있게 합니다.

[!code-cs[PickInputFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetPickInputFile)]

**StorageFile** 개체의 [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227116) 메서드를 호출하여 이미지 데이터가 포함된 임의 액세스 스트림을 가져옵니다. 정적 메서드 [**BitmapDecoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/br226182)를 호출하여 지정된 스트림에 대한 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) 클래스 인스턴스를 가져옵니다. [
            **GetSoftwareBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn887332)를 호출하여 이미지가 포함된 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) 개체를 가져옵니다.

[!code-cs[CreateSoftwareBitmapFromFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateSoftwareBitmapFromFile)]

## BitmapEncoder를 사용하여 파일에 SoftwareBitmap 저장

파일에 **SoftwareBitmap**을 저장하려면 이미지를 저장할 **StorageFile** 인스턴스를 가져옵니다. 이 예제에서는[**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871)를 사용하여 사용자가 출력 파일을 선택할 수 있게 합니다.

[!code-cs[PickOuputFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetPickOuputFile)]

**StorageFile** 개체의 [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227116) 메서드를 호출하여 이미지를 기록할 임의 액세스 스트림을 가져옵니다. 정적 메서드 [**BitmapEncoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/br226211)를 호출하여 지정된 스트림에 대한 [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206) 클래스 인스턴스를 가져옵니다. **CreateAsync**의 첫 번째 매개 변수는 이미지를 인코드하는 데 사용할 코덱을 나타내는 GUID입니다. **BitmapEncoder** 클래스는 인코더에서 지원하는 각 코덱에 대한 ID(예: [**JpegEncoderId**](https://msdn.microsoft.com/library/windows/apps/br226226))가 포함된 속성을 노출합니다.

[
            **SetSoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887337) 메서드를 사용하여 인코드될 이미지를 설정합니다. 이미지가 인코드되는 동안 이미지에 기본적인 변환을 적용할 [**BitmapTransform**](https://msdn.microsoft.com/library/windows/apps/br226254) 속성의 값을 설정할 수 있습니다. [
            **IsThumbnailGenerated**](https://msdn.microsoft.com/library/windows/apps/br226225) 속성은 인코더에서 미리 보기가 생성되는지 여부를 결정합니다. 일부 파일 형식은 미리 보기를 지원하지 않으므로, 이 기능을 사용할 때는 미리 보기가 지원되지 않는 경우 throw되는 지원되지 않는 작업 오류가 발생합니다.

인코더가 지정 파일에 이미지 데이터를 쓰도록 [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/br226216)를 호출합니다.

[!code-cs[SaveSoftwareBitmapToFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSaveSoftwareBitmapToFile)]

새 [**BitmapPropertySet**](https://msdn.microsoft.com/library/windows/apps/hh974338) 개체를 만들고 인코더 설정을 나타내는 하나 이상의 [**BitmapTypedValue**](https://msdn.microsoft.com/library/windows/apps/hh700687) 개체로 채워 **BitmapEncoder**를 만들 때 추가 인코딩 옵션을 지정할 수 있습니다. 지원되는 인코더 옵션의 목록을 보려면 [BitmapEncoder 옵션 참조](bitmapencoder-options-reference.md)를 참조하세요.

[!code-cs[UseEncodingOptions](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetUseEncodingOptions)]

## XAML 이미지 컨트롤과 함께 SoftwareBitmap 사용

[
            **Image**](https://msdn.microsoft.com/library/windows/apps/br242752) 컨트롤을 사용하여 XAML 페이지에 이미지를 표시하려면 먼저 XAML 페이지에 **Image** 컨트롤을 정의합니다.

[!code-xml[ImageControl](./code/ImagingWin10/cs/MainPage.xaml#SnippetImageControl)]

새 [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854) 개체를 만듭니다. [
            **SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn997856)를 호출하고 **SoftwareBitmap**을 전달하여 원본 개체의 콘텐츠를 설정합니다. 그러면 **Image** 컨트롤의 [**Source**](https://msdn.microsoft.com/library/windows/apps/br242760) 속성을 새로 만든 **SoftwareBitmapSource**로 설정할 수 있습니다.

[!code-cs[SoftwareBitmapToWriteableBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmapToWriteableBitmap)]

**SoftwareBitmapSource**를 사용하여 **SoftwareBitmap**을 [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101)의 [**ImageSource**](https://msdn.microsoft.com/library/windows/apps/br210105)로 설정할 수도 있습니다.

## WriteableBitmap에서 SoftwareBitmap 만들기

[
            **SoftwareBitmap.CreateCopyFromBuffer**](https://msdn.microsoft.com/library/windows/apps/dn887370)를 호출하고 **WriteableBitmap**의 **PixelBuffer** 속성을 제공해 픽셀 데이터를 설정하는 방법으로 기존 **WriteableBitmap**에서 **SoftwareBitmap**을 만들 수 있습니다. 두 번째 인수를 사용하면 새로 만든 **WriteableBitmap**에 대해 픽셀 형식을 요청할 수 있습니다. **WriteableBitmap**의 [**PixelWidth**](https://msdn.microsoft.com/library/windows/apps/br243253) 및 [**PixelHeight**](https://msdn.microsoft.com/library/windows/apps/br243251) 속성을 사용하여 새 이미지의 크기를 지정할 수 있습니다.

[!code-cs[WriteableBitmapToSoftwareBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetWriteableBitmapToSoftwareBitmap)]

## 프로그래밍 방식으로 SoftwareBitmap 만들기 또는 편집

지금까지 이 항목에서는 이미지 파일 작업에 대해 설명했습니다. 코드에서 프로그래밍 방식으로 새 **SoftwareBitmap**을 만들고 같은 기술을 사용하여 **SoftwareBitmap**의 픽셀 데이터에 액세스하고 이 데이터를 수정할 수도 있습니다.

**SoftwareBitmap**은 COM interop을 사용하여 픽셀 데이터가 포함된 원시 버퍼를 노출합니다.

COM interop을 사용하려면 프로젝트의 **System.Runtime.InteropServices** 네임스페이스에 참조를 포함해야 합니다.

[!code-cs[InteropNamespace](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetInteropNamespace)]

네임스페이스 내에 다음 코드를 추가하여 [**IMemoryBufferByteAccess**](https://msdn.microsoft.com/library/windows/desktop/mt297505) COM 인터페이스를 초기화합니다.

[!code-cs[COMImport](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCOMImport)]

원하는 픽셀 형식 및 크기를 사용하여 새 **SoftwareBitmap**을 만듭니다. 또는 기존 **SoftwareBitmap**을 사용하여 픽셀 데이터를 편집할 수도 있습니다. [
            **SoftwareBitmap.LockBuffer**](https://msdn.microsoft.com/library/windows/apps/dn887380)를 호출하여 픽셀 데이터 버퍼를 나타내는 [**BitmapBuffer**](https://msdn.microsoft.com/library/windows/apps/dn887325) 클래스 인스턴스를 가져옵니다. **BitmapBuffer**를 **IMemoryBufferByteAccess** COM 인터페이스로 캐스팅한 다음 [**IMemoryBufferByteAccess.GetBuffer**](https://msdn.microsoft.com/library/windows/desktop/mt297506)를 호출하여 바이트 배열에 데이터를 채웁니다. [
            **BitmapBuffer.GetPlaneDescription**](https://msdn.microsoft.com/library/windows/apps/dn887330) 메서드를 사용하여 각 픽셀의 버퍼에 대한 오프셋을 계산하는 데 유용한 [**BitmapPlaneDescription**](https://msdn.microsoft.com/library/windows/apps/dn887342) 개체를 가져옵니다.

[!code-cs[CreateNewSoftwareBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateNewSoftwareBitmap)]

이 메서드는 Windows 런타임 형식의 기반이 되는 원시 버퍼에 액세스하므로 **unsafe** 키워드를 사용하여 선언해야 합니다. 또한, 안전하지 않은 코드의 컴파일을 허용하도록 Microsoft Visual Studio에서 프로젝트를 구성해야 합니다. 그러려면 프로젝트의 **속성** 페이지를 열고 **빌드** 속성 페이지를 클릭한 후 **안전하지 않은 코드 허용** 확인란을 선택합니다.

## Direct3D 화면에서 SoftwareBitmap 만들기

Direct3D 화면에서 **SoftwareBitmap** 개체를 만들려면 프로젝트에 [**Windows.Graphics.DirectX.Direct3D11**](https://msdn.microsoft.com/library/windows/apps/dn895104) 네임스페이스를 포함해야 합니다.

[!code-cs[Direct3DNamespace](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetDirect3DNamespace)]

[
            **CreateCopyFromSurfaceAsync**](https://msdn.microsoft.com/library/windows/apps/dn887373)를 호출하여 이 화면에서 새 **SoftwareBitmap**을 만듭니다. 이름에서 알 수 있듯이, 새 **SoftwareBitmap**에는 별도의 이미지 데이터 복사본이 있습니다. **SoftwareBitmap**을 수정해도 Direct3D 화면에는 아무런 영향이 없습니다.

[!code-cs[CreateSoftwareBitmapFromSurface](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateSoftwareBitmapFromSurface)]

## SoftwareBitmap을 다른 픽셀 형식으로 변환

**SoftwareBitmap** 클래스는 정적 메서드 [**Convert**](https://msdn.microsoft.com/library/windows/apps/dn887362)를 제공하며, 이 메서드를 사용하면 지정된 픽셀 형식과 알파 모드를 사용하는 새로운 **SoftwareBitmap**을 기존 **SoftwareBitmap**에서 쉽게 만들 수 있습니다. 새로 만들어진 비트맵에는 별도의 이미지 데이터 복사본이 있습니다. 새 비트맵을 수정해도 소스 비트맵에는 영향이 없습니다.

[!code-cs[Convert](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetConvert)]

## 이미지 파일 코드 변환

이미지 파일을 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176)에서 [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206)로 바로 코드 변환할 수 있습니다. 코드 변환할 파일에서 [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241731)을 만들 수 있습니다. 입력 스트림에서 새 **BitmapDecoder**를 만듭니다. 인코더가 [**BitmapEncoder.CreateForTranscodingAsync**](https://msdn.microsoft.com/library/windows/apps/br226214)에 쓰고 이 메서드를 호출하여 메모리 내 스트림 및 디코더 개체를 전달하도록 새 [**InMemoryRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241720)을 만듭니다. 원하는 인코딩 속성을 설정합니다. 입력 이미지 파일의 속성 중 인코더에서 특정하게 설정하지 않은 속성은 변경되지 않은 상태로 출력 파일에 기록됩니다. 인코더가 메모리 내 스트림에 인코드하도록 [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/br226216)를 호출합니다. 마지막으로, 파일 스트림 및 메모리 내 스트림을 시작 부분에서 찾고 [**CopyAsync**](https://msdn.microsoft.com/library/windows/apps/hh701827)를 호출하여 메모리 내 스트림을 파일 스트림에 씁니다.

[!code-cs[TranscodeImageFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetTranscodeImageFile)]

## 관련 항목

* [BitmapEncoder 옵션 참조](bitmapencoder-options-reference.md)
* [이미지 메타데이터](image-metadata.md)
 

 






<!--HONumber=Mar16_HO1-->


