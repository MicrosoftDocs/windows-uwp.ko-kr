---
description: 오디오 입력 품질로 인 한 음성 인식 정확도 문제를 관리 하는 방법에 대해 알아봅니다.
title: 오디오 입력 관련 문제 관리
ms.assetid: 3E36C683-C96A-4FEE-AD52-FDB87E0CC299
label: Manage audio input issues
template: detail.hbs
keywords: 음성, 음성, 음성 인식, 자연어, 받아쓰기, 입력, 사용자 조작
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a5d74346aec4784cea9be2dfcef3ec2efccd8b56
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034696"
---
# <a name="manage-issues-with-audio-input"></a>오디오 입력 관련 문제 관리


오디오 입력 품질로 인 한 음성 인식 정확도 문제를 관리 하는 방법에 대해 알아봅니다.

> **중요 한 api** : [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer), [**RecognitionQualityDegrading**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognitionqualitydegrading), [**SpeechRecognitionAudioProblem**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionAudioProblem)


## <a name="assess-audio-input-quality"></a>오디오 입력 품질 평가


음성 인식이 활성화 된 경우 음성 인식기의 [**RecognitionQualityDegrading**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognitionqualitydegrading) 이벤트를 사용 하 여 하나 이상의 오디오 문제로 음성 입력을 방해할 수 있는지 여부를 확인 합니다. 이벤트 인수 ( [**SpeechRecognitionQualityDegradingEventArgs**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionQualityDegradingEventArgs))는 오디오 입력에서 검색 된 문제를 설명 하는 [**문제**](/uwp/api/windows.media.speechrecognition.speechrecognitionqualitydegradingeventargs.problem) 속성을 제공 합니다.

인식은 너무 많은 배경 소음, 음소거 된 마이크 및 스피커의 볼륨 또는 속도의 영향을 받을 수 있습니다.

여기서는 음성 인식기를 구성 하 고 [**RecognitionQualityDegrading**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognitionqualitydegrading) 이벤트 수신 대기를 시작 합니다.

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
    speechRecognizer.UIOptions.ExampleText = "Ex. 'weather for London'";
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

## <a name="manage-the-speech-recognition-experience"></a>음성 인식 환경 관리


[**문제**](/uwp/api/windows.media.speechrecognition.speechrecognitionqualitydegradingeventargs.problem) 속성에서 제공 하는 설명을 사용 하 여 사용자가 인식 조건을 향상 시킬 수 있습니다.

여기서는 낮은 볼륨 수준을 확인 하는 [**RecognitionQualityDegrading**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognitionqualitydegrading) 이벤트에 대 한 처리기를 만듭니다. 그런 다음 [**SpeechSynthesizer**](/uwp/api/Windows.Media.SpeechSynthesis.SpeechSynthesizer) 개체를 사용 하 여 사용자가 보다 크게 시도 하는 것을 제안 합니다.

```CSharp
private async void speechRecognizer_RecognitionQualityDegrading(
    Windows.Media.SpeechRecognition.SpeechRecognizer sender,
    Windows.Media.SpeechRecognition.SpeechRecognitionQualityDegradingEventArgs args)
{
    // Create an instance of a speech synthesis engine (voice).
    var speechSynthesizer =
        new Windows.Media.SpeechSynthesis.SpeechSynthesizer();

    // If input speech is too quiet, prompt the user to speak louder.
    if (args.Problem == Windows.Media.SpeechRecognition.SpeechRecognitionAudioProblem.TooQuiet)
    {
        // Generate the audio stream from plain text.
        Windows.Media.SpeechSynthesis.SpeechSynthesisStream stream;
        try
        {
            stream = await speechSynthesizer.SynthesizeTextToStreamAsync("Try speaking louder");
            stream.Seek(0);
        }
        catch (Exception)
        {
            stream = null;
        }

        // Send the stream to the MediaElement declared in XAML.
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.High, () =>
        {
            this.media.SetSource(stream, stream.ContentType);
        });
    }
}
```

## <a name="related-articles"></a>관련된 문서


* [음성 조작](speech-interactions.md)

**샘플**
* [음성 인식 및 음성 합성 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
 

 
