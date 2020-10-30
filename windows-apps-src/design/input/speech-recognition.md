---
description: 음성 인식을 사용 하 여 입력을 제공 하 고, 작업 또는 명령을 지정 하 고, 작업을 수행할 수 있습니다.
title: 음성 인식
ms.assetid: 553C0FB7-35BC-4894-9EF1-906139E17552
label: Speech recognition
template: detail.hbs
keywords: 음성, 음성, 음성 인식, 자연어, 받아쓰기, 입력, 사용자 조작
ms.date: 10/25/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ad721bc64de87fc8bb1a56f687860738bebed56c
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030056"
---
# <a name="speech-recognition"></a>음성 인식


음성 인식을 사용 하 여 입력을 제공 하 고, 작업 또는 명령을 지정 하 고, 작업을 수행할 수 있습니다.

> **중요 한 api** : [ **SpeechRecognition**](/uwp/api/Windows.Media.SpeechRecognition)

음성 인식은 음성 인식, 받아쓰기 및 웹 검색에 대 한 바로 사용할 수 있는 문법 및 사용자가 음성 인식 기능을 검색 하 고 사용 하는 데 도움이 되는 기본 시스템 UI를 프로그래밍 하기 위한 인식 Api로 구성 됩니다.

## <a name="configure-speech-recognition"></a>음성 인식 구성

앱에서 음성 인식을 지원 하려면 사용자가 장치에서 마이크를 연결 하 고 사용 하도록 설정 하 고 앱에 사용 권한을 부여 하는 Microsoft 개인 정보 취급 방침에 동의 해야 합니다.

사용자에 게 마이크의 오디오 피드에 액세스 하 고 사용할 수 있는 권한을 요청 하는 시스템 대화 상자를 자동으로 표시 하려면 (예: 아래에 표시 된 [음성 인식 및 음성 합성 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis) 에서) [앱 패키지 매니페스트에서](/uwp/schemas/appxpackage/appx-package-manifest) **마이크** [장치 기능](/uwp/schemas/appxpackage/appxmanifestschema/element-devicecapability) 을 설정 합니다. 자세한 내용은 [앱 기능 선언](../../packaging/app-capability-declarations.md)을 참조 하세요.

![마이크 액세스를 위한 개인 정보 취급 방침](images/speech/privacy.png)

사용자가 예를 클릭 하 여 마이크에 대 한 액세스 권한을 부여 하는 경우 설정-> 개인 정보-> 마이크 페이지의 승인 된 응용 프로그램 목록에 앱이 추가 됩니다. 그러나 사용자가 언제 든 지이 설정을 해제 하도록 선택할 수 있으므로 앱을 사용 하기 전에 앱이 마이크에 액세스할 수 있는지 확인 해야 합니다.

또한 받아쓰기, Cortana 또는 기타 음성 인식 서비스 (토픽 제약 조건에 정의 된 [미리 정의 된 문법](#predefined-grammars) )를 지원 하려는 경우 **온라인 음성 인식** (설정-> 개인 정보-> 음성)이 사용 되도록 설정 되어 있는지도 확인 해야 합니다.

이 코드 조각은 앱에서 마이크가 있는지 여부와 해당 마이크가 사용할 권한이 있는지 여부를 확인 하는 방법을 보여 줍니다.

```csharp
public class AudioCapturePermissions
{
    // If no microphone is present, an exception is thrown with the following HResult value.
    private static int NoCaptureDevicesHResult = -1072845856;

    /// <summary>
    /// Note that this method only checks the Settings->Privacy->Microphone setting, it does not handle
    /// the Cortana/Dictation privacy check.
    ///
    /// You should perform this check every time the app gets focus, in case the user has changed
    /// the setting while the app was suspended or not in focus.
    /// </summary>
    /// <returns>True, if the microphone is available.</returns>
    public async static Task<bool> RequestMicrophonePermission()
    {
        try
        {
            // Request access to the audio capture device.
            MediaCaptureInitializationSettings settings = new MediaCaptureInitializationSettings();
            settings.StreamingCaptureMode = StreamingCaptureMode.Audio;
            settings.MediaCategory = MediaCategory.Speech;
            MediaCapture capture = new MediaCapture();

            await capture.InitializeAsync(settings);
        }
        catch (TypeLoadException)
        {
            // Thrown when a media player is not available.
            var messageDialog = new Windows.UI.Popups.MessageDialog("Media player components are unavailable.");
            await messageDialog.ShowAsync();
            return false;
        }
        catch (UnauthorizedAccessException)
        {
            // Thrown when permission to use the audio capture device is denied.
            // If this occurs, show an error or disable recognition functionality.
            return false;
        }
        catch (Exception exception)
        {
            // Thrown when an audio capture device is not present.
            if (exception.HResult == NoCaptureDevicesHResult)
            {
                var messageDialog = new Windows.UI.Popups.MessageDialog("No Audio Capture devices are present on this system.");
                await messageDialog.ShowAsync();
                return false;
            }
            else
            {
                throw;
            }
        }
        return true;
    }
}
```

```cpp
/// <summary>
/// Note that this method only checks the Settings->Privacy->Microphone setting, it does not handle
/// the Cortana/Dictation privacy check.
///
/// You should perform this check every time the app gets focus, in case the user has changed
/// the setting while the app was suspended or not in focus.
/// </summary>
/// <returns>True, if the microphone is available.</returns>
IAsyncOperation<bool>^  AudioCapturePermissions::RequestMicrophonePermissionAsync()
{
    return create_async([]() 
    {
        try
        {
            // Request access to the audio capture device.
            MediaCaptureInitializationSettings^ settings = ref new MediaCaptureInitializationSettings();
            settings->StreamingCaptureMode = StreamingCaptureMode::Audio;
            settings->MediaCategory = MediaCategory::Speech;
            MediaCapture^ capture = ref new MediaCapture();

            return create_task(capture->InitializeAsync(settings))
                .then([](task<void> previousTask) -> bool
            {
                try
                {
                    previousTask.get();
                }
                catch (AccessDeniedException^)
                {
                    // Thrown when permission to use the audio capture device is denied.
                    // If this occurs, show an error or disable recognition functionality.
                    return false;
                }
                catch (Exception^ exception)
                {
                    // Thrown when an audio capture device is not present.
                    if (exception->HResult == AudioCapturePermissions::NoCaptureDevicesHResult)
                    {
                        auto messageDialog = ref new Windows::UI::Popups::MessageDialog("No Audio Capture devices are present on this system.");
                        create_task(messageDialog->ShowAsync());
                        return false;
                    }

                    throw;
                }
                return true;
            });
        }
        catch (Platform::ClassNotRegisteredException^ ex)
        {
            // Thrown when a media player is not available. 
            auto messageDialog = ref new Windows::UI::Popups::MessageDialog("Media Player Components unavailable.");
            create_task(messageDialog->ShowAsync());
            return create_task([] {return false; });
        }
    });
}
```

```js
var AudioCapturePermissions = WinJS.Class.define(
    function () { }, {},
    {
        requestMicrophonePermission: function () {
            /// <summary>
            /// Note that this method only checks the Settings->Privacy->Microphone setting, it does not handle
            /// the Cortana/Dictation privacy check.
            ///
            /// You should perform this check every time the app gets focus, in case the user has changed
            /// the setting while the app was suspended or not in focus.
            /// </summary>
            /// <returns>True, if the microphone is available.</returns>
            return new WinJS.Promise(function (completed, error) {

                try {
                    // Request access to the audio capture device.
                    var captureSettings = new Windows.Media.Capture.MediaCaptureInitializationSettings();
                    captureSettings.streamingCaptureMode = Windows.Media.Capture.StreamingCaptureMode.audio;
                    captureSettings.mediaCategory = Windows.Media.Capture.MediaCategory.speech;

                    var capture = new Windows.Media.Capture.MediaCapture();
                    capture.initializeAsync(captureSettings).then(function () {
                        completed(true);
                    },
                    function (error) {
                        // Audio Capture can fail to initialize if there's no audio devices on the system, or if
                        // the user has disabled permission to access the microphone in the Privacy settings.
                        if (error.number == -2147024891) { // Access denied (microphone disabled in settings)
                            completed(false);
                        } else if (error.number == -1072845856) { // No recording device present.
                            var messageDialog = new Windows.UI.Popups.MessageDialog("No Audio Capture devices are present on this system.");
                            messageDialog.showAsync();
                            completed(false);
                        } else {
                            error(error);
                        }
                    });
                } catch (exception) {
                    if (exception.number == -2147221164) { // REGDB_E_CLASSNOTREG
                        var messageDialog = new Windows.UI.Popups.MessageDialog("Media Player components not available on this system.");
                        messageDialog.showAsync();
                        return false;
                    }
                }
            });
        }
    })
```

## <a name="recognize-speech-input"></a>음성 입력 인식

*제약 조건은* 앱이 음성 입력에서 인식 하는 단어 및 구 (어휘)를 정의 합니다. 제약 조건은 음성 인식의 핵심 이며, 앱에서 음성 인식의 정확도를 보다 강력 하 게 제어할 수 있습니다.

음성 입력을 인식 하는 데 다음 형식의 제약 조건을 사용할 수 있습니다.

### <a name="predefined-grammars"></a>미리 정의 된 문법

미리 정의 된 받아쓰기 및 웹 검색 문법은 문법을 작성할 필요 없이 앱에 대 한 음성 인식을 제공 합니다. 이러한 문법을 사용 하는 경우 원격 웹 서비스에서 음성 인식이 수행 되며 결과가 장치에 반환 됩니다.

기본 자유 텍스트 받아쓰기 문법은 사용자가 특정 언어로 말할 수 있는 대부분의 단어와 구를 인식할 수 있으며, 짧은 구를 인식 하도록 최적화 되어 있습니다. [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) 개체에 대 한 제약 조건을 지정 하지 않으면 미리 정의 된 받아쓰기 문법이 사용 됩니다. 자유 텍스트 받아쓰기는 사용자가 말할 수 있는 항목의 종류를 제한 하지 않으려는 경우에 유용 합니다. 일반적인 용도에는 메모 만들기 또는 메시지 내용 받아쓰기가 포함 됩니다.

받아쓰기 문법 같은 웹 검색 문법에는 사용자가 말할 수 있는 많은 수의 단어와 구가 포함 되어 있습니다. 그러나 일반적으로 웹을 검색할 때 사용자가 사용 하는 용어를 인식 하도록 최적화 되어 있습니다.

> [!NOTE]
> 미리 정의 된 받아쓰기 및 웹 검색 문법이 클 수 있으며 장치가 온라인 상태 (장치에 있지 않음) 이기 때문에 장치에 설치 된 사용자 지정 문법에 따라 성능이 빠르지 않을 수 있습니다.     

이러한 미리 정의 된 문법은 최대 10 초 음성 입력을 인식 하는 데 사용 될 수 있으며 사용자의 작성 노력이 필요 하지 않습니다. 그러나 네트워크에 연결 해야 합니다.

웹 서비스 제약 조건을 사용 하려면 **설정-> 개인 정보-> 음성, 잉크 및 입력** 에서 "자세히 알아보기" 옵션을 설정 하 여 음성 입력 및 받아쓰기 지원을 **설정** 에서 사용 하도록 설정 해야 합니다.

여기서는 음성 입력을 사용 하도록 설정 했는지 여부를 테스트 하 고, 그렇지 않은 경우 개인 정보 > 음성, 수동 입력 및 입력 페이지를 > 하는 방법을 보여 줍니다.

먼저 HResultPrivacyStatementDeclined (전역 변수)를 0x80045509의 HResult 값으로 초기화 합니다. [C \# 또는 Visual Basic의 예외 처리](/previous-versions/windows/apps/dn532194(v=win.10))를 참조 하세요.

```csharp
private static uint HResultPrivacyStatementDeclined = 0x80045509;
```

그런 다음 recogntion 중에 표준 예외를 catch 하 고 [**HResult**](/uwp/api/Windows.Foundation.HResult) 값이 HResultPrivacyStatementDeclined 변수의 값과 같은지 테스트 합니다. 이 경우 경고를 표시 하 고를 호출 `await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts"));` 하 여 설정 페이지를 엽니다.

```csharp
catch (Exception exception)
{
  // Handle the speech privacy policy error.
  if ((uint)exception.HResult == HResultPrivacyStatementDeclined)
  {
    resultTextBlock.Visibility = Visibility.Visible;
    resultTextBlock.Text = "The privacy statement was declined." + 
      "Go to Settings -> Privacy -> Speech, inking and typing, and ensure you" +
      "have viewed the privacy policy, and 'Get To Know You' is enabled.";
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

[**SpeechRecognitionTopicConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint)를 참조 하세요.

### <a name="programmatic-list-constraints"></a>프로그래밍 목록 제약 조건 

프로그래밍 목록 제약 조건은 단어 또는 구의 목록을 사용 하 여 간단한 문법을 만드는 간단한 방법을 제공 합니다. List 제약 조건은 짧고 고유한 문구를 인식 하는 데 적합 합니다. 음성 인식 엔진이 일치 항목을 확인 하기 위해 음성만 처리 해야 하므로 문법의 모든 단어를 명시적으로 지정 하면 인식 정확도도 향상 됩니다. 목록을 프로그래밍 방식으로 업데이트할 수도 있습니다.

목록 제약 조건은 앱이 인식 작업에 대해 허용할 음성 입력을 나타내는 문자열 배열로 구성 됩니다. 음성 인식 목록 제약 조건 개체를 만들고 문자열 배열을 전달 하 여 앱에서 목록 제약 조건을 만들 수 있습니다. 그런 다음 해당 개체를 인식기의 제약 조건 컬렉션에 추가 합니다. 음성 인식기가 배열의 문자열 중 하나를 인식할 때 인식이 성공 합니다.

[**SpeechRecognitionListConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint)를 참조 하세요.

### <a name="srgs-grammars"></a>SRGS 문법

SRGS (음성 인식 문법 사양) 문법은 프로그래밍 목록 제약 조건과 달리 [SRGS 버전 1.0](https://www.w3.org/TR/speech-grammar/)에 정의 된 XML 형식을 사용 하는 정적 문서입니다. SRGS 문법은 단일 인식에서 여러 의미 체계 의미를 캡처할 수 있도록 하 여 음성 인식 경험을 가장 효과적으로 제어할 수 있도록 합니다.

 [**SpeechRecognitionGrammarFileConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint)를 참조 하세요.

### <a name="voice-command-constraints"></a>음성 명령 제약 조건

VCD (음성 명령 정의) XML 파일을 사용 하 여 사용자가 앱을 활성화할 때 작업을 시작 하는 데 사용할 수 있는 명령을 정의 합니다. 자세한 내용은 [Cortana를 통해 음성 명령을 사용 하 여 포그라운드 앱 활성화](/cortana/voice-commands/launch-a-foreground-app-with-voice-commands-in-cortana)를 참조 하세요.

[ **SpeechRecognitionVoiceCommandDefinitionConstraint** 참조](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionVoiceCommandDefinitionConstraint)/

**참고**  사용할 제약 조건 형식의 유형은 만들려는 인식 환경의 복잡성에 따라 달라 집니다. 는 특정 인식 작업에 가장 적합 한 선택 이며 앱에서 모든 유형의 제약 조건에 대 한 사용을 찾을 수 있습니다.
제약 조건을 시작 하려면 [사용자 지정 인식 제약 조건 정의](define-custom-recognition-constraints.md)를 참조 하세요.

미리 정의 된 유니버설 Windows 앱 받아쓰기 문법을 언어로 된 대부분의 단어와 짧은 구를 인식 합니다. 사용자 지정 제약 조건 없이 음성 인식기 개체가 인스턴스화될 때 기본적으로 활성화 됩니다.

이 예제에서는 다음을 수행 하는 방법을 보여 줍니다.

- 음성 인식기를 만듭니다.
- 기본 유니버설 Windows 앱 제약 조건을 컴파일합니다 (음성 인식기의 문법 집합에 문법이 추가 되지 않음).
- [**RecognizeWithUIAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizewithuiasync) 메서드에서 제공 하는 기본 인식 UI 및 TTS 피드백을 사용 하 여 음성 수신 대기를 시작 합니다. 기본 UI가 필요 하지 않은 경우 [**RecognizeAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizeasync) 메서드를 사용 합니다.

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


앱이 [**SpeechRecognizer**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizewithuiasync)를 호출 하 여 음성 인식을 시도 하면 몇 가지 화면이 다음 순서로 표시 됩니다.

미리 정의 된 문법 (받아쓰기 또는 웹 검색)을 기반으로 하는 제약 조건을 사용 하는 경우:

-   **수신 대기** 화면입니다.
-   **생각** 화면입니다.
-   **말하는** 화면 또는 오류 화면입니다.

단어 또는 구 목록을 기반으로 하는 제약 조건을 사용 하거나 SRGS 문법 파일을 기반으로 하는 제약 조건을 사용 하는 경우:

-   **수신 대기** 화면입니다.
-   **확인 내용** 화면 - 사용자가 말한 내용을 둘 이상의 가능한 결과로 해석할 수 있는 경우
-   **말하는** 화면 또는 오류 화면입니다.

다음 이미지는 SRGS 문법 파일을 기반으로 제약 조건을 사용 하는 음성 인식기 화면 간 흐름의 예를 보여 줍니다. 이 예제에서는 음성 인식이 성공 했습니다.

![sgrs 문법 파일을 기반으로 하는 제약 조건에 대 한 초기 인식 화면](images/speech-listening-initial.png)

![sgrs 문법 파일을 기반으로 하는 제약 조건에 대 한 중간 인식 화면](images/speech-listening-intermediate.png)

![sgrs 문법 파일을 기반으로 하는 제약 조건에 대 한 최종 인식 화면](images/speech-listening-complete.png)

**수신 대기** 화면은 앱에서 인식할 수 있는 단어 또는 구의 예를 제공할 수 있습니다. 여기서는 [**SpeechRecognizer**](/uwp/api/windows.media.speechrecognition.speechrecognizer.uioptions) 속성을 호출 하 여 가져온 [**SpeechRecognizerUIOptions**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizerUIOptions) 클래스의 속성을 사용 하 여 **수신** 화면에서 콘텐츠를 사용자 지정 하는 방법을 보여 줍니다.

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

## <a name="related-articles"></a>관련된 문서

* [음성 조작](speech-interactions.md)

**샘플**

* [음성 인식 및 음성 합성 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
