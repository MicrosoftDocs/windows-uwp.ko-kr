---
title: Windows 10 용 Powertoy 비디오 회의 음소거 유틸리티
description: 사용자가 컴퓨터에 포커스를 둔 응용 프로그램에 관계 없이 한 번의 키 입력으로 오디오를 신속 하 게 음소거 하 고 카메라 (비디오)를 끌 수 있는 유틸리티입니다.
ms.date: 12/07/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e9a7ec901ffc31e565adb94a1a043fd149064c12
ms.sourcegitcommit: 5dac88ad541b71ebe85b78951e6b357a3db176cc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97612177"
---
# <a name="video-conference-mute-preview"></a>비디오 회의 음소거 (미리 보기)

> [!IMPORTANT]
> 이 기능은 미리 보기 기능이 며 Powertoy의 시험판 버전에만 포함 되어 있습니다. 이 시험판을 실행 하려면 Windows 10 버전 1903 (빌드 18362) 이상이 필요 합니다.

컴퓨터에 포커스를 둔 응용 프로그램에 관계 없이 마이크 (오디오)를 신속 하 게 음소거 하 고 카메라 (비디오)를 한 번의 키 입력으로 설정 합니다.

## <a name="usage"></a>사용

비디오 회의 음소거를 사용 하는 기본 설정은 다음과 같습니다.

- <kbd>⊞ Win</kbd> + <kbd>N</kbd> 오디오와 비디오를 동시에 전환 하려면
- <kbd>⊞ Win</kbd> + <kbd>Shift</kbd> + 마이크 <kbd>를 전환 하기 위한</kbd> 입니다.
- <kbd>⊞ Win</kbd> + <kbd>Shift</kbd> + 비디오를 전환 하려면 <kbd>O</kbd>

![오디오 및 비디오 음소거 알림 스크린샷](../images/pt-video-audio-mute-notification.png)

마이크 및/또는 카메라를 설정/해제 하는 경우 바로 가기 키를 사용 하 여 마이크와 카메라를 켜기, 끄기 또는 사용 하지 않음으로 설정할지 여부를 알려 주는 작은 도구 모음 창이 표시 됩니다. Powertoy 설정의 비디오 컨퍼런스 음소거 탭에서이 도구 모음의 위치를 설정할 수 있습니다.

> [!NOTE]
> 이 유틸리티를 사용 하려면 Powertoy 설정에서 비디오 회의 음소거 기능을 사용 하도록 설정 하 여 [시험판/실험적 버전의 powertoy](https://github.com/microsoft/PowerToys/releases/) 가 설치 되 고 실행 중 이어야 합니다.

## <a name="settings"></a>설정

Powertoy 설정의 비디오 회의 음소거 탭에는 다음과 같은 옵션이 제공 됩니다.

- **바로 가기:** 마이크, 카메라 또는 둘 다를 음소거 하는 데 사용 되는 바로 가기 키를 변경 합니다.
- **선택한 마이크:** 이 유틸리티를 대상으로 하는 컴퓨터의 마이크를 선택 합니다.
- **선택한 카메라:** 이 유틸리티를 대상으로 하는 컴퓨터의 카메라를 선택 합니다.
- **카메라 오버레이 이미지:** 카메라가 꺼진 경우 자리 표시자로 사용할 이미지를 선택 합니다. 이 유틸리티를 사용 하 여 카메라를 끄면 기본적으로 검은색 화면이 표시 됩니다.
- **도구 모음:** 설정/해제 될 때 *카메라의 마이크가* 표시 되는 위치 (기본적으로 오른쪽 위)를 결정 합니다.
- **도구 모음 표시 위치:** 도구 모음을 주 모니터에만 표시할지 (기본값) 또는 모든 모니터에 표시할지 여부를 선택 합니다.
- **카메라와 마이크가 모두 unmuted 경우 도구 모음 숨기기**:이 옵션을 설정/해제 하는 데 사용할 수 있는 확인란이 있습니다.

![Powertoy 설정의 비디오 회의 음소거 옵션](../images/pt-video-conference-mute-settings.png)

## <a name="how-does-this-work-under-the-hood"></a>내부적으로이 작업을 수행 하는 방법

응용 프로그램은 다양 한 방식으로 오디오 및 비디오와 상호 작용 합니다.

카메라의 작동이 중지 되는 경우이를 사용 하는 응용 프로그램은 API가 전체 다시 설정 될 때까지 복구 하지 않는 경향이 있습니다. 응용 프로그램에서 카메라를 사용 하는 동안 글로벌 개인 정보 카메라를 설정 및 해제 하려면 일반적으로 작동이 중단 되 고 복구 되지 않습니다.

따라서 Powertoy는이를 어떻게 처리 하 여 스트리밍을 유지할 수 있나요?

- **오디오:** Powertoy는 Windows의 글로벌 마이크 음소거 API를 사용 합니다.  이를 설정 및 해제할 때 앱을 복구 해야 합니다.
- **비디오:** Powertoy에는 카메라에 대 한 가상 드라이버가 있습니다. 비디오는 드라이버를 통해 라우팅되고 응용 프로그램으로 돌아갑니다. 비디오 회의 음소거 바로 가기 키를 선택 하면 스트리밍이 비디오를 중지 하지만 응용 프로그램에서 비디오를 수신 한다고 생각 하는 경우 비디오는 단순히 설정에서 저장 한 이미지 자리 표시자 나 검정색으로 대체 됩니다.

### <a name="debug-the-camera-driver"></a>카메라 드라이버 디버그

카메라 드라이버를 디버그 하려면 컴퓨터에서 다음 폴더를 찾습니다.

```markdown
C:\Windows\ServiceProfiles\LocalService\AppData\Local\Temp\PowerToysVideoConference.log
```

동일한 디렉터리에 빈을 만들어 `PowerToysVideoConferenceVerbose.flag` 드라이버에서 자세한 로깅 모드를 사용 하도록 설정할 수도 있습니다.

## <a name="known-issues"></a>알려진 문제

비디오 회의 음소거 유틸리티에서 현재 열려 있는 모든 알려진 문제를 보려면 [GitHub의 powertoy 추적 문제 #6246](https://github.com/microsoft/PowerToys/issues/6246)를 참조 하세요. Powertoy development 팀 및 참가자 커뮤니티는 이러한 문제를 해결 하기 위해 적극적으로 노력 하 고 있으며, 필수 문제가 해결 될 때까지 유틸리티를 시험판으로 유지할 계획입니다.

![5 명의 참가자 및 장치 설정이 표시 된 Powertoy video 컨퍼런스 스크린샷](../images/pt-video-conference.png)
