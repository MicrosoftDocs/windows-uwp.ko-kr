---
author: Karl-Bridge-Microsoft
Description: Learn how to manage issues with speech-recognition accuracy caused by audio-input quality.
title: 오디오 입력 관련 문제 관리
ms.assetid: 3E36C683-C96A-4FEE-AD52-FDB87E0CC299
label: Manage audio input issues
template: detail.hbs
keywords: 음성 명령, 목소리, 음성 인식, 자연어, 받아쓰기, 입력, 사용자 조작
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 094acdbcb5c5b3bf45bad757344be5187ae37cbc
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6839000"
---
# <a name="manage-issues-with-audio-input"></a>오디오 입력 관련 문제 관리


오디오 입력 품질로 인해 발생하는 음성 인식 정확도와 관련된 문제를 관리하는 방법을 알아봅니다.

> **중요 API**: [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226), [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243), [**SpeechRecognitionAudioProblem**](https://msdn.microsoft.com/library/windows/apps/dn631406)


## <a name="assess-audio-input-quality"></a>오디오 입력 품질 평가


음성 인식이 활성 상태일 때는 음성 인식기의 [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243) 이벤트를 사용해서 하나 이상의 오디오 문제로 인해 음성 입력이 방해되는지 여부를 확인합니다. 이벤트 인수([**SpeechRecognitionQualityDegradingEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn631430))는 오디오 입력으로 검색된 문제를 기술하는 [**Problem**](https://msdn.microsoft.com/library/windows/apps/dn631431) 속성을 제공합니다.

배경 소음이 너무 많거나, 마이크가 음소거되었거나, 발언자의 목소리 또는 말하는 속도가 적절하지 않을 경우, 인식에 영향을 줄 수 있습니다.

여기에서는 음성 인식기를 구성하고 [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243) 이벤트에 대한 수신을 시작합니다.

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


[**Problem**](https://msdn.microsoft.com/library/windows/apps/dn631431) 속성에서 제공하는 설명을 사용하여 사용자가 인식 조건을 향상시킬 수 있도록 지원합니다.

여기에서는 낮은 볼륨 수준을 확인하는 [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243) 이벤트에 대한 처리기를 만듭니다. 그런 다음 [**SpeechSynthesizer**](https://msdn.microsoft.com/library/windows/apps/dn298152) 개체를 사용하여 사용자가 더 크게 말해 보도록 제안합니다.

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

## <a name="related-articles"></a>관련 문서


* [음성 조작](speech-interactions.md)

**샘플**
* [음성 인식 및 음성 합성 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




