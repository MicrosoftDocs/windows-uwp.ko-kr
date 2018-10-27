---
author: Karl-Bridge-Microsoft
Description: Use speech recognition to provide input, specify an action or command, and accomplish tasks.
title: 음성 인식
ms.assetid: 553C0FB7-35BC-4894-9EF1-906139E17552
label: Speech recognition
template: detail.hbs
keywords: 음성 명령, 목소리, 음성 인식, 자연어, 받아쓰기, 입력, 사용자 조작
ms.author: kbridge
ms.date: 10/25/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b9148b2d57c55bdff09be9a9d6bb8a6b65d93f12
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5707432"
---
# <a name="speech-recognition"></a>음성 인식


음성 인식 기능을 사용하여 입력을 제공하고, 동작이나 명령을 지정하고 작업을 수행합니다.

> **중요 API**: [**Windows.Media.SpeechRecognition**](https://msdn.microsoft.com/library/windows/apps/dn653262)

음성 인식은 음성 런타임, 런타임을 프로그래밍하기 위한 인식 API, 바로 사용할 수 있는 받아쓰기 및 웹 검색 문법, 사용자가 음성 인식 기능을 검색하고 사용하는 데 도움이 되는 기본 시스템 UI로 구성됩니다.

## <a name="configure-speech-recognition"></a>음성 인식 구성

앱을 사용 하 여 음성 인식 기능을 지원 하기 위해 사용자 해야 연결가 장치에 마이크를 사용 하도록 설정 및 사용 하려면 앱에 대 한 권한을 부여 Microsoft 개인 정보 보호 정책을 적용 합니다.

자동으로 액세스 하 고 마이크를 사용할 수 있는 권한을 요청 하는 시스템 대화 상자를 사용 하 여 묻는 오디오 피드 정당한 집합 (예)에서 [음성 인식 및 음성 합성 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619897) 아래 표시 된 **마이크** [장치 접근 권한 값](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-devicecapability) [앱 패키지 매니페스트](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)에 있습니다. 자세한 내용은 [앱 기능 선언](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)를 참조 하세요.

![마이크 액세스에 대 한 개인 정보 취급 방침](images/speech/privacy.png)

앱 설정에 승인 된 응용 프로그램 목록에 추가 되는 마이크에 대 한 액세스 권한을 부여 하는 예를 클릭 하면-> 개인 정보-> 마이크 페이지. 하지만 언제 든 지가이 설정을 사용 하지 않으려면 사용자 수 선택 하는 것을 사용 하기 전에 앱에서 마이크에 대 한 액세스에는 확인 해야 합니다.

Cortana, 받아쓰기를 지원 하려는 경우, 다른 음성 인식 서비스 (예: 항목 제약 조건에 정의 된 경우 [미리 정의 된 문법](#predefined-grammars) ) 또한 확인 해야 하는 **온라인 음성 인식** (설정-> 개인 정보-> 음성)은 사용할 수 있습니다.

이 조각 마이크 존재 하 고 사용할 수 있는 권한이 있으면 앱 어떻게 확인할 수를 보여 줍니다.

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

*제약 조건*은 앱이 음성 입력에서 인식하는 단어와 구(어휘)를 정의합니다. 제약 조건은 음성 인식의 핵심이며 앱의 음성 인식 정확도를 높입니다.

음성 입력 인식에 대 한 다음과 같은 유형의 제약 조건 사용할 수 있습니다.

### <a name="predefined-grammars"></a>미리 정의된 문법

미리 정의된 받아쓰기 및 웹 검색 문법은 사용자가 문법을 작성할 필요 없이 앱에 대한 음성 인식 기능을 제공합니다. 이러한 문법을 사용할 때 음성 인식은 원격 웹 서비스에서 수행되고 결과가 장치로 반환됩니다.

기본 자유 텍스트 받아쓰기 문법은 사용자가 특정 언어로 말할 수 있는 대부분의 단어와 구를 인식할 수 있으며 짧은 구를 인식하도록 최적화되어 있습니다. [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) 개체에 대한 제약 조건을 지정하지 않으면 미리 정의된 받아쓰기 문법이 사용됩니다. 자유 텍스트 받아쓰기는 사용자가 말할 수 있는 내용을 제한하지 않으려는 경우에 유용합니다. 일반적인 사용에는 노트 만들기나 메시지 내용 받아쓰기가 포함됩니다.

받아쓰기 문법과 같은 웹 검색 문법에는 사용자가 말할 수 있는 매우 많은 단어 및 구가 포함되어 있습니다. 그러나 웹 검색 문법은 사람들이 일반적으로 웹을 검색할 때 사용하는 용어를 인식하도록 최적화되어 있습니다.

**참고**고 온라인 미리 정의 된 받아쓰기 및 웹 검색 문법은 용량이 클 수 있기 때문에 (디바이스)에 없는 성능 하지 않을 디바이스에 설치 된 사용자 지정 문법에서와 마찬가지로 빠릅니다.     

이러한 미리 정의된 문법은 최대 10초의 음성 입력을 인식하는 데 사용할 수 있으며 특별한 작성 작업이 필요하지 않습니다. 그러나 네트워크에 연결되어 있어야 합니다.

웹 서비스 제약 조건을 사용하려면 **설정 -> 개인 정보 -> 음성, 수동 입력 및 입력** 페이지에서 "내 정보 표시" 옵션을 켜고 **설정**에서 음성 입력 및 받아쓰기 지원을 사용하도록 설정해야 합니다.

여기에서는 음성 입력이 사용되도록 설정되어 있는지 테스트하고 설정되어 있지 않으면 설정 -> 개인 정보 -> 음성, 수동 입력 및 입력 페이지를 여는 방법을 보여 줍니다.

먼저, 전역 변수(HResultPrivacyStatementDeclined)를 0x80045509의 HResult 값으로 초기화합니다. [C\# 또는 Visual Basic으로 작성된 예외 처리](https://msdn.microsoft.com/library/windows/apps/dn532194)를 참조하세요.

```csharp
private static uint HResultPrivacyStatementDeclined = 0x80045509;
```

그런 다음 인식하는 동안 표준 예외를 catch하여 [**HResult**](https://msdn.microsoft.com/library/windows/apps/br206579) 값이 HResultPrivacyStatementDeclined 변수 값과 같은지 테스트합니다. 같으면 경고를 표시하고 `await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts"));`를 호출하여 설정 페이지를 엽니다.

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

[**SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631446)를 참조 하세요.

### <a name="programmatic-list-constraints"></a>프로그래밍 방식 목록 제약 조건 

프로그래밍 방식 목록 제약 조건은 단어 또는 구 목록을 사용하여 간단한 문법을 만드는 가벼운 방법을 제공합니다. 짧고 고유한 구를 인식하는 데는 목록 제약 조건이 유용합니다. 음성 인식 엔진이 일치를 확인하기 위해서만 음성을 처리해야 하므로 문법의 모든 단어를 명시적으로 지정하면 인식 정확도도 향상됩니다. 또한 목록은 프로그래밍 방식으로도 업데이트할 수 있습니다.

목록 제약 조건은 앱이 인식 작업에 대해 받아들이는 음성 입력을 나타내는 문자열 배열로 구성됩니다. 음성 인식 목록 제약 조건 개체를 만들고 문자열 배열을 전달하여 앱에서 목록 제약 조건을 만들 수 있습니다. 그런 다음 인식기 제약 조건 컬렉션에 해당 개체를 추가합니다. 음성 인식기가 배열에 있는 문자열 중 하나를 인식하면 인식에 성공합니다.

[**SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631421)를 참조 하세요.

### <a name="srgs-grammars"></a>SRGS 문법

SRGS(Speech Recognition Grammar Specification) 문법은 프로그래밍 방식 목록 제약 조건과 달리 [SRGS 버전 1.0](http://go.microsoft.com/fwlink/p/?LinkID=262302)에서 정의한 XML 형식을 사용하는 정적 문서입니다. SRGS 문법을 사용하면 단일 인식에서 여러 시맨틱 의미를 캡처할 수 있으므로 음성 인식 환경을 가장 잘 제어할 수 있습니다.

 [**SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412)를 참조 하세요.

### <a name="voice-command-constraints"></a>음성 명령 제약 조건

VCD(음성 명령 정의) XML 파일을 사용하여 사용자가 앱을 활성화할 때 동작을 시작하기 위해 말할 수 있는 명령을 정의합니다. 자세한 내용은 [Cortana에서 음성 명령으로 포그라운드 앱 시작](https://msdn.microsoft.com/cortana/voicecommands/launch-a-foreground-app-with-voice-commands-in-cortana)을 참조하세요.

[ **SpeechRecognitionVoiceCommandDefinitionConstraint** 를 참조 하세요.](https://msdn.microsoft.com/library/windows/apps/dn653220)/

**참고**형식을 사용 하는 제약 조건 유형은 만들려는 인식 환경의 복잡성에 따라 달라 집니다. 어떤 유형이나 특정 인식 작업에 가장 적합한 선택이 될 수 있으며, 앱에서 모든 제약 조건 유형의 용도를 찾을 수 있습니다.
제약 조건을 시작하려면 [사용자 지정 인식 제약 조건 정의](define-custom-recognition-constraints.md)을 참조하세요.

미리 정의된 유니버설 Windows 앱 받아쓰기 문법은 언어의 단어와 짧은 구를 대부분 인식합니다. 사용자 지정 제약 조건 없이 음성 인식기 개체를 인스턴스화할 때 기본적으로 활성화됩니다.

이 예제에서는 다음 작업을 수행하는 방법을 보여 줍니다.

- 음성 인식기 만들기
- 기본 유니버설 Windows 앱 제약 조건 컴파일(음성 인식기의 문법 집합에 추가된 문법 없음)
- 기본 인식 UI 및 [**RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245) 메서드에서 제공하는 TTS 피드백을 사용하여 음성 수신 대기 시작 기본 UI가 필요하지 않은 경우 [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn653244) 메서드를 사용합니다.

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

## <a name="related-articles"></a>관련 문서


**개발자**
* [음성 조작](speech-interactions.md)
**디자이너**
* [음성 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn596121)
**샘플**
* [음성 인식 및 음성 합성 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




