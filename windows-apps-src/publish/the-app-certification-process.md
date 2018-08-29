---
author: jnHs
Description: When you finish creating your app's submission and click Submit to the Store, the submission enters the certification step.
title: 앱 인증 프로세스
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
ms.author: wdg-dev-content
ms.date: 07/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: 창 10, uwp, 게시, 전처리, 인증, 해제, 보류, 전송, 게시, 상태, 시간
ms.localizationpriority: medium
ms.openlocfilehash: 8372f316786d83d72dff8ef7a0a8fd53e5390743
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/29/2018
ms.locfileid: "2918864"
---
# <a name="the-app-certification-process"></a>앱 인증 프로세스

앱 제출 생성을 완료하고 **Store에 제출**을 클릭하면 제출이 인증 단계로 들어갑니다. 이 프로세스는 대개 몇 시간 내에 완료되지만 경우에 따라 영업일 기준으로 3일까지 걸릴 수 있습니다. 등록 인증에 통과 한 후 고객이 새 제출 하거나 패키지를 변경 내용으로 업데이트 된 미션에 대 한 응용 프로그램의 목록을 볼 수에 대 한 최대 24 시간까지 걸릴 수 있습니다. 업데이트 내용을 나열 하는 정보 저장소를 변경 하면 게시 과정 1 시간 이내에 완료 됩니다.  제출물을 게시 하 고 대시보드에서 응용 프로그램 상태에 **저장**됩니다 때 알려 줍니다.

## <a name="preprocessing"></a>전처리

앱 패키지를 업로드하고 인증을 위해 앱을 제출한 후 패키지는 테스트를 위해 대기합니다. 전처리하는 동안 오류가 검색되면 메시지를 표시합니다. 가능한 오류에 대한 자세한 내용은 [제출 오류 해결](resolve-submission-errors.md)을 참조하세요.

## <a name="certification"></a>인증

이 단계에서 여러 가지 테스트가 수행됩니다.

-   **보안 테스트:** 첫 번째 테스트에서는 앱 패키지에 바이러스 및 맬웨어가 있는지 확인합니다. 앱이 이 테스트에 실패하면 최신 바이러스 백신 소프트웨어를 실행하여 개발 시스템을 확인한 다음 클린 시스템에서 앱 패키지를 다시 빌드해야 합니다.
-   **기술 준수 테스트:** 기술 준수는 Windows 앱 인증 키트로 테스트합니다. 스토어에 앱을 제출하기 전에 항상 [Windows 앱 인증 키트로 앱을 테스트](../debug-test-perf/windows-app-certification-kit.md)해야 합니다.
-   **콘텐츠 준수:** 이 작업에 걸리는 시간은 앱의 복잡성, 포함된 시각적 콘텐츠 양 및 최근에 제출된 앱 수에 따라 달라집니다. [인증에 대한 참고 사항](notes-for-certification.md) 페이지에 테스터가 알고 있어야 하는 정보를 제공해야 합니다.

인증 프로세스가 완료되면 앱의 인증 통과 여부를 알리는 인증 보고서를 받게 됩니다. 앱이 인증을 통과하지 못한 경우 보고서에 실패한 테스트나 충족되지 않은 [정책](https://docs.microsoft.com/legal/windows/agreements/store-policies)이 표시됩니다. 문제를 해결한 후 앱에 대한 새 제출을 만들어서 인증 프로세스를 다시 시작할 수 있습니다.

## <a name="release"></a>릴리스

응용 프로그램 인증을 통과 하는 경우 **게시** 프로세스를 진행할 준비가 됩니다.

- 등록 (기본 옵션) 최대한 빨리 게시 될 말씀 게시 프로세스가 즉시 시작 됩니다.
- 이것은 처음으로 응용 프로그램에 게시 된 및 **출시 날짜** 는 [일정](configure-precise-release-scheduling.md#release) 섹션에서 지정한, 앱 **릴리스 날짜** 선택에 따라 사용할 수 있습니다.
- 사용한 경우 [게시 옵션을 유지](manage-submission-options.md#publishing-hold-options) 하 게 해야 해제 될 특정 날짜를 지정 하려면, **변경을 릴리스 날짜**를 선택 하지 않은 경우 게시 프로세스를 시작 하려면 해당 날짜가 될 때까지 기다릴 게 우리.
- [게시 옵션을 누르고](manage-submission-options.md#publishing-hold-options) 직접 제출 게시 되도록 지정 하려면를 사용한 선택 **지금 게시** (또는 **변경 릴리스 날짜** 를 선택 하는 특정 날짜를 선택) 될 때까지 게시 프로세스 시작 없습니다 우리가.


## <a name="publishing"></a>게시

앱 패키지에 디지털 서명하여 앱이 릴리스된 후 변경되지 않도록 보호합니다. 이 단계가 시작된 후에는 제출을 취소하거나 릴리스 날짜를 변경할 수 없습니다.

새 응용 프로그램 및 응용 프로그램의 패키지의 변경 내용을 포함 하는 업데이트에 대 한 게시 프로세스는 24 시간 이내 완료 됩니다. 만 저장소 세부 정보를 나열 하는 등의 옵션 변경 하지 않고 변경 된 응용 프로그램 패키지는 업데이트를 게시 프로세스 1 시간 걸립니다.

응용 프로그램 게시 단계에서 이지만, 알 새 패키지 및 저장소 세부 정보를 나열 합니다. 각 프로그램 지원 되는 운영 체제 버전에는 고객에 게 사용할 수 있는 경우 응용 프로그램의 제출에 대 한 상태 열에서 **세부 정보 표시** 링크가 있습니다. 아직 완료되지 않은 단계들은 **보류 중**으로 표시됩니다. 응용 프로그램 게시 하는 작업이 완료 될 때까지 단계에 남아 있게 됩니다, 그리고은 모든 잠재 고객에 게 응용 프로그램에 사용할 수 있는 새 패키지를 의미 하는 또는 자세한 정보를 나열 합니다.

## <a name="in-the-store"></a>Store에 있음 

위의 단계를 성공적으로 수행하면 제출의 상태가 **게시 중**에서 **Store에 있음**으로 변경됩니다. 그런 다음 고객이 다운로드할 수 있도록 제출이 Microsoft Store에 제공됩니다(다른 [검색 기능](choose-visibility-options.md#discoverability) 옵션을 선택하지 않은 경우). 

> [!NOTE]
> 잠재적 문제를 식별하고 모든 [Microsoft Store 정책](https://docs.microsoft.com/legal/windows/agreements/store-policies)을 준수하는지 확인할 수 있으려면 앱이 게시된 후에 스팟 체킹을 수행합니다. 문제가 발견되면 해당하는 경우 또는 앱이 스토어에서 제거된 경우 문제와 문제 해결 방법에 대한 알림을 받게 됩니다.

 

 

 




