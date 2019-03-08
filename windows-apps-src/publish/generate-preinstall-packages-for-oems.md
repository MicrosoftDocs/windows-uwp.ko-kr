---
Description: 개발자 계정에 적절한 권한이 부여되었으면 OEM이 OS 이미지에 앱을 포함할 수 있도록 사전 설치 패키지를 생성하고 다운로드할 수 있습니다.
title: OEM용 사전 설치 패키지 생성
ms.assetid: AC3A45E8-7BBD-44E9-B2D3-B74B7C9B2BC9
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1ab17adc80a643c04ac7793945486c3ff975fde5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643138"
---
# <a name="generate-preinstall-packages-for-oems"></a>OEM용 사전 설치 패키지 생성

개발자 계정에 적절한 권한이 부여되었으면 OEM이 OS 이미지에 앱을 포함할 수 있도록 사전 설치 패키지를 생성하고 다운로드할 수 있습니다. 사전 설치 권한은 OEM이 후원하는 개발자 계정에서만 사용하도록 설정됩니다.


## <a name="important-preinstall-policy--limitations"></a>중요 사전 설치 정책 및 제한 사항

사전 설치 된 앱을 통해 인증 되어야 합니다 [파트너 센터](https://partner.microsoft.com/dashboard) 저장소에 연결 하 고 앱 업데이트를 수신 하는 일을 할 수 있도록 최신 저장소 라이선스가 있어야 합니다.

사전 설치되어 있는 앱은 모든 시장에 제공되고 무료를 유지해야 합니다.


## <a name="generating-preinstall-packages"></a>사전 설치 패키지 생성

사전 설치 권한으로 계정이 사용하도록 설정되면 다음 단계를 완료하세요.

1.  파트너 센터에서 미리 설치 된 앱으로 이동 합니다.
2.  왼쪽 탐색 메뉴에서 **앱 관리**를 확장하고 **현재 패키지**를 선택합니다.
3.  **OS 사전 설치용 패키지 요청** 섹션에서 **다운로드 가능한 패키지 사용**을 선택합니다.
4.  확인 대화 상자에서 **활성화**를 선택합니다.
5.  다운로드할 패키지를 찾고 해당하는 **패키지 생성** 링크를 클릭합니다.

    > [!NOTE]
    > 사전 설치 패키지의 생성 시간은 선택한 패키지 크기에 따라 달라집니다. 이 페이지를 떠났다가 나중에 돌아올 수 있으며, 패키지가 생성되는 동안 페이지를 열린 상태로 남겨 둘 수 있습니다.

6.  패키지가 생성되고 나면 **패키지 다운로드** 링크가 나타납니다. 이 링크를 선택하여 .zip 파일을 다운로드합니다.

OS 이미지에 포함시키도록 이 .zip 파일을 OEM에게 제공할 수 있습니다.


## <a name="support"></a>지원

추가 사전 설치 패키지를 생성 하는 방법에 대 한 질문이 있는, 경우 전자 메일 <partnerops@microsoft.com>합니다.

 

 




