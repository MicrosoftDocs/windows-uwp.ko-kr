---
title: 다중 사용자 애플리케이션 소개
description: 단일 장치에서 협조적으로 게임을 플레이 하는 여러 사용자를 지 원하는 Xbox One 다중 사용자 모델에 대 한 개략적인 소개를 확인 하세요.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 2dde6ed3-7f53-48a6-aebe-2605230decb8
ms.localizationpriority: medium
ms.openlocfilehash: 52a740f3dec2f0106beb76a6a96903ac71bf5035
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157797"
---
# <a name="introduction-to-multi-user-applications"></a>다중 사용자 애플리케이션 소개

이 항목은 Xbox 다중 사용자 모델에 대 한 간단한 개요를 제공 하기 위한 것입니다.

Xbox One 사용자 모델은 단일 장치에서 협조적으로 게임을 실행 하는 여러 사용자를 지 원하는 게임 콘솔의 요구 사항에 맞게 조정 됩니다. 이를 통해 고유한 컨트롤러를 사용 하는 여러 사용자가 동시에 로그인 하 고 한 번의 대화형 세션에서 콘솔을 사용할 수 있습니다. 이는 다른 Windows 장치와 다릅니다. 예:
* **Windows 데스크톱 pc** 에서는 여러 사용자가 동일한 장치를 사용할 수 있지만 각 사용자는 고유한 대화형 세션을 가지 며 각 세션은 장치의 다른 세션과 완전히 독립적입니다.
* **Windows** phone에서는 단일 사용자만 장치를 사용할 수 있습니다. 이 단일 사용자는 OOBE (기본 제공 환경) 중에 결정 되며 사용자가 로그인 한 후에는 로그 아웃할 수 없습니다. 실제로 다른 사용자가 장치를 사용 하려는 경우 장치를 다시 설정 해야 합니다. 
* **Xbox one** 을 사용 하면 여러 사용자가 로그인 하 여 단일 대화형 세션에서 동시에 장치를 사용할 수 있습니다.

Xbox One 사용자 모델의 각 사용자는 로컬 사용자 계정으로 지원 됩니다. 이 로컬 사용자 계정은 Xbox Live 계정 (따라서 Microsoft 계정)과 연결 되어 있습니다. Xbox Live 계정 및 Microsoft 계정에 대 한 Xbox 사용자 계정에 대 한 엄격한 일대일 매핑이 있음을 의미 합니다.

## <a name="single-user-applications"></a>단일 사용자 응용 프로그램
기본적으로 UWP (유니버설 Windows 플랫폼) 앱은 응용 프로그램을 시작한 사용자의 컨텍스트에서 실행 됩니다. 이러한 suas ( *단일 사용자 응용 프로그램* )는 해당 단일 사용자만 인식 하 고 다른 Windows 장치의 사용자 모델과 호환 되는 모드에서 실행 됩니다. Xbox 사용자 모델은 앱과 연결 된 사용자를 관리 하 고 앱이 시작 될 때 사용자가 로그인 되도록 보장 합니다. 이 모델에서 UWP 앱 및 게임 작성자는 Xbox에서 실행 하기 위해 특별 한 작업을 수행할 필요가 없습니다. 

## <a name="multi-user-applications"></a>다중 사용자 응용 프로그램
UWP 게임은 Xbox One 다중 사용자 모델을 옵트인 (opt in) 하도록 선택할 수 있습니다. 이러한 *다중 사용자 응용 프로그램* (MUAs)은 시스템 계정 (기본 계정 이라고 함)의 컨텍스트에서 실행 되며 Xbox one 사용자 모델의 유연성과 성능을 모두 활용할 수 있습니다. 이러한 게임의 경우 Xbox 사용자 모델은 게임과 연결 된 사용자를 관리 하지 않으며 게임 실행을 위해 사용자의 로그인을 요구 하지도 않습니다. 즉, 로그인 한 사용자가 필요한 지 여부, 현재 사용자의 개념을 구현 하는지 여부, 여러 사용자의 동시 입력 허용 여부 등의 사용자 요구 사항을 명시적으로 인식 하도록 작성 해야 합니다.
   
다중 사용자 모델을 옵트인 (opt in) 하려면:   
1. Visual Studio에서 프로젝트를 엽니다.   
2. package.appxmanifest.xml 파일을 선택 합니다.   
3. 마우스 오른쪽 단추를 클릭 하 고 **코드 보기**를 선택 합니다.   
4. 섹션에 다음 줄을 추가 합니다 `<Properties></Properties>` .

```
<uap:SupportedUsers>multiple</uap:SupportedUsers>
```

### <a name="identifying-users-and-inputs"></a>사용자 및 입력 식별
개발자는 KeyUp 및 KeyDown 라우트된 이벤트에서 사용 되는 KeyRoutedEventArgs를 사용 하 여 여러 입력에서 생성 된 이벤트를 구분할 수 있습니다.
Windows.System를 사용 합니다. UserDeviceAssociation. FindUserFromDeviceId 메서드는 특정 입력에 연결 된 사용자를 식별 하는 데 도움이 됩니다.

자세한 내용은 [KeyRoutedEventArgs](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.deviceid) 항목을 참조 하세요.


## <a name="guidance-on-which-model-to-choose"></a>선택할 모델에 대 한 지침
모든 UWP 앱 및 대다수의 단일 사용자 게임은 SUAs로 작성 될 수 있습니다. 공동 복수 플레이어 게임 에서만 Xbox One 다중 사용자 모델 옵트인을 고려 하는 것이 좋습니다.

## <a name="see-also"></a>참고 항목
- [Xbox One의 UWP](index.md)