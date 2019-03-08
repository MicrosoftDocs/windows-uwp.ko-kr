---
title: Visual Studio에서 Unity 게임 테스트
description: Visual Studio에서 Unity의 테스트에 성공 하기 위한 검사 목록 생성 됩니다.
ms.date: 03/19/2018
ms.topic: get-started-article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, unity
ms.openlocfilehash: 4d5a1a5596beba2839e01ca5be6e6d2dbff0c148
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589878"
---
# <a name="test-your-unity-game-build-in-visual-studio"></a>Visual Studio에서 Unity 게임 빌드를 테스트 합니다.

실제 데이터를 사용 하 여 Unity 게임 Xbox Live 기능을 테스트 하려면 빌드해야 게임에 설명 된 대로 합니다 **빌드 및 테스트 프로젝트** 섹션을 [Unity 문서의 Xbox Live 구성](configure-xbox-live-in-unity.md)합니다. 다음 문서를 Visual Studio에서 Unity 게임의 성공적인 테스트 수 있도록 하는 항목의 검사 목록을 제공 합니다.

1. **Unity 게임을 사용 하 여 연결을 올바르게 구성 된 제목 했는지 다시 확인 합니다.**
    숙지 해야 Xbox Live 연결 마법사를 통해 Xbox Live를 활성화 한 경우 [파트너 센터](https://partner.microsoft.com/dashboard)합니다. 파트너 센터를 제목의 Xbox Live 설정을 구성할 수 있습니다 및 Xbox Live와 통신 하 여 제목에 대 한 순서 대로 올바르게 설정 되어 있어야 합니다. 이 문서 [새 Xbox Live 크리에이터 스 프로그램 제목을 만들기 및 테스트 환경에 게시](create-and-test-a-new-creators-title.md) 파트너 센터 설치 과정을 단계별로 안내 합니다. 이미 있는 경우 설치 프로그램을 통해 게임 합니다 **Xbox 구성 마법사** Unity 섹션으로 건너뛸 수 있습니다 [게임에서 서비스 구성을 테스트 Xbox Live](create-and-test-a-new-creators-title.md#test-xbox-live-service-configuration-in-your-game)합니다. 파트너 센터에서 작업 하는 동안 Unity 게임에 Xbox Live 구성에서 정보를 게임에 대 한 파트너 센터 구성과 일치 하는지 확인 해야 합니다.

2. **제목 로그인 수에 제목에는 권한이 부여 된 Microsoft Account(with gamertag)를 갖는지 확인 합니다.**
    권한 있는 계정이 없어도 됩니다 할 전체 로그인 제목으로 테스트 하는 동안 또는 다른 Xbox Live 기능을 사용할 수 있을 합니다. 했는지 권한이 부여 된 Microsoft 계정 및 게이머 태그를 읽어보세요 [테스트 환경에서 Xbox Live 계정을 권한 부여](authorize-xbox-live-accounts.md)합니다.

3. **테스트에 대 한 제목 출간 되 확인 합니다.**
    만들면 변경 하면 제목에 해당 변경 내용을 게시 해야 모래 상자에 사용할 수 있습니다. 푸시 수 있는지 확인 합니다 **테스트** 구성에 변경 내용을 게시 하는 단추입니다.

    ![테스트 이미지에 대 한 게시](../images/creators_udc/creators_udc_xboxlive_config_test.png)

    합니다 **테스트** 단추가 있으면 [파트너 센터](https://partner.microsoft.com/dashboard) 타이틀의 Xbox Live에서 서비스 설정 합니다. 푸시 하려는 새 공인된 테스트 계정을 추가 하는 등 구성에 모든 변경을 수행한 경우는 **테스트** 단추를 새 구성 설정을 Xbox Live 서비스에 게시 합니다.

4. **PC가 개발자 모드를 적절 한 Xbox Live 샌드박스를 사용 하는 있는지 확인 합니다.**
    제목에 대 한 게시 된 테스트 샌드박스 라는 특정 환경에 게시 됩니다. 개발 PC는 샌드박스를 사용 하도록 설정 되어 있지 않으면 Xbox Live 기능을 테스트할 수 없습니다. 확인 하 고 사용 하 여 PC의 샌드박스를 변경 하는 방법을 알아봅니다 합니다 [샌드박스 소개 Xbox Live](xbox-live-sandboxes-creators.md)합니다.

5. **X64로 프로젝트를 빌드하면 있는지 PC에서 빌드할 수 있도록 로컬 컴퓨터를 대상으로 빌드**

    ![빌드 설정](../images/unity/get-started-with-creators/vsBuildSettings.JPG)