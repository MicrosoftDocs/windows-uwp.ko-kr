---
author: stevewhims
Description: Design your app to provide bidirectional text support (BiDi) so that you can combine script from left-to-right (LTR) and right-to-left (RTL) writing systems, which generally contain different types of alphabets.
title: 양방향 텍스트를 위한 앱 디자인
template: detail.hbs
ms.author: stwhi
ms.date: 11/10/2017
ms.topic: article
keywords: Windows 10, uwp, 세계화, 지역화, 현지화, 오른쪽에서 왼쪽, 왼쪽에서 오른쪽,
ms.localizationpriority: medium
ms.openlocfilehash: 24e4c5dfce4aa3e773ab8c334ca732ac5ed53030
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5943877"
---
# <a name="design-your-app-for-bidirectional-text"></a>양방향 텍스트를 위한 앱 디자인

양방향(양방향 텍스트 지원)를 제공하도록 앱을 디자인하여 RTL(오른쪽에서 왼쪽) 및 LTR(왼쪽에서 오른쪽) 쓰기 시스템의 스크립트를 결합할 수 있도록 합니다. 이렇게 하면 일반적으로 다른 유형의 알파벳이 포함됩니다.

중동, 중앙 및 남아시아, 아프리카에서 사용되는 시스템과 같이 오른쪽에서 왼쪽 쓰기 시스템에는 고유한 디자인 요구 사항이 있습니다. 이러한 쓰기 시스템에는 양방향(양방향 텍스트 지원)가 필요합니다. 양방향 지원을 사용하면 RTL(오른쪽에서 왼쪽) 또는 LTR(왼쪽에서 오른쪽) 순으로 텍스트 레이아웃을 입력하고 표시할 수 있습니다.

Windows에는 총 9개의 양방향 언어가 포함되어 있습니다.
- 다음은 완전히 번역된 언어입니다. 아랍어 및 히브리어.
- 신흥 지역/시장에 대한 7개의 언어 인터페이스 팩은 다음과 같습니다. 이란어 우르두어, 다리어, 중앙 쿠르드어, 신디어, 펀잡어 (파키스탄) 및 위구르어.

이 항목에는 Windows 양방향 디자인 철학 및 양방향 디자인 고려 사항을 나타내는 사례 연구가 나와 있습니다.

## <a name="bidi-design-elements"></a>양방향 디자인 요소

4가지 요소가 Windows의 양방향 디자인 결정에 영향을 미칩니다.

- **UI(사용자 인터페이스) 미러링**. UI(사용자 인터페이스) 흐름을 사용하면 오른쪽에서 왼쪽 표기 콘텐츠가 기본 레이아웃으로 표시됩니다. UI 디자인이 양방향 시장에서 현지 언어와 가깝게 느껴집니다.
- **사용자 환경의 일관성**. 오른쪽에서 왼쪽 방향에 있어 디자인이 자연스럽게 느껴집니다. UI 요소가 일관된 레이아웃 방향을 사용하고 사용자가 기대하는 대로 표시됩니다.
- **터치 최적화**. 미러링되지 않은 UI의 터치 UI와 마찬가지로 요소는 접근하기 쉽고 터치 상호 작용에 자연스럽게 동작합니다.
- **혼합 텍스트 지원**. 텍스트 방향성 지원을 통해 혼합 텍스트를 훌륭하게 표현할 수 있습니다(양방향 빌드에서 영문 텍스트 및 영문 빌드에서 양방향 텍스트).

## <a name="feature-design-overview"></a>디자인 특징 개요

Windows는 4가지 양방향 디자인 요소를 지원합니다. Windows의 주요 관련 기능 몇 가지를 살펴보고 앱에 어떤 영향을 미치는지에 관한 몇 가지 컨텍스트를 알아보겠습니다.

### <a name="navigate-in-the-direction-that-feels-natural"></a>자연스럽게 느껴지는 방향 탐색

텍스트가 오른쪽에서 왼쪽으로 진행될 수 있도록 Windows는 인쇄 그리드의 방향을 조정합니다. 이렇게 하면 그리드의 첫 번째 파일이 오른쪽 상단에 위치하고, 마지막 타일이 왼쪽 하단에 위치하게 됩니다. 책, 잡지와 같은 인쇄된 출판물의 RTL 패턴과 일치하며, 읽기 패턴은 항상 오른쪽 상단 모서리에서 출발하여 왼쪽으로 진행됩니다.

![양방향 시작 메뉴](images/56283_BIDI_01_startscreen_resized.png)
![참이 포함된 양방향 시작 메뉴](images/56283_BIDI_02_startscreen_charm_resized.png)

일관된 UI 흐름을 유지하려면 타일의 콘텐츠가 오른쪽에서 왼쪽 레이아웃을 유지해야 합니다. 즉, 앱 UI 언어에 관계없이 앱 이름 및 로고가 타일의 오른쪽 하단에 위치해야 합니다.

#### <a name="bidi-tile"></a>양방향 타일

![양방향 타일](images/56284_BIDI_03_tile_callouts_withKey.png)

#### <a name="english-tile"></a>영어 타일

![영어 타일](images/56284_BIDI_03_tile_callouts_en-us.png)

### <a name="get-tile-notifications-that-read-correctly"></a>올바르게 읽기 위한 타일 알림 받기

타일은 혼합 텍스트를 지원합니다. 알림 지역에는 알림 언어를 기반으로 텍스트 정렬을 조정하기 위한 내장된 유연성이 포함되어 있습니다.  앱에서 아랍어, 히브리어 또는 다른 양방향 언어 알림을 보내는 경우 텍스트는 오른쪽으로 정렬됩니다. 영어(또는 기타 LTR) 알림이 도착하는 경우 왼쪽으로 정렬됩니다.

![타일 알림](images/56285_BIDI_04_bidirectional_tiles_white.png)

### <a name="a-consistent-easy-to-touch-rtl-user-experience"></a>일관되고 터치하기 쉬운 RTL 사용자 환경

Windows UI의 모든 요소는 RTL 방향에 적합합니다. 참 및 플라이아웃은 검색 결과와 겹치지 않고 터치 최적화를 저하시키지 않도록 화면의 왼쪽 가장자리에 위치됩니다. 엄지 손가락으로 쉽게 누를 수 있습니다.

![양방향 스크린샷](images/56286_BIDI_05_search_flyout_resized.png)
![양방향 스크린샷](images/56286_BIDI_06_print_flyout_resized.png)

![양방향 스크린샷](images/56286_BIDI_07_settings_flyout_resized.png)
![양방향 스크린샷](images/56286_BIDI_08_app_bars_resized.png)

### <a name="text-input-in-any-direction"></a>방향에 관계없이 텍스트 입력

Windows는 클러터가 없는 깔끔한 화면 터치 키보드를 제공합니다. 양방향 언어의 경우 텍스트 입력 방향을 필요에 따라 전환할 수 있도록 텍스트 방향 컨트롤 키가 있습니다.

![양방향 언어를 위한 터치 키보드](images/56287_BIDI_09_keyboard_layout_resized.png)

### <a name="use-any-app-in-any-language"></a>모든 언어에 모든 앱 사용

가장 좋아하는 앱을 어떤 언어로도 설치하고 사용할 수 있습니다. 앱은 Windows의 양방향이 아닌 버전에서와 같이 나타나고 작동합니다. 앱에 포함된 요소는 항상 일관되고 예측 가능한 위치에 놓입니다.

![양방향 콘텐츠를 보여주는 영어 앱](images/56288_BIDI_10_english_app_resized.png)

### <a name="display-parentheses-correctly"></a>올바른 괄호 표시

BPA(양방향 괄호 알고리즘)의 도입으로 쌍으로 이루어진 괄호는 언어 또는 텍스트 정렬 속성에 관계없이 항상 올바르게 표시됩니다.

#### <a name="incorrect-parentheses"></a>잘못된 괄호

![잘못된 괄호가 포함된 양방향 앱](images/56289_BIDI_11_parentheses_resized.png)

#### <a name="correct-parentheses"></a>올바른 괄호

![올바른 괄호가 포함된 양방향 앱](images/56289_BIDI_12_parentheses_fixed_resized.png)

### <a name="typography"></a>입력 체계

Windows는 모든 양방향 언어에 대해 맑은 고딕 UI를 사용합니다. 이 글꼴은 Windows UI에 맞게 모양이 형성되고 크기가 조정됩니다.

![양방향 언어를 위한 맑은 고딕 글꼴](images/56290_BIDI_13_start_screen_segoe.png)
![양방향 언어를 위한 맑은 고딕 글꼴](images/56290_BIDI_13_start_screen_segoe_arabic.png)

## <a name="case-study-1-a-bidi-music-app"></a>사례 1: 양방향 음악 앱

### <a name="overview"></a>개요

멀티미디어 앱은 양방향이 아닌 언어와 유사하게 일반적으로 왼쪽에서 오른쪽 레이아웃을 가지므로 디자인하기 꽤 까다롭습니다.

![왼쪽에서 오른쪽 미디어 컨트롤](images/56291_BIDI_1415_music_player_layouts_left-withcallouts.png)

![오른쪽에서 왼쪽 미디어 컨트롤](images/56291_BIDI_1415_music_player_layouts_right-withcallouts.png)

### <a name="establishing-ui-directionality"></a>UI 방향 설정

오른쪽에서 왼쪽 UI 흐름을 보존하는 것이 양방향 시장에 일관된 디자인 제공에 중요합니다. 뒤로 버튼과 같은 몇 가지 탐색 요소가 오디오 컨트롤의 뒤로 버튼의 기본 방향과 충돌할 수 있으므로 이 컨텍스트에서 왼쪽에서 오른쪽 흐름을 갖는 요소를 추가하는 것은 어렵습니다.

![음악 앱 추적 페이지](images/56292_BIDI_16_app_layout_callouts_resized.png)

이 음악 앱은 오른쪽에서 왼쪽 방향 그리드를 유지합니다. 이 방향은 Windows UI 간에 이미 이 방향으로 탐색하고 있는 사용자가 더욱 자연스럽게 느끼도록 합니다. 주 요소가 오른쪽에서 왼쪽으로 진행될 뿐만 아니라 섹션 헤더에서 올바르게 정렬되어 UI 흐름을 유지하는 데 도움을 주기 때문에 이 흐름을 유지합니다.

![음악 앱 앨범 페이지](images/56292_BIDI_17_app_layout_callouts_resized.png)

### <a name="text-handling"></a>텍스트 처리

위의 스크린샷에서 아티스트 약력은 왼쪽으로 정렬되어 있지만 앨범 및 트랙 이름과 같은 기타 아티스트 관련 텍스트 부분은 오른쪽 정렬로 유지되어 있습니다. 약력 필드는 다소 큰 텍스트 요소입니다. 더 넓은 텍스트 블록을 읽을 때에는 행간을 추적하기 어려우므로 오른쪽으로 정렬된 경우 가독성이 떨어집니다. 일반적으로 5자 이상의 단어를 포함하는 행이 2~3개 이상으로 구성된 텍스트 요소는 비슷한 정렬 예외 사항을 고려해야 합니다. 텍스트 블록 정렬은 전체 앱 양방향 레이아웃의 정렬과 반대입니다.

앱 간에 정렬을 조작하면 단순해 보일 수 있지만 양방향 문자열 간에 중립적인 문자 배치 면에서 렌더링 엔진의 경계와 제한에 직면할 수 있습니다. 예를 들어 다음 문자열은 정렬에 따라 다르게 표시될 수 있습니다.

| | 영어 문자열(LTR) | 히브리어 문자열(RTL) |
| -------------- | ------------------- | ------------------- |
| **왼쪽 맞춤** | Hello, World! | בוקר טוב! |
| **오른쪽 맞춤** | !Hello, World | בוקר טוב! |

음악 앱 간에 아티스트 정보가 올바르게 표시되도록 하려면 개발 팀은 텍스트 레이아웃 속성을 정렬과 분리해야 합니다. 즉, 대부분의 경우 아티스트 정보는 오른쪽 맞춤으로 표시될 수 있지만 문자열 레이아웃 조정은 사용자화된 백그라운드 처리를 기반으로 설정됩니다. 백그라운드 처리를 통해 문자열 콘텐츠를 기반으로 최적의 레이아웃 방향 설정을 결정합니다.

![음악 앱 아티스트 페이지](images/56292_BIDI_18_app_layout_callouts_resized.png)

예를 들어, 사용자 지정 문자열 레이아웃 처리가 없는 경우 아티스트 이름은 "The Contoso Band"입니다. ".The Contoso Band"로 나타날 수 있습니다.

### <a name="specialized-string-direction-preprocessing"></a>특수 문자열 방향 전처리

앱이 미디어 메타데이터를 위해 서버에 접속하는 경우 사용자에게 표시하기 전에 각 문자열을 전처리합니다. 이러한 전처리 과정에서 앱은 변환을 통해 텍스트 방향을 일관되게 만듭니다. 이렇게 하려면 문자열 끝에 중립 문자가 있는지 확인합니다. 문자열의 텍스트 방향이 Windows 언어 설정의 앱 방향과 반대인 경우 유니코드 방향 마커를 다음에 추가(때에 따라 앞에 추가)합니다. 변환 기능은 다음과 같습니다.

```csharp
string NormalizeTextDirection(string data) 
{
    if (data.Length > 0) {
        var lastCharacterDirection = DetectCharacterDirection(data[data.Length - 1]);

        // If the last character has strong directionality (direction is not null), then the text direction for the string is already consistent.
        if (!lastCharacterDirection) {
            // If the last character has no directionality (neutral character, direction is null), then we may need to add a direction marker to
            // ensure that the last character doesn't inherit directionality from the outside context.
            var appTextDirection = GetAppTextDirection(); // checks the <html> element's "dir" attribute.
            var dataTextDirection = DetectStringDirection(data); // Run through the string until a non-neutral character is encountered,
                                                                 // which determines the text direction.

            if (appTextDirection != dataTextDirection) {
                // Add a direction marker only if the data text runs opposite to the directionality of the app as a whole,
                // which would cause the neutral characters at the ends to flip.
                var directionMarkerCharacter =
                    dataTextDirection == TextDirections.RightToLeft ?
                        UnicodeDirectionMarkers.RightToLeftDirectionMarker : // "\u200F"
                        UnicodeDirectionMarkers.LeftToRightDirectionMarker; // "\u200E"

                data += directionMarkerCharacter;

                // Prepend the direction marker if the data text begins with a neutral character.
                var firstCharacterDirection = DetectCharacterDirection(data[0]);
                if (!firstCharacterDirection) {
                    data = directionMarkerCharacter + data;
                }
            }
        }
    }

    return data;
}
```

추가된 유니코드 문자를 너비가 0이므로 문자열 간격에 영향을 미치지 않습니다. 문자열의 방향 감지는 중립 문자가 아닌 문자를 만날 때까지 문자열을 계속 읽어야 하므로 이 코드는 잠재적으로 성능을 떨어뜨릴 수 있습니다. 중립성이 검사된 각 문자는 먼저 몇몇 유니코드 범위에 대해 비교되므로 간단한 검사가 아닙니다.

## <a name="case-study-2-a-bidi-mail-app"></a>사례 2: 양방향 메일 앱

### <a name="overview"></a>개요

UI 레이아웃 요구 사항과 관련하여 메일 클라이언트는 디자인하기가 간단합니다. Windows의 메일 앱은 기본적으로 미러링됩니다. 텍스트 처리 측면에서 메일 앱은 혼합된 텍스트 시나리오를 수용하기 위해 더욱 철저한 텍스트 표시 및 작성 기능이 요구됩니다.

### <a name="establishing-ui-directionality"></a>UI 방향 설정

메일 앱의 UI 레이아웃이 미러링됩니다. 폴더 패널이 화면의 오른쪽 가장자리에 배치되고, 메일 항목 목록 패널이 왼쪽에, 이메일 구성 패널이 배치되도록 3개 패널의 방향이 다시 정렬되었습니다.

![미러링된 메일 앱](images/56293_BIDI_19_icon_realignment_cropped_resized.png)

전체 UI 흐름을 일치시키고 터치가 최적화되도록 추가 항목의 방향이 조정되었습니다. 여기에는 앱 바와 작성, 회신, 삭제 아이콘이 포함되어 있습니다.

![앱 바가 있는 미러링된 메일 앱](images/56294_BIDI_20_email_orientation_email_resized.png)

### <a name="text-handling"></a>텍스트 처리

#### <a name="ui"></a>UI

UI 간의 텍스트 정렬은 일반적으로 오른쪽 맞춤입니다. 여기에는 폴더 패널과 항목 패널이 포함됩니다. 항목 패널은 두 줄 텍스트(주소 및 제목)로 제한됩니다. 콘텐츠의 방향이 UI 방향 흐름과 반대되는 경우 텍스트 블록이 읽기 어려워지는 문제가 발생하지 않도록 오른쪽에서 왼쪽 정렬을 유지하는 것이 중요합니다.

#### <a name="text-editing"></a>텍스트 편집

텍스트를 편집하려면 오른쪽에서 왼쪽 및 왼쪽에서 오른쪽 형식으로 작성할 수 있어야 합니다. 또한 구성 레이아웃은 양방향 정보를 저장할 수 있는 서식 있는 텍스트와 같은 형식을 사용하여 유지되어야 합니다.

![왼쪽에서 오른쪽 메일 앱](images/56294_BIDI_21_email_orientation_LtR_resized.png)

![오른쪽에서 왼쪽 메일 앱](images/56294_BIDI_22_email_orientation_RtL_resized.png)
