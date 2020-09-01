---
ms.assetid: c43d4af3-9a1a-4eae-a137-1267c293c1b5
description: 이 문서에서는 모바일 장치에만 제공 되는 특수 카메라 UI 기능을 활용 하는 방법을 보여 줍니다.
title: 모바일 디바이스용 카메라 UI 기능
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: eaee05ebc1d65a4d2f920daa43c7a012a02f4ef0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161037"
---
# <a name="camera-ui-features-for-mobile-devices"></a>모바일 디바이스용 카메라 UI 기능

이 문서에서는 모바일 장치에만 제공 되는 특수 카메라 UI 기능을 활용 하는 방법을 보여 줍니다. 

## <a name="add-the-mobile-extension-to-your-project"></a>프로젝트에 모바일 확장 추가 

이러한 기능을 사용 하려면 범용 앱 플랫폼용 Microsoft 모바일 확장 SDK에 대 한 참조를 프로젝트에 추가 해야 합니다.

**하드웨어 카메라에 대 한 모바일 확장 SDK 단추 지원에 대 한 참조를 추가 하려면**

1.  **솔루션 탐색기**에서 **참조**를 마우스 오른쪽 단추로 클릭하고, **참조 추가**를 선택합니다.

2.  **Windows 유니버설** 노드를 확장 하 고 **확장**을 선택 합니다.

3.  **유니버설 앱 플랫폼용 Microsoft Mobile EXTENSION SDK** 확인란을 선택 합니다.

## <a name="hide-the-status-bar"></a>상태 표시줄 숨기기

모바일 장치에는 장치에 대 한 상태 정보를 사용자에 게 제공 하는 [**StatusBar**](/uwp/api/Windows.UI.ViewManagement.StatusBar) 컨트롤이 있습니다. 이 컨트롤은 미디어 캡처 UI를 방해할 수 있는 화면 공간을 차지 합니다. [**HideAsync**](/uwp/api/windows.ui.viewmanagement.statusbar.hideasync)를 호출 하 여 상태 표시줄을 숨길 수 있지만 [**IsTypePresent**](/uwp/api/windows.foundation.metadata.apiinformation.istypepresent) 메서드를 사용 하 여 API를 사용할 수 있는지 여부를 확인 하는 조건부 블록 내에서이 호출을 수행 해야 합니다. 이 메서드는 상태 표시줄을 지 원하는 모바일 장치 에서만 true를 반환 합니다. 앱이 시작 되거나 카메라에서 미리 보기를 시작할 때 상태 표시줄을 숨겨야 합니다.

[!code-cs[HideStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHideStatusBar)]

앱이 종료 되거나 사용자가 앱의 미디어 캡처 페이지에서 벗어날 때 컨트롤이 다시 표시 되도록 설정할 수 있습니다.

[!code-cs[ShowStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetShowStatusBar)]

## <a name="use-the-hardware-camera-button"></a>하드웨어 카메라 단추 사용

일부 모바일 장치에는 일부 사용자가 화면 제어에서 선호 하는 전용 하드웨어 카메라 단추가 있습니다. 하드웨어 카메라 단추를 누를 때 알리도록 하려면 [**HardwareButtons. CameraPressed**](/uwp/api/windows.phone.ui.input.hardwarebuttons.camerapressed) 이벤트에 대 한 처리기를 등록 합니다. 이 API는 모바일 장치 에서만 사용할 수 있기 때문에 **IsTypePresent** 를 다시 사용 하 여 액세스를 시도 하기 전에 api가 현재 장치에서 지원 되는지 확인 해야 합니다.

[!code-cs[PhoneUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPhoneUsing)]

[!code-cs[RegisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterCameraButtonHandler)]

**CameraPressed** 이벤트에 대 한 처리기에서 사진 캡처를 시작할 수 있습니다.

[!code-cs[CameraPressed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCameraPressed)]

앱을 종료 하거나 사용자가 앱의 미디어 캡처 페이지에서 다른 위치로 이동 하는 경우 하드웨어 단추 처리기를 등록 취소 합니다.

[!code-cs[UnregisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUnregisterCameraButtonHandler)]

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)