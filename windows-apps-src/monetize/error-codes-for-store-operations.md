---
author: mcleanbyron
description: 이 문서에서는 응용 프로그램 및 응용 프로그램에서 구입, 라이선스 및 응용 프로그램 자체 설치 업데이트를 포함 하 여 추가 기능에 대 한 저장소 작업에 대 한 일반적인 오류 코드에 설명 합니다.
title: Store 작업에 대한 오류 코드
ms.author: mcleans
ms.date: 08/24/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 응용 프로그램에서 구입, IAPs, 추가 기능, 오류 코드
ms.localizationpriority: medium
ms.openlocfilehash: 0931397e24eaba44cdf04092f5f367c43a91dd82
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2018
ms.locfileid: "959385"
---
# <a name="error-codes-for-store-operations"></a>Store 작업에 대한 오류 코드

<!-- confirm whether symbolic names are defined for app developers, or do they just handle direct error code values -->

이 문서에서는 개발 (영문) 하거나 응용 프로그램에서 저장소와 관련 된 작업을 테스트 하는 동안 발생할 수 있는 일반적인 오류 코드에 설명 합니다.

## <a name="in-app-purchase-error-codes"></a>앱에 구매 오류 코드

다음과 같은 오류 코드에 앱 구매 작업 관련이 있습니다.

|  오류 코드  |  설명  |
|--------------|---------------|
| 0x803F6100   | 아이 모서리 활성 상태 이므로 앱의 구매를 완료할 수 없습니다. 구입을 완료 하려면 Microsoft 계정 사용 하 여 장치에 로그인 하 고 응용 프로그램을 다시 실행 합니다.               |
| 0x803F6101   | 지정된 앱을 찾을 수 없습니다. 응용 프로그램은 저장소에서 더이상 사용할 수 없습니다 또는 응용 프로그램에 대 한 잘못 된 저장소 ID 제공 했습니다 될 수 없습니다.     |
| 0x803F6102   | 지정된 추가 기능을 찾을 수 없습니다. 추가 기능 이상에서 사용할 수는 저장소 또는 힘 추가 기능에 대 한 잘못 된 저장소 ID를 제공 합니다.                                               |
| 0x803F6103   | 지정한 제품을 찾을 수 없습니다. 제품은 저장소에서 더이상 사용할 수 없습니다 또는 제품에 대 한 잘못 된 저장소 ID 제공 했습니다 될 수 없습니다.                                          |
| 0x803F6104   | 응용 프로그램의 평가판 버전을 실행 하는 때문에 앱의 구매를 완료할 수 없습니다. 응용 프로그램에서 구입을 완료 하려면 응용 프로그램의 정식 버전을 설치 합니다.               |
| 0x803F6105   | Microsoft 계정을 사용 하 여 서명 되지 않은 때문에 앱의 구매를 완료할 수 없습니다.                                              |
| 0x803F6107   | 다음은 현재 작업을 처리 하는 동안 발생 하는 예기치 않은 합니다.                                             |
| 0x803F6108   | 앱에 구매에 앱 라이선스 정보 없기 때문에 완료할 수 없습니다. 앱에 대 한 서버쪽 부하를 하는 경우이 오류가 발생할 수 있습니다. 이 문제를 해결 하려면 응용 프로그램을 제거 하 고 앱 라이선스를 새로 고칠 수 저장소에서 다시 설치 하십시오.                                          |
| 0x803F6109   | 지정한 수량이 나머지 균형 보다 크면 때문에 추가 기능 사용 가능 주문 실행을 완료할 수 없습니다.        |
| 0x803F610A   | 저장소 사용자 계정에 대 한 지정한 공급자 유형이 지원 되지 않습니다.                                            |
| 0x803F610B   | 지정 된 저장소 작업이 지원 되지 않습니다.                                             |
| 0x803F610C   | 응용 프로그램에서 지정 된 백그라운드 작업 계약을 지원 하지 않습니다.                                             |
| 0x80040001   | 추가 기능 제품의 제공된 된 목록 Id 올바르지 않습니다.                        |
| 0x80040002   | 제공된 된 키워드 목록을 올바르지 않습니다.                   |
| 0x80040003   | 주문 실행 대상 올바르지 않습니다.                       |

## <a name="licensing-error-codes"></a>라이선스 오류 코드

다음 오류 코드가 앱 또는 추가 기능에 대 한 작업을 라이선스 관련이 있습니다.

|  오류 코드  |  설명  |
|--------------|---------------|
| 0x803F700C   | 장치가 현재 오프 라인 상태입니다. 파일을 장치 오프 라인일 때이 응용 프로그램을 사용 하려면 저장소 설정을 열고 **오프 라인 사용 권한** 설정을 전환 합니다.            |
| 0x803F8001   | 제품에 대 한 권리를 갖지 않습니다. 제품을 구입에 사용 했던 것 보다 다른 Microsoft 계정을 사용 하는 될 수도 있습니다.           |
| 0x803F8002   | 제품에 대 한 사용자 자격이 만료 되었습니다.           |
| 0x803F8003   | 제품에 대 한 사용자 자격 라이선스는 계속 만들 수 없도록 하는 잘못 된 상태입니다.   |
| 0x803F8009<br/>0x803F800A   | 응용 프로그램에 대 한 평가 기간이 만료 되었습니다.   |
| 0x803F8190   |  라이선스 현재 국가 또는 지역을 장치의 사용할 제품을 허용 하지 않습니다.  |
| 0x803F81F5<br/>0x803F81F6<br/>0x803F81F7<br/>0x803F81F8<br/>0x803F81F9   |  게임 및 저장소에서 앱와 함께 사용할 수 있는 장치의 최대 수에 도달 했습니다. 이 게임 또는 응용 프로그램에서 현재 장치를 사용 하려면 먼저 계정에서 다른 장치를 제거 합니다.  |
| 0x803F9000<br/>0x803F9001    |  라이선스 만료 되었거나 손상 되었습니다. 이 오류를 해결 하는데 저장소 캐시를 다시 설정 하려면 [Windows 앱에 대 한 문제 해결사](https://support.microsoft.com/help/4027498/windows-run-the-troubleshooter-for-windows-apps) 를 실행 하십시오.     |
| 0x803F9006    |  이 제품 자격이 있는 사용자가 자신의 Microsoft 계정 사용 하 여 장치에 로그인 하지 작업을 완료할 수 없습니다.            |
| 0x803F9008<br/>0x803F9009    |  장치 오프 라인입니다. 장치는이 제품을 사용 하 여 온라인 있어야 합니다.            |
| 0x803F900A    |  구독이 만료 되었습니다.            |


## <a name="self-install-update-error-codes"></a>업데이트 오류 코드를 자체 설치

다음 오류 코드가 [패키지 업데이트를 설치 하는 자체](../packaging/self-install-package-updates.md)관련이 있습니다.

|  오류 코드  |  설명  |
|--------------|---------------|
| 0x803F6200   | 사용자의 동의 패키지 업데이트를 다운로드 해야 합니다.               |
| 0x803F6201   | 사용자의 동의 다운로드 하 고 패키지 업데이트를 설치 해야 합니다.                                                  |
| 0x803F6203   | 사용자의 동의 패키지 업데이트를 설치 해야 합니다.                                         |
| 0x803F6204   | 사용자의 동의 업데이트를 다운로드 패키지 다운로드 측정 기능이 네트워크 연결에서 수행 됩니다 때문에 필요 합니다.                                             |
| 0x803F6206   | 사용자의 동의 다운로드 하 고 다운로드 측정 기능이 네트워크 연결에서 수행 됩니다 때문에 패키지 업데이트를 설치 해야 합니다.     |


## <a name="related-topics"></a>관련 항목

* [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md)
* [앱 및 추가 기능에 대한 제품 정보 가져오기](get-product-info-for-apps-and-add-ons.md)
* [앱 및 추가 기능에 대한 라이선스 정보 가져오기](get-license-info-for-apps-and-add-ons.md)
* [앱에서 바로 앱 및 추가 기능 구매 사용](enable-in-app-purchases-of-apps-and-add-ons.md)
* [소모성 추가 기능 구매 사용](enable-consumable-add-on-purchases.md)
* [앱에 구독 추가 기능을 사용하도록 설정](enable-subscription-add-ons-for-your-app.md)
* [앱의 평가판 구현](implement-a-trial-version-of-your-app.md)