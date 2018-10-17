---
author: Mtoepke
title: Xbox One 개발자 모드 활성화
description: 정품 모드와 개발자 모드 간에 전환할 수 있도록 개발자 모드를 활성화하는 방법
ms.author: scotmi
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: ade80769-17ae-46e9-9c2f-bf08ae5a51ee
ms.localizationpriority: medium
ms.openlocfilehash: 730c345fe1746bf3284f9c0ce2c9bbeaa7ab0501
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4748822"
---
# <a name="xbox-one-developer-mode-activation"></a>Xbox One 개발자 모드 활성화

## <a name="how-developer-mode-works"></a>개발자 모드의 작동 방식
Xbox One에는 *정품* 모드(**1**) 및 *개발자* 모드(**2**)의 두 가지 모드가 있습니다. 정품 모드에서는 Xbox One 본체의 모든 고객 또는 사용자가 콘솔을 사용할 수 있는 상태이며 사용자로서 게임을 플레이하고 앱을 실행할 수 있습니다. 개발자 모드에서는 콘솔에 대한 소프트웨어를 개발할 수 있지만 정품 게임을 플레이하거나 소매 앱을 실행할 수 없습니다.

모든 정품 Xbox One 본체에서 개발자 모드를 사용할 수 있습니다. 개발자 모드를 사용하도록 설정한 후 정품(**2a**)과 개발자 모드(**2b**) 사이를 전환할 수 있습니다.

![Xbox One 모드](images/dev-mode-flow.png)

## <a name="activate-developer-mode-on-your-retail-xbox-one-console"></a>정품 Xbox One 본체에서 개발자 모드 활성화

1.  Xbox One 본체를 시작합니다.

2.  Microsoft Store에서 **개발자 모드 활성화** 앱을 검색하여 설치합니다.

    ![개발자 모드 활성화 앱 설치](images/devkit-activation-1.png)

3.  Microsoft Store 페이지에서 앱을 시작합니다.

    ![개발자 모드 활성화 앱](images/devkit-activation-2.png)

4.  개발자 모드 활성화 앱에 표시된 코드를 메모해 둡니다.

    ![활성화 5단계](images/activation-step-5.png)  
    
5.  [Partner.microsoft.com/xboxactivate](https://partner.microsoft.com/xboxactivate)으로 이동 합니다.

6.  개발자 센터 계정을 사용하여 개발자 센터에 로그인합니다.

7.  개발자 모드 활성화 앱에 표시된 활성화 코드를 입력합니다. 계정에 연결된 활성화 수에는 제한이 있습니다. 개발자 모드가 활성화되면 개발자 센터가 계정과 연결된 정품 인증 중 하나를 사용했음을 나타냅니다.

    ![활성화 8단계](images/activation-step-8-rs2.png)    
    
8.  **동의 및 활성화**를 클릭합니다. 페이지가 다시 로드되고 표에 디바이스가 표시됩니다. Xbox One 개발자 모드 정품 인증 프로그램 계약 조건은 [Xbox One 개발자 모드 정품 인증 프로그램](http://go.microsoft.com/fwlink/p/?LinkId=760399)에서 확인할 수 있습니다.

9.  활성화 코드를 입력한 후 활성화 절차에 대한 진행 화면이 콘솔에 표시됩니다.  
    
10. 활성화가 완료되면 개발자 모드 활성화 앱을 열고 **전환 및 다시 시작**을 클릭하여 개발자 모드로 이동합니다. 이 작업은 평소보다 더 오래 걸립니다.

    ![활성화 12단계](images/activation-step-12.png)   

## <a name="switch-between-retail-and-developer-mode"></a>정품 및 개발자 모드 간 전환
콘솔에서 개발자 모드를 사용하도록 설정한 후 **개발자 홈**을 사용하여 정품 모드와 개발자 모드 간을 전환할 수 있습니다. 개발자 홈 시작 및 사용에 대한 자세한 내용은 [Xbox One 도구 소개](introduction-to-xbox-tools.md)를 참조하세요.

* 정품 모드로 전환하려면 **개발자 홈**을 엽니다. **바로 가기** 아래에서 **개발자 모드 나가기**를 선택합니다. 이렇게 하면 정품 모드로 콘솔이 다시 시작됩니다.    

  ![활성화 13단계](images/activation-step-13-rs4.png)  
  
* 개발자 모드로 전환하려면 개발자 모드 활성화 앱을 사용합니다. 앱을 열고 **전환 및 다시 시작**을 선택합니다. 이렇게 하면 개발자 모드로 콘솔이 다시 시작됩니다.  

  ![활성화 14단계](images/activation-step-12.png)  

## <a name="see-also"></a>참고 항목
- [Xbox One 개발자 모드 비활성화](devkit-deactivation.md)
- [Xbox One의 UWP](index.md)
