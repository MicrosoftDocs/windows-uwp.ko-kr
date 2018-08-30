---
author: PatrickFarley
ms.assetid: 16976d00-1564-49fe-81ad-2568e25e9e41
title: 디버깅, 테스트 및 성능
description: Microsoft Visual Studio를 사용하여 앱을 디버깅 및 테스트할 수 있습니다. Windows 스토어 인증 프로세스에 맞게 앱을 준비하려면 Windows 앱 인증 키트를 사용합니다.
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 2d358d96a59128e3d272456a0fcf9f8535407d4f
ms.sourcegitcommit: 73ea31d42a9b352af38b5eb5d3c06504b50f6754
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/27/2017
ms.locfileid: "852796"
---
# <a name="debugging-testing-and-performance"></a>디버깅, 테스트 및 성능

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

Microsoft Visual Studio를 사용하여 앱을 디버깅 및 테스트할 수 있습니다. Windows 스토어 인증 프로세스에 맞게 앱을 준비하려면 Windows 앱 인증 키트를 사용합니다.

| 항목 | 설명 |
|-------|-------------|
| [UWP 앱 배포 및 디버그](deploying-and-debugging-uwp-apps.md) | 이 문서에서는 다양한 배포를 대상으로 지정하고 대상을 디버깅하는 단계를 안내합니다. |
| [PLM(프로세스 수명 관리) 테스트 및 디버깅 도구](testing-debugging-plm.md) | 프로세스 수명 관리에서 앱의 작동 방식을 디버깅 및 테스트하기 위한 도구와 기술입니다. |
| [Windows 10 Mobile용 Microsoft 에뮬레이터로 테스트](test-with-the-emulator.md) | Windows 10 Mobile용 Microsoft 에뮬레이터에 포함된 도구를 사용하여 실제 디바이스 조작을 시뮬레이트하고 앱의 기능을 테스트합니다. 이 에뮬레이터는 Windows 10을 실행하는 모바일 디바이스를 에뮬레이트하는 데스크톱 응용 프로그램입니다. 이 응용 프로그램은 실제 디바이스 없이 Windows 앱을 디버그 및 테스트할 수 있는 가상화된 환경을 제공합니다. 또한 응용 프로그램 프로토타입을 위한 격리된 환경을 제공합니다. |
| [Visual Studio를 사용하여 Surface Hub 앱 테스트](test-surface-hub-apps-using-visual-studio.md) | Visual Studio 시뮬레이터는 Microsoft Surface Hub용으로 빌드한 앱을 포함하여 UWP(유니버설 Windows 플랫폼) 앱을 디자인, 개발, 디버그 및 테스트할 수 있는 환경을 제공합니다. 시뮬레이터는 Surface Hub와 동일한 사용자 인터페이스를 사용하지 않지만 Surface Hub의 화면 크기와 해상도에서 앱의 모양과 동작을 테스트하는 데 유용합니다. |
| [베타 테스트](beta-testing.md) | **베타 테스트**를 수행하면 앱 개발 팀 외부에 있는 사용자의 디바이스에서 아직 릴리스되지 않은 앱을 시험해 보고 사용자의 피드백에 따라 앱을 향상시킬 수 있습니다. |
| [Windows Device Portal](device-portal.md) | Windows Device Portal을 사용하면 네트워크 또는 USB 연결을 통해 원격으로 디바이스를 구성하고 관리할 수 있습니다. |
| [Windows 앱 인증 키트](windows-app-certification-kit.md) | 앱이 Windows 스토어에 게시될 가능성 또는 Windows 인증을 받을 가능성을 최대한 높이려면 인증을 받기 위해 앱을 제출하기 전에 로컬로 앱의 유효성을 검사하고 테스트하세요. 이 항목에서는 Windows 앱 인증 키트를 설치하고 실행하는 방법을 보여 줍니다. |
| [성능](performance-and-xaml-ui.md) | 사용자는 앱이 응답성을 유지하고 자연스러운 느낌을 주며 배터리를 소모하지 않기를 기대합니다. 기술적으로 성능은 기능적 요구 사항이 아니지만 성능을 기능으로 간주하는 것이 사용자의 기대를 충족하는 데 도움이 됩니다. 목표 지정 및 측정이 중요한 요소입니다. 성능에 중요한 시나리오가 무엇인지 결정하고 우수한 성능이란 무엇인지 정의하세요. 그런 다음 초기에 측정하고 프로젝트의 수명 주기 동안 목표를 달성할 수 있는지 확인합니다. |
| [버전 적응 앱](version-adaptive-apps.md) | 최대한 많은 사용자에게 노출되도록 하면서 최신 API와 기능을 활용하십시오. 런타임 API 확인을 사용하여 런타임에 코드와 XAML이 앱이 실행되는 Windows 10 버전에 사용할 수 있는 기능에 맞게 조정합니다. |