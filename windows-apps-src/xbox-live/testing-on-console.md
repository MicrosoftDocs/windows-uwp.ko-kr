---
title: Xbox 1 본체에서 테스트
description: Xbox Live Console에서 Xbox Live 서비스를 테스트 하는 방법 알아보기
ms.date: 08/15/2018
ms.topic: article
keywords: windows 10, uwp, 게임, xbox, xbox live, xbox 하나
ms.localizationpriority: low
ms.openlocfilehash: aa14071f5ddc9af778fc59cc20891bf83310088b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628018"
---
# <a name="testing-on-the-xbox-one-console"></a>Xbox One 콘솔에서 테스트

Xbox One 제품군 콘솔의 제목을 개발 하는 경우 보내 주신 제목과 실제 콘솔에서 Xbox Live 기능을 테스트 하려는 자연 스러운만 됩니다. 가지 하드웨어의 제목과 테스트에 대 한 몇 가지 옵션이 있습니다. 콘솔의 개발자 모드를 활성화 하 여 유니버설 Windows 플랫폼 (UWP) title 또는 앱을 테스트 하려면 모든 소매 Xbox 1 본체를 사용할 수 있습니다. 이 옵션은 모든 개발자가 액세스할으로 Xbox Live 크리에이터 스 프로그램 개발자를 위한 유일한 옵션입니다. ID@Xbox 및 관리 되는 파트너는 Xbox 개발 키트를 사용 하 여 정렬 옵션이 있습니다.

## <a name="retail-console-testing-xbox-live-creators"></a>소매 콘솔 테스트: Xbox Live 크리에이터 스

Xbox One 소매 콘솔에서 개발자 모드가 활성화를 사용 하면 Visual Studio 빌드를 사용 하 여 연결 하 여 Xbox 1 본체로 UWP 제목 및 앱을 배포할 수 있습니다. 이 Xbox Live 크리에이터 스 프로그램 개발자를 위한 옵션을 테스트 콘솔. 소매 Xbox One 콘솔에서 XDK 게임을 테스트할 수 없습니다.

* 수행 합니다 [개발자 모드 활성화 지침을 제공](../xbox-apps/devkit-activation.md) 소매 콘솔에서 테스트 개발 합니다.  
* Xbox One에 제목에 따라 로드 된 [Xbox One 지침이 설정](../xbox-apps/development-environment-setup.md#setting-up-your-xbox-one)합니다.  
* 에 따라 합니다 [개발자 모드 비활성화 지침](../xbox-apps/devkit-deactivation.md) 소매 모드로 콘솔을 다시 설정 하거나 개발 환경에서 소매 콘솔을 제거 합니다.  
* 콘솔 모드인 경우에 개발자 통해 액세스할 수 있습니다이 원격으로 PC를 사용 하 여 합니다 [Xbox에 대 한 Windows 장치 포털](../debug-test-perf/device-portal-xbox.md)합니다.  

## <a name="xbox-development-kit-testing-idxbox-and-managed-partners"></a>Xbox 개발 키트 테스트: ID@Xbox 및 파트너 관리

파트너를 관리 되는 및 ID@Xbox 개발자는 Xbox 개발자 키트에서 구매 하는 옵션이 합니다 [Xbox 개발 저장소](https://gamedevstore.partners.extranet.microsoft.com/), 관리 되는 개발자 계정으로 액세스할 수만 있는 합니다. Xbox 개발자 키트를 로드할 수 있도록 XDK 게임 콘솔을 테스트, 개발 키트에서 UWP 게임을 테스트할 수도 있습니다. 개발자 키트 하드웨어 옵션 및 자세한 성능 테스트 및 콘솔 관리를 허용 하는 테스트 기능을 사용 하 여 제공 합니다.

읽을 Xbox 개발자 키트를 사용 하 여 경험을 시작 하는 [Xbox 개발 문서를 사용 하 여 시작](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/atoc-getting-started) 관리 되는 파트너 설명서 사이트의. 이 설명서는 권한이 부여 된 개발자에 게 액세스할 수만 ID@Xbox 프로그램과 관리 되는 파트너입니다.