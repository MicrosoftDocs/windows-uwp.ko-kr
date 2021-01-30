---
title: Cortana를 통해 음성 명령으로 포그라운드 앱 활성화-Cortana UWP 디자인 및 개발
description: 음성 명령을 사용 하 여 응용 프로그램을 포그라운드로 활성화 하 고 앱 내에서 작업 또는 명령을 실행 합니다.
ms.assetid: e4bf3714-6f62-466f-9e7c-3b03ee86a117
ms.date: 01/28/2021
ms.topic: article
keywords: 나
ms.openlocfilehash: a9df2d107482234fa8783d92a1f4fbb075e7d505
ms.sourcegitcommit: d7efd35c1749f695aebbc0db99d8b62b70fb72da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99057788"
---
# <a name="activate-a-foreground-app-with-voice-commands-through-cortana"></a>Cortana를 통해 음성 명령으로 포그라운드 앱 활성화

>[!WARNING]
> 이 기능은 Windows 10 5 월 2020 업데이트 (버전 2004, 코드명 "20H1")에서 더 이상 지원 되지 않습니다.

**Cortana** 내에서 음성 명령을 사용 하 여 시스템 기능에 액세스 하는 것 외에도 **cortana** 를 앱에서 기능 및 기능으로 확장할 수 있습니다. 음성 명령을 사용 하면 앱을 포그라운드 및 앱 내에서 실행 되는 작업 또는 명령에 대해 활성화할 수 있습니다.

> [!NOTE]
> **중요 API**
>
> - [**VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD 요소 및 특성 v 1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

앱이 전경에 있는 음성 명령을 처리할 때 포커스를 가져오고 Cortana를 해제 합니다. 원한다 면 앱을 활성화 하 고 백그라운드 작업으로 명령을 실행할 수 있습니다. 이 경우 Cortana는 포커스를 유지 하 고 앱은 **cortana** 캔버스 및 **cortana** 음성을 통해 모든 피드백 및 결과를 반환 합니다.

추가 컨텍스트 또는 사용자 입력이 필요한 음성 명령 (예: 특정 연락처에 메시지 보내기)은 포그라운드 앱에서 가장 잘 처리 되지만, 기본 명령 (예: 예정 된 여행 나열)은 백그라운드 앱을 통해 **Cortana** 에서 처리 될 수 있습니다.

음성 명령을 사용 하 여 백그라운드에서 앱을 활성화 하려면 [음성 명령을 사용 하 여 Cortana에서 백그라운드 앱 활성화](cortana-launch-a-background-app-with-voice-commands.md)를 참조 하세요.

> [!NOTE]
> 음성 명령은 설치 된 앱에서 **Cortana** 를 통해 전달 되는 특정 의도가 포함 된 단일 Utterance (오디오 명령 정의) 파일에 정의 되어 있습니다.
>
> VCD 파일은 각각 고유한 의도를 가진 하나 이상의 음성 명령을 정의 합니다.
>
> 음성 명령 정의는 복잡성이 다를 수 있습니다. 단일 제한 된 utterance의 모든 항목을 동일한 의도를 나타내는 보다 유연 하 고 자연 언어 길이 발언 컬렉션으로 지원할 수 있습니다.

포그라운드 앱 기능을 시연 하기 위해 [Cortana 음성 명령 샘플](https://go.microsoft.com/fwlink/p/?LinkID=619899)에서 **놀이 Works** 라는 여행 계획 및 관리 앱을 사용 합니다.

**Cortana** 없이 새 **어드벤처** 여행 여행을 만들려면 사용자가 앱을 시작 하 고 **새 여행** 페이지로 이동 합니다. 기존 여행을 보려면 사용자가 앱을 시작 하 고 **예정 된 여행** 페이지로 이동 하 여 여행을 선택 합니다.

**Cortana** 를 통해 음성 명령을 사용 하 여 사용자는 "여행 추가 여행" 또는 "어드벤처에 여행 추가"를 대신 사용 하 여 앱을 시작 하 고 **새 여행** 페이지로 이동할 수 있습니다. 또한 "놀이 Works, 런던으로 내 여행 표시" 라는 메시지가 표시 되 면 앱을 시작 하 고 다음과 같이 **여행** 세부 정보 페이지로 이동 합니다.

:::image type="content" source="images/cortana/cortana-foreground-with-adventureworks.png" alt-text="Cortana 시작 포그라운드 앱 스크린샷":::

음성 명령 기능을 추가 하 고 음성 또는 키보드 입력을 사용 하 여 Cortana를 앱에 통합 하는 기본 단계는 다음과 같습니다.

1. VCD 파일을 만듭니다. 사용자가 앱을 활성화할 때 작업을 시작 하거나 명령을 호출 하는 데 사용할 수 있는 모든 음성 명령을 정의 하는 XML 문서입니다. [**VCD 요소 및 특성 v 1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)를 참조 하세요.
2. 앱이 시작 될 때 VCD 파일에 명령 집합을 등록 합니다.
3. 음성 명령과 앱 내 탐색을 처리 하 고 명령 실행을 처리 합니다.

> [!TIP]
> **전제 조건**
>
> UWP (유니버설 Windows 플랫폼) 앱을 처음 개발 하는 경우 여기에 설명 된 기술에 익숙해질 수 있도록 다음 항목을 참조 하세요.
>
> - [첫 번째 앱 만들기](/windows/uwp/get-started/your-first-app)
> - [이벤트 및 라우트된 이벤트](/windows/uwp/xaml-platform/events-and-routed-events-overview) 를 사용 하 여 이벤트에 대해 알아보기 개요
>
> **사용자 환경 지침**
>
> Cortana 및 [음성 상호 작용](speech-interactions.md) **을 사용 하** 여 앱을 통합 하는 방법에 대 한 정보는 [cortana 디자인 지침](cortana-design-guidelines.md) 을 참조 하세요.

## <a name="create-a-new-solution-with-project-in-visual-studio"></a>Visual Studio에서 프로젝트를 사용 하 여 새 솔루션 만들기

1. 2015 Microsoft Visual Studio 시작 합니다.

    Visual Studio 2015 시작 페이지가 나타납니다.

2. **파일** 메뉴에서 **새로 만들기** > **프로젝트** 를 선택합니다.

    **새 프로젝트** 대화 상자가 나타납니다. 대화 상자의 왼쪽 창에서 표시할 템플릿 형식을 선택할 수 있습니다.

3. 왼쪽 창에서 **설치 된 > 템플릿 > Visual C \# > Windows** 를 확장 한 다음 **유니버설** 템플릿 그룹을 선택 합니다. 대화 상자의 가운데 창에 UWP (유니버설 Windows 플랫폼) 앱에 대 한 프로젝트 템플릿 목록이 표시 됩니다.
4. 가운데 창에서 **비어 있는 앱 (유니버설 Windows)** 템플릿을 선택 합니다.

    **비어 있는 앱** 템플릿은 컴파일 및 실행 되는 최소 UWP 앱을 만들지만 사용자 인터페이스 컨트롤이 나 데이터를 포함 하지 않습니다. 이 자습서의 과정을 통해 앱에 컨트롤을 추가 합니다.

5. **이름** 텍스트 상자에 프로젝트 이름을 입력 합니다. 이 예에서는 "AdventureWorks"를 사용 합니다.
6. **확인** 을 클릭하여 프로젝트를 만듭니다.

    Microsoft Visual Studio 프로젝트를 만들고 **솔루션 탐색기** 에 표시 합니다.

## <a name="add-image-assets-to-project-and-specify-them-in-the-app-manifest"></a>프로젝트에 이미지 자산을 추가 하 고 응용 프로그램 매니페스트에서 지정 합니다.

UWP 앱은 특정 설정 및 장치 기능 (고대비, 유효 픽셀, 로캘 등)에 따라 가장 적합 한 이미지를 자동으로 선택할 수 있습니다. 이미지를 제공 하기만 하면 응용 프로그램 프로젝트 내에서 다른 리소스 버전에 대 한 적절 한 명명 규칙 및 폴더 조직을 사용 하 게 됩니다. 권장 리소스 버전을 제공 하지 않으면 사용자의 기본 설정, 기능, 장치 유형 및 위치에 따라 접근성, 지역화 및 이미지 품질이 저하 될 수 있습니다.

고대비 및 배율 인수에 대 한 이미지 리소스에 대 한 자세한 내용은 [타일 및 아이콘 자산에 대 한 지침](/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast)을 참조 하세요.

한정자를 사용 하 여 리소스 이름을 표시 합니다. 리소스 한정자는 특정 버전의 리소스를 사용 해야 하는 컨텍스트를 식별 하는 폴더 및 파일 이름 한정자입니다.

표준 명명 규칙은 `foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext` 입니다. 예를 들어, `images/logo.scale-100_contrast-white.png` 루트 폴더와 파일 이름를 사용 하 여 코드에서 참조할 수 있는입니다. `images/logo.png` [한정자를 사용 하 여 리소스 이름을 결정 하는 방법을](/previous-versions/windows/apps/hh965324(v=win.10))참조 하세요.

`en-US\resources.resw` `logo.scale-100.png` 현재 지역화 된 또는 여러 해결 리소스를 제공 하지 않는 경우에도 문자열 리소스 파일 (예:)과 이미지의 기본 배율 인수 (예:)에 기본 언어를 표시 하는 것이 좋습니다. 그러나 최소 100, 200 및 400 배율 인수에 대 한 자산을 제공 하는 것이 좋습니다.

> [!IMPORTANT]
> **Cortana** 캔버스의 제목 영역에 사용 되는 앱 아이콘은 "appxmanifest.xml" 파일에 지정 된 Square44x44Logo 아이콘입니다.

## <a name="create-a-vcd-file"></a>VCD 파일 만들기

1. Visual Studio에서 기본 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가 > 새 항목** 을 선택 합니다. **XML 파일** 을 추가 합니다.
2. [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 파일의 이름 (이 예에서는 "AdventureWorksCommands.xml")을 입력 하 고 추가를 클릭 합니다.
3. **솔루션 탐색기** 에서 [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 파일을 선택 합니다.
4. **속성** 창에서 **빌드 작업** 을 **내용** 으로 설정 하 고 **출력 디렉터리로 복사** 를 **새 버전이 면 복사** 로 설정 합니다.

## <a name="edit-the-vcd-file"></a>VCD 파일 편집

을 가리키는 **xmlns** 특성을 사용 하 여 **VoiceCommands** 요소를 추가 `https://schemas.microsoft.com/voicecommands/1.2` 합니다.

1. 앱에서 지 원하는 각 언어에 대해 앱에서 지원 되는 음성 명령을 포함 하는 [**Commandset**](/previous-versions/windows/dn722331(v=win.10)) 요소를 만듭니다.

   각각 다른 [**xml: lang**](/previous-versions/windows/dn722331(v=win.10)) 특성을 사용 하 여 여러 [**commandset**](/previous-versions/windows/dn722331(v=win.10)) 요소를 선언 하 여 앱을 다른 시장에서 사용할 수 있습니다. 예를 들어 미국의 앱에는 영어에 대 한 [**commandset**](/previous-versions/windows/dn722331(v=win.10)) 와 스페인어 용 [**commandset**](/previous-versions/windows/dn722331(v=win.10)) 이 포함 되어 있을 수 있습니다.

   > [!CAUTION]
   > 앱을 활성화 하 고 음성 명령을 사용 하 여 작업을 시작 하려면 앱은 사용자가 장치에 대해 선택한 음성 언어와 일치 하는 언어를 가진 [**Commandset**](/previous-versions/windows/dn722331(v=win.10)) 가 포함 된 VCD 파일을 등록 해야 합니다. 음성 언어는 **설정 > System > speech > 음성 언어** 에 있습니다.

2. 지원 하려는 각 명령에 대 한 **명령** 요소를 추가 합니다. [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 파일에 선언 된 각 **명령** 에는 다음 정보가 포함 되어야 합니다.

   - 런타임에 음성 명령을 식별 하기 위해 응용 프로그램에서 사용 하는 **AppName** 특성입니다.
   - 사용자가 명령을 호출할 수 있는 방법을 설명 하는 구가 포함 된 **예제** 요소입니다. **Cortana** 는 사용자에 게 "무엇을 할 수 있나요?", "Help" 또는 **자세히 보기** 를 탭 할 때이 예를 보여 줍니다.
   - 앱이 명령으로 인식 하는 단어 또는 구를 포함 하는 **ListenFor** 요소입니다. 각 **ListenFor** 요소는 명령과 관련 된 특정 단어를 포함 하는 하나 이상의 **Phraselist** 요소에 대 한 참조를 포함할 수 있습니다.
  
> [!NOTE]
> **ListenFor** 요소는 프로그래밍 방식으로 수정할 수 없습니다. 그러나 **ListenFor** 요소와 연결 된 **Phraselist** 요소는 프로그래밍 방식으로 수정할 수 있습니다. 응용 프로그램은 사용자가 앱을 사용할 때 생성 되는 데이터 집합에 따라 런타임에 **Phraselist** 의 콘텐츠를 수정 해야 합니다. [CORTANA VCD 문구 목록 동적 수정](cortana-dynamically-modify-voice-command-definition-vcd-phrase-lists.md)을 참조 하세요.

응용 프로그램이 시작 될 때 **Cortana** 가 표시 하 고 말할 텍스트를 포함 하는 **피드백** 요소입니다.

**Navigate** 요소는 음성 명령이 앱을 포그라운드로 활성화 함을 나타냅니다. 이 예제에서 `showTripToDestination` 명령은 전경 작업입니다.

**VoiceCommandService** 요소는 음성 명령이 백그라운드에서 앱을 활성화 함을 나타냅니다. 이 요소의 **Target** 특성 값은 appxmanifest.xml 파일에 있는 [**uap: AppService**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-appservice) 요소의 **Name** 특성 값과 일치 해야 합니다. 이 예제에서 `whenIsTripToDestination` 및 `cancelTripToDestination` 명령은 app service의 이름을 "AdventureWorksVoiceCommandService"로 지정 하는 백그라운드 작업입니다.

자세한 내용은 [**VCD 요소 및 특성 v 1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 참조를 참조 하세요.

다음은 **놀이 Works** 앱에 대 한 en-us 음성 명령을 정의 하는 [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 파일의 일부입니다.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.2">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works </AppName>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> Show trip to London </Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> show [my] trip to {destination} </ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> show [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate />
    </Command>

    <Command Name="whenIsTripToDestination">
      <Example> When is my trip to Las Vegas?</Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Looking for trip to {destination}</Feedback>
      <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>
    
    <Command Name="cancelTripToDestination">
      <Example> Cancel my trip to Las Vegas </Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> cancel [my] trip to {destination}</ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> cancel [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Cancelling trip to {destination}</Feedback>
      <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>

    <PhraseList Label="destination">
      <Item>London</Item>
      <Item>Las Vegas</Item>
      <Item>Melbourne</Item>
      <Item>Yosemite National Park</Item>
    </PhraseList>
  </CommandSet>
```

## <a name="install-the-vcd-commands"></a>VCD 명령 설치

앱은 VCD를 설치 하기 위해 한 번 실행 해야 합니다.

> [!NOTE]
> 음성 명령 데이터는 앱 설치 간에 유지 되지 않습니다. 앱에 대 한 음성 명령 데이터가 그대로 유지 되도록 하려면 앱이 시작 되거나 활성화 될 때마다 VCD 파일을 초기화 하거나, 현재 VCD가 설치 되어 있는지 여부를 나타내는 설정을 유지 관리 하는 것이 좋습니다.

"App.xaml.cs" 파일에서 다음을 수행 합니다.

1. 다음 using 지시문을 추가합니다.

    ```csharp
    using Windows.Storage;
    ```

1. "OnLaunched" 메서드를 async 한정자로 표시 합니다.  

    ```csharp
    protected async override void OnLaunched(LaunchActivatedEventArgs e)
    ```

1. [**Onlaunched**](/uwp/api/Windows.UI.Xaml.Application) 된 처리기에서 [**InstallCommandDefinitionsFromStorageFileAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager) 을 호출 하 여 시스템에서 인식 해야 하는 음성 명령을 등록 합니다.

  놀이 Works 샘플에서는 먼저 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 개체를 정의 합니다.

  그런 다음 [**GetFileAsync**](/uwp/api/Windows.Storage.StorageFolder) 를 호출 하 여 "AdventureWorksCommands.xml" 파일을 사용 하 여 초기화 합니다.

  그런 다음이 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 개체는 [**InstallCommandDefinitionsFromStorageFileAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager)에 전달 됩니다.

```csharp
try
{
  // Install the main VCD. 
  StorageFile vcdStorageFile = 
  await Package.Current.InstalledLocation.GetFileAsync(
  @"AdventureWorksCommands.xml");

  await Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
InstallCommandDefinitionsFromStorageFileAsync(vcdStorageFile);

  // Update phrase list.
  ViewModel.ViewModelLocator locator = App.Current.Resources["ViewModelLocator"] as ViewModel.ViewModelLocator;
  if(locator != null)
  {
     await locator.TripViewModel.UpdateDestinationPhraseList();
  }
}
catch (Exception ex)
{
  System.Diagnostics.Debug.WriteLine("Installing Voice Commands Failed: " + ex.ToString());
}
```

## <a name="handle-activation-and-execute-voice-commands"></a>활성화 및 음성 명령 실행 처리

이후 음성 명령 활성화에 앱이 응답 하는 방법을 지정 합니다 (한 번 이상 실행 되 고 음성 명령 집합이 설치 된 후).

1. 음성 명령으로 앱을 활성화 했는지 확인 합니다.

    [**응용 프로그램. OnActivated**](/uwp/api/Windows.UI.Xaml.Application) 된 이벤트를 재정의 하 고 [**IActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs)여부를 확인 합니다. [**VoiceCommand**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind) [**종류**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs) 입니다.

2. 명령의 이름과 음성 정보를 확인 합니다.

    [**IActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs) 에서 [**VoiceCommandActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.VoiceCommandActivatedEventArgs) 개체에 대 한 참조를 가져오고 [**SpeechRecognitionResult**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) 개체의 [**Result**](/uwp/api/Windows.ApplicationModel.Activation.VoiceCommandActivatedEventArgs) 속성을 쿼리 합니다.

    사용자가 지정 하는 항목을 확인 하려면 [**SpeechRecognitionSemanticInterpretation**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) 사전에서 인식 된 구의 [**텍스트**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) 값 또는 의미 체계 속성을 확인 합니다.

3. 원하는 페이지로 이동 하는 것과 같이 앱에서 적절 한 작업을 수행 합니다.

이 예에서는 3 단계: VCD 파일 편집에서 VCD를 다시 참조 합니다.

음성 명령의 음성 인식 결과를 얻은 후에는 [**RulePath**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) 배열의 첫 번째 값에서 명령 이름을 가져옵니다. VCD 파일이 둘 이상의 가능한 음성 명령을 정의 했으므로 VCD의 명령 이름과 값을 비교 하 여 적절 한 조치를 취해야 합니다.

응용 프로그램에서 수행할 수 있는 가장 일반적인 작업은 음성 명령의 컨텍스트와 관련 된 콘텐츠가 있는 페이지로 이동 하는 것입니다. 이 예에서는 **TripPage** 페이지로 이동 하 여 음성 명령의 값, 명령이 입력 된 방법 및 인식 된 "대상" 구문 (해당 하는 경우)을 전달 합니다. 또는 페이지를 탐색할 때 앱이 [**SpeechRecognitionResult**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) 에 탐색 매개 변수를 보낼 수 있습니다.

[**SpeechRecognitionSemanticInterpretation**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) 사전에서 **commandmode** 키를 사용 하 여 앱을 시작한 음성 명령이 실제로 음성으로 입력 되었는지 또는 텍스트 형식으로 입력 되었는지 확인할 수 있습니다. 해당 키의 값은 "음성" 또는 "텍스트"입니다. 키 값이 "voice" 이면 앱에서 음성 합성 ([**SpeechSynthesis**](/uwp/api/Windows.Media.SpeechSynthesis))을 사용 하 여 사용자에 게 음성 피드백을 제공 하는 것이 좋습니다.

[**SpeechRecognitionSemanticInterpretation**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) 를 사용 하 여 **ListenFor** 요소의 **Phraselist** 또는 **phrasetopic** 제약 조건에서 음성으로 콘텐츠를 확인 합니다. 사전 키는 **Phraselist** 또는 **Phrasetopic** 요소의 **레이블** 특성 값입니다. 여기서는 **{destination}** 구 값에 액세스 하는 방법을 보여 줍니다.

```csharp
/// <summary>
/// Entry point for an application activated by some means other than normal launching. 
/// This includes voice commands, URI, share target from another app, and so on. 
/// 
/// NOTE:
/// A previous version of the VCD file might remain in place 
/// if you modify it and update the app through the store. 
/// Activations might include commands from older versions of your VCD. 
/// Try to handle these commands gracefully.
/// </summary>
/// <param name="args">Details about the activation method.</param>
protected override void OnActivated(IActivatedEventArgs args)
{
    base.OnActivated(args);

    Type navigationToPageType;
    ViewModel.TripVoiceCommand? navigationCommand = null;

    // Voice command activation.
    if (args.Kind == ActivationKind.VoiceCommand)
    {
        // Event args can represent many different activation types. 
        // Cast it so we can get the parameters we care about out.
        var commandArgs = args as VoiceCommandActivatedEventArgs;

        Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = commandArgs.Result;

        // Get the name of the voice command and the text spoken. 
        // See VoiceCommands.xml for supported voice commands.
        string voiceCommandName = speechRecognitionResult.RulePath[0];
        string textSpoken = speechRecognitionResult.Text;

        // commandMode indicates whether the command was entered using speech or text.
        // Apps should respect text mode by providing silent (text) feedback.
        string commandMode = this.SemanticInterpretation("commandMode", speechRecognitionResult);
        
        switch (voiceCommandName)
        {
            case "showTripToDestination":
                // Access the value of {destination} in the voice command.
                string destination = this.SemanticInterpretation("destination", speechRecognitionResult);

                // Create a navigation command object to pass to the page. 
                navigationCommand = new ViewModel.TripVoiceCommand(
                    voiceCommandName,
                    commandMode,
                    textSpoken,
                    destination);

                // Set the page to navigate to for this voice command.
                navigationToPageType = typeof(View.TripDetails);
                break;
            default:
                // If we can't determine what page to launch, go to the default entry point.
                navigationToPageType = typeof(View.TripListView);
                break;
        }
    }
    // Protocol activation occurs when a card is clicked within Cortana (using a background task).
    else if (args.Kind == ActivationKind.Protocol)
    {
        // Extract the launch context. In this case, we're just using the destination from the phrase set (passed
        // along in the background task inside Cortana), which makes no attempt to be unique. A unique id or 
        // identifier is ideal for more complex scenarios. We let the destination page check if the 
        // destination trip still exists, and navigate back to the trip list if it doesn't.
        var commandArgs = args as ProtocolActivatedEventArgs;
        Windows.Foundation.WwwFormUrlDecoder decoder = new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
        var destination = decoder.GetFirstValueByName("LaunchContext");

        navigationCommand = new ViewModel.TripVoiceCommand(
                                "protocolLaunch",
                                "text",
                                "destination",
                                destination);

        navigationToPageType = typeof(View.TripDetails);
    }
    else
    {
        // If we were launched via any other mechanism, fall back to the main page view.
        // Otherwise, we'll hang at a splash screen.
        navigationToPageType = typeof(View.TripListView);
    }

    // Repeat the same basic initialization as OnLaunched() above, taking into account whether
    // or not the app is already active.
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active.
    if (rootFrame == null)
    {
        // Create a frame to act as the navigation context and navigate to the first page.
        rootFrame = new Frame();
        App.NavigationService = new NavigationService(rootFrame);

        rootFrame.NavigationFailed += OnNavigationFailed;

        // Place the frame in the current window.
        Window.Current.Content = rootFrame;
    }

    // Since we're expecting to always show a details page, navigate even if 
    // a content frame is in place (unlike OnLaunched).
    // Navigate to either the main trip list page, or if a valid voice command
    // was provided, to the details page for that trip.
    rootFrame.Navigate(navigationToPageType, navigationCommand);

    // Ensure the current window is active
    Window.Current.Activate();
}

/// <summary>
/// Returns the semantic interpretation of a speech result. 
/// Returns null if there is no interpretation for that key.
/// </summary>
/// <param name="interpretationKey">The interpretation key.</param>
/// <param name="speechRecognitionResult">The speech recognition result to get the semantic interpretation from.</param>
/// <returns></returns>
private string SemanticInterpretation(string interpretationKey, SpeechRecognitionResult speechRecognitionResult)
{
  return speechRecognitionResult.SemanticInterpretation.Properties[interpretationKey].FirstOrDefault();
}
```

## <a name="related-articles"></a>관련된 문서

- [Windows 앱에서 Cortana 상호 작용](cortana-interactions.md)
- [음성 명령을 사용 하 여 Cortana에서 백그라운드 앱 활성화](cortana-launch-a-background-app-with-voice-commands.md)
- [VCD 요소 및 특성 v 1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana 디자인 지침](cortana-design-guidelines.md)
- [Cortana 음성 명령 샘플](https://go.microsoft.com/fwlink/p/?LinkID=619899)
