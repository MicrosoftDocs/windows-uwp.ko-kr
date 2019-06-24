---
ms.assetid: ''
description: 이 문서에서는 OpenCV(오픈 소스 컴퓨터 비전 라이브러리)와 함께 SoftwareBitmap 클래스를 사용하는 방법에 대해 설명합니다.
title: OpenCV로 비트맵 처리
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp, opencv, softwarebitmap
ms.localizationpriority: medium
ms.openlocfilehash: a137a4bddd7f86e7aed91a63033c54583dc71f08
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318522"
---
# <a name="process-bitmaps-with-opencv"></a>OpenCV로 비트맵 처리

이 문서에서는 다양한 이미지 처리 알고리즘을 제공하는 오픈 소스 네이티브 코드 라이브러리인 오픈 소스 컴퓨터 비전 라이브러리(OpenCV)를 사용하여 이미지를 나타내기 위해 여러 UWP API에 사용되는 **[SoftwareBitmap](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)** 클래스 사용법에 대해 설명합니다. 

이 문서의 예제는 C#을 사용하여 생성된 앱을 포함하여 UWP 앱에서 사용할 수 있는 네이티브 코드 Windows 런타임 구성 요소를 만드는 과정을 안내합니다. 이 도우미 구성 요소는 OpenCV의 흐림 이미지 처리 기능을 사용할 단일 메서드 **Blur**를 노출합니다. 이 구성 요소는 OpenCV 라이브러리에서 직접 사용할 수 있는 기본 이미지 데이터 버퍼에 대한 포인터를 가져오는 private 메서드를 구현하므로 다른 OpenCV 처리 기능을 구현하기 위해 도우미 구성 요소를 쉽게 확장할 수 있습니다. 

* **SoftwareBitmap** 사용에 대한 소개는 [비트맵 이미지 만들기, 편집 및 저장](imaging.md)을 참조하세요. 
* OpenCV 라이브러리를 사용하는 방법을 알아보려면 [https://opencv.org](https://opencv.org)로 이동하세요.
* 이 문서에서 설명한 OpenCV 도우미 구성 요소를 **[MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader)** 와 함께 사용해 카메라에서 프레임의 실시간 이미지 처리를 구현하는 방법을 알아보려면 [MediaFrameReader와 함께 OpenCV 사용](use-opencv-with-mediaframereader.md)을 참조하세요.
* 여러 가지 효과를 구현하는 전체 코드 예제를 보려면, Windows 유니버설 샘플 GitHub 리포의 [카메라 프레임 + OpenCV 샘플](https://go.microsoft.com/fwlink/?linkid=854003)을 참조하세요.

> [!NOTE] 
> 이 문서에서 자세히 설명한 OpenCVHelper 구성 요소에 사용되는 기술은 처리할 GPU 메모리가 아닌 CPU 메모리에 이미지 데이터가 있어야 합니다. 따라서 **[MediaCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture)** 클래스와 같은 이미지의 메모리 위치를 요청할 수 있는 API의 경우, CPU 메모리를 지정해야 합니다.

## <a name="create-a-helper-windows-runtime-component-for-opencv-interop"></a>OpenCV 상호 운용성을 위한 도우미 Windows 런타임 구성 요소 만들기

### <a name="1-add-a-new-native-code-windows-runtime-component-project-to-your-solution"></a>1. 새 네이티브 코드로 Windows 런타임 구성 요소 프로젝트를 솔루션에 추가

1. 솔루션 탐색기에서 솔루션을 마우스 오른쪽 단추로 클릭하고 **추가->새 프로젝트**를 선택하여 Visual Studio에서 솔루션에 새 프로젝트를 추가합니다. 
2. **Visual C++** 범주 아래에서 **Windows 런타임 구성 요소(유니버설 Windows)** 를 선택합니다. 이 예에서는 프로젝트 이름을 "OpenCVBridge"로 지정하고 **확인**을 클릭합니다. 
3. **새로운 Windows 유니버설 프로젝트** 대화 상자에서 앱의 최소 OS 버전과 대상을 선택하고 **확인**을 클릭합니다.
4. 솔루션 탐색기에서 자동 생성된 파일 Class1.cpp를 마우스 오른쪽 단추로 클릭하고, 확인 대화 상자가 나타나면 **제거**를 선택하고 **삭제**를 선택합니다. 그런 다음 Class1.h 헤더 파일을 삭제합니다.
5. OpenCVBridge 프로젝트 아이콘을 마우스 오른쪽 단추로 클릭 하 고 선택 **추가-> 클래스...** . 에 **클래스 추가** 대화 상자에 입력된 "OpenCVHelper" 합니다 **클래스 이름** 필드를 클릭 한 다음 **확인**합니다. 이후 단계에서 생성된 클래스 파일에 코드가 추가됩니다.

### <a name="2-add-the-opencv-nuget-packages-to-your-component-project"></a>2. 구성 요소 프로젝트에 OpenCV NuGet 패키지 추가

1. 솔루션 탐색기에서 OpenCVBridge 프로젝트 아이콘을 마우스 오른쪽 단추로 클릭하고 **NuGet 패키지 관리...** 를 선택합니다.
2. NuGet 패키지 관리자 대화 상자가 열리면 **찾아보기** 탭을 선택하고 검색 상자에 "OpenCV.Win"을 입력합니다.
3. "OpenCV.Win.Core"를 선택하고 **설치**를 클릭합니다. **미리 보기** 대화 상자에서 **확인**을 클릭합니다.
4. 같은 방법으로 "OpenCV.Win.ImgProc" 패키지를 설치합니다.

> [!NOTE]
> OpenCV.Win.Core 및 OpenCV.Win.ImgProc는 정기적으로 업데이트되지 않지만 이 페이지의 설명대로 OpenCVHelper를 만드는 데 계속 권장됩니다.

### <a name="3-implement-the-opencvhelper-class"></a>3. OpenCVHelper 클래스 구현

다음 코드를 OpenCVHelper.h 헤더 파일에 붙여 넣습니다. 이 코드는 설치된 *Core* 및 *ImgProc* 패키지에 대한 OpenCV 헤더 파일을 포함하고, 다음 단계에 표시될 세 가지 메서드를 선언합니다.

[!code-cpp[OpenCVHelperHeader](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.h#SnippetOpenCVHelperHeader)]

OpenCVHelper.cpp 파일의 기존 콘텐츠를 삭제한 후 다음 include 지시문을 추가합니다. 

[!code-cpp[OpenCVHelperInclude](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperInclude)]

include 지시문 뒤에 **using** 지시문을 추가합니다. 

[!code-cpp[OpenCVHelperUsing](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperUsing)]

그런 다음 메서드 **GetPointerToPixelData**를 OpenCVHelper.cpp에 추가합니다. 이 메서드는 **[SoftwareBitmap](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)** 을 가져오고, 일련의 대화를 통해 픽셀 데이터의 COM 인터페이스 표현을 가져옵니다. 그러면 기본 데이터 버퍼에 **char** 어레이로 포인터를 가져올 수 있습니다. 

먼저, **[LockBuffer](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.lockbuffer)** 를 호출하고 읽기쓰기 버퍼를 요청하여 OpenCV 라이브러리가 픽셀 데이터를 수정할 수 있도록 픽셀 데이터가 포함된 **[BitmapBuffer](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapbuffer)** 를 가져옵니다.  **[CreateReference](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapbuffer.CreateReference)**  을 가져오기 위해 호출 되는 **[IMemoryBufferReference](https://docs.microsoft.com/uwp/api/windows.foundation.imemorybufferreference)** 개체입니다. 그런 다음 **IMemoryBufferByteAccess** 인터페이스가 모든 Windows 런타임 클래스의 기본 인터페이스인 **IInspectable**로 캐스팅되고, 픽셀 데이터 버퍼를 **char** 어레이로 가져올 수 있는 **[IMemoryBufferByteAccess](https://docs.microsoft.com/previous-versions/mt297505(v=vs.85))** COM 인터페이스를 가져오기 위해 **[QueryInterface](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))** 가 호출됩니다. 마지막으로 **[IMemoryBufferByteAccess::GetBuffer](https://docs.microsoft.com/windows/desktop/WinRT/imemorybufferbyteaccess-getbuffer)** 를 호출하여 **char** 어레이를 채웁니다. 이 메서드에서 변환 단계 중 하나라도 실패하면 메서드는 **false**를 반환하여 추가 처리를 계속할 수 없음을 나타냅니다.

[!code-cpp[OpenCVHelperGetPointerToPixelData](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperGetPointerToPixelData)]

다음으로, 아래 표시된 **TryConvert** 메서드를 추가합니다. 이 메서드는 **SoftwareBitmap**을 가져오고, 매트릭스 개체 OpenCV가 이미지 데이터 버퍼를 나타내기 위해 사용하는 **Mat** 개체로 변환하려 합니다. 이 메서드는 위에서 픽셀 데이터 버퍼의 **char** 어레이 표현을 가져오도록 정의된 **GetPointerToPixelData** 메서드를 호출합니다. 이것이 성공하면 **Mat** 클래스의 생성자가 호출되고, 소스 **SoftwareBitmap** 개체에서 가져온 픽셀 너비와 높이를 전달합니다. 

> [!NOTE] 
> 이 예에서는 CV_8UC4 상수를 생성된 **Mat** 개체의 픽셀 형식으로 지정합니다. 즉, 이 예가 작동하기 위해서는 이 메서드로 전달된 **SoftwareBitmap**에 CV_8UC4와 동등한 프리멀티플라이된 알파와 함께 **[BGRA8](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapPixelFormat)** 이라는 **[BitmapPixelFormat](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapPixelFormat)** 속성 값이 있어야 합니다.

생성된 **Mat** 개체의 단순 복사본이 메서드에서 반환되어, 이 버퍼의 사본이 아닌 **SoftwareBitmap**이 참조하는 동일한 데이터 픽셀 데이터 버퍼에서 추가 처리가 이루어집니다.

[!code-cpp[OpenCVHelperTryConvert](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperTryConvert)]

마지막으로 이 예제의 도우미 클래스는 단일 이미지 처리 메서드인 **Blur**를 구현합니다. 이 메서드는 위에서 정의한 **TryConvert** 메서드를 사용해 흐림 작업의 소스 비트맵과 대상 비트맵을 나타내는 **Mat** 개체를 검색한 후 OpenCV ImgProc 라이브러리에서 **blur** 메서드를 호출합니다. **blur**의 다른 파라미터가 X 및 Y 방향으로 흐림 효과의 크기를 지정합니다.

[!code-cpp[OpenCVHelperBlur](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperBlur)]


## <a name="a-simple-softwarebitmap-opencv-example-using-the-helper-component"></a>도우미 구성 요소를 사용하는 단순한 SoftwareBitmap OpenCV 예제
이제 OpenCVBridge 구성 요소가 만들어졌으므로, OpenCV **blur** 메서드를 사용해 **SoftwareBitmap**을 수정하는 단순한 C# 앱을 만들 수 있습니다. UWP 앱에서 Windows 런타임 구성 요소에 액세스하려면 먼저 해당 구성 요소에 참조를 추가해야 합니다. 솔루션 탐색기에서 마우스 오른쪽 단추로 클릭 합니다 **참조** UWP 앱 프로젝트 및 선택 노드에서 **참조를 추가 하는 중...** . 참조 관리자 대화 상자에서 선택 **프로젝트-> 솔루션**합니다. OpenCVBridge 프로젝트 옆의 상자를 선택하고 **확인**을 클릭합니다.

사용자는 아래의 예제 코드를 통해 이미지 파일을 선택한 후 **[BitmapDecoder](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder)** 를 사용해 이미지의 **SoftwareBitmap** 표현을 만들 수 있습니다. **SoftwareBitmap** 작업 방법에 대한 자세한 내용은 [비트맵 이미지 만들기, 편집 및 저장](https://docs.microsoft.com/windows/uwp/audio-video-camera/imaging)을 참조하세요.

이 문서의 앞 부분에 언급한 바와 같이 **OpenCVHelper** 클래스의 경우, 제출하는 모든 **SoftwareBitmap** 이미지는 프리멀티플라이된 알파 값이 있는 BGRA8 픽셀 형식을 사용하여 인코딩되어야 합니다. 따라서 이미지가 이 형식이 아닌 경우 예제 코드가 **[변환](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapAlphaMode)** 을 호출하여 이미지를 예정된 형식으로 변환합니다.

그런 다음, 흐림 작업의 대상으로 사용될 **SoftwareBitmap**이 생성됩니다. 입력된 이미지 속성이 생성자에 대한 인수로 사용되어, 해당하는 형식으로 비트맵을 만듭니다.

**OpenCVHelper**의 새 인스턴스가 형성되고 **Blur** 메서드가 호출되어 원본과 대상 비트맵에 전달됩니다. 마지막으로 **SoftwareBitmapSource**가 생성되어 출력 이미지를 XAML **이미지** 컨트롤에 할당합니다.

이 샘플 코드는 기본 프로젝트 템플릿에 의해 포함 된 네임 스페이스 외에도 다음 네임 스페이스에서 Api를 사용 합니다.

[!code-cs[OpenCVMainPageUsing](./code/ImagingWin10/cs/MainPage.OpenCV.xaml.cs#SnippetOpenCVMainPageUsing)]

[!code-cs[OpenCVBlur](./code/ImagingWin10/cs/MainPage.OpenCV.xaml.cs#SnippetOpenCVBlur)]

## <a name="related-topics"></a>관련 항목

* [BitmapEncoder 옵션 참조](bitmapencoder-options-reference.md)
* [이미지 메타 데이터](image-metadata.md)
 

 




