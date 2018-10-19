---
author: andrewleader
Description: Learn about when and where you should use secondary tiles in your UWP app.
title: 보조 타일
label: Secondary tiles
template: detail.hbs
ms.author: wdg-dev-content
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 보조 타일, 안내, 지침, 모범 사례
ms.localizationpriority: medium
ms.openlocfilehash: 1e3d31376b9ac155dab6bffa7739cb880af1cff9
ms.sourcegitcommit: 310a4555fedd4246188a98b31f6c094abb33ec60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/19/2018
ms.locfileid: "5130007"
---
# <a name="secondary-tile-guidance"></a>보조 타일 지침


보조 타일은 사용자가 시작 메뉴에서 앱 내의 특정 영역에 직접 액세스할 수 있는 일관적이고 효율적인 방법을 제공합니다. 시작 메뉴에 보조 타일을 "고정"할지 여부는 사용자가 선택하지만 앱에서 고정 가능한 영역은 개발자가 결정합니다. 자세한 요약 정보는 [보조 타일 개요](secondary-tiles.md)를 참조하세요. 보조 타일을 사용하도록 설정하고 앱에서 연결된 UI를 설계할 때에는 다음 지침을 고려해야 합니다.

> [!NOTE]
> 오직 사용자만이 시작 메뉴에 보조 타일을 고정할 수 있으며, 앱은 프로그래밍 방식으로 보조 타일을 고정할 수 없습니다. 또한 사용자는 타일 제거를 제어하며, 시작 메뉴에서 또는 상위 앱 내에서 보조 타일을 제거할 수 있습니다.


## <a name="recommendations"></a>권장 사항

앱에서 보조 타일을 사용하도록 설정할 때에는 다음 권장 사항을 고려해야 합니다.

* 포커스 상태의 콘텐츠를 고정할 수 있는 경우 해당 사용자에 대한 보조 타일을 만드는 "시작에 고정" 단추가 앱 바에 포함되어야 합니다.
* 사용자가 "시작에 고정"을 클릭하면 개발자는 그 즉시 UI 스레드에서 [보조 타일을 고정](secondary-tiles-pinning.md)하는 API를 호출해야 합니다.
* 포커스 상태의 콘텐츠가 이미 고정된 경우 앱 바의 "시작에 고정" 단추를 "시작에서 제거" 단추로 바꿔야 합니다. "시작에서 제거" 단추는 기존 보조 타일을 제거해야 합니다.
* 포커스 상태의 콘텐츠를 고정할 수 없는 경우에는 "시작에 고정" 단추를 표시하지 마세요(또는 비활성화된 "시작에 고정" 단추를 표시할 것).
* "시작에 고정" 및 "시작에서 제거" 단추에 시스템에서 제공한 문자를 사용합니다(Windows.UI.Xaml.Controls.Symbol 또는 WinJS.UI.AppBarIcon의 멤버 고정 및 고정 해제 참조).
* 표준 단추 텍스트 "시작에 고정" 및 "시작에서 제거"를 사용합니다. 시스템에서 제공한 고정 및 고정 해제 문자를 사용하는 경우 기본 텍스트를 재정의해야 합니다.
* 보조 타일을 상위 앱과 상호 작용하기 위한 가상 명령 단추로 사용하지 마세요(예: "다음 트랙으로 건너뛰기").


## <a name="additional-usage-guidance-for-devs"></a>개발자를 위한 추가 사용법 지침

* 앱이 시작되고 앱이 알지 못하는 추가 또는 삭제가 이루어진 경우 앱이 항상 보조 타일을 열거해야 합니다. 시작 화면 앱 바를 통해 보조 타일을 삭제하면 Windows는 단순히 해당 타일을 제거합니다. 보조 타일에서 사용하던 리소스를 해제하는 일은 앱 자체에서 해야 합니다. 보조 타일이 클라우드를 통해 복사되면 현재 타일 또는 보조 타일의 배지 알림, 예정된 알림, 푸시 알림 채널, 주기적 알림에 사용되는 URI(Uniform Resource Identifier)는 보조 타일과 함께 복사되지 않으며 다시 설정해야 합니다.
* 앱이 보조 타일에 의미 있고 다시 만들 수 있는 고유의 ID를 사용해야 합니다. 앱에 의미 있는 예측 가능한 보조 타일 ID를 사용하면 새 컴퓨터에 새로 설치하는 동안 이러한 타일이 표시될 때 타일로 무엇을 해야 하는지 앱이 이해하는 데 도움이 됩니다.
  * 런타임에 앱은 특정 타일의 존재 여부를 쿼리할 수 있습니다.
  * 특정 앱에 속한 모든 보조 타일을 반환하라고 보조 타일 플랫폼에 요청할 수 있습니다. 이러한 타일에 의미 있는 고유의 ID를 사용하면 앱이 보조 타일 집합을 검사하고 적절한 조치를 취하는 데 도움이 될 수 있습니다. 예를 들어 소셜 미디어 앱의 경우 ID를 보면 타일이 누구를 위해 생성되었는지 개별 연락처를 식별할 수 있습니다.
* 시작 화면의 모든 타일과 같은 보조 타일은 자주 새로운 내용으로 업데이트될 수 있는 동적 콘센트입니다. 보조 타일은 다른 타일과 동일한 메커니즘을 사용하여 업데이트 및 알림을 표시할 수 있습니다. 자세한 내용은 [알림 전달 방법 선택](choosing-a-notification-delivery-method.md)을 참조하세요.


## <a name="related"></a>관련 항목

* [보조 타일 개요](secondary-tiles.md)
* [보조 타일 고정](secondary-tiles-pinning.md)
* [타일 자산](app-assets.md)
* [타일 콘텐츠 설명서](create-adaptive-tiles.md)
* [로컬 타일 알림 보내기](sending-a-local-tile-notification.md)
