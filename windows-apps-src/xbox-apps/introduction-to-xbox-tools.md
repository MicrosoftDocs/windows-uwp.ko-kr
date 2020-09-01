---
title: Xbox One 도구 소개
description: Xbox one 개발 키트의 개발 홈 앱을 사용 하 여 Xbox one 장치 포털에 액세스 하는 방법을 알아봅니다.
ms.date: 10/04/2017
ms.topic: article
keywords: windows 10, uwp, xbox one, 도구
ms.assetid: 6eaf376f-0d7c-49de-ad78-38e689b43658
ms.localizationpriority: medium
ms.openlocfilehash: ed9df02ba929d170eca5b37e4376220e93e4902f
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238318"
---
# <a name="introduction-to-xbox-one-tools"></a>Xbox One 도구 소개

이 섹션에서는 개발자 홈 앱을 통해 Xbox 장치 포털에 액세스 하는 방법을 설명 합니다.

## <a name="dev-home"></a>개발자 홈

Dev Home은 개발자의 생산성을 지원 하도록 설계 된 Xbox One 개발 키트의 도구 환경입니다. Dev Home은 dev kit를 관리 하 고 구성 하는 기능을 제공 합니다.

개발자 홈은 콘솔이 개발자 모드에서 부팅 될 때 열리는 기본 앱입니다. 홈 화면에서 **Dev 홈** 타일을 선택 하 여 dev home을 열 수도 있습니다. 타일이 없으면 콘솔이 개발자 모드에 있지 않습니다.

개발자 홈에 대 한 자세한 내용은 [콘솔의 개발자 홈 (Dev home)](dev-home.md)을 참조 하십시오.

## <a name="xbox-device-portal"></a>Xbox 장치 포털
Xbox 장치 포털은 게임 및 앱을 추가 하 고, Xbox Live 테스트 계정을 추가 하 고, 샌드박스를 변경 하는 등의 기능을 지 원하는 브라우저 기반 장치 관리 도구입니다.

Xbox one 콘솔에서 Xbox 장치 포털을 사용 하도록 설정 하려면 다음을 수행 합니다.

1. 홈 화면에서 **Dev 홈** 타일을 선택 합니다.

  ![개발자 홈 타일 선택](images/introduction-to-xbox-one-tools-1.png)

2. 개발자 홈에서 **홈** 탭으로 이동 하 고 **원격 액세스** 섹션에서 **원격 액세스 설정**을 선택 합니다.

  ![원격 관리 도구](images/introduction-to-xbox-one-tools-2.png)

3. **Xbox Device Portal 사용** 확인란을 선택 합니다.

4. **인증**에서 **웹 또는 PC 도구에서이 콘솔에 원격으로 액세스 하려면 인증 필요** 확인란을 선택 합니다.

5. **사용자 이름** 및 __암호__를 입력 하 고 **저장**을 선택 합니다. 이러한 자격 증명은 브라우저에서 dev kit에 대 한 액세스를 인증 하는 데 사용 됩니다.

6. **닫기**를 선택 하 고 **홈** 탭에서 **원격 액세스** 도구에 나열 된 URL을 확인 합니다.

7. 브라우저에 URL을 입력 합니다. Xbox One 콘솔에서 서명 된 보안 인증서는 잘 알려진 신뢰할 수 있는 게시자로 간주 되지 않으므로 다음 스크린샷에와 같이 제공 된 인증서에 대 한 경고가 표시 됩니다. Edge에서 **세부 정보** 를 클릭 한 다음 **웹 페이지로 이동** 하 여 Xbox 장치 포털에 액세스 합니다.

    ![보안 인증서 경고](images/introduction-to-xbox-one-tools-3.png)

8. 구성한 자격 증명으로 로그인 합니다.

## <a name="xbox-dev-mode-companion"></a>Xbox 개발자 모드 도우미
Xbox Dev 모드 부록은 PC를 떠나지 않고도 콘솔에서 작업을 수행할 수 있도록 하는 도구입니다. 앱을 사용 하면 콘솔 화면을 보고 입력을 보낼 수 있습니다. 자세한 내용은 [Xbox Dev 모드 도우미](xbox-dev-mode-companion.md)를 참조 하세요.

## <a name="see-also"></a>참고 항목
- [UWP용으로 개발하는 경우 Xbox One에서 Fiddler를 사용하는 방법](uwp-fiddler.md)
- [Windows 장치 포털 개요](../debug-test-perf/device-portal.md)
- [Xbox One의 UWP](index.md)