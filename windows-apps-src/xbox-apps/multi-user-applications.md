---
author: Mtoepke
title: 다중 사용자 응용 프로그램 소개
description: Xbox 다중 사용자 모델에 대한 높은 수준의 간략한 소개입니다.
ms.author: scotmi
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.assetid: 2dde6ed3-7f53-48a6-aebe-2605230decb8
ms.localizationpriority: medium
ms.openlocfilehash: 7534b6764bc98c415b557d100d869df186453626
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6182966"
---
# <a name="introduction-to-multi-user-applications"></a>다중 사용자 응용 프로그램 소개

이 항목은 Xbox 다중 사용자 모델에 대한 높은 수준의 소개를 간략히 드리기 위한 것입니다.

Xbox One 사용자 모델은 하나의 디바이스에서 여러 사용자가 협력해서 게임을 하는 기능을 지원하는 게임 콘솔의 요구 사항에 맞게 조정되어 있습니다. 따라서 각각 자신의 컨트롤러가 있는 여러 사용자가 로그인하고 단일 대화형 세션에서 동시에 콘솔을 사용할 수 있습니다. 이는 다른 Windows 디바이스와 다른 점입니다. 예제:
* **Windows 데스크톱 PC**에서는 여러 사용자가 동일한 디바이스를 사용할 수 있지만, 사용자마다 고유한 대화형 세션이 있으며 각 세션이 디바이스의 다른 세션과 완전히 독립적입니다.
* **Windows Phone**에서는 단일 사용자만 디바이스를 사용할 수 있습니다. 이 단일 사용자는 OOBE(첫 실행 경험) 중에 결정되며 사용자가 로그인한 후 로그아웃할 수 없습니다. 실제로 다른 사용자가 디바이스를 사용하려는 경우 디바이스를 초기화해야 합니다. 
* **Xbox One**을 사용하면 여러 사용자가 로그인하고 단일 대화형 세션에서 동시에 디바이스를 사용할 수 있습니다.

Xbox One 사용자 모델의 각 사용자는 로컬 사용자 계정의 지원을 받습니다. 이 로컬 사용자 계정은 Xbox Live 계정(및 Microsoft 계정)과 연결됩니다. 따라서 Xbox 사용자 계정과 Xbox Live 계정 및 Microsoft 계정 간에 엄격한 일대일 매핑이 있습니다.

## <a name="single-user-applications"></a>단일 사용자 응용 프로그램
기본적으로 UWP(유니버설 Windows 플랫폼) 앱은 응용 프로그램을 시작한 사용자의 컨텍스트에서 실행됩니다. 이러한 SUA(*단일 사용자 응용 프로그램*)는 해당 단일 사용자만 인식하고 다른 Windows 디바이스의 사용자 모델과 호환되는 모드로 실행됩니다. Xbox 사용자 모델은 앱과 연결된 사용자를 관리하고, 앱을 시작할 때 사용자가 로그인되었는지 확인합니다. 이 모델에서 UWP 앱 및 게임 작성자는 Xbox에서 실행하기 위해 특별한 작업을 수행할 필요가 없습니다. 

## <a name="multi-user-applications"></a>다중 사용자 응용 프로그램
UWP 게임은 Xbox One 다중 사용자 모델에 옵트인(opt in)하도록 선택할 수 있습니다. 이러한 MUA(*다중 사용자 응용 프로그램*)는 시스템 계정(기본 계정이라고 함)의 컨텍스트에서 실행되며, Xbox One 사용자 모델의 유연성과 기능을 최대한 활용할 수 있습니다. 이러한 게임의 경우 Xbox 사용자 모델은 게임과 연결된 사용자를 관리하지 않으며, 게임을 실행하기 위해 사용자 로그인을 요구하지도 않습니다. 따라서 로그인한 사용자를 요구하는지 여부, 현재 사용자의 개념을 구현하는지 여부, 여러 사용자의 동시 입력을 허용하는지 여부 등의 사용자 요구 사항을 명시적으로 인식하고 관리하도록 게임을 작성해야 합니다.
   
다중 사용자 모델을 옵트인(opt in)하려면:   
1. Visual Studio에서 프로젝트를 엽니다.   
2. package.appxmanifest.xml 파일을 선택합니다.   
3. 마우스 오른쪽 단추를 클릭하고 **코드 보기**를 선택합니다.   
4. `<Properties></Properties>` 섹션에서 다음 줄을 추가합니다.

```
<uap:SupportedUsers>multiple</uap:SupportedUsers>
```

### <a name="identifying-users-and-inputs"></a>사용자 및 입력 식별
개발자는 KeyUp 및 KeyDown 라우트된 이벤트에서 사용되는 KeyRoutedEventArgs.DeviceId를 사용하여 다른 입력에서 생성된 이벤트를 차별화할 수 있습니다.
Windows.System.UserDeviceAssociation.FindUserFromDeviceId 메서드를 사용하면 특정 입력과 연결된 사용자를 식별할 수 있습니다.

자세한 내용은 [KeyRoutedEventArgs.DeviceId](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.keyroutedeventargs.deviceid) 항목을 참조하세요.


## <a name="guidance-on-which-model-to-choose"></a>선택할 모델에 대한 지침
모든 UWP 앱과 대다수 단일 사용자 게임은 SUA로 작성할 수 있습니다. 협력적인 멀티 플레이어 게임의 경우에만 Xbox One 다중 사용자 모델을 옵트인(opt in)하는 것이 좋습니다.

## <a name="see-also"></a>참고 항목
- [Xbox One의 UWP](index.md)
