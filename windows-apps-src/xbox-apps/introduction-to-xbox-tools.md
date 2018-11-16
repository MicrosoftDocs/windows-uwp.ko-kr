---
author: eliotcowley
title: Xbox One 도구 소개
description: Windows 장치 포털을 사용하는 Xbox One 특정 도구 개발자 홈입니다.
ms.author: elcowle
ms.date: 10/04/2017
ms.topic: article
keywords: Windows 10, uwp, xbox one, 도구
ms.assetid: 6eaf376f-0d7c-49de-ad78-38e689b43658
ms.localizationpriority: medium
ms.openlocfilehash: 71fd9f3ad1c3fcf02420502692518310b896f52a
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6975763"
---
# <a name="introduction-to-xbox-one-tools"></a>Xbox One 도구 소개

이 섹션에서는 개발자 홈 앱을 통해 Xbox 장치 포털에 액세스하는 방법을 설명합니다.

## <a name="dev-home"></a>개발자 홈

개발자 홈은 개발자의 생산성을 지원하도록 설계된 Xbox One 개발 키트의 도구 환경입니다. 개발자 홈은 개발 키트를 관리하고 구성하는 기능을 제공합니다.

개발자 홈은 개발자 모드에서 콘솔을 부팅할 때 열리는 기본 앱입니다. 홈 화면에서 **개발자 홈** 타일을 선택하여 개발자 홈을 열 수도 있습니다. 타일이 없으면 콘솔이 개발자 모드가 아닙니다.

개발자 홈에 대한 자세한 내용은 [콘솔의 개발자 홈(개발자 홈)](dev-home.md)을 참조하세요.

## <a name="xbox-device-portal"></a>Xbox 장치 포털
Xbox 장치 포털은 게임 및 앱을 추가하고, Xbox Live 테스트 계정을 추가하고, 샌드박스를 변경하고 그 외 많은 작업을 할 수 있는 브라우저 기반 장치 관리 도구입니다.

Xbox One 본체에서 Xbox 장치 포털을 사용하도록 설정하려면:

1. 홈 화면에서 **개발자 홈** 타일을 선택합니다.

  ![개발자 홈 타일 선택](images/introduction-to-xbox-one-tools-1.png)

2. 개발자 홈에서 **홈** 탭의 **원격 액세스** 섹션 아래 **원격 액세스 설정**을 선택합니다.

  ![원격 관리 도구](images/introduction-to-xbox-one-tools-2.png)

3. **Xbox 장치 포털 사용** 확인란을 선택합니다.

4. **인증** 아래에서 **웹 또는 PC 도구에서 원격으로 이 콘솔에 액세스할 때 인증 요구** 확인란을 선택합니다.

5. **사용자 이름** 및 __암호__를 입력하고 **저장**을 선택합니다. 이러한 자격 증명은 브라우저에서 개발 키트에 대한 액세스를 인증하는 데 사용됩니다.

6. **닫기**를 선택하고 **홈** 탭에서 **원격 액세스** 도구에 나열된 URL을 메모합니다.

7. 브라우저에 URL을 입력합니다. Xbox One 본체에서 서명한 보안 인증서는 신뢰할 수 있는 잘 알려진 게시자로 간주되지 않으므로 제공된 인증서에 대해 다음 스크린샷과 유사한 경고가 표시됩니다. Edge에서 **세부 정보**를 클릭한 다음 **웹 페이지로 이동**하여 Xbox 장치 포털에 액세스할 수 있습니다.

    ![보안 인증서 경고](images/introduction-to-xbox-one-tools-3.png)

8. 구성한 자격 증명으로 로그인합니다.

## <a name="xbox-dev-mode-companion"></a>Xbox 개발자 모드 도우미
Xbox 개발자 모드 도우미는 PC를 종료하지 않고 콘솔에서 작업할 수 있는 도구입니다. 앱에서 콘솔 화면을 보고 입력을 보낼 수 있습니다. 자세한 내용은 [Xbox 개발자 모드 도우미](xbox-dev-mode-companion.md)를 참조하세요.

## <a name="see-also"></a>참고 항목
- [UWP용으로 개발하는 경우 Xbox One에서 Fiddler를 사용하는 방법](uwp-fiddler.md)
- [Windows 장치 포털 개요](../debug-test-perf/device-portal.md)
- [Xbox One의 UWP](index.md)