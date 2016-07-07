---
author: drewbatgit
ms.assetid: 42A06423-670F-4CCC-88B7-3DCEEDDEBA57
description: "이 문서에서는 카메라 프로필을 사용하여 여러 다양한 비디오 캡처 디바이스의 기능을 검색 및 관리하는 방법을 설명합니다."
title: "카메라 프로필"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 755b2747b2250c4ad19970095aed220551389471

---

# 카메라 프로필

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


이 문서에서는 카메라 프로필을 사용하여 여러 다양한 비디오 캡처 디바이스의 기능을 검색 및 관리하는 방법을 설명합니다.

**참고**  
이 문서는 기본 사진 및 비디오 캡처 구현 단계를 설명하는 [MediaCapture를 사용하여 사진 및 비디오 캡처](capture-photos-and-video-with-mediacapture.md)에 설명된 개념 및 코드를 토대로 작성되었습니다. 좀 더 수준 높은 캡처 시나리오를 진행하기 전에 해당 문서의 기본 미디어 캡처 패턴을 좀 더 잘 이해하는 것이 좋습니다. 이 문서의 코드는 앱에 적절히 초기화된 MediaCapture의 인스턴스가 이미 있다고 가정합니다.

 

## 카메라 프로필 정보

여러 다른 디바이스의 카메라는 지원되는 캡처 해상도 집합, 비디오 캡처의 프레임 속도, HDR 또는 가변 프레임 속도 캡처 지원 여부를 비롯한 여러 다른 기능을 지원합니다. UWP(유니버설 Windows 플랫폼) 미디어 캡처 프레임워크는 [**MediaCaptureVideoProfileMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926695)에 이러한 기능 집합을 저장합니다. [**MediaCaptureVideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn926694) 개체로 나타내는 카메라 프로필은 사진 캡처용 1개, 비디오 캡처용 1개, 비디오 미리 보기용 1개로 이루어진 3개의 미디어 설명 컬렉션을 포함합니다.

[MediaCapture](capture-photos-and-video-with-mediacapture.md) 개체를 초기화하기 전에 현재 디바이스의 캡처 디바이스를 쿼리하여 지원되는 프로필을 확인할 수 있습니다. 지원되는 프로필을 선택하면 캡처 디바이스가 프로필 미디어 설명의 모든 기능을 지원하는지 알 수 있습니다. 이렇게 하면 특정 디바이스에서 지원되는 기능 조합을 파악하기 위해 시행착오를 거칠 필요가 없습니다.

기본 미디어 캡처에 대한 문서, [MediaCapture를 사용하여 사진 및 비디오 캡처](capture-photos-and-video-with-mediacapture.md)에서 미디어 캡처를 초기화하는 데 사용되는 [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/br226573)는 초기화에 필요한 최소 데이터 크기인 캡처 디바이스의 ID 문자열로만 만들어집니다.

[!code-cs[BasicInitExample](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetBasicInitExample)]

이 문서의 코드 예제에서는 이러한 최소한의 초기화를 다양한 기능을 지원하는 카메라 프로필 검색으로 바꿉니다. 그런 후 이러한 검색 기능을 사용하여 미디어 캡처 디바이스를 초기화할 수 있습니다.

## 카메라 프로필을 지원하는 비디오 디바이스 찾기

지원되는 카메라 프로필을 검색하기 전에 카메라 프로필의 사용을 지원하는 캡처 디바이스를 찾아야 합니다. 아래 예제에 정의된 **GetVideoProfileSupportedDeviceIdAsync** 도우미 메서드는[**DeviceInformaion.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) 메서드를 사용하여 사용 가능한 모든 비디오 캡처 디바이스 목록을 검색합니다. 목록의 모든 디바이스를 루핑하고 각 디바이스에 대해 정적 메서드인 [**IsVideoProfileSupported**](https://msdn.microsoft.com/library/windows/apps/dn926714)를 호출하여 비디오 프로필을 지원하는지 여부를 확인합니다. 또한 각 디바이스에 대해 [**EnclosureLocation.Panel**](https://msdn.microsoft.com/library/windows/apps/br229906) 속성을 호출하여 카메라를 디바이스 전방에 둘지 또는 후방에 둘지를 지정할 수 있습니다.

카메라 프로필을 지원하는 디바이스가 지정된 패널에 있는 경우 디바이스의 ID 문자열이 포함된 [**Id**](https://msdn.microsoft.com/library/windows/apps/br225437) 값이 반환됩니다.

[!code-cs[GetVideoProfileSupportedDeviceIdAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetVideoProfileSupportedDeviceIdAsync)]

**GetVideoProfileSupportedDeviceIdAsync** 도우미 메서드에서 반환된 디바이스 ID가 null 또는 빈 문자열이면 지정된 패널에 카메라 프로필을 지원하는 디바이스가 없는 것입니다. 이 경우 프로필을 사용하지 않고 미디어 캡처 디바이스를 초기화해야 합니다.

[!code-cs[GetDeviceWithProfileSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetDeviceWithProfileSupport)]

## 지원되는 해상도 및 프레임 속도에 따라 프로필 선택

특정 해상도 및 프레임 속도에 도달하는 기능을 비롯한 특정 기능을 갖는 프로필을 선택하려면 먼저 위에 정의된 도우미 메서드를 호출하여 카메라 프로필 사용을 지원하는 캡처 디바이스의 ID를 가져옵니다.

새 [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/br226573) 개체를 만들고 선택한 디바이스 ID를 제공합니다. 다음으로, 정적 메서드 [**MediaCapture.FindAllVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926708)를 호출하여 디바이스에서 지원하는 모든 카메라 프로필 목록을 가져옵니다.

이 예제에서는 using **System.Linq** 네임스페이스에 포함된 Linq 쿼리 메서드를 사용하여 [**Width**](https://msdn.microsoft.com/library/windows/apps/dn926700)와 [**Height**](https://msdn.microsoft.com/library/windows/apps/dn926697) 및 [**FrameRate**](https://msdn.microsoft.com/library/windows/apps/dn926696) 속성이 요청된 값과 일치하는 [**SupportedRecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926705) 개체가 포함된 프로필을 선택합니다. 일치하는 개체가 있으면 **MediaCaptureInitializationSettings**의 [**VideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn926679) 및 [**RecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926678)이 Linq 쿼리에서 반환된 무명 형식의 값으로 설정됩니다. 일치하는 개체가 없으면 기본 프로필이 사용됩니다.

[!code-cs[FindWVGA30FPSProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindWVGA30FPSProfile)]

원하는 카메라 프로필로 **MediaCaptureInitializationSettings**를 채운 후에는 미디어 캡처 개체에 대해 [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598)를 호출하여 원하는 프로필로 구성하면 됩니다.

[!code-cs[InitCaptureWithProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitCaptureWithProfile)]

## 동시 작업을 지원하는 프로필 선택

카메라 프로필을 사용하여 디바이스가 여러 카메라의 비디오 캡처를 동시에 지원하는지 여부를 확인할 수 있습니다. 이 시나리오에서는 전방 카메라와 후방 카메라에 1개씩 모두 2개의 캡처 개체 집합을 만들어야 합니다. 각 카메라에 대해 **MediaCapture**, **MediaCaptureInitializationSettings** 및 캡처 디바이스 ID를 보유하는 문자열을 만듭니다. 또한 동시성 지원 여부를 추적하는 부울 변수를 추가합니다.

[!code-cs[ConcurrencySetup](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConcurrencySetup)]

정적 메서드 [**MediaCapture.FindConcurrentProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926709)는 동시 작동도 지원할 수 있는 지정된 캡처 디바이스에서 지원하는 카메라 프로필의 목록을 반환합니다. Linq 쿼리를 사용하여 동시 작동을 지원하고 전방 및 후방 카메라 둘 다에서 지원하는 프로필을 찾습니다. 이러한 요구 사항을 만족하는 프로필이 있으면 각 **MediaCaptureInitializationSettings** 개체에 대해 해당 프로필을 설정하고 부울 동시 추적 변수를 true로 설정합니다.

[!code-cs[FindConcurrencyDevices](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindConcurrencyDevices)]

앱 시나리오의 기본 카메라에 대해 **MediaCapture.InitializeAsync**를 호출합니다. 동시 작업이 지원되는 경우 두 번째 카메라도 초기화합니다.

[!code-cs[InitConcurrentMediaCaptures](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitConcurrentMediaCaptures)]

## 알려진 프로필을 사용하여 HDR 동영상을 지원하는 프로필 찾기

HDR을 지원하는 프로필을 선택하는 작업은 다른 시나리오의 경우처럼 시작됩니다. **MediaCaptureInitializationSettings** 및 캡처 디바이스 ID를 보유하는 문자열을 만듭니다. HDR 동영상 지원 여부를 추적하는 부울 변수를 추가합니다.

[!code-cs[GetHdrProfileSetup](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetHdrProfileSetup)]

위에 정의된 **GetVideoProfileSupportedDeviceIdAsync** 도우미 메서드를 사용하여 카메라 프로필을 지원하는 캡처 디바이스의 디바이스 ID를 가져옵니다.

[!code-cs[FindDeviceHDR](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindDeviceHDR)]

정적 메서드 [**MediaCapture.FindKnownVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926710)는 지정된 디바이스에서 지원되는 카메라 프로필을 지정된 [**KnownVideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn948843) 값에 따라 분류해서 반환합니다. 이 시나리오에서 **VideoRecording** 값은 반환되는 카메라 프로필을 비디오 녹화를 지원하는 것으로 제한하기 위해 지정됩니다.

반환된 카메라 프로필 목록 전체를 루핑합니다. 각 카메라 프로필에 대해 [**IsHdrVideoSupported**](https://msdn.microsoft.com/library/windows/apps/dn926698) 속성이 true인지를 확인하기 위해 프로필의 각 [**VideoProfileMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926695) 전체를 루핑합니다. 적합한 미디어 설명을 찾으면 루프를 중단하고 프로필 및 설명 개체를 **MediaCaptureInitializationSettings** 개체에 할당합니다.

[!code-cs[FindHDRProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindHDRProfile)]

## 디바이스가 동시 사진 및 비디오 캡처를 지원하는지 확인

많은 디바이스는 사진 및 비디오의 동시 캡처를 지원합니다. 캡처 디바이스가 이러한 기능을 지원하는지 확인하려면 [**MediaCapture.FindAllVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926708)를 호출하여 디바이스에서 지원되는 모든 카메라 프로필을 가져옵니다. 링크 쿼리를 사용하여 [**SupportedPhotoMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926703) 및 [**SupportedRecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926705) 둘 다에 대해 하나 이상의 항목을 갖는 프로필을 찾습니다. 이러한 프로필이 동시 캡처를 지원하는 것입니다.

[!code-cs[GetPhotoAndVideoSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPhotoAndVideoSupport)]

이 쿼리를 구체화하여 동시 비디오 녹화 외의 기타 기능이나 특정 해상도를 지원하는 프로필을 찾을 수 있습니다. [**MediaCapture.FindKnownVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926710)를 사용하고 [**BalancedVideoAndPhoto**](https://msdn.microsoft.com/library/windows/apps/dn948843) 값을 지정하여 동시 캡처를 지원하는 프로필을 검색할 수도 있지만 모든 프로필을 쿼리하면 더 완전한 결과가 제공됩니다.

## 관련 항목

* [MediaCapture를 사용하여 사진 및 비디오 캡처](capture-photos-and-video-with-mediacapture.md)
 

 







<!--HONumber=Jun16_HO4-->


