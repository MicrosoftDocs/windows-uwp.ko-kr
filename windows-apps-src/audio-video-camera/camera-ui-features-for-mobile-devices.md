---
author: drewbatgit
ms.assetid: c43d4af3-9a1a-4eae-a137-1267c293c1b5
description: "이 문서에서는 모바일 장치에만 존재하는 특수한 카메라 UI 기능을 활용하는 방법을 보여 줍니다."
title: "모바일 장치용 카메라 UI 기능"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: ab74d720369bd95c10c8804836be1cc747d57931
ms.lasthandoff: 02/08/2017

---

#<a name="camera-ui-features-for-mobile-devices"></a>모바일 장치용 카메라 UI 기능

이 문서에서는 모바일 장치에만 존재하는 특수한 카메라 UI 기능을 활용하는 방법을 보여 줍니다. 

## <a name="add-the-mobile-extension-to-your-project"></a>프로젝트에 모바일 확장 추가 

이러한 기능을 사용하려면 유니버설 앱 플랫폼용 Microsoft 모바일 확장 SDK에 대한 참조를 프로젝트에 추가해야 합니다.

**하드웨어 카메라 단추 지원에 위해 모바일 확장 SDK에 대한 참조를 추가하려면**

1.  **솔루션 탐색기**에서 **참조**를 마우스 오른쪽 단추로 클릭한 다음 **참조 추가**를 선택합니다.

2.  **Windows 유니버설** 노드를 확장하고 **확장**을 선택합니다.

3.  **유니버설 앱 플랫폼용 Microsoft 모바일 확장 SDK** 확인란을 선택합니다.

## <a name="hide-the-status-bar"></a>상태 표시줄 숨기기

모바일 장치에는 사용자에게 장치에 대한 상태 정보를 제공하는 [**StatusBar**](https://msdn.microsoft.com/library/windows/apps/dn633864) 컨트롤이 있습니다. 이 컨트롤은 화면에서 공간을 차지하여 미디어 캡처 UI를 방해할 수 있습니다. [**HideAsync**](https://msdn.microsoft.com/library/windows/apps/dn610339)를 호출하여 상태 표시줄을 숨길 수 있지만 [**ApiInformation.IsTypePresent**](https://msdn.microsoft.com/library/windows/apps/dn949016) 메서드를 사용하여 API를 사용할 수 있는지 확인하는 조건부 블록 내에서 이 호출을 수행해야 합니다. 이 메서드는 상태 표시줄을 지원하는 모바일 장치에서만 true를 반환합니다. 앱이 시작되거나 카메라에서 미리 보기를 시작할 때 상태 표시줄을 숨겨야 합니다.

[!code-cs[HideStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHideStatusBar)]

앱이 종료되거나 사용자가 앱의 미디어 캡처 페이지를 닫으면 컨트롤을 다시 보이게 할 수 있습니다.

[!code-cs[ShowStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetShowStatusBar)]

## <a name="use-the-hardware-camera-button"></a>하드웨어 카메라 단추 사용

일부 모바일 장치에는 일부 사용자가 화면 컨트롤보다 선호하는 전용 하드웨어 카메라 단추가 있습니다. 하드웨어 카메라 단추를 누를 때 알림을 받으려면 [**HardwareButtons.CameraPressed**](https://msdn.microsoft.com/library/windows/apps/dn653805) 이벤트에 대한 처리기를 등록합니다. 이 API는 모바일 장치에서만 사용할 수 있으므로 **IsTypePresent**를 사용하여 액세스를 시도하기 전에 API가 현재 장치에서 지원되는지를 확인해야 합니다.

[!code-cs[PhoneUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPhoneUsing)]

[!code-cs[RegisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterCameraButtonHandler)]

**CameraPressed** 이벤트에 대한 처리기에서 사진 캡처를 시작할 수 있습니다.

[!code-cs[CameraPressed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCameraPressed)]

앱이 종료되거나 사용자가 앱의 미디어 캡처 페이지를 벗어나 이동하면 하드웨어 단추 처리기를 등록 취소합니다.

[!code-cs[UnregisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUnregisterCameraButtonHandler)]

> [!NOTE]
> 이 문서는 UWP(유니버설 Windows 플랫폼) 앱을 작성하는 Windows 10 개발자용입니다. Windows 8.x 또는 Windows Phone 8.x를 개발하는 경우 [보관된 문서](http://go.microsoft.com/fwlink/p/?linkid=619132)를 참조하세요.                                                                                   |

## <a name="related-topics"></a>관련 항목

* [카메라](camera.md)
* [MediaCapture를 사용하여 기본적인 사진, 비디오 및 오디오 캡처](basic-photo-video-and-audio-capture-with-MediaCapture.md)






