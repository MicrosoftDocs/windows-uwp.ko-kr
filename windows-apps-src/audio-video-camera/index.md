---
ms.assetid: 0fc12d26-f1cf-4da7-b5a7-735a5074b74a
description: 이 섹션에서는 사진, 비디오 또는 오디오를 캡처하거나 재생하거나 편집하는 UWP(유니버설 Windows 플랫폼) 앱을 만드는 방법에 대한 정보를 제공합니다.
title: 오디오 비디오 및 카메라
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: db6ee6e5b447bd54f69e35b5fd3736b03778d5a5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163957"
---
# <a name="audio-video-and-camera"></a>오디오 비디오 및 카메라


이 섹션에서는 사진, 비디오 또는 오디오를 캡처하거나 재생하거나 편집하는 UWP(유니버설 Windows 플랫폼) 앱을 만드는 방법에 대한 정보를 제공합니다.
 
| 항목                                                                                             | 설명                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [카메라](camera.md) | UWP 앱에 사용할 수 있는 카메라 기능 및 사용 방법을 보여 주는 방법 문서의 링크가 나와 있습니다. |
| [미디어 재생](media-playback.md) | 오디오 및 비디오 재생을 사용하는 UWP 앱을 만드는 방법에 대한 정보를 제공합니다. |
| [이미지 또는 동영상에서 얼굴 감지](detect-and-track-faces-in-an-image.md) | 비디오 프레임의 시퀀스에서 시간에 따라 얼굴을 추적하도록 [FaceTracker](/uwp/api/Windows.Media.FaceAnalysis.FaceTracker)를 사용하는 방법을 보여 줍니다. |
| [미디어 컴퍼지션 및 편집](media-compositions-and-editing.md) | [  **Windows.Media.Editing**](/uwp/api/Windows.Media.Editing) 네임스페이스의 API를 사용하여 사용자가 오디오 및 비디오 원본 파일에서 미디어 컴퍼지션을 만들 수 있게 허용하는 앱을 신속하게 개발하는 방법에 대해 설명합니다. |
| [사용자 지정 비디오 효과](custom-video-effects.md) | 비디오 스트림에 대한 사용자 지정 효과를 만들 수 있도록 하는 **IBasicVideoEffect** 인터페이스를 구현하는 Windows 런타임 구성 요소를 만드는 방법을 설명합니다. |
| [사용자 지정 오디오 효과](custom-audio-effects.md) | 오디오 스트림에 대한 사용자 지정 효과를 만들 수 있도록 하는 **IBasicAudioEffect** 인터페이스를 구현하는 Windows 런타임 구성 요소를 만드는 방법을 설명합니다. |
| [비트맵 이미지 만들기, 편집 및 저장](imaging.md) | [SoftwareBitmap](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 개체로 비트맵 이미지를 나타내어 이미지 파일을 로드하고 저장하는 방법을 설명합니다.  |
| [오디오 디바이스 정보 속성](audio-device-information-properties.md)  | 오디오 디바이스와 관련된 디바이스 정보 속성이 나열되어 있습니다. |
| [오디오 상태 변경 감지 및 대응](detect-and-respond-to-audio-state-changes.md)  | UWP 앱이 오디오 스트림 수준에서 시스템에 시작된 변경을 감지하고 이에 대응하는 방법에 대해 설명합니다. |
| [미디어 파일 코드 변환](transcode-media-files.md) | [Windows.Media.Transcoding](/uwp/api/Windows.Media.Transcoding) API를 사용하여 비디오 파일을 한 형식에서 다른 형식으로 트랜스코딩하는 방법을 보여 줍니다. |
| [백그라운드에서 미디어 파일 처리](process-media-files-in-the-background.md) | [MediaProcessingTrigger](/uwp/api/Windows.ApplicationModel.Background.MediaProcessingTrigger) 및 백그라운드 작업을 사용하여 백그라운드에서 미디어 파일을 처리하는 방법을 보여 줍니다. |
| [오디오 그래프](audio-graphs.md) | [Windows.Media.Audio](/uwp/api/Windows.Media.Audio) 네임스페이스의 API를 사용하여 오디오 라우팅, 믹싱 및 처리 시나리오에 대한 오디오 그래프를 만드는 방법을 보여 줍니다. |
| [MIDI](midi.md) | MIDI(Musical Instrument Digital Interface) 디바이스를 열거하고 UWP 앱에서 MIDI 메시지를 보내고 받는 방법을 보여 줍니다. |
| [디바이스에서 미디어 가져오기](import-media-from-a-device.md) | 사용 가능한 미디어 원본 검색, 비디오, 사진 및 사이드카 파일과 같은 파일 가져오기, 원본 디바이스에서 가져온 파일 삭제 등 디바이스에서 미디어를 가져오는 방법을 설명합니다. |
| [카메라 독립적 플래시](camera-independent-flashlight.md) | 디바이스 램프가 있는 경우 이러한 램프에 액세스하고 사용하는 방법을 보여 줍니다. 램프 기능은 디바이스의 카메라 및 카메라 플래시 기능과는 별도로 관리됩니다. |
| [지원되는 코덱](supported-codecs.md) | UWP 앱의 오디오, 비디오, 이미지 코덱 및 지원 형식이 나열되어 있습니다. |
| [설치된 코덱 쿼리](codec-query.md) | 디바이스에 설치된 오디오와 비디오의 인코더 및 디코더를 쿼리하는 방법을 보여 줍니다. |
| [화면 캡처](screen-capture.md) | [Windows.Graphics.Capture 네임스페이스](/uwp/api/windows.graphics.capture)를 사용하여 디스플레이 또는 애플리케이션 창에서 프레임을 가져오고 비디오 스트림 또는 스냅샷을 만들어 협업 및 대화형 환경을 빌드하는 방법에 대해 설명합니다. |

## <a name="see-also"></a>참고 항목
- [UWP 앱 개발](../develop/index.md)

 

 

 