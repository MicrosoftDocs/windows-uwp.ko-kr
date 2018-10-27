---
author: QuinnRadich
Description: Design an instructional user interface (UI) that teaches users how to work with your UWP app.
title: 사용 안내 UI 디자인에 대한 지침
label: Instructional UI
template: detail.hbs
ms.author: quradic
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: c87e2f06-339d-4413-b585-172752964f56
ms.localizationpriority: medium
ms.openlocfilehash: 9c97b6b5eca82d309a4b65a914041adeb1e782db
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5689946"
---
# <a name="instructional-ui-guidelines"></a>사용 안내 UI 지침



상황에 따라 특정 터치 조작 등 사용자에게 명확하지 않을 수 있는 앱의 기능에 대해 설명하는 것이 유용할 수 있습니다. In these cases, you need to present instructions to the user through the user interface (UI), so that they can use those features they might have missed.

## <a name="when-to-use-instructional-ui"></a>사용 안내 UI를 사용하는 경우

사용 안내 UI를 사용할 때는 주의해야 합니다. 과도하게 사용할 경우 쉽게 무시되거나 사용자를 방해하여 비효과적일 수 있습니다.

사용 안내 UI는 사용자가 터치 제스처 또는 관심이 있는 설정과 같은 앱의 중요하고 명확하지 않은 기능을 찾을 수 있도록 지원하는 데 사용해야 합니다. 그렇지 않을 경우 간과되었을 수 있는 새로운 기능이나 앱의 변경 내용을 사용자에게 알리는 데 사용할 수도 있습니다.

앱이 터치 제스처에 의존하지 않는 한 사용 안내 UI를 사용하여 앱의 기본 기능을 설명해서는 안 됩니다.

## <a name="principles-of-writing-instructional-ui"></a>사용 안내 UI 작성 원칙

좋은 사용 안내 UI는 사용자에게 관련된 교육 정보를 제공하며 사용자 환경을 개선합니다. 다음 원칙을 따라야 합니다.

-   **단순성:** 사용자는 복잡한 정보로 경험이 중단되는 것을 원하지 않습니다.
-   **기억 가능:** 사용자는 작업을 시도할 때마다 동일한 지침을 보고 싶어 하지 않으므로 한 번에 기억할 수 있는 지침이어야 합니다.
-   **즉각적인 관련성:** 사용 안내 UI가 즉시 수행하려는 작업에 대해 설명하지 않을 경우 주목할 이유가 없습니다.

사용 안내 UI를 과도하게 사용하는 것을 피하고 올바른 항목을 선택해야 합니다. 다음 사항은 설명하지 마세요.

-   **기본 기능:** 사용자에게 앱을 사용하는 지침이 필요한 경우 앱 디자인을 보다 직관적으로 만드는 것이 좋습니다.
-   **명백한 기능:** 사용자가 지침 없이 기능을 이해할 수 있는 경우 사용 안내 UI는 방해가 될 뿐입니다.
-   **복잡한 기능:** 사용 안내 UI는 간결해야 하며, 일반적으로 복잡한 기능에 관심이 있는 사용자는 기꺼이 지침을 검색하므로 제공할 필요가 없습니다.

사용 안내 UI로 인해 사용자에게 불편을 주지 않도록 합니다. 다음과 같은 작업은 하지 마세요.

-   **중요한 정보를 가림:** 사용 안내 UI가 앱의 다른 기능에 방해가 되면 안 됩니다.
-   **사용자 참여 강제:** 사용자가 사용 안내 UI를 무시하고 앱을 진행할 수 있어야 합니다.
-   **반복 정보 표시:** 처음에 무시한 경우에도 사용 안내 UI를 계속 표시하여 사용자를 귀찮게 하지 마세요. 사용 안내 UI를 표시하는 설정을 다시 추가하는 것이 좋습니다.

## <a name="examples-of-instructional-ui"></a>사용 안내 UI의 예

다음은 사용 안내 UI를 통해 사용자에게 정보를 제공할 수 있는 몇 가지 경우입니다.

-   **사용자의 터치 조작 검색 지원.** 다음 스크린샷은 Cut the Rope 게임에서 터치 제스처를 사용하는 방법을 플레이어에게 설명하는 사용 안내 UI를 보여 줍니다.

    ![사용 안내 UI 메시지 "slide acress to cut the rope"를 보여 주는 게임의 스크린샷](images/in-game-controls-3.png)

-   **좋은 첫인상 주기.** 나만의 동영상을 처음 실행하면 사용 안내 UI에서 사용자 경험을 방해하지 않고 동영상 만들기를 시작하라는 메시지를 표시합니다.

    ![나만의 동영상 앱의 실행 화면](images/instructional-ui-movie.png)

-   **사용자를 복잡한 작업의 다음 단계로 안내.** Windows 메일 앱에서 받은 편지함의 맨 아래에 있는 힌트는 사용자에게 이전 메시지를 액세스하려면 **설정**으로 이동하도록 알려줍니다.

    ![사용 안내 UI 메시지를 보여 주는 Windows 메일 앱의 잘린 스크린샷](images/instructional-ui-mail-inbox.png)

    사용자가 메시지를 클릭하면 앱의 **설정** 플라이아웃이 화면 오른쪽에 표시되어 사용자가 작업을 완료할 수 있게 합니다. 다음 스크린샷은 사용자가 사용 안내 UI 메시지를 클릭하기 전과 이후의 메일 앱을 보여 줍니다.

    | 이전                                                               | 이후                                                                                                        |
    |----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
    | ![Windows 메일 앱의 스크린샷](images/instructional-ui-mail.png) | ![설정 플라이아웃이 확장된 Windows 메일 앱의 스크린샷](images/instructional-ui-mail-flyout.png) |

## <a name="related-articles"></a>관련 문서

* [앱 도움말에 대한 지침](guidelines-for-app-help.md)
