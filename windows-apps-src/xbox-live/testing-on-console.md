---
author: aablackm
title: Xbox One 콘솔에서 테스트
description: Xbox Live 콘솔에서 Xbox Live 서비스를 테스트 하는 방법에 알아봅니다.
ms.author: aablackm
ms.date: 08/15/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 게임, xbox, xbox live, xbox 하나
ms.localizationpriority: low
ms.openlocfilehash: 5500f6f396d6dae179e434283097c34274d9b829
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5444973"
---
# <a name="testing-on-the-xbox-one-console"></a>Xbox One 콘솔에서 테스트

Xbox One 콘솔 제품군 용 타이틀을 개발 하는 경우 제목 및 실제 콘솔에서 Xbox Live 기능을 테스트할 수 있도록 하려는 자연 스러운만 됩니다. 하드웨어에서 타이틀을 테스트 하는 몇 가지 옵션이 있습니다. 콘솔의 개발자 모드를 활성화 하 여 유니버설 Windows 플랫폼 (UWP) 제목이 나 앱을 테스트 하려면 모든 정품 Xbox One 콘솔을 사용할 수 있습니다. 이 옵션 모든 개발자에 게 액세스할 수 있으며 Xbox Live 크리에이터 스 프로그램 개발자를 위한 유일한 옵션입니다. ID@Xbox및 관리 파트너 주문 및 Xbox 개발 키트를 사용 하 여 수 있는 옵션이 있습니다.

## <a name="retail-console-testing-xbox-live-creators"></a>소매 콘솔 테스트: Xbox Live 크리에이터 스

정품 Xbox One 콘솔에서 개발자 모드 활성화를 사용 하면 Visual Studio 빌드를 사용 하 여 페어링 하 여 Xbox One 본체에 UWP 타이틀이 및 앱을 배포할 수 있습니다. Xbox Live 크리에이터 스 프로그램 개발자를 위한 옵션을 테스트 하는 콘솔입니다. 정품 Xbox One 콘솔에서 XDK 게임을 테스트할 수 없습니다.

* 개발 정품 콘솔에서 테스트를 허용 하려면 [개발자 모드 활성화 지침](../xbox-apps/devkit-activation.md) 을 따릅니다.  
* [Xbox One 명령 설정](../xbox-apps/development-environment-setup.md#setting-up-your-xbox-one)에 따라 타이틀에 Xbox One을 로드 합니다.  
* 콘솔이 정품 모드로 다시 전환 또는 개발 환경 정품 콘솔에서 제거 하려면 [개발자 모드 비활성화 지침](../xbox-apps/devkit-deactivation.md) 을 따릅니다.  
* 본체가 개발자 모드에 있는 동안 하면 액세스할 수 원격으로 PC를 통해 [Xbox 용 Windows 장치 포털](../debug-test-perf/device-portal-xbox.md)을 사용 하 여.  

## <a name="xbox-development-kit-testing-idxbox-and-managed-partners"></a>Xbox 개발 키트 테스트: ID@Xbox 및 파트너 관리

관리 파트너 및 ID@Xbox 개발자만 있는 사용자는 관리 되는 개발자 계정에 액세스할 수는 [Xbox 개발자 스토어](https://gamedevstore.partners.extranet.microsoft.com/)Xbox 개발자 키트를 구매 하는 옵션이 있습니다. Xbox 개발자 키트 XDK 로드할 수 게임 콘솔을 테스트를 위해을 허용 하 여 UWP 게임 개발 키트에서 테스트할 수도 있습니다. 개발자 키트 하드웨어 옵션 및 자세한 성능 테스트 및 콘솔 관리에 대 한 수 있는 테스트 기능이 함께 제공 됩니다.

키트는 Xbox 개발자와 여정을 시작 하는 [Xbox One 개발 문서를 사용 하 여 시작](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/atoc-getting-started) 에서 관리 되는 파트너 설명서 사이트를 읽습니다. 이 문서는 권한 있는 개발자가 액세스할 수만 합니다 ID@Xbox 프로그램 및 관리 파트너입니다.