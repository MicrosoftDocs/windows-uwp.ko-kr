---
title: Cortana에서 보다 자연스러운 음성 명령 지원 - Cortana UWP 디자인 및 개발
description: 사용자가 명령의 어디에서 나 앱 이름을 말할 수 있는 보다 유연 하 고 자연 스러운 음성 명령으로 Cortana를 확장 합니다.
author: kbridge
label: Conceptual
ms.assetid: c2959c1b-c2f2-4a8d-8f3e-79585f69afcf
ms.date: 01/28/2021
ms.topic: article
keywords: 나
ms.openlocfilehash: c6777a0c7ad9a8e2658b759e516b659bbd4eb9a9
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101824507"
---
# <a name="support-natural-language-voice-commands-in-cortana"></a>Cortana에서 자연어 음성 명령을 지원 합니다.

>[!WARNING]
> 이 기능은 Windows 10 5 월 2020 업데이트 (버전 2004, 코드명 "20H1")에서 더 이상 지원 되지 않습니다.
>
> Cortana에서 최신 생산성 환경을 변형 하는 방법에 대 한 [Microsoft 365 cortana](/microsoft-365/admin/misc/cortana-integration) 를 참조 하세요.

사용자가 명령의 어디에서 나 앱 이름을 말할 수 있는 보다 유연 하 고 자연 스러운 음성 명령으로 **Cortana** 를 확장 합니다.

> [!NOTE]
> **중요 API**
>
> - [**VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD 요소 및 특성 v 1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

음성 명령을 사용 하 여 앱의 기능으로 Cortana를 확장 하려면 사용자가 앱과 실행할 명령 또는 함수를 모두 지정 해야 합니다. 이는 일반적으로 음성 명령의 시작 부분 또는 끝 부분에 앱 이름을 발표 하 여 수행 됩니다. 예를 들어 "놀이 Works, Las Vegas에 새 여행 추가"를 추가 합니다.

그러나 이러한 방식으로 응용 프로그램 이름을 지정 하는 것은 매우 불편할 수 있습니다. 대부분의 경우 명령의 다른 위치에서 앱 이름을 말할 수 있는 것이 더 편안 하 고 자연 스러운 것 이며, 사용자에 게 더욱 직관적이 고 유용한 상호 작용을 지 원하는 것입니다. 이전 예제 인 "놀이 Works, Las Vegas에 새 여행 추가" "Las Vegas에 새 어드벤처 여행 추가"로 rephrased 수 있습니다. 또는 "놀이 Works를 사용 하 여 Las Vegas에 새 여행 추가"를 추가 합니다.

앱 이름을으로 지원 하도록 음성 명령을 설정할 수 있습니다.

- Prefix-명령 문구 앞
- 중 위-명령 문구 내
- 접미사-명령 문구 다음에

> [!TIP]
> **필수 구성 요소**
>
> 이 항목에서는 [음성 명령을 사용 하 여 Cortana에서 백그라운드 앱을 활성화 하는](cortana-launch-a-background-app-with-voice-commands.md)방법을 설명 합니다. 여기서는 **놀이 Works** 라는 여행 계획 및 관리 앱을 사용 하 여 기능을 시연 합니다.
>
> UWP (유니버설 Windows 플랫폼) 앱을 처음 개발 하는 경우 여기에 설명 된 기술에 익숙해질 수 있도록 다음 항목을 참조 하세요.
>
> - [첫 번째 앱 만들기](../../get-started/your-first-app.md)
> - [이벤트 및 라우트된 이벤트](../../xaml-platform/events-and-routed-events-overview.md) 를 사용 하 여 이벤트에 대해 알아보기 개요
>
> **사용자 환경 지침**
>
> Cortana 및 [음성 상호 작용](speech-interactions.md) **을 사용 하** 여 앱을 통합 하는 방법에 대 한 정보는 [cortana 디자인 지침](cortana-design-guidelines.md) 을 참조 하세요.

## <a name="specify-an-appname-element-in-the-vcd"></a>VCD에서 **AppName** 요소 지정

**AppName** 요소는 음성 명령에서 사용자에 게 친숙 한 앱 이름을 지정 하는 데 사용 됩니다.

```XML
<AppName>Adventure Works</AppName>
```

## <a name="specify-where-the-app-name-can-be-spoken-in-the-voice-command"></a>음성 명령에서 앱 이름을 읽을 수 있는 위치를 지정 합니다.

**ListenFor** 요소에는 앱 이름이 음성 명령에서 나타날 수 있는 위치를 지정 하는 **requireappname** 특성이 있습니다. 이 특성은 4 개의 값을 지원 합니다.

1. **BeforePhrase**

   기본값

   사용자가 명령 문구 앞에 앱 이름을 입력 해야 함을 나타냅니다.

   여기에서 Cortana는 "Las Vegas으로 여행 하는 경우 놀이 Works"를 수신 합니다.

   ```xml
   <ListenFor RequireAppName="BeforePhrase"> show [my] trip to {destination} </ListenFor>
   ```

2. **AfterPhrase**

    사용자가 명령 문구 뒤에 앱 이름을 입력 해야 함을 나타냅니다.

    Prepositional 접속사의 지역화 된 문구 목록은 시스템에서 제공 합니다. 여기에는 "using", "with" 및 "on"과 같은 구가 포함 됩니다.

    여기에서 Cortana는 "놀이 works에서 Las Vegas에 대 한 다음 여행 표시" 및 "놀이 Works를 사용 하 여 Las Vegas로의 다음 여행 표시"와 같은 명령을 수신 합니다.

    ```xml
    <ListenFor RequireAppName="AfterPhrase">show [my] next trip to {destination} </ListenFor>
    ```

3. **BeforeOrAfterPhrase**

    사용자가 명령 문구 앞 이나 뒤에 앱 이름을 입력 해야 함을 나타냅니다.

    접미사 버전의 경우 prepositional 접속사의 지역화 된 문구 목록은 시스템에서 제공 합니다. 여기에는 "using", "with" 및 "on"과 같은 구가 포함 됩니다.

    여기에서 Cortana는 "놀이 Works, Las Vegas에 대 한 다음 여행 표시" 또는 "놀이 works의 차세대 to Last Vegas"와 같은 명령을 수신 대기 합니다.

    ``` xml
    <ListenFor RequireAppName="BeforeOrAfterPhrase">show [my] next trip to {destination}</ListenFor>
    ```

4. **ExplicitlySpecified**

    사용자가 명령 구에 지정 된 위치에서 앱 이름을 정확 하 게 지정 해야 함을 나타냅니다. 사용자는 문구 앞 이나 뒤에 앱 이름을 말할 필요가 없습니다.

    **{Builtin: AppName}** 태그를 사용 하 여 앱 이름을 명시적으로 참조 해야 합니다.

    여기에서 Cortana는 "놀이 Works, Las Vegas에 대 한 다음 여행 표시" 또는 "다음 어드벤처를 Las Vegas에 표시"와 같은 명령을 수신 대기 합니다.

    ```xml
    <ListenFor RequireAppName="ExplicitlySpecified">show [my] next {builtin:AppName} trip to {destination} </ListenFor>
    ```

## <a name="special-cases"></a>특수 사례

**ListenFor** 요소를 선언할 때 **Requireappname** 이 "Afterphrase" 또는 "ExplicitlySpecified" 인 경우 특정 요구 사항이 충족 되는지 확인 해야 합니다.

1. **{builtin: AppName}은 (** 는) **Requireappname** 이 "ExplicitlySpecified" 인 경우 한 번만 나타나야 합니다.

    이 값을 사용 하면 시스템에서 앱 이름이 음성 명령에 표시 될 수 있는 위치를 유추할 수 없습니다. 위치를 명시적으로 지정 해야 합니다.

2. 음성 명령은 **Phrasetopic** 요소로 시작할 수 없습니다 .이 요소는 일반적으로 많은 어휘 음성 인식에 사용 됩니다. 하나 이상의 단어가 앞에와 야 합니다.

    이렇게 하면 명령에 utterance의 어디에 나 앱 이름이 포함 된 경우 **Cortana** 가 앱을 시작할 가능성을 최소화할 수 있습니다.

    사용자가 "Kinect 놀이 works에 대 한 리뷰 표시"와 같은 것으로 표시 되는 경우 **Cortana** 가 **놀이 works** 앱을 시작 하 게 될 수 있는 잘못 된 선언입니다.
  
    ```xml
    <ListenFor RequireAppName="ExplicitlySpecified">{searchPhrase} {builtin:AppName}</ListenFor>
    ```

3. **ListenFor** 문자열에는 앱 이름과 **Phrasetopic** 요소에 대 한 참조 외에 두 개 이상의 단어가 있어야 합니다.

    사례 2와 마찬가지로, 사용자가 실수로 앱을 시작 하는 기회를 최소화 하기 위해 명령에 충분 한 음성 내용이 포함 되어 있는지 확인 해야 합니다.

    이를 통해 응용 프로그램을 최적의 성공으로 설정할 수 있으므로 사용자에 게 "Find Kinect 어드벤처 works"가 표시 될 때 응용 프로그램이 제대로 시작 되지 않습니다.

    사용자가 "Kinect 어드벤처" 또는 "Find 어드벤처"와 같은 무언가를 표시 하는 경우 **Cortana** 가 **놀이 works** 앱을 시작할 수 있는 잘못 된 선언입니다.

    ```xml
    <ListenFor RequireAppName="ExplicitlySpecified">Hey {builtin:AppName}</ListenFor>
    <ListenFor RequireAppName="ExplicitlySpecified">Find {searchPhrase} {builtin:AppName}</ListenFor>
    ```

## <a name="remarks"></a>설명

**Cortana** 의 사용자가 음성 명령을 재생 하는 방법에 대 한 더 많은 변형을 지원 하기 때문에 앱의 일반적인 유용성이 향상 됩니다.

"안녕하세요 \[ app name \] "을 **AppName** 으로 사용 하지 마십시오. 사용자는 음성 활성화를 통해 Cortana를 호출 하는 "안녕하세요" Cortana에 게 \[ utterance에 "안녕하세요 app name"이 있으면 \] 자연스럽 게 들리지 않습니다. 예를 들어, "안녕하세요. Cortana는 Las Vegas to a next 트립 to a to a.

기존 음성 명령에 중 위/접미사 변형을 추가 하는 것이 좋습니다. 여기에 나와 있는 것 처럼 기존 **ListenFor** 요소에 추가 특성을 추가 하 고 접미사 변형을 지원 하기 위해 많은 노력이 필요 하지 않습니다. 이는 "cortana, 어드벤처, Las Vegas에 대 한 다음 여행 표시"를 "안녕하세요. cortana, 놀이 Works, Las Vegas에 대 한 다음 여행 표시" 보다 훨씬 자연스럽 게 생각 합니다.

음성 명령이 기존 **Cortana** 기능과 충돌 하는 경우 (호출, 메시징 등) 앱 이름을 접두사로 사용 하는 것이 좋습니다. 예를 들어 "놀이 Works, \[ \] Las Vegas 여행에 대 한 메시지 여행 에이전트"입니다.

## <a name="complete-example"></a>전체 예제

다음은 다양 한 자연어 음성 명령을 제공 하는 다양 한 방법을 보여 주는 VCD 파일입니다.

> [!NOTE]
> 여러 **ListenFor** 요소를 포함할 수 있으며 각 요소는 서로 다른 **requireappname** 특성 값을 포함 합니다.

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.1">
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

## <a name="related-articles"></a>관련 문서

- [Windows 앱에서 Cortana 상호 작용](cortana-interactions.md)
- [Cortana 디자인 지침](cortana-design-guidelines.md)
- [VCD 요소 및 특성 v 1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana 음성 명령 샘플](https://go.microsoft.com/fwlink/p/?LinkID=619899)