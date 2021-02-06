---
title: Cortana VCD 문구 목록 동적 수정-Cortana UWP 디자인 및 개발
description: 음성 인식 결과를 사용 하 여 런타임에 VCD (음성 명령 정의) 파일에서 지원 되는 구 (PhraseList 요소) 목록에 액세스 하 고 업데이트 합니다.
ms.assetid: b497145b-c7a0-454a-8329-6bc1228953bb
ms.date: 01/28/2021
ms.topic: article
keywords: 나
ms.openlocfilehash: 1f61a08e9eeb66371ed39b44eb39dacbc1bf3cf5
ms.sourcegitcommit: 8fe992f3a6d8f7975af4911ad88e855bee50083e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99606038"
---
# <a name="dynamically-modify-cortana-vcd-phrase-lists"></a>Cortana VCD 문구 목록 동적 수정

>[!WARNING]
> 이 기능은 Windows 10 5 월 2020 업데이트 (버전 2004, 코드명 "20H1")에서 더 이상 지원 되지 않습니다.
>
> Cortana에서 최신 생산성 환경을 변형 하는 방법에 대 한 [Microsoft 365 cortana](/microsoft-365/admin/misc/cortana-integration) 를 참조 하세요.

음성 인식 결과를 사용 하 여 런타임에 VCD (음성 명령 정의) 파일에서 지원 되는 구 (**Phraselist** 요소) 목록에 액세스 하 고 업데이트 합니다.

> [!NOTE]
> 음성 명령은 설치 된 앱에서 **Cortana** 를 통해 전달 되는 특정 의도가 포함 된 단일 Utterance (오디오 명령 정의) 파일에 정의 되어 있습니다.
>
> VCD 파일은 각각 고유한 의도를 가진 하나 이상의 음성 명령을 정의 합니다.
>
> 음성 명령 정의는 복잡성이 다를 수 있습니다. 단일 제한 된 utterance의 모든 항목을 동일한 의도를 나타내는 보다 유연 하 고 자연 언어 길이 발언 컬렉션으로 지원할 수 있습니다.

런타임에 구문 목록을 동적으로 수정 하는 것은 음성 명령이 일종의 사용자 정의 또는 임시 응용 프로그램 데이터를 포함 하는 작업과 관련 된 경우에 유용 합니다.

> [!NOTE]
> **중요 API**
>
> - [**VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD 요소 및 특성 v 1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

예를 들어 사용자가 대상을 입력할 수 있는 여행 앱이 있고, 사용자가 앱 이름 다음에 "여행 표시"를 통해 앱을 시작할 수 있도록 하려는 경우를 가정해 보겠습니다 &lt; &gt; . **ListenFor** 요소 자체에서 다음과 같이 지정 합니다. `<ListenFor> Show trip to {destination}  </ListenFor>` 여기서 "Destination"은 **phraselist** 에 대 한 **레이블** 특성의 값입니다.

런타임에 구문 목록을 업데이트 하면 가능한 각 대상에 대해 별도의 **ListenFor** 요소를 만들 필요가 없습니다. 대신, 사용자가 여정을를 입력할 때 지정 된 대상으로 **Phraselist** 를 동적으로 채울 수 있습니다.

**Phraselist** 및 기타 VCD 요소에 대 한 자세한 내용은 [**vcd 요소 및 특성 v 1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 참조를 참조 하세요.

> [!TIP]
> **필수 구성 요소**
>
> UWP (유니버설 Windows 플랫폼) 앱을 처음 개발 하는 경우 여기에 설명 된 기술에 익숙해질 수 있도록 다음 항목을 참조 하세요.
>
> - [첫 번째 앱 만들기](/windows/uwp/get-started/your-first-app)
> - [이벤트 및 라우트된 이벤트](/windows/uwp/xaml-platform/events-and-routed-events-overview) 를 사용 하 여 이벤트에 대해 알아보기 개요
>
> **사용자 환경 지침**
>
> Cortana 및 [음성 상호 작용](speech-interactions.md) **을 사용 하** 여 앱을 통합 하는 방법에 대 한 정보는 [cortana 디자인 지침](cortana-design-guidelines.md) 을 참조 하세요.

## <a name="identify-the-command-and-update-the-phrase-list"></a>명령을 식별 하 고 구 목록을 업데이트 합니다.

다음은 **놀이 Works** 여행 앱에서 대상에 대 한 세 가지 옵션을 정의 하는 "ShowTripToDestination" **명령** 및 **phraselist** 를 정의 하는 예제 VCD 파일입니다. 사용자가 앱에서 대상을 저장 하 고 삭제 하면 앱은 **Phraselist** 의 옵션을 업데이트 합니다.

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works, </AppName>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> show trip to London  </Example>
      <ListenFor> show trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate/>
    </Command>

    <PhraseList Label="destination">
      <Item> London </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>

  </CommandSet>

<!-- Other CommandSets for other languages -->

</VoiceCommands>
```

VCD 파일에서 **Phraselist** 요소를 업데이트 하려면 구 목록이 포함 된 **commandset** 요소를 가져옵니다. [**VoiceCommandManager**](/uwp/api/Windows.Media.SpeechRecognition.VoiceCommandManager) 속성에 액세스 하 고 [**VoiceCommandSet**](/uwp/api/Windows.Media.SpeechRecognition.VoiceCommandSet) 참조를 가져오기 위한 키로 **commandset** 요소의 **name** 특성 (**이름이** VCD 파일에서 고유 해야 함)을 사용 합니다.

명령 집합을 식별 한 후 수정 하려는 구 목록에 대 한 참조를 가져오고 [**Setphraselistasync**](/uwp/api/Windows.Media.SpeechRecognition.VoiceCommandSet) 메서드를 호출 합니다. **Phraselist** 요소의 **Label** 특성과 문자열 배열을 구 목록의 새 내용으로 사용 합니다.

> [!NOTE]
> 구 목록을 수정 하면 전체 구 목록이 바뀝니다. 구 목록에 새 항목을 삽입 하려는 경우 [**Setphraselistasync**](/uwp/api/Windows.Media.SpeechRecognition.VoiceCommandSet)호출에서 기존 항목과 새 항목을 모두 지정 해야 합니다.

이 예제에서는 이전 예제에 표시 된 **Phraselist** 를 추가 대상으로 Phoenix를 업데이트 합니다.

```CSharp
Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinition.VoiceCommandSet commandSetEnUs;

if (Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
      InstalledCommandSets.TryGetValue(
        "AdventureWorksCommandSet_en-us", out commandSetEnUs))
{
  await commandSetEnUs.SetPhraseListAsync(
    "destination", new string[] {“London”, “Dallas”, “New York”, “Phoenix”});
}
```

## <a name="remarks"></a>설명

**Phraselist** 를 사용 하 여 비교적 작은 집합이 나 단어에 대 한 인식을 제한 합니다. 단어 집합 (예: 수백 개의 단어)이 너무 크거나 제한 되지 않아야 하는 경우에는 **Phrasetopic** 요소와 **Subject** 요소를 사용 하 여 확장성을 향상 시키기 위해 음성 인식 결과의 관련성을 구체화 합니다.

이 예제에서는 "검색" **시나리오** 와 함께 **Phrasetopic** 를 사용 하 여 "도시 주"의 **제목을** 추가로 구체화 \\ 합니다.

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works, </AppName>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> show trip to London  </Example>
      <ListenFor> show trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate/>
    </Command>

    <PhraseList Label="destination">
      <Item> London </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>

    <PhraseTopic Label="destination" Scenario="Search">
      <Subject>City/State</Subject>
    </PhraseTopic>

  </CommandSet>
```

## <a name="related-articles"></a>관련 문서

- [Windows 앱에서 Cortana 상호 작용](cortana-interactions.md)
- [VCD 요소 및 특성 v 1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana를 통해 음성 명령으로 포그라운드 앱 활성화](cortana-launch-a-foreground-app-with-voice-commands.md)
- [음성 명령을 사용 하 여 Cortana에서 백그라운드 앱 활성화](cortana-launch-a-background-app-with-voice-commands.md)
- [Cortana 디자인 지침](cortana-design-guidelines.md)
- [Cortana 음성 명령 샘플](https://go.microsoft.com/fwlink/p/?LinkID=619899)
