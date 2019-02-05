---
title: 테스트 환경에서 Xbox Live 계정에 권한을 부여합니다
description: 개발 환경에서 테스트에 사용할 공개 Xbox Live 계정에 권한을 부여 하는 방법을 알아봅니다.
ms.assetid: 9772b8f1-81db-4c57-a54d-4e9ca9142111
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 계정, 테스트 계정
ms.localizationpriority: medium
ms.openlocfilehash: 662f85f985baf58eef050060f8132f5a4b92444d
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/05/2019
ms.locfileid: "9049252"
---
# <a name="authorize-xbox-live-accounts-for-testing-in-your-environment"></a>사용자 환경에서 테스트에 대 한 Xbox Live 계정에 권한을 부여합니다

이 항목에서는 Xbox Live 계정 게시자 테스트 환경 설정 하는 프로세스를 통과

## <a name="prerequisites"></a>필수 조건

Xbox Live 테스트 계정에 권한을 부여 하려면 다음이 필요 합니다.

* [Xbox Live 계정](https://support.xbox.com/browse/my-account/manage-account/Create%20account)

## <a name="navigate-to-the-xbox-test-account-page"></a>Xbox 테스트 계정 페이지로 이동

파트너 센터의 계정 설정 섹션에 있는이

이 섹션의 두 가지 방법 중 하나에 도달할 수 있습니다.

1. [파트너 센터](https://partner.microsoft.com/dashboard/windows/overview)대시보드의 오른쪽 상단에 설정 기어 ⚙️ 클릭 하 고 드롭다운 메뉴에서 **개발자 설정** 을 선택 합니다. Xbox Live 드롭다운 및 **Xbox 테스트 계정** 연결을 선택 하는 화면의 왼쪽에서 탐색 메뉴가 열립니다.
2. Xbox Live 크리에이터 스 구성 페이지에서 테스트 섹션을 찾아 **테스트 환경에 대 한 권한을 부여 Xbox Live 계정** 자격이 링크 클릭

## <a name="authorize-an-xbox-live-account-for-your-test-environment"></a>테스트 환경에 대 한 Xbox Live 계정에 권한을 부여합니다

* 한 번 Xbox 테스트 계정 페이지 내에서 모든 현재 권한이 있는 계정 목록이 표시 됩니다. 새 계정을 부여할 추가 계정 단추를 클릭 합니다.

![Xbox Live 계정 추가](../images/creators_udc/add_test_account.png)

* 원하는 계정의 메일 주소를 입력할 수 있는 텍스트 상자를 사용 하 여 화면으로 모달 팝 해야

![모달 계정을 Xbox Live 추가](../images/creators_udc/add_test_account_modal.png)

* 전자 메일 주소 있고 연결 된 Xbox Live 계정에 있는지 확인 하기 위한 추가 단추를 클릭 합니다. 이 검사를 통과 하는 경우 모달 사라집니다 및 테스트 환경에 대 한 것을 나타내는 테이블에 새 계정이 성공적으로 인증 이제 표시 됩니다.

![Xbox Live 계정 성공 추가](../images/creators_udc/add_test_account_success.png)

## <a name="troubleshooting"></a>문제 해결

모달에 입력 한 메일은 Xbox Live 계정 연결 되어 있는지 확인 하는 조회를 포함 하 여 몇 가지 검사를 통해 실행 됩니다. 이러한 검사 중 하나라도 실패 계정 하지 테이블에 추가 되 고 따라서 권한이 없는 및 "죄송 하지만, 전자 메일 주소를 추가 하는 문제 발생" 오류가 발생할 수 있습니다.

문제가 있는 경우 좋은 검사 시도 [Xbox.com](https://www.xbox.com/live/)에서 계정으로 로그인 하는 것입니다. 로그인 할 수 없는 경우 다음 계정 하지 Xbox Live 계정이입니다.
