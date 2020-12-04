---
title: Xbox One 개발자 모드 활성화
description: 정품 모드와 개발자 모드 간을 전환할 수 있도록 개발자 모드를 활성화 하는 방법입니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ade80769-17ae-46e9-9c2f-bf08ae5a51ee
ms.localizationpriority: medium
ms.openlocfilehash: d93fef1b06fa52616b7e5eddf7b3ac0530856dc2
ms.sourcegitcommit: a15bc17aa0640722d761d0d33f878cb2a822e8ed
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96577105"
---
# <a name="xbox-one-developer-mode-activation"></a>Xbox One 개발자 모드 활성화

## <a name="how-developer-mode-works"></a>개발자 모드 작동 방법
이 문서는 Xbox One 및 Xbox 시리즈 X |에만 적용 됩니다. S 콘솔은 소매 채널을 통해 획득 됩니다. 관리 되는 개발 프로그램을 통해 HW 하는 개발 키트는이 문서의 끝에 있는 참고를 참조 하세요.

Xbox retail 콘솔에는 두 가지 모드, 즉 정품 모드 (1)와 개발자 모드 (2)를 사용할 수 있습니다. 소매점 모드에서 콘솔은 표준 상태입니다. 게임을 재생 하 고 Xbox 스토어를 통해 구입한 앱을 실행할 수 있습니다. 개발자 모드에서 콘솔에 대 한 소프트웨어를 개발 하 고 테스트할 수 있지만 소매 게임을 플레이 하거나 소매점 앱을 실행할 수 없습니다.

개발자 모드는 Xbox 스토어에 있는 "개발자 키트 변환 전 변환" 앱을 통해 모든 소매 Xbox 콘솔에서 사용 하도록 설정할 수 있습니다. 소매 콘솔에서 개발자 모드를 사용 하도록 설정한 후에는 일반 정품 (2a)와 개발자 모드 (2b) 사이에서 전환할 수 있습니다.

> [!NOTE]
> Xbox 관리 프로그램 (예:)을 통해 제공 되는 Xbox development 하드웨어에서이 변환 앱을 실행 하지 마세요 ID@Xbox . 또는 게임을 개발 하는 동안 오류 및 지연이 발생할 수 있습니다. 관리 되는 파트너인 경우 개발 하드웨어 활성화에 대 한 자세한 정보를 얻을 수 있습니다. https://developer.microsoft.com/en-us/games/xbox/docs/gdk/provisioning-role로 이동합니다.

<br></br>

![Xbox One 모드](images/dev-mode-flow.png)

## <a name="activate-developer-mode-on-your-retail-xbox-console"></a>소매 Xbox 콘솔에서 개발자 모드 활성화

1.  Xbox 콘솔을 시작 합니다.

2.  Xbox One 스토어에서 **개발 모드 정품 인증** 앱을 검색 하 고 설치 합니다.

    ![개발 모드 정품 인증 앱 설치](images/devkit-activation-1.png)

3.  스토어 페이지에서 앱을 시작 합니다.

    ![개발 모드 정품 인증 앱](images/devkit-activation-2.png)

4.  개발 모드 정품 인증 앱에 표시 되는 코드를 확인 합니다.

    ![활성화 5 단계](images/activation-step-5.png)  
    
5.  [파트너 센터에 앱 개발자 계정을 등록](https://developer.microsoft.com/store/register)합니다.  게임을 게시 하기 위한 첫 번째 단계 이기도 합니다.

6.  유효한 현재 파트너 센터 앱 개발자 계정으로 [파트너 센터](https://partner.microsoft.com/dashboard) 에 로그인 합니다.  왼쪽 탐색 창에 여러 옵션이 표시 되지 않거나 **개요** 섹션에 **새 앱 만들기** 옵션이 표시 되지 않으면 다음 단계와 활성화 링크가 _작동 하지_ 않습니다. 이전 단계에서 앱 개발자 계정을 완전히 등록 했는지 확인 합니다.

7.  [Partner.microsoft.com/xboxconfig/devices](https://partner.microsoft.com/xboxconfig/devices)로 이동 합니다.

8.  개발 모드 정품 인증 앱에 표시 된 활성화 코드를 입력 합니다. 계정에 연결 된 활성화 수가 제한 되어 있습니다. 개발자 모드가 활성화 된 후 파트너 센터는 계정과 연결 된 활성화 중 하나를 사용 했음을 표시 합니다.

    ![활성화 8 단계](images/activation-step-8-rs2.png)    
    
9.  **동의 및 활성화를** 클릭 합니다. 그러면 페이지가 다시 로드 되 고 장치에서 표를 채우는 것을 볼 수 있습니다. Xbox one 개발자 모드 정품 인증 프로그램 계약에 대 한 용어는 [Xbox One Developer Mode Activation program](/legal/windows/agreements/xbox-one-developer-mode-activation)에서 찾을 수 있습니다.

10. 활성화 코드를 입력한 후 활성화 절차에 대한 진행 화면이 콘솔에 표시됩니다.  
    
11. 활성화가 완료 되 면 개발 모드 정품 인증 앱을 열고 전환을 클릭 한 다음 **다시 시작** 을 클릭 하 여 개발자 모드로 이동 합니다. 이는 평소 보다 시간이 오래 걸립니다.

    ![활성화 12 단계](images/activation-step-12.png)   

## <a name="switch-between-retail-and-developer-mode"></a>소매점 모드와 개발자 모드 간 전환
콘솔에서 개발자 모드를 사용 하도록 설정한 후에는 **Dev Home** 을 사용 하 여 소매 모드와 개발자 모드 간을 전환 합니다. 개발자 홈을 시작 하 고 사용 하는 방법에 대해 자세히 알아보려면 [Xbox one 도구 소개](introduction-to-xbox-tools.md)를 참조 하세요.

* 소매 모드로 전환 하려면 **개발자 홈** 을 엽니다. **빠른 작업** 에서 **개발 모드 유지** 를 선택 합니다. 그러면 콘솔이 정품 모드로 다시 시작 됩니다.    

  ![활성화 13 단계](images/activation-step-13-rs4.png)  
  
* 개발자 모드로 전환 하려면 개발 모드 정품 인증 앱을 사용 합니다. 앱을 열고 **전환 및 다시 시작** 을 선택 합니다. 그러면 콘솔이 개발자 모드로 다시 시작 됩니다.  

  ![활성화 14 단계](images/activation-step-12.png)  

## <a name="see-also"></a>참고 항목
- [Xbox One Developer 모드 비활성화](devkit-deactivation.md)
- [Xbox One의 UWP](index.md)
