---
ms.assetid: 79C284CA-C53A-4C24-807E-6D4CE1A29BFA
description: 이 섹션에서는 이전 Windows 8.1 버전에서 Windows 10 버전으로 변경 된 내용을 지원 하도록 PlayReady 웹 앱을 수정 하는 방법을 설명 합니다.
title: PlayReady 암호화된 미디어 확장
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4145fbc67c6788a1d742fb0db616ecbc719e4b34
ms.sourcegitcommit: 2dbf4a3f3473c1d3a0ad988bcbae6e75dfee3640
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82619317"
---
# <a name="playready-encrypted-media-extension"></a>PlayReady 암호화된 미디어 확장



이 섹션에서는 이전 Windows 8.1 버전에서 Windows 10 버전으로 변경 된 내용을 지원 하도록 PlayReady 웹 앱을 수정 하는 방법을 설명 합니다.

Internet Explorer의 PlayReady 미디어 요소를 사용하면 개발자가 웹앱을 만들어, 콘텐츠 공급자가 정의한 액세스 규칙을 적용하는 한편 사용자에게 PlayReady 콘텐츠를 제공할 수 있습니다. 이 섹션에서는 HTML5와 JavaScript만 사용하여 기존 웹앱에 PlayReady 미디어 요소를 추가하는 방법을 설명합니다.

## <a name="whats-new-in-playready-encrypted-media-extension"></a>PlayReady 암호화 된 미디어 확장의 새로운 기능

이 섹션에서는 Windows 10에서 PlayReady 콘텐츠 보호를 사용 하도록 설정 하기 위해 PlayReady EME (암호화 된 미디어 확장)에 적용 된 변경 내용 목록을 제공 합니다.

다음 목록에서는 Windows 10 용 PlayReady 암호화 된 미디어 확장에 대 한 새로운 기능 및 변경 내용을 설명 합니다.

-   하드웨어 DRM (디지털 권한 관리)이 추가 되었습니다.

    하드웨어 기반 콘텐츠 보호 지원을 사용 하면 여러 장치 플랫폼에서 HD (high definition) 및 UHD (ultra high definition) 콘텐츠를 안전 하 게 재생할 수 있습니다. 키 자료 (개인 키, 콘텐츠 키 및 언급 된 키를 파생 시키거나 잠금 해제 하는 데 사용 되는 기타 모든 키 자료 포함), 암호 해독 된 압축 및 압축 되지 않은 비디오 샘플은 하드웨어 보안을 활용 하 여 보호 됩니다.

-   영구적이 지 않은 라이선스의 사전 획득 획득을 제공 합니다.
-   단일 메시지에서 여러 라이선스의 획득을 제공 합니다.

    Windows 8.1에서와 같이 여러 키 식별자 (KeyIDs)가 포함 된 PlayReady 개체를 사용 하거나 여러 KeyIDs로 [콘텐츠 암호 해독 모델 데이터 (CDMData)](https://docs.microsoft.com/previous-versions/windows/apps/dn457361(v=ieb.10)?redirectedfrom=MSDN) 를 사용할 수 있습니다.

    > [!NOTE]
    > Windows 10에서는 &lt; CDMData의 KeyID에서 여러 키 식별자를 사용할 수 &gt; 있습니다.

-   실시간 만료 지원 또는 제한 된 기간 라이선스 (LDL)가 추가 되었습니다.

    라이선스에 대 한 실시간 만료를 설정 하는 기능을 제공 합니다.

-   HDCP 유형 1 (버전 2.2) 정책 지원이 추가 되었습니다.
-   이제 Miracast는 출력으로 암시적입니다.
-   보안 중지를 추가 했습니다.

    Secure stop은 PlayReady 장치가 지정 된 콘텐츠에 대해 미디어 재생이 중지 되었음을 미디어 스트리밍 서비스에 자신 있게 어설션할 수 있는 수단을 제공 합니다.

-   오디오 및 비디오 라이선스 분리를 추가 했습니다.

    별도의 트랙은 비디오가 오디오로 디코딩되는 것을 방지 합니다. 더 강력한 콘텐츠 보호를 사용 하도록 설정 합니다. 새로운 표준에는 오디오 및 시각적 추적을 위한 별도의 키가 필요 합니다.

-   MaxResDecode를 추가 했습니다.

    이 기능은 더 강력한 지원 키 (라이선스 제외)가 있는 경우에도 콘텐츠 재생을 최대 해상도로 제한 하기 위해 추가 되었습니다. 여러 스트림 크기가 단일 키로 인코딩된 경우를 지원 합니다.

## <a name="encrypted-media-extension-support-in-playready"></a>PlayReady에서 암호화된 미디어 확장 지원

이 섹션에서는 PlayReady에서 지 원하는 W3C 암호화 된 미디어 확장의 버전을 설명 합니다.

Web Apps에 대 한 PlayReady는 현재 [2013 년 5 월 10 일 W3C 암호화 된 미디어 확장 (EME) 초안](https://www.w3.org/TR/2013/WD-encrypted-media-20130510/)에 바인딩되어 있습니다. 이 지원은 이후 버전의 Windows에서 업데이트 된 EME 사양으로 변경 될 예정입니다.

## <a name="use-hardware-drm"></a>하드웨어 DRM 사용

이 섹션에서는 웹 앱이 PlayReady 하드웨어 DRM을 사용 하는 방법 및 보호 된 콘텐츠가 지원 하지 않는 경우 하드웨어 DRM을 사용 하지 않도록 설정 하는 방법을 설명 합니다.

PlayReady 하드웨어 DRM을 사용 하려면 JavaScript 웹 앱에서의 키 시스템 식별자와 함께 **isTypeSupported** EME 메서드를 사용 하 여 `com.microsoft.playready.hardware` 브라우저에서 playready 하드웨어 DRM 지원을 쿼리해야 합니다.

일부 콘텐츠가 하드웨어 DRM에서 지원 되지 않는 경우도 있습니다. 하드웨어 DRM에서는 칵테일 콘텐츠가 지원 되지 않습니다. 칵테일 콘텐츠를 재생 하려는 경우 하드웨어 DRM을 옵트아웃 해야 합니다. 일부 하드웨어 DRM은 HEVC를 지원 하 고 일부는 지원 하지 않습니다. HEVC 콘텐츠를 재생 하려는 경우 하드웨어 DRM이이를 지원 하지 않으면 옵트아웃 (opt out) 할 수 있습니다.

> [!NOTE]
> HEVC 콘텐츠가 지원 되는지 여부를 확인 하려면 인스턴스화 후 `com.microsoft.playready` [**PlayReadyStatics 하드웨어**](https://docs.microsoft.com/uwp/api/windows.media.protection.playready.playreadystatics.checksupportedhardware) 메서드를 사용 합니다.

## <a name="add-secure-stop-to-your-web-app"></a>웹앱에 보안 중지 추가

이 섹션에서는 웹 앱에 보안 중지를 추가 하는 방법을 설명 합니다.

Secure stop은 PlayReady 장치가 지정 된 콘텐츠에 대해 미디어 재생이 중지 되었음을 미디어 스트리밍 서비스에 자신 있게 어설션할 수 있는 수단을 제공 합니다. 이 기능을 통해 미디어 스트리밍 서비스는 지정 된 계정에 대 한 다양 한 장치에 대 한 사용 제한 사항을 정확 하 게 적용 하 고 보고할 있습니다.

보안 중지 챌린지를 전송 하는 두 가지 기본 시나리오는 다음과 같습니다.

-   콘텐츠의 끝에 도달했거나 사용자가 미디어 프레젠테이션을 중간에 중지했으므로 미디어 프레젠테이션이 중지되는 경우.
-   이전 세션이 예기치 않게 종료 된 경우 (예: 시스템 또는 앱 작동 중단으로 인해) 응용 프로그램은 시작 또는 종료 시, 진행 중인 모든 보안 중지 세션 및 다른 미디어 재생과 별도로 챌린지를 전송 하 여 쿼리 해야 합니다.

다음 절차에서는 다양 한 시나리오에 대해 보안 중지를 설정 하는 방법을 설명 합니다.

프레젠테이션의 정상적인 종료를 위한 보안 중지를 설정 하려면 다음을 수행 합니다.

1.  재생이 시작 되기 전에 **Onended** 이벤트를 등록 합니다.
2.  **Onended** 이벤트 처리기는 `removeAttribute("src")` media foundation에서 토폴로지를 분리 하 고, 암호 해독기를 삭제 하 고, 중지 상태를 설정 하기 위해 media foundation이 트리거하는 **NULL** 로 설정 하기 위해 video/audio 요소 개체에서를 호출 해야 합니다.
3.  처리기 내에서 secure stop CDM 세션을 시작 하 여이 시점에서 재생이 중지 되었음을 알리는 보안 중지 챌린지를 서버에 보낼 수 있지만 나중에 수행할 수도 있습니다.

사용자가 페이지에서 다른 곳으로 이동 하거나 탭 또는 브라우저를 닫을 때 보안 중지를 설정 하려면 다음을 수행 합니다.

-   중지 상태를 기록 하는 데 앱 작업은 필요 하지 않습니다. 사용자를 위해 기록 됩니다.

사용자 지정 페이지 컨트롤이 나 사용자 작업 (예: 사용자 지정 탐색 단추 또는 현재 프레젠테이션이 완료 되기 전에 새 프레젠테이션 시작)에 대 한 보안 중지를 설정 하려면 다음을 수행 합니다.

-   사용자 지정 사용자 작업이 발생 하면 앱은 미디어 파운데이션을 트리거하여 토폴로지를 중단 하 고 암호 해독기를 삭제 하 고 중지 상태를 설정 하는 소스를 **NULL** 로 설정 해야 합니다.

다음 예제에서는 웹 앱에서 보안 중지를 사용 하는 방법을 보여 줍니다.

```JavaScript
// JavaScript source code

var g_prkey = null;
var g_keySession = null;
var g_fUseSpecificSecureStopSessionID = false;
var g_encodedMeteringCert = 'Base64 encoded of your metering cert (aka publisher cert)';

// Note: g_encodedLASessionId is the CDM session ID of the proactive or reactive license acquisition 
//       that we want to initiate the secure stop process.
var g_encodedLASessionId = null;

function main()
{
    ...

    g_prkey = new MSMediaKeys("com.microsoft.playready");

    ...

    // add 'onended' event handler to the video element
    // Assume 'myvideo' is the ID of the video element
    var videoElement = document.getElementById("myvideo");
    videoElement.onended = function (e) { 

        //
        // Calling removeAttribute("src") will set the source to null
        // which will trigger the MF to tear down the topology, destroy the
        // decryptor(s) and set the stop state.  This is required in order
        // to set the stop state.
        //
        videoElement.removeAttribute("src");
        videoElement.load();

        onEndOfStream();
    };
}

function onEndOfStream()
{
    ...

    createSecureStopCDMSession();

    ...    
}

function createSecureStopCDMSession()
{
    try{    
        var targetMediaCodec = "video/mp4";
        var customData = "my custom data";

        var encodedSessionId = g_encodedLASessionId;
        if( !g_fUseSpecificSecureStopSessionID )
        {
            // Use "*" (wildcard) as the session ID to include all secure stop sessions
            // TODO: base64 encode "*" and place encoded result to encodedSessionId
        }

        var int8ArrayCDMdata = formatSecureStopCDMData( encodedSessionId, customData,  g_encodedMeteringCert );
        var emptyArrayofInitData = new Uint8Array();

        g_keySession = g_prkey.createSession(targetMediaCodec, emptyArrayofInitData, int8ArrayCDMdata);

        addPlayreadyKeyEventHandler();

    } catch( e )
    {
        // TODO: Handle exception
    }
}

function addPlayreadyKeyEventHandler()
{
    // add 'keymessage' eventhandler   
    g_keySession.addEventListener('mskeymessage', function (event) {

        // TODO: Get the keyMessage from event.message.buffer which contains the secure stop challenge
        //       The keyMessage format for the secure stop is similar to LA as below:
        //
        //            <PlayReadyKeyMessage type="SecureStop" >
        //              <SecureStop version="1.0" >
        //                <Challenge encoding="base64encoded">
        //                    secure stop challenge
        //                </Challenge>
        //                <HttpHeaders>
        //                    <HttpHeader>
        //                      <name>Content-Type</name>
        //                         <value>"content type data"</value>
        //                    </HttpHeader>
        //                    <HttpHeader>
        //                         <name>SOAPAction</name>
        //                         <value>soap action</value>
        //                     </HttpHeader>
        //                    ....
        //                </HttpHeaders>
        //              </SecureStop>
        //            </PlayReadyKeyMessage>
                
        // TODO: send the secure stop challenge to a server that handles the secure stop challenge

        // TODO: Receive and response and call event.target.Update() to proecess the response
    });
    
    // add 'keyerror' eventhandler
    g_keySession.addEventListener('mskeyerror', function (event) {
        var session = event.target;
        
        ...

        session.close();
    });
    
    // add 'keyadded' eventhandler
    g_keySession.addEventListener('mskeyadded', function (event) {
        
        var session = event.target;

        ...

        session.close();             
    });
}

/**
* desc@ formatSecureStopCDMData
*   generate playready CDMData
*   CDMData is in xml format:
*   <PlayReadyCDMData type="SecureStop">
*     <SecureStop version="1.0">
*       <SessionID>B64 encoded session ID</SessionID>
*       <CustomData>B64 encoded custom data</CustomData>
*       <ServerCert>B64 encoded server cert</ServerCert>
*     </SecureCert>
* </PlayReadyCDMData>        
*/
function formatSecureStopCDMData(encodedSessionId, customData, encodedPublisherCert) 
{
    var encodedCustomData = null;

    // TODO: base64 encode the custom data and place the encoded result to encodedCustomData

    var CDMDataStr = "<PlayReadyCDMData type=\"SecureStop\">" +
                     "<SecureStop version=\"1.0\" >" +
                     "<SessionID>" + encodedSessionId + "</SessionID>" +
                     "<CustomData>" + encodedCustomData + "</CustomData>" +
                     "<ServerCert>" + encodedPublisherCert + "</ServerCert>" +
                     "</SecureStop></PlayReadyCDMData>";
    
    var int8ArrayCDMdata = null

    // TODO: Convert CDMDataStr to Uint8 byte array and palce the converted result to int8ArrayCDMdata

    return int8ArrayCDMdata;
}
```

> [!NOTE]
> 위의 샘플에서 보안 중지 데이터는 `<SessionID>B64 encoded session ID</SessionID>` 별표 ()가 될 수 있으며 \* ,이는 기록 된 모든 보안 중지 세션의 와일드 카드입니다. 즉, **SessionID** 태그를 특정 세션이 나 와일드 카드 ( \* )로 설정 하 여 모든 보안 중지 세션을 선택할 수 있습니다.

## <a name="programming-considerations-for-encrypted-media-extension"></a>암호화 된 미디어 확장에 대 한 프로그래밍 고려 사항

이 섹션에는 Windows 10 용 PlayReady 사용 웹 앱을 만들 때 고려해 야 할 프로그래밍 고려 사항이 나열 되어 있습니다.

앱이 종료 될 때까지 앱에서 만든 **Msmediakeys** 및 **Msmediakeysession** 개체는 모두 활성 상태로 유지 되어야 합니다. 이러한 개체를 활성 상태로 유지 하는 한 가지 방법은 해당 개체를 전역 변수로 할당 하는 것입니다. 변수는 범위를 벗어난 것 이며 함수 내에서 지역 변수로 선언 된 경우 가비지 컬렉션에 적용 됩니다. 예를 들어 다음 샘플에서는 *g \_ msmediakeys* 및 *g \_ mediakeysession* 변수를 전역 변수로 할당 합니다. 그런 다음이 변수는 함수에서 **msmediakeys** 및 **msmediakeysession** 개체에 할당 됩니다.

``` syntax
var g_msMediaKeys;
var g_mediaKeySession;

function foo() {
  ...
  g_msMediaKeys = new MSMediaKeys("com.microsoft.playready");
  ...
  g_mediaKeySession = g_msMediaKeys.createSession("video/mp4", intiData, null);
  g_mediaKeySession.addEventListener(this.KEYMESSAGE_EVENT, function (e) 
  {
    ...
    downloadPlayReadyKey(url, keyMessage, function (data) 
    {
      g_mediaKeySession.update(data);
    });
  });
  g_mediaKeySession.addEventListener(this.KEYADDED_EVENT, function () 
  {
    ...
    g_mediaKeySession.close();
    g_mediaKeySession = null;
  });
}
```

자세한 내용은 [샘플 응용 프로그램](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/PlayReady)을 참조 하세요.

## <a name="see-also"></a>참고 항목
- [PlayReady DRM](playready-client-sdk.md)




