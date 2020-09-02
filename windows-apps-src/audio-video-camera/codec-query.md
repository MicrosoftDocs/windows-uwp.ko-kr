---
ms.assetid: 0A360481-B649-4E90-9BC4-4449BA7445EF
description: 장치에 설치 된 오디오 및 비디오 인코더 및 디코더를 쿼리 합니다.
title: 설치된 코덱 쿼리
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 코덱, 인코더, 디코더, 쿼리
ms.localizationpriority: medium
ms.openlocfilehash: f0f1ddff8336594e62ee26b6bf62b062039bf857
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363996"
---
# <a name="query-for-codecs-installed-on-a-device"></a>장치에 설치 된 코덱에 대 한 쿼리
**[CodecQuery](/uwp/api/windows.media.core.codecquery)** 클래스를 사용 하면 현재 장치에 설치 된 코덱을 쿼리할 수 있습니다. 다른 장치 제품군에 대 한 Windows 10에 포함 된 코덱 목록은 [지원 되는 코덱](supported-codecs.md)문서에 나열 되어 있지만 사용자 및 앱이 장치에 추가 코덱을 설치할 수 있으므로 런타임에 코덱 지원을 쿼리하여 현재 장치에서 사용 가능한 코덱을 확인할 수 있습니다.

CodecQuery API는 **[Windows](/uwp/api/windows.media.core)** 네임 스페이스의 멤버 이므로 앱에이 네임 스페이스를 포함 해야 합니다.

CodecQuery API는 **[Windows](/uwp/api/windows.media.core)** 네임 스페이스의 멤버 이므로 앱에이 네임 스페이스를 포함 해야 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetCodecQueryUsing":::

생성자를 호출 하 여 **CodecQuery** 클래스의 새 인스턴스를 초기화 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetNewCodecQuery":::

**[FindAllAsync](/uwp/api/windows.media.core.codecquery.findallasync)** 메서드는 제공 된 매개 변수와 일치 하는 설치 된 모든 코덱을 반환 합니다. 이러한 매개 변수에는 오디오 또는 비디오 코덱을 쿼리 하는지 여부를 지정 하는 **[CodecKind](/uwp/api/windows.media.core.codeckind)** 값, 인코더 또는 디코더를 쿼리 하는지 여부를 지정 하는 **[CodecCategory](/uwp/api/windows.media.core.codeccategory)** 값, 그리고 쿼리 하는 대상 미디어 인코딩 하위 형식 (예: H. 264 video 또는 MP3 audio)을 나타내는 문자열이 포함 됩니다.

모든 하위 형식에 대해 코덱을 반환 하려면 하위 형식 값으로 빈 문자열 또는 null을 지정 합니다. 다음 예제는 장치에 설치 된 모든 비디오 인코더를 나열 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetFindAllEncoders":::

**FindAllAsync** 에 전달 하는 하위 형식 문자열은 시스템에서 정의 된 하위 형식 GUID의 문자열 표현 이거나 하위 형식에 대 한 FOURCC 코드 일 수 있습니다. 지원 되는 미디어 하위 형식 guid 집합은 [오디오 하위 형식 guid](/windows/desktop/medfound/audio-subtype-guids) 및 [비디오 하위 형식](/windows/desktop/medfound/video-subtype-guids)Guid에 나열 되지만 **[CodecSubtypes](/uwp/api/windows.media.core.codecsubtypes)** 클래스는 지원 되는 각 하위 유형에 대 한 GUID 값을 반환 하는 속성을 제공 합니다. FOURCC 코드에 대 한 자세한 내용은 [FOURCC 코드](/windows/desktop/DirectShow/fourcc-codes) 를 참조 하세요. 

다음 예제에서는 FOURCC 코드 "H264"을 지정 하 여 장치에 h.264 비디오 디코더가 설치 되어 있는지 확인 합니다. H.264 비디오 콘텐츠를 재생 하기 전에이 쿼리를 수행할 수 있습니다. 재생 시 지원 되지 않는 코덱을 처리할 수도 있습니다. 자세한 내용은 [미디어 항목을 열 때 지원 되지 않는 코덱 및 알 수 없는 오류 처리](./media-playback-with-mediasource.md#handle-unsupported-codecs-and-unknown-errors-when-opening-media-items)를 참조 하세요.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetIsH264Supported":::

다음 예제에서는 FLAC 오디오 인코더가 현재 장치에 설치 되어 있는지 확인 하 고, 해당 하는 경우 파일에 오디오를 캡처하거나 다른 형식에서 FLAC 오디오 파일로 오디오를 변환 하는 데 사용할 수 있는 하위 형식에 대해 **[MediaEncodingProfile](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile)** 를 만듭니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetIsFLACSupported":::

## <a name="related-topics"></a>관련 항목

* [미디어 재생](media-playback.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [미디어 파일 코드 변환](transcode-media-files.md)
* [지원되는 코덱](supported-codecs.md)
 

 
