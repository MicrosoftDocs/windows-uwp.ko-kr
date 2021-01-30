---
title: Cortana 디자인 지침-Cortana UWP 디자인 및 개발
description: 이러한 지침 및 권장 사항은 앱에서 Cortana를 사용 하 여 사용자와 상호 작용 하는 방법을 설명 합니다.
ms.assetid: 332ccb95-0e56-410e-ab63-cc028fce4192
label: Cortana
template: detail.hbs
ms.date: 01/27/2021
ms.topic: article
keywords: cortana, 디자인
ms.localizationpriority: medium
ms.openlocfilehash: 0d1f27c2e70ce8bf9d77f07dd0871cf09a441bdc
ms.sourcegitcommit: d7efd35c1749f695aebbc0db99d8b62b70fb72da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99057574"
---
# <a name="cortana-design-guidelines"></a>Cortana 디자인 지침

>[!WARNING]
> 이 기능은 Windows 10 5 월 2020 업데이트 (버전 2004, 코드명 "20H1")에서 더 이상 지원 되지 않습니다.

이러한 지침 및 권장 사항은 앱에서 **Cortana** 를 사용 하 여 사용자와 상호 작용 하 고 작업을 수행 하는 데 도움이 되는 방법을 설명 하 고 모든 상황을 명확 하 게 전달 하는 방법을 설명 합니다.

**Cortana** 를 사용 하면 백그라운드에서 실행 중인 응용 프로그램에서 사용자에 게 확인 또는 명확성을 묻는 메시지를 표시 하 고, 반환에서 음성 명령의 상태에 대 한 피드백을 사용자에 게 제공할 수 있습니다. 프로세스는 간단 하 고 신속 하며 사용자가 **Cortana** 환경을 유지 하거나 컨텍스트를 응용 프로그램으로 전환 하지 않습니다.

사용자는 **cortana** 를 사용 하 여 프로세스를 간단 하 고 가능한 한 간단 하 게 만들 수 있는 반면 **cortana** 는 앱이 작업을 수행 하는 것을 명시적으로 사용할 수도 있습니다.

여기에 나와 있는 **놀이 Works** 라는 여행 계획 및 관리 앱을 사용  하 여 설명 하는 다양 한 개념 및 기능을 보여 줍니다. 자세한 내용은 [Cortana 음성 명령 샘플](https://go.microsoft.com/fwlink/p/?LinkID=619899)을 참조 하세요.

:::image type="content" source="images/cortana/cortana-overview.png" alt-text="Cortana 캔버스의 스크린샷":::

## <a name="conversational-writing"></a>대화형 쓰기

**Cortana** 를 성공적으로 조작 하려면 TTS (텍스트 음성 변환) 및 GUI 문자열을 사용할 때 몇 가지 기본 원칙을 따라야 합니다.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">원칙</th>
<th align="left">잘못 된 예제</th>
<th align="left">좋은 예제</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p></p>
<dl>
<dt>떨어지는</dt>
<dd><p>가능한 한 적은 단어를 사용 하 고 가장 중요 한 정보를 맨 앞에 배치 합니다.</p>
</dd>
</dl></td>
<td align="left"><p>무엇을 할 수 있나요? 오늘 어떤 동영상을 검색 하 시겠습니까? 많은 컬렉션이 있습니다.</p></td>
<td align="left"><p>어떤 영화를 찾고 있나요?</p></td>
</tr>
<tr class="even">
<td align="left"><p></p>
<dl>
<dt>관계가</dt>
<dd><p>작업, 콘텐츠 및 컨텍스트에서만 관련 된 정보를 제공 합니다.</p>
</dd>
</dl></td>
<td align="left"><p>이를 재생 목록에 추가 했습니다. 이 처럼 배터리가 부족 합니다.</p></td>
<td align="left"><p>이를 재생 목록에 추가 했습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p></p>
<dl>
<dt>해제</dt>
<dd><p>모호성을 방지 합니다. 기술 전문 용어 대신 일상 언어를 사용 합니다.</p>
</dd>
</dl></td>
<td align="left"><p>Las Vegas에 대 한 쿼리 트립 결과가 없습니다 &quot; &quot; .</p></td>
<td align="left"><p>Las Vegas에 대 한 여행을 찾을 수 없습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p></p>
<dl>
<dt>신뢰성이 </dt>
<dd><p>가능한 한 정확 하 게 사용할 수 있습니다. 백그라운드에서 진행 되는 작업에 대해 투명 하 게 수행 합니다. 아직 작업을 완료 하지 않은 경우에는이 작업을 수행 하지 않습니다. 개인 정보에 대해 자세히 알아보세요.</p>
</dd>
</dl></td>
<td align="left"><p>해당 동영상을 찾을 수 없는 경우 아직 릴리스되지 않아야 합니다.</p></td>
<td align="left"><p>카탈로그에서 해당 영화를 찾을 수 없습니다.</p></td>
</tr>
</tbody>
</table>

사람들에 게 말하는 방법을 씁니다. 자연 스러운 문법적으로는 문법적 정확도를 강조 하지 않습니다. 예를 들어, "원하지 않음" 또는 "달리기"와 같은 귀 모양의 바로 가기는 TTS 읽기를 위해 적절 합니다.

가능 하면 내재 된 첫 번째 사용자 시제를 사용 합니다. 예를 들어 "다음 놀이 놀이를 찾는 중"은 다른 사람이 찾고 있음을 의미 하지만, "I" 단어를 사용 하 여를 지정 하지는 않습니다.

약간의 변형을 사용 하 여 앱 소리를 보다 자연스럽 게 만들 수 있도록 합니다. 서로 다른 버전의 TTS 및 GUI 문자열을 제공 하 여 같은 작업을 효과적으로 수행 하세요. 예를 들어 "어떤 동영상을 표시 하 시겠습니까?" "시청 하려는 동영상은 무엇 인가요?"와 같은 대안이 있을 수 있습니다. 사용자는 매번 정확히 동일한 방식으로 동일한 것으로 말할 수 없습니다. TTS와 GUI 버전을 동기화 상태로 유지 해야 합니다.

응답에 "확인" 및 "좋은" 같은 문구를 신중 하 게 사용 합니다. 승인 및 진행률을 제공할 수 있지만 변형 없이 너무 자주 사용 하는 경우에도 반복적으로 사용할 수 있습니다.

> [!NOTE]
> TTS 에서만 승인 문구를 사용 합니다. **Cortana** 캔버스의 공간이 제한 되어 있으므로 해당 GUI 문자열에서 반복 하지 마세요.

**Cortana** 캔버스에 보다 자연스럽 게 상호 작용 하 고 추가 공간을 절약 하려면 응답에 축약를 사용 합니다. 예를 들어 "영화를 찾을 수 없습니다." 대신 "영화를 찾을 수 없습니다." 눈이 아닌 귀를 위해 씁니다.

시스템에서 인식 하는 언어를 사용 합니다. 사용자는 표시 된 용어를 반복 하는 경향이 있습니다. 표시 되는 내용을 확인 합니다.

대체 응답의 컬렉션에서 임의로 선택 하 여 응답에 일부 변형을 사용 합니다. 예를 들어 "어떤 동영상을 표시 하 시겠습니까?" "어떤 필름을 시청 하 시겠습니까?" 이렇게 하면 앱 소리를 보다 자연스럽 게 만들 수 있습니다.

## <a name="localization"></a>지역화

음성 명령을 사용 하 여 작업을 시작 하려면 앱은 사용자가 장치에서 선택한 언어로 음성 명령을 등록 해야 합니다 (설정 &gt; 시스템 &gt; 음성 &gt; 음성 언어).

앱이 응답 하는 음성 명령과 모든 TTS 및 GUI 문자열을 지역화 해야 합니다.

긴 GUI 문자열을 사용 하지 않는 것이 좋습니다. **Cortana** 캔버스는 응답에 대해 3 개의 줄을 제공 하 고 그 보다 긴 문자열을 자릅니다.

자세한 내용은 [세계화 및 지역화 섹션](/windows/uwp/design/globalizing/guidelines-and-checklist-for-globalizing-your-app)을 참조 하세요.

## <a name="image-resources-and-scaling"></a>이미지 리소스 및 크기 조정

UWP (유니버설 Windows 플랫폼) 앱은 특정 설정 및 장치 기능 (고대비, 유효 픽셀, 로캘 등)에 따라 가장 적합 한 앱 로고 이미지를 자동으로 선택할 수 있습니다. 이미지를 제공 하기만 하면 응용 프로그램 프로젝트 내에서 다른 리소스 버전에 대 한 적절 한 명명 규칙 및 폴더 조직을 사용 하 게 됩니다. 권장 리소스 버전을 제공 하지 않으면 사용자의 기본 설정, 기능, 장치 유형 및 위치에 따라 접근성, 지역화 및 이미지 품질이 저하 될 수 있습니다.

고대비 및 배율 인수에 대 한 이미지 리소스에 대 한 자세한 내용은 [타일 및 아이콘 자산에 대 한 지침](/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast)을 참조 하세요.

한정자를 사용 하 여 리소스 이름을 표시 합니다. 리소스 한정자는 특정 버전의 리소스를 사용 해야 하는 컨텍스트를 식별 하는 폴더 및 파일 이름 한정자입니다.

표준 명명 규칙은 "foldername/qualifiername-value \[ \_ qualifiername \] /filename.qualifiername-value \[ \_ qualifiername-value \] . ext"입니다. 예: images/100 사이의 \_contrast-white.png은 단순히 루트 폴더와 파일 이름: images/logo.png를 사용 하 여 코드에서 참조 됩니다. [언어 및 지역 관리](/windows/uwp/design/globalizing/manage-language-and-region) 및 [한정자를 사용 하 여 리소스 이름을 결정 하는 방법을](/previous-versions/windows/apps/hh965324(v=win.10))참조 하세요.

현재 지역화 된 또는 여러 해결 리소스를 제공 하지 않는 경우에도 문자열 리소스 파일 (예: "en-us \\ 리소스. resw") 및 이미지 (예: "logo.scale-100.png")의 기본 배율 인수에 대 한 기본 언어를 표시 하는 것이 좋습니다. 그러나 최소 100, 200 및 400 배율 인수에 대 한 자산을 제공 하는 것이 좋습니다.

> [!IMPORTANT]
> Cortana 캔버스의 제목 영역에 사용 되는 앱 아이콘은 "appxmanifest.xml" 파일에 지정 된 Square44x44Logo 아이콘입니다.

사용자 쿼리의 각 결과 타일에 대 한 아이콘을 지정할 수도 있습니다. 결과 아이콘의 올바른 이미지 크기는 다음과 같습니다.

- 68w x 68h
- 68w x 92h
- 280w x 140h

## <a name="result-tile-templates"></a>결과 타일 템플릿

Cortana 캔버스에 표시 되는 결과 타일에 대 한 템플릿 집합이 제공 됩니다. 이러한 템플릿을 사용 하 여 타일 제목을 지정 하 고 타일에 텍스트와 결과 아이콘 이미지가 포함 되는지 여부를 지정 합니다. 각 타일에는 지정 된 템플릿에 따라 최대 3 줄의 텍스트와 하나의 이미지가 포함 될 수 있습니다.

다음은 지원 되는 템플릿 (예:)입니다.

| 이름 | 예제 |
| --- | --- |
| 제목만  | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titleonly-small.png" alt-text="제목만 표시 하는 Cortana 캔버스의 스크린샷"::: |
| 텍스트가 있는 제목 | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titlewithtext-small.png" alt-text="텍스트를 사용 하 여 제목을 표시 하는 Cortana 캔버스의 스크린샷"::: |
| 68x68 아이콘이 있는 제목 | 이미지 없음 |
| 68x68 아이콘 및 텍스트가 있는 제목 | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titlewith68x68iconandtext-small.png" alt-text="68x68 아이콘 및 텍스트를 사용 하 여 제목을 표시 하는 Cortana 캔버스의 스크린샷"::: |
| 68x92 아이콘이 있는 제목 | 이미지 없음 |
| 68x92 아이콘 및 텍스트가 있는 제목 | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titlewith68x92iconandtext-small.png" alt-text="68x92 아이콘 및 텍스트를 사용 하 여 제목을 표시 하는 Cortana 캔버스의 스크린샷"::: |
| 280x140 아이콘이 있는 제목 | 이미지 없음 |
| 280x140 아이콘 및 텍스트를 포함 하는 제목 | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titlewith280x140iconandtext-small.png" alt-text="280x140 아이콘과 텍스트를 표시 하는 Cortana 캔버스의 스크린샷"::: |

Cortana 템플릿에 대 한 자세한 정보는 [VoiceCommandContentTileType](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTileType) 를 참조 하세요.

## <a name="example"></a>예제

이 예제에서는 **Cortana** 의 백그라운드 앱에 대 한 종단 간 작업 흐름을 보여 줍니다. Las Vegas에 대 한 여행을 취소 하기 위해 **놀이 Works** 앱을 사용 하 고 있습니다. 이 예제에서는 "제목에 68x68 아이콘 및 텍스트" 템플릿이 사용 됩니다.

:::image type="content" source="images/cortana/e2e-canceltrip.png" alt-text="종단 간 Cortana 캔버스 백그라운드 앱 흐름에 대 한 Cortana 캔버스 스크린샷":::

이 이미지에 설명 된 단계는 다음과 같습니다.

1. 사용자가 마이크를 탭 하 여 **Cortana** 를 시작 합니다.
2. 사용자가 백그라운드에서 **놀이 works** 앱을 시작 하는 "Vegas에 대 한 놀이 동 여행 취소" 라고 표시 됩니다. 앱은 **Cortana** 음성 및 캔버스를 모두 사용 하 여 사용자와 상호 작용 합니다.
3. **Cortana** 는 사용자 승인 피드백을 제공 하는 전달 화면으로 전환 됩니다 ("해당 하는 놀이를 받고 싶습니다."), 상태 표시줄 및 취소 단추를 제공 합니다.
4. 이 경우 사용자에 게 쿼리와 일치 하는 여러 번의 라운드트립이 있으므로 앱은 일치 하는 결과를 모두 나열 하 고 "취소 하려는 항목"을 묻는 명확성 화면을 제공 합니다.
5. 사용자는 "Vegas Tech 컨퍼런스" 항목을 지정 합니다.
6. 취소를 취소할 수 없으므로 앱은 사용자가 의도를 확인 하도록 요청 하는 확인 화면을 제공 합니다.
7. 사용자에 게 "예" 라고 표시 됩니다.
8. 그러면 앱이 작업의 결과를 보여 주는 완료 화면을 제공 합니다.

이러한 단계는 여기에서 자세히 살펴봅니다.

### <a name="handoff"></a>전달

핸드 오프를 제공 하지 않는 *여행 adventureworks "예정 된 여행"* :::image type="content" source="images/cortana/cortana-backgroundapp-result.png" alt-text="없이 adventureworks를 사용 하 여 종단 간 cortana 백그라운드 앱 흐름에 대 한 Cortana 캔버스의 스크린샷":::

:::image type="content" source="images/cortana/cortana-backgroundapp-progress-result.png" alt-text="종단 간 cortana 캔버스의 스크린샷 스크린샷을 사용 하 여 향후 전달":::*adventureworks "예정 된 여행" 여행 화면*

앱에서 응답 하 고 사용자가 추가 정보를 요구 하지 않는 것 보다 작은 작업은 완료 화면을 표시 하는 것 외에 **Cortana** 의 추가 참여가 없어도 완료할 수 있습니다.

응용 프로그램에서 응답 하는 500ms 이상이 필요한 경우 **Cortana** 는 핸드 오프 화면을 제공 합니다. 앱 아이콘 및 이름이 표시 되 고, GUI와 TTS 전달 문자열을 모두 제공 하 여 음성 명령이 올바르게 인식 되었음을 나타내야 합니다. 전달 화면은 최대 5 초 동안 표시 됩니다. 앱이이 시간 내에 응답 하지 않으면 **Cortana** 는 일반 오류 화면을 표시 합니다.

### <a name="gui-and-tts-guidelines-for-handoff-screens"></a>핸드 오프 화면에 대 한 GUI 및 TTS 지침

작업이 진행 중임을 명확 하 게 표시 합니다.

현재 시제를 사용 합니다.

시작 되는 작업을 확인 하 고 특정 엔터티를 참조 하는 작업 동사를 사용 합니다.

요청 된 불완전 한 작업에 커밋하지 않는 제네릭 동사를 사용 합니다. 예를 들어 "여행 취소" 대신 "여행을 찾고 있습니다." 이 경우 결과가 반환 되지 않으면 사용자는 "Las Vegas으로의 여행 취소 중 ... Las Vegas에 대 한 여행을 찾을 수 없습니다. "

앱에서 요청 된 엔터티를 계속 확인 해야 하는 경우에는 작업이 아직 수행 되지 않은 것입니다. 예를 들어, 0 개 이상의 이동이 일치할 수 있으며 결과를 아직 알 수 없기 때문에 "여행 취소" 대신 "여행 검색" 이라고 말할 수 있습니다.

GUI와 TTS 문자열은 동일할 수 있지만 반드시 같을 필요는 없습니다. 다른 시각적 자산의 잘림 및 중복을 방지 하기 위해 GUI 문자열을 짧게 유지 합니다.

| TTS | GUI |
| --- | --- |
| 다음 어드벤처 여행을 찾고 있습니다. | 다음 여행 검색 중 ... |
| 도시에 대 한 놀이 여행 여행 검색 | 도시를 벗어난 여행 검색 중 ... |

### <a name="progress"></a>진행률

:::image type="content" source="images/cortana/e2e-canceltrip-progress.png" alt-text="종단 간 cortana 캔버스의 스크린샷 adventureworks 취소 여행 진행률":::*Adventureworks "여행 취소" 진행률*

작업을 수행 하는 동안 단계 사이에서 작업을 수행 하 고 진행률 화면에서 발생 하는 상황에 대 한 사용자를 업데이트 해야 합니다. 앱 아이콘이 표시 되 고 GUI와 TTS 진행률 문자열을 모두 제공 하 여 작업이 진행 되 고 있음을 나타내야 합니다.

앱을 적절 한 상태로 시작 하려면 시작 매개 변수를 사용 하 여 앱에 대 한 링크를 제공 해야 합니다. 이렇게 하면 사용자가 작업을 직접 보거나 완료할 수 있습니다. **Cortana** 는 링크 텍스트 (예: "놀이 Works로 이동")를 제공 합니다.

진행률 화면은 각각 5 초 동안 표시 됩니다. 그 후에는 다른 화면이 표시 되어야 합니다. 그렇지 않으면 작업이 시간 초과 됩니다.

이러한 화면은 진행률 화면을 따를 수 있습니다.

- 진행률
- 확인 (명시적, 나중에 설명)
- 명확성
- Completion

### <a name="gui-and-tts-guidelines-for-progress-screens"></a>진행률 화면에 대 한 GUI 및 TTS 지침

현재 시제를 사용 합니다.

작업이 진행 중임을 확인 하는 작업 동사를 사용 합니다.

**GUI**: 엔터티가 표시 되는 경우이에 대 한 참조 ("이 여행 취소 중 ...")를 사용 합니다. 엔터티가 표시 되지 않으면 엔터티를 명시적으로 호출 합니다 ("Vegas Tech 컨퍼런스 '" 취소).

**Tts**: 첫 번째 진행률 화면에는 TTS 문자열만 포함 해야 합니다. 추가 진행률 화면이 필요한 경우에는 빈 문자열인 {} 를 TTS 문자열로 보내고 GUI 문자열만 제공 합니다.

| 조건  | TTS | GUI |
| --- | --- | --- |
| 표시에 표시 된 이전 턴/엔터티에서 읽은 엔터티     | 이 여행 취소 중 ...          | 이 여행 취소 중 ...          |
| 표시에 표시 된 이전 턴/엔터티에서 엔터티를 읽지 않았습니다. | Vegas에 대 한 여행을 취소 하는 중 ... | 이 여행 취소 중 ...          |
| 이전 턴/엔터티는 표시 되지 않은 엔터티는 읽지 않음        | Vegas에 대 한 여행을 취소 하는 중 ... | Vegas에 대 한 여행을 취소 하는 중 ... |

### <a name="confirmation"></a>확인

:::image type="content" source="images/cortana/e2e-canceltrip-confirmation.png" alt-text="종단 간 cortana 캔버스의 스크린샷 AdventureWorks 취소 여행 확인":::*Adventureworks "여행 취소" 확인*

일부 작업은 사용자 명령의 특성에 따라 암시적으로 확인 될 수 있습니다. 다른 항목은 잠재적으로 더 중요 하며 명시적 확인이 필요 합니다. 다음은 명시적 및 암시적 확인을 사용 하는 경우에 대 한 몇 가지 지침입니다.

확인 화면에서 GUI와 TTS 문자열이 모두 앱에서 지정 되 고, 제공 된 경우 앱 아이콘 (제공 된 경우)이 **Cortana** 아바타 대신 표시 됩니다.

고객이 확인에 응답 한 후에는 응용 프로그램이 진행률 화면으로 이동 하지 않도록 500 밀리초 내에 다음 화면을 제공 해야 합니다.

명시적으로 다음을 사용 합니다.

- 콘텐츠는 사용자 (예: 문자 메시지, 전자 메일 또는 소셜 게시물)를 벗어납니다.
- 작업을 실행 취소할 수 없습니다 (예: 구매 또는 항목 삭제).
- 결과는 당혹 수 있습니다 (예: 잘못 된 사람 호출).
- 더 복잡 한 인식이 필요 합니다 (예: 여는 끝 기록).

암시적 경우 사용 ...

- 콘텐츠는 사용자에 대해서만 저장 됩니다 (예: 메모 대 자체).
- 쉽게 백업 하는 방법 (예: 경보 설정 또는 해제)이 있습니다.
- 작업은 신속 하 게 수행 해야 합니다 (예: 잊지 않기 전에 아이디어를 빠르게 캡처하기).
- 정확도가 높습니다 (예: 간단한 메뉴).

### <a name="gui-and-tts-guidelines-for-confirmation-screens"></a>확인 화면에 대 한 GUI 및 TTS 지침

현재 시제를 사용 합니다.

"Yes" 또는 "No"로 대답할 수 있는 명확한 질문을 사용자에 게 물어 보세요. 질문은 사용자가 수행 하려는 작업을 명시적으로 확인 해야 하며 다른 명확한 옵션은 없어야 합니다.

음성 명령이 처음으로 인식 되지 않는 경우 다시 프롬프트에 대 한 질문의 변형을 제공 합니다.

**GUI**: 엔터티가 표시 되는 경우이에 대 한 참조를 사용 합니다. 엔터티가 표시 되지 않으면 엔터티를 명시적으로 호출 합니다.

**TTS**: 명확 하 게 하기 위해 이전에 시스템에서 읽지 않은 경우 항상 특정 항목 또는 엔터티를 참조 합니다.

| 조건 | TTS | GUI |
| --- | --- | --- |
| 표시에 표시 된 이전 턴/엔터티에서 엔터티를 읽지 않았습니다. | Vegas Tech 컨퍼런스를 취소 하 시겠습니까? | 이 여행 취소                             |
| 이전 턴/엔터티는 표시 되지 않은 엔터티는 읽지 않음        | Vegas Tech 컨퍼런스를 취소 하 시겠습니까? | Vegas Tech 컨퍼런스를 취소 하 시겠습니까?                 |
| 이전에 표시 되지 않은 이전 전환/엔터티에 대 한 엔터티 읽기            | 이 여행 작업을 취소 하 시겠습니까?             | 이 여행 취소                             |
| 표시 된 엔터티를 사용 하 여 다시 확인                              | 이 여행 작업을 취소 하 시겠습니까?            | 이 여행 작업을 취소 하 시겠습니까?             |
| 엔터티가 표시 되지 않은 다시 확인                          | 이 여행 작업을 취소 하 시겠습니까?            | Vegas Tech 컨퍼런스를 취소 하 시겠습니까? |

### <a name="disambiguation"></a>명확성

:::image type="content" source="images/cortana/cortana-disambiguation-screen.png" alt-text="종단 간 cortana 캔버스의 스크린샷 adventureworks 취소 여행 명확성":::*Adventureworks "여행 취소" 명확성* 을 사용 하는 응용 프로그램 흐름

일부 작업의 경우 사용자가 엔터티 목록에서 선택 하 여 작업을 완료 해야 할 수 있습니다.

명확성 화면에서 GUI와 TTS 문자열은 모두 앱에서 지정 되 고, 제공 된 경우 앱 아이콘 (제공 된 경우)이 **Cortana** 아바타 대신 표시 됩니다.

고객이 명확성 질문에 응답 한 후에 응용 프로그램은 진행률 화면으로 이동 하지 않도록 500 밀리초 내에 다음 화면을 제공 해야 합니다.

### <a name="gui-and-tts-guidelines-for-disambiguation-screens"></a>명확성 화면에 대 한 GUI 및 TTS 지침

현재 시제를 사용 합니다.

표시 되는 엔터티의 제목 또는 텍스트 줄로 대답할 수 있는 명확한 질문을 사용자에 게 요청 합니다.

최대 10 개의 엔터티가 표시 될 수 있습니다.

각 엔터티는 고유한 제목을 가져야 합니다.

음성 명령이 처음으로 인식 되지 않는 경우 다시 프롬프트에 대 한 질문의 변형을 제공 합니다.

**TTS**: 명확 하 게 하기 위해 이전 턴에서 말한 경우가 아니면 항상 특정 항목 또는 엔터티를 참조 합니다.

**TTS**: 3 개 이하인 경우와 짧은 경우에만 엔터티 목록을 읽지 마세요.

| 조건                 | TTS                                                                            | GUI                              |
|----------------------------|--------------------------------------------------------------------------------|----------------------------------|
| 프롬프트-3 개 이하의 항목  | 어떤 Vegas 여행를 취소 하 시겠습니까? Vegas에서 Tech 컨퍼런스 또는 파티를 Vegas? | 어떤 항목을 취소 하 시겠습니까? |
| 프롬프트-3 개 이상 항목 | 어떤 Vegas 여행를 취소 하 시겠습니까?                                          | 어떤 항목을 취소 하 시겠습니까? |
| 다시 프롬프트                   | 어떤 Vegas 여행를 취소 하 시겠습니까?                                         | 어떤 항목을 취소 하 시겠습니까? |

### <a name="completion"></a>Completion

:::image type="content" source="images/cortana/e2e-canceltrip-completion.png" alt-text="종단 간 cortana 캔버스의 스크린샷 adventureworks 취소 여행 완료":::*Adventureworks "여행 취소" 완료*

작업이 성공적으로 완료 되 면 앱에서 요청 된 작업이 성공적으로 완료 되었음을 사용자에 게 알립니다.

완료 화면에서 GUI와 TTS 문자열이 모두 앱에서 지정 되 고, 제공 된 경우 앱 아이콘 (제공 된 경우)이 **Cortana** 아바타 대신 표시 됩니다.

앱을 적절 한 상태로 시작 하려면 시작 매개 변수를 사용 하 여 앱에 대 한 링크를 제공 해야 합니다. 이렇게 하면 사용자가 작업을 직접 보거나 완료할 수 있습니다. **Cortana** 는 링크 텍스트 (예: "놀이 Works로 이동")를 제공 합니다.

### <a name="gui-and-tts-guidelines-for-completion-screens"></a>완료 화면에 대 한 GUI 및 TTS 지침

과거 시제를 사용 합니다.

작업 동사를 사용 하 여 작업 완료를 명시적으로 상태를 지정할 수 있습니다.

엔터티가 표시 되거나 이전 턴에서 참조 된 경우에만 참조 합니다.

| 조건                                       | TTS                                             | GUI                                |
|--------------------------------------------------|-------------------------------------------------|------------------------------------|
| 이전 턴에서 엔터티 표시/엔터티 읽기         | 이 여행을 취소 했습니다.                       | 이 여행을 취소 했습니다.               |
| 이전 턴에서 엔터티를 표시 하지 않거나 엔터티를 읽지 않았습니다. | Vegas Tech 회의 여행을 취소 했습니다. | "Vegas Tech 컨퍼런스"를 취소 했습니다. |

### <a name="error"></a>오류

:::image type="content" source="images/cortana/e2e-canceltrip-error.png" alt-text="종단 간 cortana 캔버스의 스크린샷 adventureworks 취소 여행 오류":::*Adventureworks "여행 취소" 오류* 를 사용 하 여 종단 간 cortana 백그라운드 앱 흐름

다음 오류 중 하나가 발생 하면 **Cortana** 는 동일한 일반 오류 메시지를 표시 합니다.

- App service가 예기치 않게 종료 됩니다.
- **Cortana** 가 app service와 통신 하지 못합니다.
- **Cortana** 가 5 초 동안 핸드 오프 화면 또는 진행률 화면을 표시 한 후 앱에서 화면을 제공 하지 못합니다.

## <a name="related-articles"></a>관련된 문서

- [Windows 앱에서 Cortana 상호 작용](cortana-interactions.md)
- [VCD 요소 및 특성 v 1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana 음성 명령 샘플](https://go.microsoft.com/fwlink/p/?LinkID=619899)
