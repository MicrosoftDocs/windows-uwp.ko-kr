---
title: Xbox One 개발자 모드 비활성화
description: 개발자 모드를 비활성화하는 방법
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 244124dd-d80a-4a72-91db-1c9c2fbc7c3c
ms.localizationpriority: medium
ms.openlocfilehash: 5606a8fa6db5b439aa71f5d72b34c0f519d7eea9
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8754568"
---
# <a name="xbox-one-developer-mode-deactivation"></a>Xbox One 개발자 모드 비활성화

더 이상 개발을 위해 콘솔을 사용하지 않으려는 경우 다음 단계를 사용하여 개발자 모드를 비활성화합니다.

## <a name="switch-to-retail-mode"></a>정품 모드로 전환

먼저 Xbox One 본체를 정품 모드로 되돌립니다.

1. **개발자 홈**을 엽니다.

2. **개발자 모드 나가기**를 선택합니다.  콘솔이 정품 모드로 다시 시작됩니다.  

   ![개발자 모드 종료](images/devkit-deactivation-1.png)

이제 다음 방법 중 하나를 사용하여 콘솔을 비활성화합니다.

## <a name="deactivate-your-console-using-the-dev-mode-activation-app"></a>개발자 모드 활성화 앱을 사용하여 콘솔 비활성화

콘솔에서 개발자 모드를 비활성화하는 기본 방법은 **개발자 모드 활성화** 앱을 사용하는 것입니다. 

1. **게임 및 앱** > **앱**으로 이동합니다.
  
   ![활성화 3단계](images/devkit-deactivation-5.png)    
   
2.  개발자 모드 활성화 앱을 엽니다.

3.  **비활성화**를 선택합니다.
  
    ![콘솔 비활성화](images/deactivation-app.png)

**개발자 모드 활성화** 앱에 대한 자세한 내용은 [Xbox One 개발자 모드 활성화](devkit-activation.md)를 참조하세요. 

## <a name="reset-your-console"></a>콘솔 다시 설정

콘솔을 초기화하여 개발자 모드를 비활성화할 수도 있습니다.  

> [!NOTE]
> 콘솔을 초기화하면 모든 로컬 저장 게임 데이터가 손실됩니다.

콘솔을 초기화하려면 다음 단계를 수행합니다.

1.  **My games &amp; apps(내 게임 및 앱)** 로 이동합니다.

2.  **앱**을 선택한 다음 **설정**을 선택합니다.

3.  왼쪽 창의 **시스템**으로 이동한 다음 오른쪽 창의 **콘솔 정보**를 선택합니다.   
   
    ![콘솔 정보 및 업데이트](images/devkit-deactivation-2.png)  
    
4.  **콘솔 다시 설정**을 선택합니다.
    
    ![콘솔 다시 설정](images/devkit-deactivation-3.png)
    
5.  다음으로 **다시 설정 및 모두 제거**를 선택합니다. 이 옵션은 콘솔을 원래 정품 상태로 다시 설정합니다.  모든 앱, 게임 및 로컬 저장 데이터가 삭제됩니다. 다른 옵션인 **Reset and keep my games &amp; apps(내 게임 및 앱 다시 설정 및 유지)** 를 선택하면 개발자 프로그램에서 콘솔을 제거하지 않습니다.  
   
    ![다시 설정 및 모두 제거](images/devkit-deactivation-4.png)

## <a name="deactivate-your-console-using-partner-center"></a>파트너 센터를 사용 하 여 콘솔 비활성화

어떤 이유로 든 콘솔에 액세스할 수 없는 경우 파트너 센터를 사용 하 여 콘솔에서 개발자 모드를 비활성화할 수도 있습니다.

1. 파트너 센터에서 [Xbox One 본체 관리](https://partner.microsoft.com/xboxdevices) 페이지로 이동 합니다. 로그인 하 라는 메시지가 표시 될 수 있습니다.

2. 콘솔 목록에서 일련 번호, 콘솔 ID 또는 디바이스 ID와 일치하는 비활성화하려는 콘솔을 찾습니다.  

3. **비활성화**를 클릭합니다.  
  
![개발자 센터를 사용하여 비활성화](images/devkit-deactivation-6.png)

이전에 Xbox One 본체가 정품 모드로 되돌아가지 않았으면 [정품 모드로 전환](#switch-to-retail-mode)에 설명된 것처럼 지금 전환합니다.

## <a name="see-also"></a>참고 항목
- [Xbox One 개발자 모드 활성화](devkit-activation.md)
- [Xbox One의 UWP](index.md)
