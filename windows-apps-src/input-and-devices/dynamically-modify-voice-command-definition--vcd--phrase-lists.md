---
author: Karl-Bridge-Microsoft
Description: 런타임 시 음성 인식 결과를 사용하여 VCD(음성 명령 정의) 파일의 지원되는 구 목록(PhraseList 요소)에 액세스하여 이를 업데이트 하는 방법에 알아봅니다.
title: VCD 구 목록을 동적으로 수정
ms.assetid: 98024EAC-EC0E-44AA-AEC5-A611BA7C5884
label: Modify VCD phrase lists
template: detail.hbs
---

# VCD 구 목록을 동적으로 수정





**중요 API**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**VCD 요소 및 특성 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

런타임 시 음성 인식 결과를 사용하여 VCD(음성 명령 정의) 파일의 지원되는 구 목록(**PhraseList** 요소)에 액세스하고 업데이트합니다.

런타임 시 구 목록을 동적으로 수정하는 기능은 음성 명령이 일종의 사용자 정의 또는 임시 앱 데이터를 사용한 작업과 관련이 있는 경우에 유용할 수 있습니다. 

예를 들어 사용자가 목적지를 입력하고, 앱 이름 다음에 “&lt;목적지&gt;에 대한 여행 표시”라고 말하여 앱을 시작할 수 있는 여행 앱이 있다고 가정해 보겠습니다. **ListenFor** 요소 자체에서 `<ListenFor> Show trip to {destination}  </ListenFor>`라고 말할 수 있습니다. 여기서 "destination"은 **PhraseList**의 **Label** 특성 값입니다.

런타임 시 구 목록을 업데이트하면 가능한 각 대상에 대해 별도의 **ListenFor** 요소를 만들 필요가 없습니다. 대신 여정을 입력할 때 사용자가 지정한 대상으로 **PhraseList**를 동적으로 채울 수 있습니다. 

**PhraseList** 및 기타 VCD 요소에 대한 자세한 내용은 [**VCD 요소 및 특성 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)를 참조를 확인하세요.

**필수 조건:  **

이 항목은 [Cortana에서 음성 명령으로 포그라운드 앱 시작](launch-a-foreground-app-with-voice-commands-in-cortana.md)을 기반으로 합니다. 여기서는 **Adventure Works**라는 여행 계획 및 관리 앱으로 계속 기능을 소개합니다.

UWP(유니버설 Windows 플랫폼) 앱을 처음 개발하는 경우 다음 항목을 검토하여 여기서 설명하는 기술에 대해 알아보세요.

-   [첫 번째 앱 만들기](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   이벤트에 대한 자세한 내용은 [이벤트 및 라우트된 이벤트 개요](https://msdn.microsoft.com/library/windows/apps/mt185584)를 참조하세요.

**사용자 환경 지침:  **

**Cortana**와 앱을 통합하는 방법에 대한 자세한 내용은 [Cortana 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn974233)을 참조하고, 유용하고 매력적인 음성 사용 앱 디자인에 도움이 되는 팁은 [음성 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn596121)을 참조하세요.

## <span id="Identify_the_command"></span><span id="identify_the_command"></span><span id="IDENTIFY_THE_COMMAND"></span>명령 식별 및 구 목록 업데이트

다음은 **Command** "showTripToDestination"을 정의하는 VCD 파일과 **Adventure Works** 여행 앱에서 목적지를 선택할 수 있는 3가지 옵션을 정의하는 **PhraseList** 예제입니다. 사용자가 앱에서 목적지를 저장하고 삭제할 때 앱은 **PhraseList**에서 옵션을 업데이트합니다.

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <CommandPrefix> Adventure Works, </CommandPrefix>
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

VCD 파일의 **PhraseList** 요소를 업데이트하려면 구 목록이 포함된 **CommandSet** 요소를 가져옵니다. 해당 **CommandSet** 요소의 **Name** 특성(**Name**이 VCD 파일에서 고유해야 함)을 키로 사용하여 [**VoiceCommandManager.InstalledCommandSets**](https://msdn.microsoft.com/library/windows/apps/dn653257) 속성에 액세스하고 [**VoiceCommandSet**](https://msdn.microsoft.com/library/windows/apps/dn653258) 참조를 가져옵니다.

명령 집합을 식별한 후 수정할 구 목록에 대한 참조를 가져오고 **PhraseList** 요소의 **Label** 특성과 문자열 배열을 구 목록의 새 콘텐츠로 사용하여 [**SetPhraseListAsync**](https://msdn.microsoft.com/library/windows/apps/dn653261) 메서드를 호출합니다.

**참고** 구 목록을 수정하면 전체 구 목록이 바뀝니다. 구 목록에 새 항목을 삽입하려는 경우 [**SetPhraseListAsync**](https://msdn.microsoft.com/library/windows/apps/dn653261) 호출에 기존 항목과 새 항목을 둘 다 지정해야 합니다.

이 예제에서 이전 예제의 **PhraseList**를 Phoenix에 대한 추가 대상으로 업데이트하는 방법이 나와 있습니다.

```CSharp
Windows.ApplicationModel.VoiceCommands.VoiceCommnadDefinition.VoiceCommandSet commandSetEnUs;

if (Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
      InstalledCommandSets.TryGetValue(
        "AdventureWorksCommandSet_en-us", out commandSetEnUs))
{
  await commandSetEnUs.SetPhraseListAsync(
    "destination", new string[] {“London”, “Dallas”, “New York”, “Phoenix”});
}
```

## <span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>설명


상대적으로 적은 집합 또는 단어의 경우 **PhraseList**를 사용하여 인식을 제한하는 것이 적합합니다. 단어 집합이 너무 크거나(예를 들어 수백 개 단어) 전혀 제한되지 않는 경우 **PhraseTopic** 요소 및 **Subject** 요소를 사용하여 음성 인식 결과의 관련성을 구체화함으로써 확장성을 개선합니다.

이 예제에서는 "시\\주"의 **Subject**로 구체화된 "검색" **Scenario**가 있는 **PhraseTopic**이 있습니다.

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <CommandPrefix> Adventure Works, </CommandPrefix>
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

## <span id="related_topics"></span>관련 문서


**개발자**
* [Cortana 조작](cortana-interactions.md)
* [Cortana에서 음성 명령으로 포그라운드 앱 시작](launch-a-foreground-app-with-voice-commands-in-cortana.md)
* [Cortana에서 음성 명령으로 백그라운드 앱 시작](launch-a-background-app-with-voice-commands-in-cortana.md)
* [**VCD 요소 및 특성 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**디자이너**
* [Cortana 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [음성 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn596121)

**샘플**
* [Cortana 음성 명령 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=May16_HO2-->


