---
Description: 사용자가 명령의 어느 위치에서든 앱 이름을 말할 수 있도록 더 유연하고 자연스러운 음성 명령으로 Cortana를 확장하는 방법을 알아봅니다.
title: Cortana에서 자연어 음성 명령 지원
ms.assetid: 281E068A-336A-4A8D-879A-D8715C817911
label: Support natural language voice commands
template: detail.hbs
---

# Cortana에서 자연어 음성 명령 지원


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

사용자가 명령의 어느 위치에서든 앱 이름을 말할 수 있는 더 유연하고 자연스러운 음성 명령으로 **Cortana**를 확장합니다.

**중요 API**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**VCD(음성 명령 정의) 요소 및 특성 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)


음성 명령을 통해 앱의 기능으로 Cortana를 확장하려면 사용자가 앱과 실행할 명령 또는 함수를 둘 다 지정해야 합니다. 이 작업은 일반적으로 이 음성 명령의 시작 또는 끝 부분에 앱 이름을 알리는 방식으로 수행됩니다. 예를 들어 "Adventure Works, 라스베이거스에 새 여행 추가"가 있습니다.

그러나 이런 방식으로 응용 프로그램 이름을 지정하는 것이 어색하거나 말이 안 되게 들리거나 의미가 통하지 않을 수도 있습니다. 대부분의 경우 명령에서 다른 위치에 앱 이름을 넣을 수 있다면 더 편하고 자연스러우며 사용자에 대해 더 직관적이며 매력적인 조작을 만드는 데 도움이 됩니다. 이전 예제인 "Adventure Works, 새 라스베이거스 여행 추가"를 "새 Adventure Works 라스베이거스 여행 추가" 또는 "Adventure Works를 사용하여 새 라스베이거스 여행 추가"처럼 다르게 말할 수 있습니다.

앱 이름을 다음과 같이 지원하도록 음성 명령을 설정할 수 있습니다.

-   접두사 - 명령 구 앞에
-   중위 - 명령 구 안에
-   접미사 - 명령 구 뒤에

**필수 조건: **

이 항목은 [Cortana에서 음성 명령으로 백그라운드 앱 시작](launch-a-background-app-with-voice-commands-in-cortana.md)을 기반으로 합니다. 여기서는 **Adventure Works**라는 여행 계획 및 관리 앱으로 계속 기능을 소개합니다.

UWP(유니버설 Windows 플랫폼) 앱을 처음 개발하는 경우 다음 항목을 검토하여 여기서 설명하는 기술에 대해 알아보세요.

-   [첫 번째 앱 만들기](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   이벤트에 대한 자세한 내용은 [이벤트 및 라우트된 이벤트 개요](https://msdn.microsoft.com/library/windows/apps/mt185584)를 참조하세요.

**사용자 환경 지침: **

**Cortana**와 앱을 통합하는 방법에 대한 자세한 내용은 [Cortana 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn974233)을 참조하고, 유용하고 매력적인 음성 사용 앱 디자인에 도움이 되는 팁은 [음성 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn596121)을 참조하세요.

## <span id="Specify_an_AppName_element_in_the_VCD"> </span> <span id="specify_an_appname_element_in_the_vcd"> </span> <span id="SPECIFY_AN_APPNAME_ELEMENT_IN_THE_VCD"> </span>**AppName** VCD 요소 지정


**AppName** 요소는 음성 명령에서 앱의 이름을 사용자 친화적으로 지정하는 데 사용됩니다.

```XML
<AppName>Adventure Works</AppName>
```

## <span id="Specify_where_the_app_name_can_be_spoken_in_the_voice_command"> </span> <span id="specify_where_the_app_name_can_be_spoken_in_the_voice_command"> </span> <span id="SPECIFY_WHERE_THE_APP_NAME_CAN_BE_SPOKEN_IN_THE_VOICE_COMMAND"> </span>음성 명령에서 앱 이름을 말할 수 있는 위치 지정


**ListenFor** 요소에는 **RequireAppName** 음성 명령에 앱 이름이 나타나는 위치를 지정하는 특성이 있습니다. 이 특성은 4개의 값을 지원합니다.

1.  **BeforePhrase**

    기본값.

    사용자가 명령 구 앞에 앱 이름을 말하도록 지정합니다.

    여기서 Cortana가 "Adventure Works 라스베이거스로 가는 내 여행 시기"를 수신 대기합니다.

```xml
<ListenFor RequireAppName="BeforePhrase"> show [my] trip to {destination} </ListenFor>
```

2.  **AfterPhrase**

    사용자가 명령 구 뒤에 앱 이름을 말하도록 지정합니다.

    전치사적인 접속사의 지역화된 구 목록은 시스템에서 제공합니다. 여기에는 "사용", "으로" 및 "에"와 같은 구가 포함됩니다.

    여기서 Cortana는 "Adventure Works에서 내 다음 라스베이거스 여행 표시" 및 "Adventure Works를 사용하여 내 다음 라스베이거스 여행 표시"와 같은 명령을 수신 대기합니다.

```xml
<ListenFor RequireAppName="AfterPhrase">show [my] next trip to {destination} </ListenFor>
```

3.  **BeforeOrAfterPhrase**

    사용자가 명령 구 앞이나 뒤에 앱 이름을 말하도록 지정합니다.

    접미사 버전의 경우 전치사적인 접속사의 지역화된 구 목록은 시스템에서 제공합니다. 여기에는 "사용", "으로" 및 "에"와 같은 구가 포함됩니다.

    여기서 Cortana는 "dventure Works, 내 다음 라스베이거스 여행 표시" 또는 "Adventure works에 내 다음 여행 표시"와 같은 명령을 수신 대기합니다.

``` xml
<ListenFor RequireAppName="BeforeOrAfterPhrase">show [my] next trip to {destination}</ListenFor>
```

4.  **ExplicitlySpecified**

    사용자가 명령 구에서 지정된 정확한 위치에 앱 이름을 말하도록 지정합니다. 사용자는 구 앞이나 뒤에 앱 이름을 말할 필요가 없습니다.

    **{builtin:AppName}** 태그를 사용하여 앱 이름을 명시적으로 나타내야 합니다.

    여기서 Cortana는 "Adventure Works, 내 다음 라스베이거스 여행 표시" 또는 "내 다음 라스베이거스 Adventure Works 여행 표시"와 같은 명령을 수신 대기합니다.

```xml
<ListenFor RequireAppName="ExplicitlySpecified">show [my] next {builtin:AppName} trip to {destination} </ListenFor>
```

## <span id="Special_cases"> </span> <span id="special_cases"> </span> <span id="SPECIAL_CASES"> </span>특수한 경우

**RequireAppName**이 "AfterPhrase" 또는 "ExplicitlySpecified"인 **ListenFor** 요소를 선언할 경우 다음과 같은 특정 요구 사항이 충족되는지 확인해야 합니다.

1.  **RequireAppName**가 "ExplicitlySpecified"인 경우 **{builtin:AppName}**이(가) 한 번만 나타나야 합니다.

    이 값을 사용하면 시스템에서 앱 이름이 음성 명령에 나타나는 위치를 유추할 수 없습니다. 위치를 명시적으로 지정해야 합니다.

2.  일반적으로 대규모 어휘 음성 인식에 사용되는 **PhraseTopic** 요소로 시작하는 음성 명령이 없을 수 있습니다. 적어도 한 단어가 앞에 와야 합니다.

    이렇게 하면 명령에 앱 이름 또는 그 일부가 있는 경우(발음상 어디에 있든) **Cortana**가 앱을 실행할 가능성을 최소화하는 데 도움이 됩니다.

    사용자가 "Kinect Adventure works에 대한 리뷰 표시"와 같은 말을 할 경우 **Cortana**에서 **Adventure Works** 앱을 시작할 수 있는 잘못된 선언은 다음과 같습니다.

```xml
<ListenFor RequireAppName="ExplicitlySpecified">{searchPhrase} {builtin:AppName}</ListenFor>
```

3.  앱 이름 옆에 있는 **ListenFor** 문자열과 **PhraseTopic** 요소에 대한 언급에 2개 이상의 단어가 있어야 합니다.

    사례 2와 마찬가지로, 의도하지 않게 앱이 시작될 가능성을 최소화하려면 명령에 충분한 음성 콘텐츠가 있는지 확인해야 합니다.

    그렇게 하면 사용자가 "Kinect Adventure works 찾기"와 같은 말을 했을 때 응용 프로그램이 잘못 시작되지 않도록 응용 프로그램의 적절한 작동을 설정하는 데 도움이 됩니다.

    사용자가 "안녕 Adventure works" 또는 "Kinect adventure works 찾기"와 같은 말을 할 경우 **Cortana**에서 **Adventure Works** 앱을 실행할 수 있는 잘못된 선언은 다음과 같습니다.

```xml
<ListenFor RequireAppName="ExplicitlySpecified">Hey {builtin:AppName}</ListenFor>
<ListenFor RequireAppName="ExplicitlySpecified">Find {searchPhrase} {builtin:AppName}</ListenFor>
```

## <span id="Remarks"> </span> <span id="remarks"> </span> <span id="REMARKS"> </span>설명

사용자가 **Cortana**에서 음성 명령을 말할 수 있는 방식 면에서 더 많은 변형을 지원하면 앱의 일반적인 유용성이 늘어납니다.

**AppName** 또는 **CommandPrefix**로 "안녕 \[앱 이름\]"을 사용하지 마세요. 사용자가 "안녕 코타나"를 말하여 음성 활성화를 통해 Cortana를 호출할 가능성이 높으며 "안녕 \[앱 이름\]"을 발음하는 것이 자연스럽게 들리지 않습니다. 예를 들어 "안녕 코타나, 내 다음 여행 라스베이거스 여행을 Adventure Works에 표시해 줘"라고 말할 수 있습니다.

기존 음성 명령을 중위/접미사 변형을 추가하는 것이 좋습니다. 여기에 나온 대로 기존의 **ListenFor** 요소에 추가 특성을 추가하고 접미사 변형 요소를 지원하기 위해 많은 작업을 할 필요는 없습니다. "안녕 코타나, Adventure Works, 내 다음 라스베이거스 여행 표시"보다 "안녕 코타나, 내 다음 여행 라스베이거스 여행을 Adventure Works에 표시해 줘"가 훨씬 더 자연스럽게 느껴질 수 있습니다.

음성 명령이 기존 **Cortana** 기능(전화, 메시지 등)과 충돌하는 경우에는 앱 이름을 접두사로 사용하는 것이 좋습니다. 예를 들어 "Adventure Works, 라스베이거스 여행에 대한 \[여행사\] 메시지"를 사용할 수 있습니다.

## <span id="Complete_example"> </span> <span id="complete_example"> </span> <span id="COMPLETE_EXAMPLE"> </span>전체 예제


더 많은 자연어 음성 명령을 제공할 수 있는 다양한 방법을 보여주는 VCD 파일은 다음과 같습니다.

**참고** 각기 다른 **RequireAppName** 특성 값을 사용하는 **ListenFor** 요소를 여러 개 사용해야 합니다.

 

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="commandSet_en-us">
    <AppName>Adventure Works</AppName>
    <Example> When is my trip to Las Vegas? </Example>
    <Command Name="whenIsTripToDestination">
      <Example> When is my trip to Las Vegas?</Example>
      <ListenFor RequireAppName="BeforePhrase">
        when is my] trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands like 
           “Show my next trip to Las Vegas on Adventure Works”; “Show my next 
           trip to Las Vegas using Adventure Works” -->
      <ListenFor RequireAppName="AfterPhrase">
        show [my] next trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands when 
           the user specifies your app name either before or after the command. 
           “Adventure Works, show my next trip to Las Vegas”; 
           “Show my next trip to Last Vegas on Adventure works” -->
      <ListenFor RequireAppName="BeforeOrAfterPhrase">
        show [my] next trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands 
           when the user specifies your app name inline. 
           “Show my next Adventure Works trip to Las Vegas” -->
      <ListenFor RequireAppName="ExplicitlySpecified">
        show [my] next {builtin:AppName} trip to {destination} </ListenFor>

      <Feedback> Looking for trip to {destination} </Feedback>
      <Navigate />
    </Command>
    <PhraseList Label="destination">
      <Item> Las Vegas </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>
  </CommandSet>
  <!-- Other CommandSets for other languages -->
</VoiceCommands>
```

## <span id="related_topics"> </span>관련 문서


**개발자**
* [Cortana 조작](cortana-interactions.md)
* [사용자 지정 인식 제약 조건 정의](define-custom-recognition-constraints.md)
* [**VCD 요소 및 특성 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**디자이너**
* [Cortana 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [음성 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn596121)

**샘플**
* [Cortana 음성 명령 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=Mar16_HO4-->


