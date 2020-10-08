---
description: 응용 프로그램을 디자인 하 여 양방향 텍스트 (LTR)와 오른쪽에서 왼쪽 (RTL) 쓰기 시스템을 결합 하 여 일반적으로 다른 유형의 영문자를 포함 하는 스크립트를 결합할 수 있습니다.
title: 양방향 텍스트를 위한 앱 디자인
template: detail.hbs
ms.date: 11/10/2017
ms.topic: article
keywords: windows 10, uwp, 세계화, 지역화 가능성, 지역화, rtl, ltr
ms.localizationpriority: medium
ms.openlocfilehash: f09aa1ac2b56c83b502e54ce631e46d2f4054943
ms.sourcegitcommit: 4f032d7bb11ea98783db937feed0fa2b6f9950ef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2020
ms.locfileid: "91829611"
---
# <a name="design-your-app-for-bidirectional-text"></a>양방향 텍스트를 위한 앱 디자인

양방향 텍스트 지원 (양방향 텍스트)을 제공 하도록 앱을 디자인 합니다. 즉, 일반적으로 다른 유형의 영문자를 포함 하는 스크립트를 오른쪽에서 왼쪽 (RTL) 및 왼쪽에서 오른쪽 (LTR)으로 결합할 수 있습니다.

중동, 중부, 동남 아시아, 아프리카에서 사용 되는 것과 같은 오른쪽에서 왼쪽 쓰기 시스템에는 고유한 디자인 요구 사항이 있습니다. 이러한 쓰기 시스템은 양방향 텍스트 지원 (양방향 텍스트)을 요구 합니다. 양방향 텍스트 지원은 오른쪽에서 왼쪽 (RTL) 또는 왼쪽에서 오른쪽 (LTR) 순서로 텍스트 레이아웃을 입력 하 고 표시 하는 기능입니다.

Windows에는 총 9 개의 양방향 언어 언어가 포함 되어 있습니다.
- 완전히 지역화 된 두 언어 아랍어 및 히브리어
- 새로운 시장을 위한 7 가지 언어 인터페이스 팩 이란어, 우르두어, Dari, 중부 쿠르드어, 신디어, 펀잡어 (파키스탄) 및 위구르어.

이 항목에서는 양방향 디자인 고려 사항을 보여 주는 Windows 양방향 디자인 원칙 및 사례 연구를 포함 합니다.

## <a name="bidi-design-elements"></a>양방향 텍스트 디자인 요소

Windows에서는 네 가지 요소가 양방향 디자인 결정에 영향을 줍니다.

- **UI (사용자 인터페이스) 미러링**. UI (사용자 인터페이스) 흐름을 사용 하면 오른쪽에서 왼쪽으로 쓰는 콘텐츠를 기본 레이아웃으로 표시할 수 있습니다. UI 디자인은 양방향의 시장에 로컬인 것으로 판단 됩니다.
- **사용자 환경에서 일관성을 유지**합니다. 디자인은 오른쪽에서 왼쪽 방향으로 진행 됩니다. UI 요소는 일관 된 레이아웃 방향을 공유 하 고 사용자가 원하는 대로 표시 됩니다.
- **터치 최적화**. 비 미러된 UI의 터치 UI와 마찬가지로 요소는 쉽게 연결할 수 있으며 터치 상호 작용에 자연스럽 게 작용 합니다.
- **혼합 텍스트 지원**. 텍스트 방향 지원을 통해 뛰어난 혼합 텍스트 프레젠테이션 (양방향 빌드의 경우 영어 텍스트, 그 반대의 경우)을 사용할 수 있습니다.

## <a name="feature-design-overview"></a>기능 디자인 개요

Windows에서는 네 개의 양방향 디자인 요소를 지원 합니다. Windows의 주요 관련 기능 중 일부를 살펴보고 앱에 영향을 주는 방법에 대 한 몇 가지 컨텍스트를 제공 하겠습니다.

### <a name="navigate-in-the-direction-that-feels-natural"></a>일반적인 방향으로 탐색

Windows는 오른쪽에서 왼쪽으로 이동 하 여 표 형태의 첫 번째 타일이 오른쪽 위 모퉁이에 배치 되 고 마지막 타일이 왼쪽 아래에 오도록 하는 입력 표의 방향을 조정 합니다. 이는 책 및 잡지와 같이 인쇄 된 발행물의 RTL 패턴과 일치 합니다. 여기서 읽기 패턴은 항상 오른쪽 위 모퉁이에서 시작 하 여 왼쪽으로 진행 됩니다.

![양방향 텍스트 시작 메뉴 ](images/56283_BIDI_01_startscreen_resized.png)
 ![ 양방향 시작 메뉴 참 메뉴](images/56283_BIDI_02_startscreen_charm_resized.png)

일관 된 UI 흐름을 유지 하기 위해 타일의 콘텐츠는 오른쪽에서 왼쪽으로 레이아웃을 유지 합니다. 즉, 앱 UI 언어에 관계 없이 응용 프로그램 이름 및 로고가 타일의 오른쪽 아래 모퉁이에 배치 됩니다.

#### <a name="bidi-tile"></a>양방향 텍스트 타일

![양방향 텍스트 타일](images/56284_BIDI_03_tile_callouts_withKey.png)

#### <a name="english-tile"></a>영어 타일

![영어 타일](images/56284_BIDI_03_tile_callouts_en-us.png)

### <a name="get-tile-notifications-that-read-correctly"></a>올바르게 읽은 타일 알림 가져오기

타일에는 혼합 된 텍스트 지원이 있습니다. 알림 영역에는 알림 언어를 기준으로 텍스트 맞춤을 조정할 수 있는 유연성이 내장 되어 있습니다.  앱에서 아랍어, 히브리어 또는 기타 양방향 언어 알림을 보낼 때 텍스트가 오른쪽에 맞춰집니다. 또한 영어 (또는 다른 LTR) 알림이 도착할 때 왼쪽에 정렬 됩니다.

![타일 알림](images/56285_BIDI_04_bidirectional_tiles_white.png)

### <a name="a-consistent-easy-to-touch-rtl-user-experience"></a>일관적이 고 터치 RTL 사용자 환경

Windows UI의 모든 요소는 RTL 방향에 맞춰집니다. 참 메뉴 및 flyouts는 검색 결과와 겹치지 않거나 터치 최적화가 저하 되지 않도록 화면 왼쪽 가장자리에 배치 되었습니다. 엄지 손가락으로 쉽게 연결할 수 있습니다.

![](images/56286_BIDI_05_search_flyout_resized.png)
 ![ 인쇄 플라이 아웃의 크기를 보여 주는 양방향 텍스트의 크기를 조정 하는 검색 플라이 아웃의 스크린샷 보여 주는 양방향 텍스트](images/56286_BIDI_06_print_flyout_resized.png)

![설정 플라이 아웃을 보여 주는 양방향 텍스트의 스크린샷 크기 ](images/56286_BIDI_07_settings_flyout_resized.png)
 ![ 조정 된 앱 바를 보여 주는 양방향 텍스트 스크린샷](images/56286_BIDI_08_app_bars_resized.png)

### <a name="text-input-in-any-direction"></a>모든 방향의 텍스트 입력

Windows는 깨끗 하 고 간단 하 게 사용할 수 있는 화상 터치 키보드를 제공 합니다. 양방향 언어의 경우 텍스트 입력 방향을 필요에 따라 전환할 수 있도록 텍스트 방향 제어 키가 있습니다.

![양방향 언어에 대 한 터치 키보드](images/56287_BIDI_09_keyboard_layout_resized.png)

### <a name="use-any-app-in-any-language"></a>모든 언어로 된 앱 사용

원하는 언어로 즐겨 사용 하는 앱을 설치 하 고 사용 합니다. 앱은 비 양방향 Windows 버전에서와 같이 표시 되 고 작동 합니다. 앱 내의 요소는 항상 일관 되 고 예측 가능한 위치에 배치 됩니다.

![양방향 콘텐츠를 표시 하는 영어 앱](images/56288_BIDI_10_english_app_resized.png)

### <a name="display-parentheses-correctly"></a>괄호를 올바르게 표시 합니다.

BPA (양방향 괄호 알고리즘)의 도입으로, 쌍을 이루는 괄호는 언어 또는 텍스트 맞춤 속성에 관계 없이 항상 제대로 표시 됩니다.

#### <a name="incorrect-parentheses"></a>괄호가 잘못 되었습니다.

![괄호가 잘못 된 양방향 텍스트 앱](images/56289_BIDI_11_parentheses_resized.png)

#### <a name="correct-parentheses"></a>올바른 괄호

![괄호가 맞는 양방향 텍스트 앱](images/56289_BIDI_12_parentheses_fixed_resized.png)

### <a name="typography"></a>입력 체계

Windows에서는 모든 양방향 언어에 대 한 맑은 고딕 글꼴을 사용 합니다. 이 글꼴은 Windows UI의 모양이 지정 되 고 크기가 조정 됩니다.

![](images/56290_BIDI_13_start_screen_segoe.png)
 ![ 시작 화면에서 Segoe 아랍어 글꼴을 보여 주는 시작 화면 스크린샷 맑은 고딕 글꼴을 보여 주는 스크린샷](images/56290_BIDI_13_start_screen_segoe_arabic.png)

## <a name="case-study-1-a-bidi-music-app"></a>사례 연구 #1: 양방향 음악 앱

### <a name="overview"></a>개요

미디어 컨트롤은 일반적으로 비 양방향 언어와 유사한 왼쪽에서 오른쪽 레이아웃으로 예상 되기 때문에 멀티미디어 앱은 매우 흥미로운 디자인 과제를 제공 합니다.

![미디어 컨트롤 (왼쪽에서 오른쪽으로)](images/56291_BIDI_1415_music_player_layouts_left-withcallouts.png)

![오른쪽에서 왼쪽으로 미디어 컨트롤](images/56291_BIDI_1415_music_player_layouts_right-withcallouts.png)

### <a name="establishing-ui-directionality"></a>UI 방향 설정

오른쪽에서 왼쪽으로 쓰는 UI 흐름을 유지 하는 것은 양방향의 시장을 위한 일관 된 디자인에 중요 합니다. 뒤로 단추와 같은 일부 탐색 요소가 오디오 컨트롤에서 뒤로 단추의 방향 방향과 일치 하지 않을 수 있기 때문에이 컨텍스트 내에서 왼쪽에서 오른쪽 흐름이 있는 요소를 추가 하는 것은 어렵습니다.

![음악 앱 트랙 페이지](images/56292_BIDI_16_app_layout_callouts_resized.png)

이 음악 앱은 오른쪽에서 왼쪽으로 향하는 표를 유지 합니다. 이렇게 하면 Windows UI에서이 방향으로 이동 하는 사용자에 게 매우 자연 스러운 느낌을 줄 수 있습니다. 기본 요소가 오른쪽에서 왼쪽으로 정렬 되지 않고 UI 흐름을 유지 하기 위해 섹션 헤더에 제대로 정렬 되어 있는지 확인 하는 방식으로 흐름이 보존 됩니다.

![음악 앱 앨범 페이지](images/56292_BIDI_17_app_layout_callouts_resized.png)

### <a name="text-handling"></a>텍스트 처리

위의 스크린샷에 있는 음악가의 경력은 왼쪽 맞춤 이지만 앨범 및 트랙 이름과 같은 다른 음악가 관련 텍스트 부분은 오른쪽 맞춤을 유지 합니다. Bio 필드는 매우 큰 텍스트 요소입니다 .이 요소는 더 넓은 텍스트 블록을 읽는 동안 줄 사이를 추적 하기는 어려울 수 있으므로 오른쪽에 정렬 하는 경우에는 잘 읽을 수 있습니다. 일반적으로 5 개 이상의 단어를 포함 하는 세 개 이상의 줄을 포함 하는 모든 텍스트 요소는 유사한 맞춤 예외에 대해 고려해 야 합니다. 여기서 텍스트 블록 맞춤은 전체 앱 방향 레이아웃의 반대와 반대입니다.

앱 전체의 맞춤을 조작 하는 것은 간단 하지만 종종 양방향 문자열의 중립 문자 배치 측면에서 렌더링 엔진의 경계 및 제한 사항을 노출 합니다. 예를 들어 다음 문자열은 맞춤에 따라 다르게 표시 될 수 있습니다.

| | 영어 문자열 (LTR) | 히브리어 문자열 (RTL) |
| -------------- | ------------------- | ------------------- |
| **왼쪽 맞춤** | Hello, World! | בוקר טוב! |
| **오른쪽 맞춤** | ! Hello World | !בוקר טוב |

음악가 정보가 음악 앱 전체에 제대로 표시 되도록 개발 팀은 맞춤에서 텍스트 레이아웃 속성을 분리 했습니다. 즉, 대부분의 경우 아티스트 정보는 오른쪽 맞춤으로 표시 될 수 있지만 사용자 지정 된 백그라운드 처리에 따라 문자열 레이아웃 조정이 설정 됩니다. 백그라운드 처리는 문자열의 내용에 따라 가장 적합 한 방향 레이아웃 설정을 결정 합니다.

![음악 앱 음악가 페이지](images/56292_BIDI_18_app_layout_callouts_resized.png)

예를 들어 사용자 지정 문자열 레이아웃 처리를 사용 하지 않고 음악가 이름 "Contoso 대역"을 사용 합니다. "로 표시 됩니다. Contoso 대역 ".

### <a name="specialized-string-direction-preprocessing"></a>특수 문자열 방향 전처리

앱은 미디어 메타 데이터를 서버에 연결 하는 경우 사용자에 게 표시 하기 전에 각 문자열을 전처리 합니다. 이 전처리를 수행 하는 동안 앱은 텍스트 방향이 일치 하도록 변환도 수행 합니다. 이렇게 하려면 문자열의 끝에 중립 문자가 있는지 여부를 확인 합니다. 또한 문자열의 텍스트 방향이 Windows 언어 설정에 설정 된 응용 프로그램 방향과 반대 되는 경우에는 유니코드 방향 표식을 추가 (및 추가) 합니다. 변환 함수는 다음과 같습니다.

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

추가 된 유니코드 문자는 너비가 0 이므로 문자열의 간격에 영향을 주지 않습니다. 이 코드는 문자열의 방향을 검색 하기 위해 중립 문자가 아닌 문자가 나타날 때까지 문자열을 통해 실행 해야 하기 때문에 잠재적인 성능 저하를 발생 시킵니다. 중립성에 대해 확인 된 각 문자는 먼저 여러 유니코드 범위와 비교 되므로 간단한 검사를 수행 하지 않습니다.

## <a name="case-study-2-a-bidi-mail-app"></a>사례 연구 #2: 양방향 메일 앱

### <a name="overview"></a>개요

UI 레이아웃 요구 사항 측면에서 메일 클라이언트는 매우 간단 하 게 디자인할 수 있습니다. Windows의 메일 앱은 기본적으로 미러링됩니다. 텍스트 처리 관점에서 볼 때 메일 앱은 혼합 텍스트 시나리오를 수용 하기 위해 더 강력한 텍스트 표시 및 컴퍼지션 기능을 제공 해야 합니다.

### <a name="establishing-ui-directionality"></a>UI 방향 설정

메일 앱에 대 한 UI 레이아웃이 미러링됩니다. 폴더 창이 화면 오른쪽 가장자리에 배치 되 고 왼쪽에 메일 항목 목록 창이 오고 나 서 전자 메일 컴퍼지션 창이 방향이 다시 지정 세 개의 창이 표시 됩니다.

![미러된 메일 응용 프로그램](images/56293_BIDI_19_icon_realignment_cropped_resized.png)

전체 UI 흐름과 터치 최적화를 일치 시키기 위해 추가 항목이 방향이 다시 지정 되었습니다. 여기에는 앱 표시줄과 작성, 회신 및 삭제 아이콘이 포함 됩니다.

![앱 바를 사용 하는 미러된 메일 앱](images/56294_BIDI_20_email_orientation_email_resized.png)

### <a name="text-handling"></a>텍스트 처리

#### <a name="ui"></a>UI

UI 전체의 텍스트 맞춤은 일반적으로 오른쪽 맞춤입니다. 여기에는 폴더 창 및 항목 창이 포함 됩니다. 항목 창은 두 줄의 텍스트 (주소 및 제목)로 제한 됩니다. 이는 콘텐츠 방향이 UI 방향 흐름과 반대 될 때 읽기 어려운 텍스트 블록을 도입 하지 않고 오른쪽에서 왼쪽 맞춤을 유지 하는 데 중요 합니다.

#### <a name="text-editing"></a>텍스트 편집

텍스트를 편집 하려면 오른쪽에서 왼쪽으로 및 왼쪽에서 오른쪽 형식으로 작성 하는 기능이 필요 합니다. 또한 &mdash; &mdash; 방향 정보를 저장할 수 있는 서식 있는 텍스트와 같은 형식을 사용 하 여 컴퍼지션 레이아웃을 유지 해야 합니다.

![왼쪽에서 오른쪽으로 메일 앱](images/56294_BIDI_21_email_orientation_LtR_resized.png)

![메일 앱 오른쪽에서 왼쪽으로](images/56294_BIDI_22_email_orientation_RtL_resized.png)
