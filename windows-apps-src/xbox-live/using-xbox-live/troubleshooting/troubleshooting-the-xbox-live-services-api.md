---
title: Xbox Live 서비스 API 문제 해결
description: Xbox Live Api를 사용 하 여 문제를 해결 하는 동안 추가 오류 정보를 기록 하는 방법에 알아봅니다.
ms.assetid: 3827bba1-902f-4f2d-ad51-af09bd9354c4
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox 하나, 문제 해결, 오류, 로그
ms.localizationpriority: medium
ms.openlocfilehash: 67735d83e5ff301ee4434a6917fce814cabe8309
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621418"
---
# <a name="troubleshooting-the-xbox-live-apis"></a>Xbox Live Api 문제 해결

## <a name="code"></a>코드

Xbox Live Services API 계층에서 오류만을 사용 하 여 오류를 진단 하는 것이 어렵습니다. 추가 오류 정보-모든 RESTful 호출의 로깅을 같은-서버에 제공 될 수 있습니다. 이 데이터를 수신 대기 하 고, 응답으로 거를 연결 및 디버그 추적을 사용 하도록 설정 합니다. 응답 로깅을 사용 하는 Fiddler 추적으로으로 유용한 경우가 많습니다에 HTTP 트래픽 및 웹 서비스 응답 코드를 확인할 수 있습니다.

### <a name="c"></a>C++

다음 코드 예제는 응답 로깅을 사용 하도록 설정 하 고 (또한 수준을 설정할 수 있습니다는 디버그 오류 실패 한 호출을 추적만 표시 하는 오류 또는 추적을 사용 하지 않도록 설정 해제) 하는 자세한 정보 표시로 디버그 오류 수준을 설정 합니다. Visual Studio에서 프로젝트를 실행 하는 경우 결과 디버그 출력은 출력 창으로 전송 됩니다.  

```cpp

        // Set up debug tracing to the Output window in Visual Studio.
            xbox::services::system::xbox_live_services_settings::get_singleton_instance()->set_diagnostics_trace_level(
                xbox_services_diagnostics_trace_level::verbose
                );
```

고유한 로그 파일에 디버그 출력을 리디렉션할 수도 있습니다 다음과 같이 합니다.

```cpp

        // Set up debug tracing of the Xbox Live Services API traffic to the game UI.
        m_xboxLiveContext->Settings->EnableServiceCallRoutedEvents = true;
        m_xboxLiveContext->Settings->ServiceCallRouted += ref new
        Windows::Foundation::EventHandler<Microsoft::Xbox::Services::XboxServiceCallRoutedEventArgs^>(
            [=] ( Platform::Object^, Microsoft::Xbox::Services::XboxServiceCallRoutedEventArgs^ args )
            {
                gameUI->Log(L"[URL]: " + args->HttpMethod + " " + args->Url->AbsoluteUri);
                gameUI->Log(L"");
                gameUI->Log(L"[Response]: " + args->HttpStatus.ToString() + " " + args->ResponseBody);
            });

```
