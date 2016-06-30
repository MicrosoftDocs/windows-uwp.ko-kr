---
author: drewbatgit
ms.assetid: 0fc12d26-f1cf-4da7-b5a7-735a5074b74a
description: "이 섹션에서는 사진, 동영상 또는 오디오를 캡처하거나 재생하거나 편집하는 유니버설 Windows 앱을 만드는 방법에 대한 정보를 제공합니다."
title: "오디오, 동영상 및 카메라"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 8480eb181b6af26eae5950a1f1bf040d0cb5db99

---

# 오디오 비디오 및 카메라

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 섹션에서는 사진, 비디오 또는 오디오를 캡처하거나 재생하거나 편집하는 유니버설 Windows 앱을 만드는 방법에 대한 정보를 제공합니다.
 
| 항목                                                                                             | 설명                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [CameraCaptureUI를 사용하여 사진 및 비디오 캡처](capture-photos-and-video-with-cameracaptureui.md) | 이 문서에서는 [CameraCaptureUI](capture-photos-and-video-with-cameracaptureui.md) 클래스를 사용하여 Windows에 기본 제공된 카메라 UI로 사진 또는 동영상을 캡처하는 방법을 설명합니다.                                                                                                            |
| [MediaCapture를 사용하여 사진 및 비디오 캡처](capture-photos-and-video-with-mediacapture.md)       | 이 문서에서는 MediaCapture의 초기화 및 종료, 디바이스 방향 변경 처리를 비롯하여 [MediaCapture](https://msdn.microsoft.com/library/windows/apps/br241124) API를 사용하여 사진 및 동영상을 캡처하는 단계를 설명합니다.                                  |
| [이미지 또는 동영상에서 얼굴 감지](detect-and-track-faces-in-an-image.md)                         | 이 항목에서는 [FaceTracker](https://msdn.microsoft.com/library/windows/apps/dn974150)를 사용하여 비디오 프레임의 시퀀스에서 시간에 따라 얼굴을 추적하는 방법을 보여 줍니다.                                                                                                               |
| [미디어 컴퍼지션 및 편집](media-compositions-and-editing.md)                               | 이 문서에서는 [**Windows.Media.Editing**](https://msdn.microsoft.com/library/windows/apps/dn640565) 네임스페이스의 API를 사용하여 사용자가 오디오 및 비디오 소스 파일에서 미디어 컴퍼지션을 만들 수 있게 허용하는 앱을 신속하게 개발하는 방법에 대해 설명합니다.                                    |
                                                                                                                                        | [사용자 지정 동영상 효과](custom-video-effects.md)                               | 이 문서에서는 비디오 스트림에 대한 사용자 지정 효과를 만들 수 있도록 하는 IBasicVideoEffect 인터페이스를 구현하는 Windows 런타임 구성 요소를 만드는 방법을 설명합니다.                                                                                                                                |
| [이미징](imaging.md)                                                                             | 이 문서에서는 [SoftwareBitmap](https://msdn.microsoft.com/library/windows/apps/dn887358) 개체로 비트맵 이미지를 나타내어 이미지 파일을 로드하고 저장하는 방법을 설명합니다.                                                                                                                     |
| [오디오 디바이스 정보 속성](audio-device-information-properties.md)                                                                             | 이 문서에는 오디오 디바이스와 관련된 디바이스 정보 속성이 나열되어 있습니다.                                                                                                                      |
| [미디어 파일 코드 변환](transcode-media-files.md)                                                 | 동영상 파일을 한 형식에서 다른 형식으로 코드 변환하려면 [Windows.Media.Transcoding](https://msdn.microsoft.com/library/windows/apps/br207105) API를 사용할 수 있습니다.                                                                                                                                |
| [백그라운드에서 미디어 파일 처리](process-media-files-in-the-background.md)                 | 이 문서에서는 [MediaProcessingTrigger](https://msdn.microsoft.com/library/windows/apps/dn806005) 및 백그라운드 작업을 사용하여 백그라운드에서 미디어 파일을 처리하는 방법을 보여 줍니다.                                                                                             |
| [MediaSource를 사용하여 미디어 재생](media-playback-with-mediasource.md)                             | [MediaSource](https://msdn.microsoft.com/library/windows/apps/dn930905) 클래스는 로컬 또는 원격 파일과 같은 여러 원본에서 미디어를 참조하고 재생하는 일반적인 방법을 제공하며 기본 미디어 형식에 상관없이 미디어 데이터에 액세스하기 위한 공통 모델을 공개합니다.  |
| [적응 스트리밍](adaptive-streaming.md)                                                       | 이 문서에서는 적응 스트리밍 멀티미디어 콘텐츠의 재생을 UWP(유니버설 Windows 플랫폼) 앱에 추가하는 방법을 설명합니다. 이 기능은 현재 HLS(Http 라이브 스트리밍) 및 DASH(Dynamic Streaming over HTTP) 콘텐츠를 지원합니다.                                          |
| [백그라운드 오디오](background-audio.md)                                                           | 이 문서에서는 백그라운드에서 오디오를 재생하는 UWP 앱을 만드는 방법을 설명합니다.                                                                                                                                                                                                               |
| [시스템 미디어 전송 컨트롤](system-media-transport-controls.md)                             | [SystemMediaTransportControls](https://msdn.microsoft.com/library/windows/apps/dn278677) 클래스를 사용하면 Windows에서 기본 제공되는 앱에서 시스템 미디어 전송 컨트롤을 사용하고 앱이 현재 재생 중인 미디어에 대해 컨트롤이 표시하는 메타데이터를 업데이트할 수 있습니다. |
| [미디어 캐스팅](media-casting.md)                                                                 | 이 문서에서는 유니버설 Windows 앱에서 원격 디바이스로 미디어를 캐스팅하는 방법을 보여 줍니다.                                                                                                                                                                                                       |
| [오디오 그래프](audio-graphs.md)                                                                   | 이 문서에서는 [Windows.Media.Audio](https://msdn.microsoft.com/library/windows/apps/dn914341) 네임스페이스의 API를 사용하여 오디오 라우팅, 믹싱 및 처리 시나리오에 대한 오디오 그래프를 만드는 방법을 보여 줍니다.                                                                            |
| [MIDI](midi.md)                                                                                   | 이 문서에서는 MIDI(Musical Instrument Digital Interface) 디바이스를 열거하고 유니버설 Windows 앱에서 MIDI 메시지를 보내고 받는 방법을 보여 줍니다.                                                                                                                                   |
| [카메라 독립적 플래시](camera-independent-flashlight.md)                                 | 이 문서에서는 디바이스 램프가 있는 경우 이러한 램프에 액세스하고 사용하는 방법을 보여 줍니다. 램프 기능은 디바이스의 카메라 및 카메라 플래시 기능과는 별도로 관리됩니다.                                                                                                                 |
| [지원되는 코덱](supported-codecs.md)                                                           | 이 문서에는 UWP 앱의 형식 지원과 오디오 및 비디오 코덱이 나열되어 있습니다.                                                                                                                                                                                                                  |
| [PlayReady DRM](playready-client-sdk.md)                                                          | 이 항목에서는 UWP(유니버설 Windows 플랫폼) 앱에 PlayReady 보호된 미디어 콘텐츠를 추가하는 방법을 설명합니다.                                                                                                                                                                                |
| [PlayReady 암호화된 미디어 확장](playready-encrypted-media-extension.md)                     | 이 섹션에서는 Windows 8.1 버전과 달리 Windows 10 버전에 작성된 변경 사항을 지원하도록 PlayReady 웹앱을 수정하는 방법을 설명합니다.                                                                                                                                       |

 

 

 







<!--HONumber=Jun16_HO4-->


