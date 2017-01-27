---
author: Karl-Bridge-Microsoft
Description: "음성 인식 기능을 사용하여 입력을 제공하고, 동작이나 명령을 지정하고 작업을 수행합니다."
title: "음성 인식"
ms.assetid: 553C0FB7-35BC-4894-9EF1-906139E17552
label: Speech recognition
template: detail.hbs
keywords: "음성 명령, 목소리, 음성 인식, 자연어, 받아쓰기, 입력, 사용자 조작"
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: 482530931fe5764f65d2564107318c272c5c7b7f
ms.openlocfilehash: a4de0955eb6bd01ef5279b5b8d553fe1d1dd50f2

---

# <a name="speech-recognition"></a>음성 인식
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

음성 인식 기능을 사용하여 입력을 제공하고, 동작이나 명령을 지정하고 작업을 수행합니다.

<div class="important-apis" >
<b>중요 API</b><br/>
<ul>
<li>[**Windows.Media.SpeechRecognition**](https://msdn.microsoft.com/library/windows/apps/dn653262)</li>
</ul>
</div>

음성 인식은 음성 런타임, 런타임을 프로그래밍하기 위한 인식 API, 바로 사용할 수 있는 받아쓰기 및 웹 검색 문법, 사용자가 음성 인식 기능을 검색하고 사용하는 데 도움이 되는 기본 시스템 UI로 구성됩니다.


## <a name="set-up-the-audio-feed"></a>오디오 피드 설정


장치에 마이크 또는 이와 동등한 요소가 있는지 확인합니다.

마이크의 오디오 피드에 액세스할 수 있도록 [앱 패키지 매니페스트](https://msdn.microsoft.com/library/windows/apps/br211430)(**package.appxmanifest** 파일)의 **마이크** 장치 기능([**DeviceCapability**](https://msdn.microsoft.com/library/windows/apps/br211474))을 설정합니다. 이렇게 하면 앱이 연결된 마이크에서 오디오를 녹음할 수 있습니다.

[앱 기능 선언](https://msdn.microsoft.com/library/windows/apps/mt270968)을 참조하세요.

## <a name="recognize-speech-input"></a>음성 입력 인식


*제약 조건*은 앱이 음성 입력에서 인식하는 단어와 구(어휘)를 정의합니다. 제약 조건은 음성 인식의 핵심이며 앱의 음성 인식 정확도를 높입니다.

음성 인식을 수행할 때 다음과 같은 다양한 형식의 제약 조건을 사용할 수 있습니다.

1.  **미리 정의된 문법**([**SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631446)).

    미리 정의된 받아쓰기 및 웹 검색 문법은 사용자가 문법을 작성할 필요 없이 앱에 대한 음성 인식 기능을 제공합니다. 이러한 문법을 사용할 때 음성 인식은 원격 웹 서비스에서 수행되고 결과가 장치로 반환됩니다.

    기본 자유 텍스트 받아쓰기 문법은 사용자가 특정 언어로 말할 수 있는 대부분의 단어와 구를 인식할 수 있으며 짧은 구를 인식하도록 최적화되어 있습니다. [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) 개체에 대한 제약 조건을 지정하지 않으면 미리 정의된 받아쓰기 문법이 사용됩니다. 자유 텍스트 받아쓰기는 사용자가 말할 수 있는 내용을 제한하지 않으려는 경우에 유용합니다. 일반적인 사용에는 노트 만들기나 메시지 내용 받아쓰기가 포함됩니다.

    받아쓰기 문법과 같은 웹 검색 문법에는 사용자가 말할 수 있는 매우 많은 단어 및 구가 포함되어 있습니다. 그러나 웹 검색 문법은 사람들이 일반적으로 웹을 검색할 때 사용하는 용어를 인식하도록 최적화되어 있습니다.

    **참고**  미리 정의된 받아쓰기 및 웹 검색 문법은 용량이 클 수 있으며 장치가 아니라 온라인상에 있으므로 장치에 설치된 사용자 지정 문법만큼 성능이 빠르지 않을 수 있습니다.

     

    이러한 미리 정의된 문법은 최대 10초의 음성 입력을 인식하는 데 사용할 수 있으며 특별한 작성 작업이 필요하지 않습니다. 그러나 네트워크에 연결되어 있어야 합니다.

    웹 서비스 제약 조건을 사용하려면 Settings -&gt; Privacy -&gt; Speech, inking, and typing 페이지에서 내 정보 표시 옵션을 설정하여 **설정**에서 음성 입력 및 받아쓰기 지원을 사용하도록 설정해야 합니다.

    여기에서는 음성 입력이 사용되도록 설정되어 있는지 테스트하고 설정되어 있지 않으면 Settings -&gt; Privacy -&gt; Speech, inking, and typing 페이지를 여는 방법을 보여 줍니다.

    먼저, 전역 변수(HResultPrivacyStatementDeclined)를 0x80045509의 HResult 값으로 초기화합니다. [C\# 또는 Visual Basic으로 작성된 예외 처리](https://msdn.microsoft.com/library/windows/apps/dn532194)를 참조하세요.

```    CSharp
private static uint HResultPrivacyStatementDeclined = 0x80045509;</code></pre></td>
    </tr>
    </tbody>
    </table>
```

    We then catch any standard exceptions during recogntion and test if the [**HResult**](https://msdn.microsoft.com/library/windows/apps/br206579) value is equal to the value of the HResultPrivacyStatementDeclined variable. If so, we display a warning and call `await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts"));` to open the Settings page.

    
```    CSharp
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">C#</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
catch (Exception exception)
    {
      // Handle the speech privacy policy error.
      if ((uint)exception.HResult == HResultPrivacyStatementDeclined)
      {
        resultTextBlock.Visibility = Visibility.Visible;
        resultTextBlock.Text = "The privacy statement was declined. 
          Go to Settings -> Privacy -> Speech, inking and typing, and ensure you 
          have viewed the privacy policy, and &#39;Get To Know You&#39; is enabled.";
        // Open the privacy/speech, inking, and typing settings page.
        await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts")); 
      }
      else
      {
        var messageDialog = new Windows.UI.Popups.MessageDialog(exception.Message, "Exception");
        await messageDialog.ShowAsync();
      }
    }
```

2.  **프로그래밍 방식 목록 제약 조건**([**SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631421))

    프로그래밍 방식 목록 제약 조건은 단어 또는 구 목록을 사용하여 간단한 문법을 만드는 가벼운 방법을 제공합니다. 짧고 고유한 구를 인식하는 데는 목록 제약 조건이 유용합니다. 음성 인식 엔진이 일치를 확인하기 위해서만 음성을 처리해야 하므로 문법의 모든 단어를 명시적으로 지정하면 인식 정확도도 향상됩니다. 또한 목록은 프로그래밍 방식으로도 업데이트할 수 있습니다.

    목록 제약 조건은 앱이 인식 작업에 대해 받아들이는 음성 입력을 나타내는 문자열 배열로 구성됩니다. 음성 인식 목록 제약 조건 개체를 만들고 문자열 배열을 전달하여 앱에서 목록 제약 조건을 만들 수 있습니다. 그런 다음 인식기 제약 조건 컬렉션에 해당 개체를 추가합니다. 음성 인식기가 배열에 있는 문자열 중 하나를 인식하면 인식에 성공합니다.

3.  **SRGS 문법**([**SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412))

    SRGS(Speech Recognition Grammar Specification) 문법은 프로그래밍 방식 목록 제약 조건과 달리 [SRGS 버전 1.0](http://go.microsoft.com/fwlink/p/?LinkID=262302)에서 정의한 XML 형식을 사용하는 정적 문서입니다. SRGS 문법을 사용하면 단일 인식에서 여러 시맨틱 의미를 캡처할 수 있으므로 음성 인식 환경을 가장 잘 제어할 수 있습니다.

4.  **음성 명령 제약 조건**([**SpeechRecognitionVoiceCommandDefinitionConstraint**](https://msdn.microsoft.com/library/windows/apps/dn653220))

    VCD(음성 명령 정의) XML 파일을 사용하여 사용자가 앱을 활성화할 때 동작을 시작하기 위해 말할 수 있는 명령을 정의합니다. 자세한 내용은 [Cortana에서 음성 명령으로 포그라운드 앱 시작](launch-a-foreground-app-with-voice-commands-in-cortana.md)을 참조하세요.

**참고**  사용할 제약 조건 유형은 만들려는 인식 환경의 복잡성에 따라 다릅니다. 어떤 유형이나 특정 인식 작업에 가장 적합한 선택이 될 수 있으며, 앱에서 모든 제약 조건 유형의 용도를 찾을 수 있습니다.
제약 조건을 시작하려면 [사용자 지정 인식 제약 조건 정의](define-custom-recognition-constraints.md)을 참조하세요.

 

미리 정의된 유니버설 Windows 앱 받아쓰기 문법은 언어의 단어와 짧은 구를 대부분 인식합니다. 사용자 지정 제약 조건 없이 음성 인식기 개체를 인스턴스화할 때 기본적으로 활성화됩니다.

이 예제에서는 다음 작업을 수행하는 방법을 보여 줍니다.

-   음성 인식기 만들기
-   기본 유니버설 Windows 앱 제약 조건 컴파일(음성 인식기의 문법 집합에 추가된 문법 없음)
-   기본 인식 UI 및 [**RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245) 메서드에서 제공하는 TTS 피드백을 사용하여 음성 수신 대기 시작 기본 UI가 필요하지 않은 경우 [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn653244) 메서드를 사용합니다.

```CSharp
private async void StartRecognizing_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Compile the dictation grammar by default.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="customize-the-recognition-ui"></a>인식 UI 사용자 지정


앱에서 [**SpeechRecognizer.RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245)를 호출하여 음성 인식을 시도하는 경우 여러 화면이 다음과 같은 순서로 표시됩니다.

미리 정의된 문법(받아쓰기 또는 웹 검색)을 기반으로 한 제약 조건을 사용하는 경우

-   **Listening** 화면
-   **생각하는 중** 화면
-   **Heard you say** 화면 또는 오류 화면

단어 또는 구 목록을 기반으로 한 제약 조건이나 SRGS 문법 파일을 기반으로 한 제약 조건을 사용하는 경우

-   **Listening** 화면
-   **확인 내용** 화면 - 사용자가 말한 내용을 둘 이상의 가능한 결과로 해석할 수 있는 경우
-   **Heard you say** 화면 또는 오류 화면

아래 이미지는 SRGS 문법 파일을 기반으로 한 제약 조건을 사용하는 음성 인식기 화면 간의 흐름 예제를 보여 줍니다. 이 예제에서는 음성 인식이 성공적이었습니다.

![SGRS 문법 파일을 기반으로 한 제약 조건의 초기 인식 화면](images/speech-listening-initial.png)

![SGRS 문법 파일을 기반으로 한 제약 조건의 중간 인식 화면](images/speech-listening-intermediate.png)

![SGRS 문법 파일을 기반으로 한 제약 조건의 최종 인식 화면](images/speech-listening-complete.png)

**Listening** 화면에는 앱에서 인식할 수 있는 단어 또는 구 예제가 제공될 수 있습니다. 여기에서는 [**SpeechRecognizerUIOptions**](https://msdn.microsoft.com/library/windows/apps/dn653234) 클래스([**SpeechRecognizer.UIOptions**](https://msdn.microsoft.com/library/windows/apps/dn653254) 속성을 호출하여 얻음)의 속성을 사용해서 **Listening** 화면에서 콘텐츠를 사용자 지정하는 방법을 보여 줍니다.

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
    speechRecognizer.UIOptions.ExampleText = @"Ex. &#39;weather for London&#39;";
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

## <a name="related-articles"></a>관련 문서


**개발자**
* [음성 조작](speech-interactions.md)
**디자이너**
* [음성 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn596121)
**샘플**
* [음성 인식 및 음성 합성 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 







<!--HONumber=Dec16_HO3-->


