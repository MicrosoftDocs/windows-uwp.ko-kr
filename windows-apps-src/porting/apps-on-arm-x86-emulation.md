---
title: ARM에서 x86 및 ARM32 에뮬레이션이 작동하는 방식
author: msatranjr
description: ARM 기반 x86 앱의 에뮬레이션 개요.
ms.author: misatran
ms.date: 02/15/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10 s, 항상 연결, ARM 기반 x86 에뮬레이션
ms.localizationpriority: medium
ms.openlocfilehash: d8304bb49b1eeeea8b0aa17f7548197a360cbcf8
ms.sourcegitcommit: 11edca90aaf7856c762e68903483079d30ad3877
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/19/2018
ms.locfileid: "1595171"
---
# <a name="how-x86-emulation-works-on-arm"></a>ARM에서 x86 에뮬레이션이 작동하는 방식
x86 앱에 대한 에뮬레이션으로 ARM에서 사용 가능한 Win32 앱을 풍부한 에코시스템으로 만들 수 있습니다. 이렇게 하면 사용자에게 앱을 전혀 수정하지 않고 기존 x86 win32 앱을 실행하는 놀라운 환경을 제공할 수 있습니다. 앱은 특정 API([IsWoW64Process2](https://msdn.microsoft.com/en-us/library/windows/desktop/mt804318.aspx))를 호출하지 않는 한 ARM 기반 Windows PC에서 실행되고 있는지 알지 못합니다.

Windows 10의 [WOW64](https://msdn.microsoft.com/en-us/library/windows/desktop/aa384249(v=vs.85).aspx) 계층은 x86 코드를 ARM64 버전의 Windows 10에서 실행할 수 있도록 합니다. 성능 향상을 위해 x86 에뮬레이션은 x86 지침의 블록을 ARM64 지침에 컴파일하여 작동합니다. 서비스는 지침 번역의 오버헤드를 줄이고 코드를 다시 실행할 때 최적화를 위해 이러한 번역된 코드 블록을 캐시로 저장합니다. 처음 시작 시 다른 앱이 활용할 수 있도록 캐시는 각 모듈에 대해 생성됩니다. 

이러한 기술에 대한 자세한 내용은 [ARM 기반 Windows 10](https://channel9.msdn.com/Events/Build/2017/P4171) Channel9 비디오를 참조하세요. 