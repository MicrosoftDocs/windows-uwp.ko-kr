---
ms.assetid: ''
description: 이 문서에서는 OpenCV (Open Source Computer Vision Library)에서 \Bitmap 클래스를 사용 하는 방법을 설명 합니다.
title: OpenCV로 비트맵 처리
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp, opencv, 고 비트맵
ms.localizationpriority: medium
ms.openlocfilehash: 9b1808c6940cbfc03c2572bd72ecf0c57cfd5010
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173677"
---
# <a name="process-bitmaps-with-opencv"></a>OpenCV로 비트맵 처리

이 문서에서는 다양 한 Windows 런타임 Api에서 이미지를 나타내는 데 사용 되는 기능 **[비트맵](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)** 클래스를 사용 하는 방법에 대해 설명 합니다 .이 클래스는 다양 한 이미지 처리 알고리즘을 제공 하는 오픈 소스, 기본 코드 라이브러리인 OpenCV (open Source Computer Vision Library)를 사용 합니다. 

이 문서의 예제에서는 c #을 사용 하 여 만든 앱을 포함 하 여 UWP 앱에서 사용할 수 있는 네이티브 코드 Windows 런타임 구성 요소를 만드는 과정을 안내 합니다. 이 도우미 구성 요소는 OpenCV의 흐림 이미지 처리 함수를 사용 하는 단일 메서드인 **흐림 효과**를 노출 합니다. 구성 요소는 기본 이미지 데이터 버퍼에 대 한 포인터를 가져오는 전용 메서드를 구현 하며,이를 통해 OpenCV 라이브러리에서 직접 사용할 수 있으며 도우미 구성 요소를 간단 하 게 확장 하 여 다른 OpenCV 처리 기능을 구현할 수 있습니다. 

* **\Bitmap**사용에 대 한 소개는 [비트맵 이미지 만들기, 편집 및 저장](imaging.md)을 참조 하세요. 
* OpenCV 라이브러리를 사용 하는 방법에 대 한 자세한 내용은을 참조 [https://opencv.org](https://opencv.org) 하세요.
* **[MediaFrameReader](/uwp/api/windows.media.capture.frames.mediaframereader)** 를 사용 하 여이 문서에 나와 있는 opencv 도우미 구성 요소를 사용 하 여 카메라의 프레임에 대 한 실시간 이미지 처리를 구현 하는 방법을 보려면 [MediaFrameReader에서 opencv 사용](use-opencv-with-mediaframereader.md)을 참조 하세요.
* 몇 가지 다른 효과를 구현 하는 전체 코드 예제는 Windows 유니버설 샘플 GitHub 리포지토리의 [카메라 프레임 + OpenCV 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV) 을 참조 하세요.

> [!NOTE] 
> 이 문서에서 자세히 설명 하는 OpenCVHelper 구성 요소에서 사용 하는 기술은 처리할 이미지 데이터가 GPU 메모리가 아닌 CPU 메모리에 있어야 합니다. 따라서 **[MediaCapture](/uwp/api/windows.media.capture.mediacapture)** 클래스와 같은 이미지의 메모리 위치를 요청 하는 데 사용할 수 있는 api의 경우 CPU 메모리를 지정 해야 합니다.

## <a name="create-a-helper-windows-runtime-component-for-opencv-interop"></a>OpenCV interop에 대 한 도우미 Windows 런타임 구성 요소 만들기

### <a name="1-add-a-new-native-code-windows-runtime-component-project-to-your-solution"></a>1. 새 네이티브 코드 Windows 런타임 구성 요소 프로젝트를 솔루션에 추가 합니다.

1. 솔루션 탐색기에서 솔루션을 마우스 오른쪽 단추로 클릭 하 고 **추가 >새 프로젝트**를 선택 하 여 Visual Studio의 솔루션에 새 프로젝트를 추가 합니다. 
2. **Visual C++** 범주 아래에서 **Windows 런타임 구성 요소 (유니버설 Windows)** 를 선택 합니다. 이 예에서는 프로젝트 이름을 "OpenCVBridge"로 하 고 **확인**을 클릭 합니다. 
3. **새 Windows 유니버설 프로젝트** 대화 상자에서 앱에 대 한 대상 및 최소 OS 버전을 선택 하 고 **확인**을 클릭 합니다.
4. 솔루션 탐색기에서 Class1 .cpp 파일을 마우스 오른쪽 단추로 클릭 하 고 **제거**를 선택 하 고 확인 대화 상자가 표시 되 면 **삭제**를 선택 합니다. 그런 다음, Class1. h 헤더 파일을 삭제 합니다.
5. OpenCVBridge 프로젝트 아이콘을 마우스 오른쪽 단추로 클릭 하 고 **추가->클래스 ...** 를 선택 합니다. **클래스 추가** 대화 상자에서 **클래스 이름** 필드에 "OpenCVHelper"를 입력 한 다음 **확인**을 클릭 합니다. 이후 단계에서 생성 된 클래스 파일에 코드가 추가 됩니다.

### <a name="2-add-the-opencv-nuget-packages-to-your-component-project"></a>2. 구성 요소 프로젝트에 OpenCV NuGet 패키지 추가

1. 솔루션 탐색기에서 OpenCVBridge 프로젝트 아이콘을 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리 ...** 를 선택 합니다.
2. NuGet 패키지 관리자 대화 상자가 열리면 **찾아보기** 탭을 선택 하 고 검색 상자에 "OpenCV. Win"을 입력 합니다.
3. "OpenCV"를 선택 하 고 **설치**를 클릭 합니다. **미리 보기** 대화 상자에서 **확인**을 클릭 합니다.
4. 동일한 절차를 사용 하 여 "OpenCV. 이미지" 패키지를 설치 합니다.

>[!NOTE]
>OpenCV. 핵심 및 OpenCV는 정기적으로 업데이트 되지 않으며 저장소 규정 준수 검사를 통과 하지 않으므로 이러한 패키지는 실험 전용입니다.

### <a name="3-implement-the-opencvhelper-class"></a>3. OpenCVHelper 클래스 구현

OpenCVHelper 헤더 파일에 다음 코드를 붙여 넣습니다. 이 코드에는 설치한 *Core* 및 *이미지 프로시저* 패키지에 대 한 OpenCV 헤더 파일이 포함 되어 있으며, 다음 단계에 표시 되는 세 가지 메서드를 선언 합니다.

[!code-cpp[OpenCVHelperHeader](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.h#SnippetOpenCVHelperHeader)]

OpenCVHelper 파일의 기존 내용을 삭제 한 후 다음 include 지시문을 추가 합니다. 

[!code-cpp[OpenCVHelperInclude](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperInclude)]

Include 지시문 뒤에 다음 **using** 지시문을 추가 합니다. 

[!code-cpp[OpenCVHelperUsing](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperUsing)]

그런 다음 **GetPointerToPixelData** 메서드를 OpenCVHelper에 추가 합니다. 이 메서드는 데이터 **[비트맵](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)** 을 사용 하 고 일련의 변환을 통해 기본 데이터 버퍼에 대 한 포인터를 **char** 배열로 가져올 수 있는 픽셀 데이터의 COM 인터페이스 표현을 가져옵니다. 

먼저,이 픽셀 데이터를 포함 하는 **[BitmapBuffer](/uwp/api/windows.graphics.imaging.bitmapbuffer)** 는, OpenCV 라이브러리에서 해당 픽셀 데이터를 수정할 수 있도록 읽기/쓰기 버퍼를 요청 하는 **[lockbuffer](/uwp/api/windows.graphics.imaging.softwarebitmap.lockbuffer)** 를 호출 하 여 가져옵니다.  **[CreateReference](/uwp/api/windows.graphics.imaging.bitmapbuffer.CreateReference)** 는 **[IMemoryBufferReference](/uwp/api/windows.foundation.imemorybufferreference)** 개체를 가져오기 위해 호출 됩니다. 그런 다음 **IMemoryBufferByteAccess** 인터페이스는 **IInspectable**로 캐스팅 되 고, 모든 Windows 런타임 클래스의 기본 인터페이스 이며, **[QueryInterface](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))** 는 **char** 배열로 픽셀 데이터 버퍼를 가져올 수 있도록 하는 **[IMemoryBufferByteAccess](/previous-versions/mt297505(v=vs.85))** COM 인터페이스를 가져오기 위해 호출 됩니다. 마지막으로 **[IMemoryBufferByteAccess:: GetBuffer](/windows/desktop/WinRT/imemorybufferbyteaccess-getbuffer)** 를 호출 하 여 **char** 배열을 채웁니다. 이 메서드의 변환 단계가 실패 하는 경우이 메서드는 **false**를 반환 하 여 추가 처리를 계속할 수 없음을 나타냅니다.

[!code-cpp[OpenCVHelperGetPointerToPixelData](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperGetPointerToPixelData)]

다음으로, 아래에 표시 된 **Trconvert** 메서드 메서드를 추가 합니다. 이 메서드는 데이터 버퍼를 나타내는 데 사용 되는 행렬 개체 인 데이터 **비트맵** 을 사용 하 여에 **지 개체로 변환** 하려고 시도 합니다. 이 메서드는 위에 정의 된 **GetPointerToPixelData** 메서드를 호출 하 여 픽셀 데이터 버퍼의 **char** 배열 표현을 가져옵니다. 이 작업이 성공 하면 **이 클래스의** 생성자가 호출 **되어 소스 개체** 의 픽셀 너비와 높이를 전달 합니다. 

> [!NOTE] 
> 이 예에서는 CV_8UC4 상수 **를 만든 대** 만 개체의 픽셀 형식으로 지정 합니다. 이는이 메서드에 **전달 된** **[BitmapPixelFormat](/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapPixelFormat)** 속성 값이이 예제에서 사용할 수 있도록 미리 증가 된 알파를 사용 하는  **[BGRA8](/uwp/api/Windows.Graphics.Imaging.BitmapPixelFormat)** (CV_8UC4에 해당)가 있어야 함을 의미 합니다.

추가 처리가이 버퍼의 복사본이 **아니라,이** 버퍼에서 참조 하는 동일한 데이터 픽셀 데이터 버퍼에서 작동 **하도록 생성 된** 서 수 개체의 단순 복사본이 메서드에서 반환 됩니다.

[!code-cpp[OpenCVHelperTryConvert](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperTryConvert)]

마지막으로,이 예제 도우미 클래스는 위에서 정의한 **Trconvert** 메서드를 사용 하 여 흐림 작업을 위한 원본 비트맵과 대상 비트맵을 나타내는 대 **만 개체를** 검색 한 다음, OpenCV 이미지 프로시저 라이브러리에서 **흐리게** 메서드를 호출 하는 단일 이미지 처리 메서드인 **흐림을**구현 합니다. **흐림** 효과에 대 한 다른 매개 변수는 X 및 Y 방향의 흐림 효과의 크기를 지정 합니다.

[!code-cpp[OpenCVHelperBlur](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperBlur)]


## <a name="a-simple-softwarebitmap-opencv-example-using-the-helper-component"></a>도우미 구성 요소를 사용 하는 간단한 고 비트맵 OpenCV 예
이제 OpenCVBridge 구성 요소가 만들어졌으므로 OpenCV **흐림** **메서드를 사용**하는 간단한 c # 앱을 만들어이를 수정할 수 있습니다. UWP 앱에서 Windows 런타임 구성 요소에 액세스 하려면 먼저 구성 요소에 대 한 참조를 추가 해야 합니다. 솔루션 탐색기에서 UWP 앱 프로젝트 아래의 **참조** 노드를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가**...를 선택 합니다. 참조 관리자 대화 상자에서 **프로젝트->솔루션**을 선택 합니다. OpenCVBridge 프로젝트 옆의 확인란을 선택 하 고 **확인**을 클릭 합니다.

아래 예제 코드를 사용 하면 사용자가 이미지 파일을 선택 하 고 **[bitmapdecoder에서](/uwp/api/windows.graphics.imaging.bitmapencoder)** 를 사용 하 여 이미지의 기능 **비트맵** 표현을 만들 수 있습니다. **\Bitmap**으로 작업 하는 방법에 대 한 자세한 내용은 [비트맵 이미지 만들기, 편집 및 저장](./imaging.md)을 참조 하세요.

이 문서 앞부분에서 설명한 대로 **OpenCVHelper** 클래스를 사용 하려면 제공 된 모든 **\bitmap** 이미지를 미리 증가 된 알파 값과 함께 BGRA8 픽셀 형식을 사용 하 여 인코딩해야 합니다. 따라서 이미지가 아직이 형식이 아니면 예제 코드에서는 **[Convert](/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapAlphaMode)** 를 호출 하 여 이미지를 필요한 형식으로 변환 합니다.

다음으로, 흐림 작업의 대상으로 사용 되는에 대 한 고 개의 **비트맵** 을 만듭니다. 입력 이미지 속성은 형식이 일치 하는 비트맵을 만들기 위해 생성자에 대 한 인수로 사용 됩니다.

**OpenCVHelper** 의 새 인스턴스가 만들어지고 **흐림** 메서드를 호출 하 여 소스 및 대상 비트맵을 전달 합니다. 마지막으로, 출력 이미지를 XAML **이미지** 컨트롤에 할당 하는 **SoftwareBitmapSource** 가 만들어집니다.

이 샘플 코드는 기본 프로젝트 템플릿에 포함 된 네임 스페이스 외에도 다음 네임 스페이스의 Api를 사용 합니다.

[!code-cs[OpenCVMainPageUsing](./code/ImagingWin10/cs/MainPage.OpenCV.xaml.cs#SnippetOpenCVMainPageUsing)]

[!code-cs[OpenCVBlur](./code/ImagingWin10/cs/MainPage.OpenCV.xaml.cs#SnippetOpenCVBlur)]

## <a name="related-topics"></a>관련 항목

* [BitmapEncoder 옵션 참조](bitmapencoder-options-reference.md)
* [이미지 메타 데이터](image-metadata.md)
 

 