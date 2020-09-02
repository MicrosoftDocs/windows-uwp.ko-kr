---
ms.assetid: ''
description: 이 문서에서는 원격 카메라에 연결 하 고 각 카메라에서 프레임을 검색 하기 위해 MediaFrameSourceGroup을 가져오는 방법을 보여 줍니다.
title: 원격 카메라에 연결
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c0e94e0ddaba027b38ecc76b1c97126204990f1a
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363986"
---
# <a name="connect-to-remote-cameras"></a>원격 카메라에 연결

이 문서에서는 하나 이상의 원격 카메라에 연결 하 고 각 카메라의 프레임을 읽을 수 있는 [**Mediaframesourcegroup**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) 개체를 가져오는 방법을 보여 줍니다. 미디어 원본에서 프레임을 읽는 방법에 대 한 자세한 내용은 [MediaFrameReader를 사용 하 여 미디어 프레임 처리](process-media-frames-with-mediaframereader.md)를 참조 하세요. 장치와의 페어링에 대 한 자세한 내용은 [페어링 장치](../devices-sensors/pair-devices.md)를 참조 하세요.

> [!NOTE] 
> 이 문서에서 설명 하는 기능은 Windows 10 버전 1903부터 사용할 수 있습니다.

## <a name="create-a-devicewatcher-class-to-watch-for-available-remote-cameras"></a>사용 가능한 원격 카메라를 감시 하는 DeviceWatcher 클래스 만들기

[**Devicewatcher**](/uwp/api/windows.devices.enumeration.devicewatcher) 클래스는 앱에서 사용할 수 있는 장치를 모니터링 하 고 장치를 추가 하거나 제거할 때 앱에 알립니다. Devicewatcher을 호출 하 여 **devicewatcher** 의 인스턴스를 가져옵니다 [**. createwatcher**](/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher#Windows_Devices_Enumeration_DeviceInformation_CreateWatcher_System_String_)는 모니터링 하려는 장치 유형을 식별 하는 AQS (고급 쿼리 구문) 문자열을 전달 합니다. 네트워크 카메라 장치를 지정 하는 AQS 문자열은 다음과 같습니다.

```
@"System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"" AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True"
```

> [!NOTE] 
> 도우미 메서드 [**MediafAQS Esourcegroup. GetDeviceSelector**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector) 는 로컬로 연결 되 고 원격 네트워크 카메라를 모니터링 하는 문자열을 반환 합니다. 네트워크 카메라만 모니터링 하려면 위에 표시 된 AQS 문자열을 사용 해야 합니다.


[**Start**](/uwp/api/windows.devices.enumeration.devicewatcher.start) 메서드를 호출 하 여 반환 된 **devicewatcher** 를 시작 하면 현재 사용할 수 있는 모든 네트워크 카메라에 대해 [**추가**](/uwp/api/windows.devices.enumeration.devicewatcher.added) 된 이벤트가 발생 합니다. [**중지**](/uwp/api/windows.devices.enumeration.devicewatcher.stop)를 호출 하 여 감시자를 중지 하기 전에는 새 네트워크 카메라 장치를 사용할 수 있게 되 면 **추가** 된 이벤트가 발생 하 고 카메라 장치를 사용할 수 없게 되 면 [**제거**](/uwp/api/windows.devices.enumeration.devicewatcher.removed) 된 이벤트가 발생 합니다.

**추가** 및 **제거** 된 이벤트 처리기에 전달 되는 이벤트 인수는 각각 [**Deviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 또는 [**deviceinformationupdate**](/uwp/api/windows.devices.enumeration.deviceinformationupdate) 개체입니다. 이러한 각 개체에는 이벤트가 발생 한 네트워크 카메라의 식별자 인 **Id** 속성이 있습니다. 이 ID를 [**MediafFromIdAsync**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) 메서드에 전달 하 여 카메라에서 프레임을 검색 하는 데 사용할 수 있는 [**mediaframesourcegroup**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) 개체를 가져옵니다.

## <a name="remote-camera-pairing-helper-class"></a>원격 카메라 페어링 도우미 클래스

다음 예제에서는 **Devicewatcher** 를 사용 하 여 **System.collections.objectmodel.observablecollection** 의 **Mediaframesourcegroup** 개체를 만들고 업데이트 하는 도우미 클래스를 보여 줍니다 .이 클래스는 카메라 목록에 대 한 데이터 바인딩을 지원 합니다. 일반적인 앱은 사용자 지정 모델 클래스에서 **Mediaframesourcegroup** 을 래핑합니다. 도우미 클래스는 앱의 [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) 에 대 한 참조를 유지 하 고 [**runasync**](/uwp/api/windows.ui.core.coredispatcher.runasync) 호출 내의 카메라 컬렉션을 업데이트 하 여 컬렉션에 바인딩된 ui가 ui 스레드에서 업데이트 되도록 합니다.

또한이 예제에서는 **추가** 및 **제거** 된 이벤트 외에도 [**devicewatcher. Updated**](/uwp/api/windows.devices.enumeration.devicewatcher.updated) 이벤트를 처리 합니다. **업데이트** 된 처리기에서 연결 된 원격 카메라 장치가에서 제거 된 다음 컬렉션에 다시 추가 됩니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/RemoteCameraPairingHelper.cs" id="SnippetRemoteCameraPairingHelper":::


## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [카메라 프레임 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [MediaFrameReader를 사용하여 미디어 프레임 처리](process-media-frames-with-mediaframereader.md)
 

 
