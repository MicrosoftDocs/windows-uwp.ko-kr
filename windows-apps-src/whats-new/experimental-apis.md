---
title: 실험적 API
description: 개발자가 사용해 볼 수 있도록 Windows 참가자 SDK를 사용하여 외부에서 실험적 API를 플라이트하는 방법을 알아봅니다.
ms.date: 11/13/2017
ms.topic: article
keywords: windows 10, uwp, 실험적, api
ms.localizationpriority: medium
ms.openlocfilehash: 5a4813e7b4ae1e3dd16017066758aa8a35d0570a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170807"
---
# <a name="experimental-apis"></a>실험적 API

실험적 API는 설계 초기 단계에 있으며 소유자가 피드백을 통합하고 추가 시나리오에 대한 지원을 추가함에 따라 변경될 수 있습니다.

이러한 API는 공식 플랫폼의 일부가 되기 전에 개발자가 테스트하고 피드백을 제공할 수 있도록 [Windows 참가자 SDK](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK)를 사용하여 외부적으로 플라이트됩니다. API가 플라이트되는 동안 안정성이나 커밋을 보장할 수 없습니다.

## <a name="consuming-experimental-apis"></a>실험적 API 사용
Intellisense를 사용하면 API가 실험적인지 여부를 알 수 있습니다. 또한 "...평가 목적으로만 사용되며 향후 업데이트에서 변경되거나 제거될 수 있는 API"와 같은 실험적 API를 사용하는 경우 컴파일러 경고를 받습니다.

이러한 경고는 프로덕션 프로젝트에서 실험적 API에 종속 항목을 만들지 못하게 합니다. 실험적 프로젝트의 경우 이러한 경고를 무시하거나 비활성화할 수 있습니다.

기본적으로 이러한 API는 런타임 시 비활성화되므로 이를 호출하면 런타임 예외가 발생합니다. 이는 부주의하게 종속 항목을 만들거나 실험적 API를 사용하는 앱을 배포하지 못하게 하기 위한 또 다른 보호 방법입니다.

이러한 API를 실험적으로 사용하려면 대상 디바이스에서 [Windows 장치 포털(WDP)](../debug-test-perf/device-portal.md) 기능 플러그인을 사용하여 호출하려는 API에 해당하는 기능을 활성화합니다.

특정한 실험적 API에 대한 설명서는 API를 소유한 팀의 재량에 달려 있습니다.

## <a name="providing-feedback"></a>사용자 의견 제공

실험적 API를 시도했으며 피드백을 제공하려는 경우 **Windows 피드백 허브**의 [개발자 플랫폼](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub) 범주를 사용하세요.