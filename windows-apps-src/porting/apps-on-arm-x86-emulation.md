---
title: ARM에서 x86 및 ARM32 에뮬레이션 작동 방법
description: X86 앱에 대 한 에뮬레이션을 통해 ARM 장치에서 기존 Win32 앱의 다양 한 에코 시스템을 사용할 수 있도록 하는 방법을 알아봅니다.
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, 항상 연결 됨, ARM의 x86 에뮬레이션
ms.localizationpriority: medium
ms.openlocfilehash: 61d994a4a022da671b4141ded80c6235ae38bae6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162327"
---
# <a name="how-x86-emulation-works-on-arm"></a>ARM에서 x86 에뮬레이션이 작동하는 방식
X86 앱에 대 한 에뮬레이션은 ARM에서 Win32 앱의 다양 한 에코 시스템을 사용할 수 있도록 합니다. 이를 통해 사용자는 앱을 수정 하지 않고 기존 x86 win32 앱을 실행할 수 있습니다. 앱은 특정 Api ([IsWoW64Process2](/windows/desktop/api/wow64apiset/nf-wow64apiset-iswow64process2))를 호출 하지 않는 한 ARM PC의 Windows에서 실행 되는 것을 인식 하지 못합니다.

Windows 10의 [WOW64](/windows/desktop/WinProg64/running-32-bit-applications) 계층을 사용 하면 x86 코드를 ARM64 버전의 windows 10에서 실행할 수 있습니다. x86 에뮬레이션은 성능을 향상 시키기 위해 최적화를 사용 하 여 x86 지침의 블록을 ARM64 명령으로 컴파일하는 방식으로 작동 합니다. 서비스는 이러한 변환 된 코드 블록을 캐시 하 여 명령 변환의 오버 헤드를 줄이고 코드를 다시 실행할 때 최적화를 허용 합니다. 캐시는 각 모듈에 대해 생성 되므로 다른 앱이 처음 시작할 때 해당 캐시를 사용할 수 있습니다. 

이러한 기술에 대 한 자세한 내용은 ARM Channel9 [의 Windows 10](https://channel9.msdn.com/Events/Build/2017/P4171) 비디오를 참조 하세요.