---
Description: 음성 인식에 대한 사용자 지정 제약 조건을 정의하고 사용하는 방법을 알아봅니다.
title: 사용자 지정 인식 제약 조건 정의
ms.assetid: 26289DE5-6AC9-42C3-A160-E522AE62D2FC
label: Define custom recognition constraints
template: detail.hbs
keywords: 음성 명령, 목소리, 음성 인식, 자연어, 받아쓰기, 입력, 사용자 조작
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4bb24002e3738213ba3e784e6b91ff55d970a26a
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66363624"
---
# <a name="define-custom-recognition-constraints"></a>사용자 지정 인식 제약 조건 정의

음성 인식에 대한 사용자 지정 제약 조건을 정의하고 사용하는 방법을 알아봅니다.

> **중요 API**: [**SpeechRecognitionTopicConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint), [**SpeechRecognitionListConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint), [**SpeechRecognitionGrammarFileConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint)

인식할 수 있는 어휘를 정의하려면 음성 인식에 하나 이상의 제약 조건이 필요합니다. 제약 조건을 지정하지 않으면 유니버설 Windows 앱의 미리 정의된 받아쓰기 문법이 사용됩니다. [음성 인식](speech-recognition.md)을 참조하세요.

## <a name="add-constraints"></a>제약 조건 추가

[  **SpeechRecognizer.Constraints**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) 속성을 사용하여 음성 인식기에 제약 조건을 추가합니다.

여기서는 앱 내에서 사용되는 세 종류의 음성 인식 제약 조건에 대해 설명합니다. (Cortana 음성 명령 제약 조건 참조 [Cortana에서 음성 명령 사용 하 여 포그라운드 앱을 시작](https://docs.microsoft.com/cortana/voice-commands/launch-a-foreground-app-with-voice-commands-in-cortana).)

- [**SpeechRecognitionTopicConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint)-미리 정의 된 문법 (받아쓰기 또는 웹 검색)를 기반으로 제약 조건입니다.
- [**SpeechRecognitionListConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint)-단어 또는 문구의 목록에 따라 제약 조건입니다.
- [**SpeechRecognitionGrammarFileConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint)-음성 인식 문법 Specification (SRGS) 파일에 정의 된 제약 조건입니다.

각 음성 인식기에 하나의 제약 조건 컬렉션을 사용할 수 있습니다. 다음과 같은 제약 조건 조합만 유효합니다.

- 단일 항목 제한 조건(받아쓰기 또는 웹 검색)
- Windows 10 Fall Creators Update(10.0.16299.15) 이상의 경우, 단일 항목 제한 조건을 목록 제한 조건과 결합할 수 있습니다.
- 목록 제약 조건 및/또는 문법 파일 제약 조건 조합

> [!Important]
> 인식 프로세스를 시작하기 전에 **[SpeechRecognizer.CompileConstraintsAsync](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync)** 메서드를 호출하여 제약 조건을 컴파일합니다.

## <a name="specify-a-web-search-grammar-speechrecognitiontopicconstraint"></a>웹 검색 문법(SpeechRecognitionTopicConstraint) 지정

음성 인식기의 제약 조건 컬렉션에 항목 제약 조건(받아쓰기 또는 웹 검색 문법)을 추가해야 합니다.

> [!NOTE]
> 이제 [SpeechRecognitionTopicConstraint](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint)와 함께 [SpeechRecognitionListConstraint](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint)를 사용하여 받아쓰는 동안 사용될 것으로 생각되는 도메인별 키워드 세트를 제공하여 받아쓰기 정확도를 향상시킬 수 있습니다.

여기서는 제약 조건 컬렉션에 웹 검색 문법을 추가합니다.

```CSharp
private async void WeatherSearch_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Listen for audio input issues.
    speechRecognizer.RecognitionQualityDegrading += speechRecognizer_RecognitionQualityDegrading;

    // Add a web search grammar to the recognizer.
    var webSearchGrammar = new Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint(Windows.Media.SpeechRecognition.SpeechRecognitionScenario.WebSearch, "webSearch");


    speechRecognizer.UIOptions.AudiblePrompt = "Say what you want to search for...";
    speechRecognizer.UIOptions.ExampleText = @"Ex. 'weather for London'";
    speechRecognizer.Constraints.Add(webSearchGrammar);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();
    //await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="specify-a-programmatic-list-constraint-speechrecognitionlistconstraint"></a>프로그래밍 방식으로 목록 제약 조건(SpeechRecognitionListConstraint) 지정

음성 인식기의 제약 조건 컬렉션에 목록 제약 조건을 추가해야 합니다.

다음 사항을 기억하세요.

- 제약 조건 컬렉션에 여러 목록 제약 조건을 추가할 수 있습니다.
- 문자열 값에 대한 **IIterable&lt;String&gt;** 을 구현하는 모든 컬렉션을 사용할 수 있습니다.

여기서는 프로그래밍 방식으로 단어 배열을 목록 제약 조건으로 지정하고 음성 인식기의 제약 조건 컬렉션에 추가합니다.

```CSharp
private async void YesOrNo_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // You could create this array dynamically.
    string[] responses = { "Yes", "No" };


    // Add a list constraint to the recognizer.
    var listConstraint = new Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint(responses, "yesOrNo");

    speechRecognizer.UIOptions.ExampleText = @"Ex. 'yes', 'no'";
    speechRecognizer.Constraints.Add(listConstraint);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="specify-an-srgs-grammar-constraint-speechrecognitiongrammarfileconstraint"></a>SRGS 문법 제약 조건(SpeechRecognitionGrammarFileConstraint) 지정

음성 인식기의 제약 조건 컬렉션에 SRGS 문법 파일을 추가해야 합니다.

SRGS 버전 1.0은 음성 인식에 대한 XML 형식 문법을 만들기 위한 산업 표준 생성 언어입니다. 유니버설 Windows 앱에서는 음성 인식 문법을 만들 때 SRGS 외에 다른 방법을 사용할 수도 있지만, 특히 더 복잡한 음성 인식 시나리오의 경우 SRGS를 사용하여 문법을 만드는 방법이 최상의 결과를 생성한다는 것을 알 수 있습니다.

SRGS 문법은 앱에 대한 복잡한 음성 조작을 구성하는 데 도움이 되는 전체 기능 집합을 제공합니다. 예를 들어 SRGS 문법을 사용하여 다음을 수행할 수 있습니다.

- 인식되는 단어와 구문을 말해야 하는 순서를 지정합니다.
- 인식되는 여러 목록과 구문의 단어를 조합합니다.
- 다른 문법에 연결합니다.
- 대체 단어나 구문에 가중치를 할당하여 음성 입력과 일치하는 데 사용될 가능성을 높이거나 낮춥니다.
- 선택적 단어나 구문을 포함합니다.
- 문법과 일치하지 않는 임의 음성이나 백그라운드 노이즈와 같은 지정되지 않거나 예상하지 못한 입력을 필터링하는 데 도움이 되는 특수 규칙을 사용합니다.
- 의미 체계를 사용하여 음성 인식이 앱에 어떤 의미인지를 정의합니다.
- 문법 또는 lexicon 링크를 통해 인라인으로 발음을 지정합니다.

SRGS 요소 및 특성에 대한 자세한 내용은 [SRGS 문법 XML 참고](https://go.microsoft.com/fwlink/p/?LinkID=269886)를 참조하세요. SRGS 문법 만들기를 시작하려면 [기본 XML 문법을 만드는 방법](https://go.microsoft.com/fwlink/p/?LinkID=269887)을 참조하세요.

다음 사항을 기억하세요.

- 제약 조건 컬렉션에 여러 문법 파일 제약 조건을 추가할 수 있습니다.
- SRGS 규칙을 준수하는 XML 기반 문법 문서에 .grxml 파일 확장명을 사용합니다.

이 예제에서는 srgs.grxml이라는 파일에 정의된 SRGS 문법을 사용합니다(나중에 설명). 파일 속성에서 **패키지 동작**은 **콘텐츠**로 설정되고 **출력 디렉터리로 복사**는 **항상 복사**로 설정됩니다.

```CSharp
private async void Colors_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Add a grammar file constraint to the recognizer.
    var storageFile = await Windows.Storage.StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///Colors.grxml"));
    var grammarFileConstraint = new Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint(storageFile, "colors");

    speechRecognizer.UIOptions.ExampleText = @"Ex. 'blue background', 'green text'";
    speechRecognizer.Constraints.Add(grammarFileConstraint);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

이 SRGS 파일(srgs.grxml)에는 의미 해석 태그도 포함되어 있습니다. 이러한 태그는 앱에 문법 일치 데이터를 반환하는 메커니즘을 제공합니다. World Wide Web Consortium (W3C) 문법 따라야 [의미 해석에 대 한 음성 인식 (SISR) 1.0](https://go.microsoft.com/fwlink/p/?LinkID=201765) 사양입니다.

여기서는 "yes" 및 "no"의 변형을 수신 대기합니다.

```CSharp
<grammar xml:lang="en-US" 
         root="yesOrNo"
         version="1.0" 
         tag-format="semantics/1.0"
         xmlns="http://www.w3.org/2001/06/grammar">

    <!-- The following rules recognize variants of yes and no. -->
      <rule id="yesOrNo">
         <one-of>
            <item>
              <one-of>
                 <item>yes</item>
                 <item>yeah</item>
                 <item>yep</item>
                 <item>yup</item>
                 <item>un huh</item>
                 <item>yay yus</item>
              </one-of>
              <tag>out="yes";</tag>
            </item>
            <item>
              <one-of>
                 <item>no</item>
                 <item>nope</item>
                 <item>nah</item>
                 <item>uh uh</item>
               </one-of>
               <tag>out="no";</tag>
            </item>
         </one-of>
      </rule>
</grammar>
```

## <a name="manage-constraints"></a>제약 조건 관리

인식에 대한 제약 조건 컬렉션이 로드된 후 앱은 제약 조건의 [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.ispeechrecognitionconstraint.isenabled) 속성을 **true** 또는 **false**로 설정하여 인식 작업에 어떤 제약 조건을 사용할지 관리할 수 있습니다. 기본 설정은 **true**입니다.

일반적으로 인식 작업별로 제약 조건을 로드, 언로드 및 컴파일하는 것이 아니라 제약 조건을 한 번 로드한 다음 필요에 따라 사용하거나 사용하지 않도록 설정하는 것이 더 효율적입니다. 필요한 경우 [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.ispeechrecognitionconstraint.isenabled) 속성을 사용합니다.

제약 조건 수를 제한하여 음성 인식기에서 음성 입력의 일치 항목을 찾는 데 필요한 데이터 양을 제한합니다. 이렇게 하면 음성 인식의 성능과 정확도를 모두 향상시킬 수 있습니다.

현재 인식 작업의 컨텍스트에서 앱에 필요할 수 있는 구에 따라 어떤 제약 조건을 사용할지 결정합니다. 예를 들어 현재 앱 컨텍스트가 색을 표시하는 것일 경우 동물 이름을 인식하는 제약 조건을 사용할 필요가 없습니다.

말할 수 있는 내용을 사용자에게 메시지로 표시하려면 [**SpeechRecognizer.UIOptions**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizeruioptions.audibleprompt) 속성을 통해 설정되는 [**SpeechRecognizerUIOptions.AudiblePrompt**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizeruioptions.exampletext) 및 [**SpeechRecognizerUIOptions.ExampleText**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.uioptions) 속성을 사용합니다. 사용자가 인식 작업 중 말할 수 있는 내용을 알면 활성 제약 조건과 일치할 수 있는 구를 말할 가능성이 증가합니다.

## <a name="related-articles"></a>관련 문서

- [음성 조작](speech-interactions.md)

### <a name="samples"></a>샘플

- [음성 인식 및 음성 합성 샘플](https://go.microsoft.com/fwlink/p/?LinkID=619897)