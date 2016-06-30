---
author: drewbatgit
ms.assetid: 708170E1-777A-4E4A-9F77-5AB28B88B107
description: "이 문서에서는 비디오 디바이스 컨트롤로 HDR 비디오 및 노출 우선 순위를 비롯한 향상된 비디오 캡처 시나리오를 구현하는 방법을 보여 줍니다."
title: "비디오 캡처를 위한 캡처 디바이스 컨트롤"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 65883f1be1a014b6c7e211e2e060ae97fbd9eb0d

---

# 비디오 캡처를 위한 캡처 디바이스 컨트롤

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


이 문서에서는 비디오 디바이스 컨트롤로 HDR 비디오 및 노출 우선 순위를 비롯한 향상된 비디오 캡처 시나리오를 구현하는 방법을 보여 줍니다.

이 문서에서 설명하는 비디오 디바이스 컨트롤은 모두 동일한 패턴을 사용하여 앱에 추가됩니다. 먼저, 앱이 실행 중인 현재 디바이스에서 컨트롤이 지원되는지를 확인합니다. 컨트롤이 지원되는 경우 컨트롤에 대해 원하는 모드를 설정합니다. 일반적으로 특정 컨트롤이 현재 디바이스에서 지원되지 않으면 사용자가 기능을 사용하도록 설정할 수 있는 UI 요소를 사용하지 않도록 설정하거나 숨겨야 합니다.

이 문서에서 다루는 모든 API 디바이스 컨트롤은 [**Windows.Media.Devices**](https://msdn.microsoft.com/library/windows/apps/br206902) 네임스페이스의 멤버입니다.

[!code-cs[VideoControllersUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoControllersUsing)]

**참고**  
이 문서는 기본 사진 및 비디오 캡처 구현 단계를 설명하는 [MediaCapture를 사용하여 사진 및 비디오 캡처](capture-photos-and-video-with-mediacapture.md)에 설명된 개념 및 코드를 토대로 작성되었습니다. 좀 더 수준 높은 캡처 시나리오를 진행하기 전에 해당 문서의 기본 미디어 캡처 패턴을 좀 더 잘 이해하는 것이 좋습니다. 이 문서의 코드는 앱에 적절히 초기화된 MediaCapture의 인스턴스가 이미 있다고 가정합니다.

## HDR 비디오

HDR(High Dynamic Range) 비디오 기능은 캡처 디바이스의 비디오 스트림에 HDR 처리를 적용합니다. [
            **HdrVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926682) 속성을 확인하여 HDR 비디오가 지원되는지 검토합니다.

HDR 비디오 컨트롤은 세 가지 모드인 켜짐, 꺼짐 및 자동을 지원합니다. 즉, 디바이스는 HDR 비디오 처리가 미디어 캡처를 향상시키는지를 동적으로 확인하고 향상시킬 경우 HDR 비디오를 사용하도록 설정합니다. 현재 디바이스에서 특정 모드가 지원되는지 확인하려면 [**HdrVideoControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926683) 컬렉션이 원하는 모드를 포함하는지 확인합니다.

[
            **HdrVideoControl.Mode**](https://msdn.microsoft.com/library/windows/apps/dn926681)를 원하는 모드로 설정하여 HDR 비디오 처리를 사용하거나 사용하지 않도록 설정합니다.

[!code-cs[SetHdrVideoMode](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetHdrVideoMode)]

## 노출 우선 순위

[
            **ExposurePriorityVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926644)을 사용할 경우 캡처 디바이스의 비디오 프레임을 평가하여 비디오가 낮은 조명 장면을 캡처하는지를 확인합니다. 따라서 이 컨트롤은 각 프레임의 노출 시간을 늘리고 캡처된 비디오의 시각적 품질을 개선하기 위해 캡처한 비디오의 프레임 속도를 낮춥니다.

[
            **ExposurePriorityVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926647) 속성을 확인하여 현재 디바이스에서 노출 우선 순위 컨트롤이 지원되는지를 확인합니다.

[
            **ExposurePriorityVideoControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn926646)를 원하는 모드로 설정하여 노출 우선순위 컨트롤을 사용하거나 사용하지 않도록 설정합니다.

[!code-cs[EnableExposurePriority](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetEnableExposurePriority)]

## 관련 항목

* [MediaCapture를 사용하여 사진 및 비디오 캡처](capture-photos-and-video-with-mediacapture.md)
 

 







<!--HONumber=Jun16_HO4-->


