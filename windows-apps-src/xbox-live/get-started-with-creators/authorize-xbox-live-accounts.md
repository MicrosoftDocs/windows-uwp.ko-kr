---
title: 테스트 환경에서 Xbox Live 계정에 권한을 부여합니다
description: 개발 환경에서 테스트에 대 한 공용 Xbox Live 계정의 권한을 부여 하는 방법을 알아봅니다.
ms.assetid: 9772b8f1-81db-4c57-a54d-4e9ca9142111
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 계정, 테스트 계정
ms.localizationpriority: medium
ms.openlocfilehash: 662f85f985baf58eef050060f8132f5a4b92444d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57663858"
---
# <a name="authorize-xbox-live-accounts-for-testing-in-your-environment"></a>사용자 환경에서 테스트 Xbox Live 계정에 권한 부여

이 항목에서는 게시자 테스트 환경으로 Xbox Live 계정을 설정 하 고 프로세스를 거치게 됩니다.

## <a name="prerequisites"></a>필수 구성 요소

Xbox Live 테스트 계정에 권한을 부여 하려면 다음이 필요 합니다.

* [Xbox Live 계정](https://support.xbox.com/browse/my-account/manage-account/Create%20account)

## <a name="navigate-to-the-xbox-test-account-page"></a>Xbox 테스트 계정 페이지로 이동

파트너 센터 계정 설정 섹션에

이 섹션에서는 두 가지 방법 중 하나에 도착할 수 있습니다.

1. [파트너 센터](https://partner.microsoft.com/dashboard/windows/overview)대시보드의 오른쪽 위에 있는 설정 기어 ⚙️을 클릭 하 고 선택 **개발자 설정을** 드롭다운 목록에서. Xbox Live 드롭다운을 선택 하 고 화면 왼쪽의 탐색 메뉴 열립니다 차례로 합니다 **Xbox 테스트 계정** 링크 합니다.
2. Xbox Live 크리에이터 스 구성 페이지에서 테스트 섹션을 찾아서 이라는 제목의 링크를 클릭 **테스트 환경에 대 한 계정에 권한을 부여 Xbox Live**

## <a name="authorize-an-xbox-live-account-for-your-test-environment"></a>테스트 환경에는 Xbox Live 계정 권한 부여

* 한 번 Xbox 테스트 계정 페이지 내에서 모든 현재 권한 있는 계정 목록이 표시 됩니다. 새 계정에 권한을 부여 하려면 계정 추가 단추

![Xbox Live 계정 추가](../images/creators_udc/add_test_account.png)

* 모달 원하는 계정의 전자 메일 주소를 입력할 수 있는 하나의 텍스트 상자를 사용 하 여 화면으로 돌아와야 합니다.

![모달 계정을 Xbox Live를 추가 합니다.](../images/creators_udc/add_test_account_modal.png)

* 전자 메일 주소가 있고 연결 된 Xbox Live 계정을 있는지 유효성을 검사 하려면 추가 단추를 클릭 합니다. 모달 사라지고 보면이 나타내는 테이블에 새 계정을 성공적으로 이제 테스트 환경에 대 한 권한이 부여 된 검사를 통과 하는 경우

![추가 Xbox Live 계정 성공](../images/creators_udc/add_test_account_success.png)

## <a name="troubleshooting"></a>문제 해결

모달에서 입력 한 전자 메일 연결 된 Xbox Live 계정을 확인 하는 조회를 비롯 한 몇 가지 검사를 통해 실행 됩니다. 이러한 검사 중 하나라도 실패 하는 경우 계정 테이블에 추가 되지 이며 따라서 권한이 없는 및 "죄송 합니다. 전자 메일 주소를 추가 하는 문제가 발생 했습니다." 오류가 발생할 수 있습니다.

문제가 있는 경우 적절 한 확인을 시도 하 고 로그온 계정으로 로그인 하는 것 [Xbox.com](https://www.xbox.com/live/)합니다. 로그인 할 수 없는 경우 다음 계정은 Xbox Live 계정이 아닙니다.
