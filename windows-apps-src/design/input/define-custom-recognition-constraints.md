---
description: 음성 인식에 대 한 사용자 지정 제약 조건을 정의 하 고 사용 하는 방법에 대해 알아봅니다.
title: 사용자 지정 인식 제약 조건 정의
ms.assetid: 26289DE5-6AC9-42C3-A160-E522AE62D2FC
label: Define custom recognition constraints
template: detail.hbs
keywords: 음성, 음성, 음성 인식, 자연어, 받아쓰기, 입력, 사용자 조작
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5cef63bab911f46e34d337957011556a0c420763
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032166"
---
# <a name="define-custom-recognition-constraints"></a>사용자 지정 인식 제약 조건 정의

음성 인식에 대 한 사용자 지정 제약 조건을 정의 하 고 사용 하는 방법에 대해 알아봅니다.

> **중요 한 api** : [**SpeechRecognitionTopicConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint), [**SpeechRecognitionListConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint), [**SpeechRecognitionGrammarFileConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint)

음성 인식은 인식할 수 있는 어휘를 정의 하는 하나 이상의 제약 조건이 필요 합니다. 제약 조건을 지정 하지 않으면 유니버설 Windows 앱의 미리 정의 된 받아쓰기 문법이 사용 됩니다. [음성 인식](speech-recognition.md)을 참조 하세요.

## <a name="add-constraints"></a>제약 조건 추가

[**SpeechRecognizer**](/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) 속성을 사용 하 여 음성 인식기에 제약 조건을 추가 합니다.

여기서는 앱 내에서 사용 되는 세 가지 종류의 음성 인식 제약 조건을 다룹니다. Cortana 음성 명령 제약 조건의 경우 [cortana에서 음성 명령을 사용 하 여 포그라운드 앱 시작](/cortana/voice-commands/launch-a-foreground-app-with-voice-commands-in-cortana)을 참조 하세요.

- [**SpeechRecognitionTopicConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint)-미리 정의 된 문법 (받아쓰기 또는 웹 검색)을 기반으로 하는 제약 조건입니다.
- [**SpeechRecognitionListConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint)-단어 또는 구의 목록을 기반으로 하는 제약 조건입니다.
- [**SpeechRecognitionGrammarFileConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint)-음성 인식 문법 사양 (SRGS) 파일에 정의 된 제약 조건입니다.

각 음성 인식기에는 하나의 제약 조건 컬렉션이 있을 수 있습니다. 이러한 제약 조건 조합만 유효 합니다.

- 단일 항목 제약 조건 (받아쓰기 또는 웹 검색)
- Windows 10 10.0.16299.15 (Windows 10) 작성자 업데이트 () 이상에서는 단일 토픽 제약 조건을 list 제약 조건과 함께 사용할 수 있습니다.
- 목록 제약 조건 및/또는 문법 파일 제약 조건의 조합입니다.

> [!Important]
> 인식 프로세스를 시작 하기 전에 **[SpeechRecognizer CompileConstraintsAsync](/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync)** 메서드를 호출 하 여 제약 조건을 컴파일합니다.

## <a name="specify-a-web-search-grammar-speechrecognitiontopicconstraint"></a>웹 검색 문법 지정 (SpeechRecognitionTopicConstraint)

토픽 제약 조건 (받아쓰기 또는 웹 검색 문법)을 음성 인식기의 제약 조건 컬렉션에 추가 해야 합니다.

> [!NOTE]
> [SpeechRecognitionTopicConstraint](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint) 와 함께 [SpeechRecognitionListConstraint](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint) 를 사용 하 여 받아쓰기 중에 사용 될 가능성이 높은 도메인 특정 키워드 집합을 제공 함으로써 받아쓰기 정확도를 높일 수 있습니다.

여기서는 웹 검색 문법을 제약 조건 컬렉션에 추가 합니다.

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

## <a name="specify-a-programmatic-list-constraint-speechrecognitionlistconstraint"></a>프로그래밍 목록 제약 조건 지정 (SpeechRecognitionListConstraint)

음성 인식기의 제약 조건 컬렉션에 목록 제약 조건을 추가 해야 합니다.

다음 사항을 염두에 두어야 합니다.

- 제약 조건 컬렉션에 여러 list 제약 조건을 추가할 수 있습니다.
- 문자열 값으로 **iiterable<t> &lt; string &gt;** 을 구현 하는 모든 컬렉션을 사용할 수 있습니다.

여기서는 프로그래밍 방식으로 단어 배열을 목록 제약 조건으로 지정 하 고이를 음성 인식기의 제약 조건 컬렉션에 추가 합니다.

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

## <a name="specify-an-srgs-grammar-constraint-speechrecognitiongrammarfileconstraint"></a>SpeechRecognitionGrammarFileConstraint (SRGS 문법 제약 조건) 지정

SRGS 문법 파일은 음성 인식기의 제약 조건 컬렉션에 추가 해야 합니다.

SRGS 버전 1.0는 음성 인식에 대 한 XML 형식 문법을 만들기 위한 업계 표준 태그 언어입니다. 유니버설 Windows 앱은 음성 인식 문법을 만드는 데 SRGS를 사용 하는 대신 사용할 수 있는 대안을 제공 하지만 SRGS를 사용 하 여 문법 만들기는 특히 더 많은 음성 인식 시나리오에 적합 한 최상의 결과를 생성 하는 것을 알 수 있습니다.

SRGS 문법은 앱에 대 한 복잡 한 음성 상호 작용을 설계 하는 데 도움이 되는 전체 기능 집합을 제공 합니다. 예를 들어 SRGS 문법을 사용 하면 다음을 수행할 수 있습니다.

- 단어와 구를 인식 하기 위해 음성 해야 하는 순서를 지정 합니다.
- 여러 목록과 구의 단어를 조합 하 여 인식 합니다.
- 다른 문법에 대 한 링크입니다.
- 대체 단어나 구에 가중치를 할당 하 여 음성 입력을 일치 시키는 데 사용 될 가능성을 늘리거나 줄입니다.
- 선택적 단어 또는 문구를 포함 합니다.
- 문법에 일치 하지 않는 임의 음성 또는 배경 노이즈와 같이 지정 되지 않거나 예기치 않은 입력을 필터링 하는 데 도움이 되는 특수 규칙을 사용 합니다.
- 의미 체계를 사용 하 여 앱에 대 한 음성 인식 의미를 정의 합니다.
- 문법에서 인라인 또는 어휘에 대 한 링크를 통해 발음를 지정 합니다.

SRGS 요소 및 특성에 대 한 자세한 내용은 [SRGS 문법 XML 참조](/previous-versions/office/developer/speech-technologies/hh361653(v=office.14)) 를 참조 하세요. SRGS 문법을 만들기 시작 하려면 [How To Create a BASIC XML grammar](/previous-versions/office/developer/speech-technologies/hh361658(v=office.14))항목을 참조 하세요.

다음 사항을 염두에 두어야 합니다.

- 제약 조건 컬렉션에 여러 문법 파일 제약 조건을 추가할 수 있습니다.
- SRGS 규칙을 준수 하는 XML 기반 문법 문서에는 grxml 파일 확장명을 사용 합니다.

이 예제에서는 SRGS 라는 파일에 정의 된 SRGS 문법을 사용 합니다 (뒷부분에서 설명). 파일 속성에서 **패키지 작업** 은 **출력 디렉터리로 복사** 가 **항상 복사** 로 설정 된 **콘텐츠로** 설정 됩니다.

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

이 SRGS 파일 (SRGS)은 의미 체계 해석 태그를 포함 합니다. 이러한 태그는 응용 프로그램에 문법을 일치 하는 데이터를 반환 하는 메커니즘을 제공 합니다. 문법은 [SISR (음성 인식) 1.0 사양에 대해](https://www.w3.org/TR/semantic-interpretation/) W3C (World Wide Web 컨소시엄) 의미 체계 해석을 준수 해야 합니다.

여기서는 "yes" 및 "no"의 변형을 수신 대기 합니다.

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

인식을 위해 제약 조건 컬렉션을 로드 한 후에는 제약 조건의 [**IsEnabled**](/uwp/api/windows.media.speechrecognition.ispeechrecognitionconstraint.isenabled) 속성을 **true** 또는 **false** 로 설정 하 여 인식 작업에 사용할 제약 조건을 앱에서 관리할 수 있습니다. 기본 설정은 **true** 입니다.

일반적으로 제약 조건을 한 번 로드 하 고 각 인식 작업에 대 한 로드, 언로드 및 컴파일 제약 조건을 사용 하지 않고 필요에 따라 사용 하지 않도록 설정 하는 것이 더 효율적입니다. 필요에 따라 [**IsEnabled**](/uwp/api/windows.media.speechrecognition.ispeechrecognitionconstraint.isenabled) 속성을 사용 합니다.

음성 인식기가 음성 입력에 대해 검색 하 고 일치 해야 하는 데이터의 양을 제한 하기 위해 제약 조건 수를 제한 합니다. 그러면 음성 인식의 성능과 정확도를 모두 높일 수 있습니다.

앱이 현재 인식 작업의 컨텍스트에서 사용할 수 있는 구에 따라 사용할 제약 조건을 결정 합니다. 예를 들어 현재 응용 프로그램 컨텍스트가 색을 표시 하는 경우 동물의 이름을 인식 하는 제약 조건을 설정할 필요가 없을 것입니다.

사용자에 게 말할 수 있는 항목을 묻는 메시지를 표시 하려면 [**SpeechRecognizer**](/uwp/api/windows.media.speechrecognition.speechrecognizer.uioptions) 속성을 사용 하 여 설정 된 AudiblePrompt 및 [**SpeechRecognizerUIOptions**](/uwp/api/windows.media.speechrecognition.speechrecognizeruioptions.exampletext) 속성을 사용 [**SpeechRecognizerUIOptions**](/uwp/api/windows.media.speechrecognition.speechrecognizeruioptions.audibleprompt) . 사용자가 인식 작업 중에 말할 수 있는 사항을 준비 하면 활성 제약 조건에 일치 시킬 수 있는 구를 말할 가능성이 높아집니다.

## <a name="related-articles"></a>관련된 문서

- [음성 조작](speech-interactions.md)

### <a name="samples"></a>샘플

- [음성 인식 및 음성 합성 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
