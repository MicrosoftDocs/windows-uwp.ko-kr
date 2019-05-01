---
ms.assetid: ''
description: 이 문서에서는 각 카메라에서 프레임을 검색할 MediaFrameSourceGroup 받고 원격 카메라에 연결 하는 방법을 보여 줍니다.
title: 원격 카메라에 연결
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bc719b8dad2adef0542edf284d257846052eac21
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63789583"
---
# <a name="connect-to-remote-cameras"></a>원격 카메라에 연결

이 문서에서는 하나 이상의 원격 카메라에 연결 하는 방법을 보여 줍니다.는 [ **MediaFrameSourceGroup** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) 각 카메라에서 프레임을 읽을 수 있는 개체입니다. 미디어 원본에서 프레임을 읽기에 대 한 자세한 내용은 참조 하세요. [MediaFrameReader 사용 하 여 미디어 프레임 처리](process-media-frames-with-mediaframereader.md)합니다. 장치 쌍에 대 한 자세한 내용은 참조 하세요. [장치 쌍](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices)합니다.

> [!NOTE] 
> 이 문서에 설명 된 기능은 Windows 10 버전 1903부터 사용할 수 있습니다.

## <a name="create-a-devicewatcher-class-to-watch-for-available-remote-cameras"></a>사용 가능한 원격 카메라에 대 한 보기를 DeviceWatcher 클래스 만들기

합니다 [ **DeviceWatcher** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher) 클래스는 앱에 사용할 수 있는 장치를 모니터링 하 고 장치 추가 또는 제거 된 경우 앱에 알립니다. 인스턴스를 가져올 **DeviceWatcher** 호출한 [ **DeviceInformation.CreateWatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher#Windows_Devices_Enumeration_DeviceInformation_CreateWatcher_System_String_)의 형식을 식별 하는 고급 쿼리 구문 (AQS) 문자열에 전달 장치를 모니터링 하려면입니다. 네트워크 웹캠 장치를 지정 하는 AQS 문자열 다음과 같습니다.

```
@"System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"" AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True"
```

> [!NOTE] 
> 도우미 메서드 [ **MediaFrameSourceGroup.GetDeviceSelector** ](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector) 로컬로 연결 및 원격 네트워크 카메라 모니터링할 AQS 문자열을 반환 합니다. 네트워크 카메라만을 모니터링 하려면 위의 AQS 문자열을 사용 해야 합니다.


시작 하는 경우 반환 된 **DeviceWatcher** 를 호출 하 여 합니다 [ **시작** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.start) 메서드를 발생 시킵니다 합니다 [ **Added** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added) 현재 사용할 수 있는 모든 네트워크 카메라에 대 한 이벤트입니다. 호출 하 여 감시자를 중지 하기 전까지 [ **중지**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.stop)의 **Added** 카메라 네트워크 장치가 새로 추가 사용할 수 있게 되 면 이벤트가 발생 하며 [ **제거 됨** ](https://docs.microsoft.com/en-us/uwp/api/windows.devices.enumeration.devicewatcher.removed) 카메라 장치를 사용할 수 없을 때 이벤트가 발생 합니다.

이벤트 인수에 전달 합니다 **Added** 및 **제거** 에 이벤트 처리기를 [ **DeviceInformation** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 또는 [  **DeviceInformationUpdate** ](https://docs.microsoft.com/en-us/uwp/api/windows.devices.enumeration.deviceinformationupdate) 개체를 각각. 이러한 각 개체에 **Id** 속성은 이벤트가 발생 하는 대 한 네트워크 카메라에 대 한 식별자입니다. 이 ID를 전달 합니다 [ **MediaFrameSourceGroup.FromIdAsync** ](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) 메서드를를 [ **MediaFrameSourceGroup** ](https://docs.microsoft.com/en-us/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) 를 사용할 수 있는 개체 카메라에서 프레임을 검색 합니다.

## <a name="remote-camera-pairing-helper-class"></a>원격 카메라 페어링 하는 도우미 클래스

다음 예제에서는 사용 하는 도우미 클래스를 **DeviceWatcher** 만들고 업데이트 하는 **ObservableCollection** 의 **MediaFrameSourceGroup** 지원 하기 위해 개체 카메라의 목록에 데이터 바인딩입니다. 일반적인 앱은 줄 바꿈 합니다 **MediaFrameSourceGroup** 사용자 지정 모델 클래스에서. 도우미 클래스를 앱에 대 한 참조를 유지 관리 하는 참고 [ **CoreDispatcher** ](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) 카메라에 대 한 호출 내에서 컬렉션을 업데이트 하 고 [ **RunAsync** ](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) UI 스레드에서 컬렉션에 바인딩된 UI가 업데이트를 확인 합니다.

또한이 예제에서는 처리를 [ **DeviceWatcher.Updated** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated) 외에 이벤트를 **Added** 및 **제거** 이벤트입니다. 에 **Updated** 처리기를 연결 된 원격 카메라 장치에서 제거 하 고 다시 컬렉션에 추가 됩니다.

[!code-cs[SnippetRemoteCameraPairingHelper](./code/Frames_Win10/Frames_Win10/RemoteCameraPairingHelper.cs#SnippetRemoteCameraPairingHelper)]


## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture 기본 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [카메라 프레임 샘플](https://go.microsoft.com/fwlink/?LinkId=823230)
* [MediaFrameReader 사용 하 여 미디어 처리 프레임](process-media-frames-with-mediaframereader.md)
 

 




