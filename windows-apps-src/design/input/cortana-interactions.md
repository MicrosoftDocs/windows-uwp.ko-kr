---
description: Windows 응용 프로그램에서 단일 작업을 시작 하 고 실행 하는 음성 명령으로 **Cortana** 의 기본 기능을 확장 합니다.
title: Windows 앱에서 Cortana 상호 작용
ms.assetid: 4C11A7CF-DA26-4CA1-A9B9-FE52670101F5
label: Cortana
template: detail.hbs
keywords: Cortana, Cortana canvas, Cortana 디자인, 사용자 인터페이스, 음성 명령, VCD
ms.date: 01/27/2021
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fca4da482585ddd4b7f9d54008c1905e372ca030
ms.sourcegitcommit: d51c3dd64d58c7fa9513ba20e736905f12df2a9a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98988735"
---
# <a name="cortana-interactions-in-windows-apps"></a>Windows 앱에서 Cortana 상호 작용

>[!WARNING]
> 이 기능은 Windows 10 5 월 2020 업데이트 (버전 2004, 코드명 "20H1")에서 더 이상 지원 되지 않습니다.

Windows 응용 프로그램에서 단일 작업을 시작 하 고 실행 하는 음성 명령으로 **Cortana** 의 기본 기능을 확장 합니다.

대상 앱은 작업의 복잡성에 따라 포그라운드 (앱에서 포커스를 가져오고 **cortana** 를 닫을 수 있음) 또는 백그라운드에서 활성화 (**cortana** 는 포커스를 유지 하지만 앱의 결과를 제공 함)에서 시작할 수 있습니다. 일반적으로 추가 컨텍스트 또는 사용자 입력이 필요한 음성 명령은 전경 앱에서 가장 효과적으로 처리 되는 반면 기본 명령은 **Cortana** 에서 백그라운드 앱을 통해 처리할 수 있습니다. 

앱의 기본 기능을 통합 하 고 사용자가 앱을 직접 열지 않고도 대부분의 작업을 수행할 수 있는 중앙 진입점을 제공 하면 **Cortana** 가 앱과 사용자 간에 서 면으로 전환 됩니다. 앱 기능에이 바로 가기를 제공 하 고 앱을 전환할 필요성을 줄여 사용자에 게 상당한 시간과 노력을 절감할 수 있습니다.

> [!NOTE]
> 음성 명령은 설치 된 앱에서 **Cortana** 를 통해 전달 되는 특정 의도가 포함 된 단일 Utterance (오디오 명령 정의) 파일에 정의 되어 있습니다.
>
> VCD 파일은 각각 고유한 의도를 가진 하나 이상의 음성 명령을 정의 합니다.
>
> 음성 명령 정의는 복잡성이 다를 수 있습니다. 단일 제한 된 utterance의 모든 항목을 동일한 의도를 나타내는 보다 유연 하 고 자연 언어 길이 발언 컬렉션으로 지원할 수 있습니다.

## <a name="other-speech-and-conversation-components"></a>기타 음성 및 대화 구성 요소

### <a name="speech-voice-and-conversation-in-windows-10"></a>Windows 10의 음성, 음성 및 대화

Windows 응용 프로그램을 빌드하는 개발자에 게 다양 한 Windows 개발 프레임 워크에서 음성 인식, 음성 합성 및 대화 지원을 제공 하는 방법에 대 한 자세한 내용은 [windows 10의 음성, 음성 및 대화](/windows/apps/speech) 를 참조 하세요.

### <a name="cortana-skills-kit"></a>Cortana 스킬 키트

사용자가 Cortana를 통해 **서비스** 와 상호 작용할 수 있도록 하는 기술을 추가 하 여 cortana를 확장 하려면 [cortana Skills 키트](/cortana/skills/) 를 참조 하세요. [사용 중단 **알림:** Microsoft 365에 맞게 cortana를 포함 하 여 최신 생산성 환경을 전환 하는 목표의 일환으로, 소비자 (개발자 플랫폼) 및이 플랫폼에서 빌드된 모든 기술을 사용 하지 않도록 설정 합니다.]

## <a name="related-articles"></a>관련 문서

* [VCD 요소 및 특성 v 1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

### <a name="designers"></a>디자이너

* [Cortana 디자인 지침](cortana-design-guidelines.md)

### <a name="samples"></a>샘플

* [Cortana 음성 명령 샘플](https://go.microsoft.com/fwlink/p/?LinkID=619899)
