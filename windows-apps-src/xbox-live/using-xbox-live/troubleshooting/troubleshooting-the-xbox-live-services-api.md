---
title: Xbox Live 서비스 API 문제 해결
author: KevinAsgari
description: Xbox Live Api를 사용 하 여 문제를 해결 하는 동안 추가 오류 정보를 기록 하는 방법을 알아봅니다.
ms.assetid: 3827bba1-902f-4f2d-ad51-af09bd9354c4
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 문제 해결, 오류, 로그
ms.localizationpriority: medium
ms.openlocfilehash: dabc6458254c6ceec7995baa466de6dbddd76e18
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5479847"
---
# <a name="troubleshooting-the-xbox-live-apis"></a>Xbox Live Api 문제 해결

## <a name="code"></a>Code

Xbox Live 서비스 API 계층에서 오류만을 사용 하 여 오류를 진단 하기가 어렵습니다. 추가 오류 정보-모든 RESTful 호출의 로깅 같은-서버를 사용할 수 있습니다. 이 데이터를 수신 대기 하도록 응답으로 거 연결 하 고 디버그 추적을 활성화 합니다. 응답 로깅을 사용 하면 유용한 Fiddler 추적 이기도 HTTP 트래픽 및 웹 서비스 응답 코드를 볼 수 있습니다.

### <a name="c"></a>C++

다음 코드 예제에서는 응답 로깅을 사용 하 고 디버그 오류 수준을 Verbose (설정할 수도 있습니다 디버그 오류 수준을 추적 호출이 실패만 표시 하려면 오류 또는 추적을 사용 하지 않도록 꺼짐)으로 설정 합니다. Visual Studio에서 프로젝트를 실행할 때 결과 디버그 출력 출력 창에 전송 됩니다.  

```cpp

        // Set up debug tracing to the Output window in Visual Studio.
            xbox::services::system::xbox_live_services_settings::get_singleton_instance()->set_diagnostics_trace_level(
                xbox_services_diagnostics_trace_level::verbose
                );
```

디버그 출력 고유한 로그 파일을 리디렉션할 수도 같이:

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
