---
author: drewbatgit
ms.assetid: 79C284CA-C53A-4C24-807E-6D4CE1A29BFA
description: 이 섹션에서는 변경 내용을 Windows8.1 버전과에서 Windows10 버전을 지원 하도록 PlayReady 웹 앱을 수정 하는 방법을 설명 합니다.
title: PlayReady 암호화된 미디어 확장
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b73464ea10aa835b82df17605e983ebdfb9cd890
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5688039"
---
# <a name="playready-encrypted-media-extension"></a>PlayReady 암호화된 미디어 확장



이 섹션에서는 변경 내용을 Windows8.1 버전과에서 Windows10 버전을 지원 하도록 PlayReady 웹 앱을 수정 하는 방법을 설명 합니다.

Internet Explorer의 PlayReady 미디어 요소를 사용하면 개발자가 웹앱을 만들어, 콘텐츠 공급자가 정의한 액세스 규칙을 적용하는 한편 사용자에게 PlayReady 콘텐츠를 제공할 수 있습니다. 이 섹션에서는 HTML5와 JavaScript만 사용하여 기존 웹앱에 PlayReady 미디어 요소를 추가하는 방법을 설명합니다.

## <a name="whats-new-in-playready-encrypted-media-extension"></a>PlayReady 암호화된 미디어 확장의 새로운 기능

이 섹션에서는 목록을 변경 하는 PlayReady eme 암호화 된 미디어 확장 () Windows10에서 PlayReady 콘텐츠 보호를 사용 하도록 설정 합니다.

다음은 새로운 기능과 변경 내용이 PlayReady 암호화 된 미디어 확장 Windows10에 대 한 설명 합니다.

-   하드웨어 DRM(디지털 권한 관리)이 추가되었습니다.

    하드웨어 기반 콘텐츠 보호 지원을 통해 여러 디바이스 플랫폼에서 고해상도(HD) 및 초고해상도(UHD) 콘텐츠를 안전하게 재생할 수 있습니다. 개인 키, 콘텐츠 키 및 이러한 키를 파생하거나 잠금 해제하는 데 사용하는 기타 키 자료를 포함하는 키 자료와 암호 해독되어 압축 및 압축 해제된 비디오 샘플은 하드웨어 보안을 활용하여 보호합니다.

-   비영구적 라이선스를 미리 취득할 수 있습니다.
-   한 메시지로 여러 라이선스를 취득할 수 있습니다.

    Windows8.1와 여러 키 id (Keyid)를 사용 하 여 PlayReady 개체를 사용 하거나 [콘텐츠 암호 해독 모델 데이터 (CDMData)를](https://go.microsoft.com/fwlink/p/?LinkID=626819) 사용 하 여 여러 keyid와 함께 수 있습니다.

    > [!NOTE]
    > Windows10, 여러 키 id 아래에서 지원 됩니다 &lt;KeyID&gt; CDMData의 합니다.

-   실시간 만료 지원 또는 LDL(제한된 기간 라이선스)이 추가되었습니다.

    라이선스에 실시간 만료를 설정하는 기능을 제공합니다.

-   HDCP 유형 1(버전 2.2) 정책 지원이 추가되었습니다.
-   이제 출력으로서의 Miracast는 암시적입니다.
-   보안 중지가 추가되었습니다.

    보안 중지를 통해 PlayReady 디바이스는 지정된 콘텐츠에 대해 미디어 재생이 중지된 미디어 스트리밍 서비스로 안정적으로 어설션됩니다.

-   오디오 및 동영상 라이선스 분리가 추가되었습니다.

    별도 트랙을 통해 동영상이 오디오로 디코드되지 않아 콘텐츠가 보다 강력하게 보호됩니다. 새 표준에서는 오디오 및 동영상 트랙에 별도의 키가 필요합니다.

-   MaxResDecode가 추가되었습니다.

    이 기능은 강력한 기능 키(라이선스는 아님)를 소유한 경우에도 콘텐츠를 최대 해상도로 재생하는 것을 제한하기 위해 추가되었습니다. 이 기능은 여러 스트림 크기가 단일 키로 인코드된 경우를 지원합니다.

## <a name="encrypted-media-extension-support-in-playready"></a>PlayReady에서 암호화된 미디어 확장 지원

이 섹션에서는 PlayReady에서 지원하는 W3C 암호화된 미디어 확장 버전을 설명합니다.

웹앱용 PlayReady는 현재 [2013년 5월 10일의 W3C EME(암호화된 미디어 확장) 초안](http://www.w3.org/TR/2013/WD-encrypted-media-20130510/)에 바인딩되어 있습니다. 이후 Windows 버전에서는 업데이트된 EME 사양으로 변경되어 지원될 예정입니다.

## <a name="use-hardware-drm"></a>하드웨어 DRM 사용

이 섹션에서는 웹앱에서 PlayReady 하드웨어 DRM을 사용하는 방법과 보호된 콘텐츠에서 하드웨어 DRM을 지원하지 않는 경우 하드웨어 DRM을 사용하지 않도록 설정하는 방법을 설명합니다.

PlayReady 하드웨어 DRM을 사용하려면 JavaScript 웹앱에서 키 시스템 식별자 `com.microsoft.playready.hardware`와 함께 **isTypeSupported** EME 메서드를 사용하여 브라우저에서 PlayReady 하드웨어 DRM 지원을 쿼리해야 합니다.

경우에 따라 일부 콘텐츠는 하드웨어 DRM에서 지원되지 않습니다. Cocktail 콘텐츠는 하드웨어 DRM에서 지원되지 않습니다. Cocktail 콘텐츠를 재생해야 하는 경우 하드웨어 DRM을 옵트아웃(opt out)해야 합니다. HEVC를 지원하는 하드웨어 DRM도 있고 지원하지 않는 하드웨어 DRM도 있습니다. HEVC 콘텐츠를 재생하려는 경우 DRM에서 HEVC 콘텐츠를 지원하지 않으면 역시 하드웨어 DRM을 옵트아웃(opt out)할 수 있습니다.

> [!NOTE]
> HEVC 콘텐츠가 지원되는지를 확인하려면 `com.microsoft.playready`를 인스턴스화한 후 [**PlayReadyStatics.CheckSupportedHardware**](https://msdn.microsoft.com/library/windows/apps/dn986441) 메서드를 사용하세요.

## <a name="add-secure-stop-to-your-web-app"></a>웹앱에 보안 중지 추가

이 섹션에서는 웹앱에 보안 중지를 추가하는 방법을 설명합니다.

보안 중지를 통해 PlayReady 디바이스는 지정된 콘텐츠에 대해 미디어 재생이 중지된 미디어 스트리밍 서비스로 안정적으로 어설션됩니다. 이 기능은 미디어 스트리밍 서비스가 지정된 계정에 대해 다양한 디바이스의 사용 제한을 정확하게 적용하고 보고하도록 합니다.

보안 중지 챌린지를 보내는 주요 시나리오는 두 가지가 있습니다.

-   콘텐츠의 끝에 도달했거나 사용자가 미디어 프레젠테이션을 중간에 중지했으므로 미디어 프레젠테이션이 중지되는 경우.
-   이전 세션이 예기치 않게 종료되는 경우(예: 시스템 또는 앱 크래시가 원인). 앱에서 시작 또는 종료 시 해결되지 않은 보안 중지 세션을 쿼리하고 다른 미디어 재생과 별도로 챌린지를 보내야 합니다.

다음 절차에서는 다양한 시나리오에 맞는 보안 중지 설정 방법을 설명합니다.

정상적으로 프레젠테이션이 끝나는 경우에 맞는 보안 중지를 설정하려면 다음을 수행합니다.

1.  재생을 시작하기 전에 **onEnded** 이벤트를 등록합니다.
2.  **onEnded** 이벤트 처리기가 비디오/오디오 요소 개체에서 `removeAttribute(“src”)`를 호출하여 소스를 **NULL**로 설정해야 합니다. 그러면 토폴로지를 구분하고 암호 해독기를 삭제하며 중지 상태를 설정하도록 미디어 파운데이션을 트리거합니다.
3.  처리기 내부에서 보안 중지 CDM 세션을 시작하여 현재 재생이 중지되었지만 나중에 다시 수행할 수 있음을 알리기 위해 서버에 보안 중지 챌린저를 보낼 수 있습니다.

사용자가 페이지 외부로 이동하거나 탭 또는 브라우저를 종료하는 경우 보안 중지되도록 설정하려면 다음을 수행합니다.

-   중지 상태를 기록하기 위해 앱에서 작업을 수행할 필요가 없습니다. 자동으로 기록됩니다.

사용자 지정 페이지 컨트롤 또는 사용자 작업(예: 사용자 지정 탐색 단추 또는 현재 프레젠테이션이 완료되기 전에 새 프레젠테이션 시작)에 맞게 보안 중지를 설정하려면 다음을 수행합니다.

-   사용자 지정 사용자 작업이 수행되면 앱에서 토폴로지를 구분하고 암호 해독기를 삭제하며 중지 상태를 설정할 **NULL**로 소스를 설정해야 합니다.

다음 예제에서는 웹앱에서 보안 중지를 사용하는 방법을 보여 줍니다.

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

        // TODO: Recevie and response and call event.target.Update() to proecess the response
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
> 위의 샘플에 있는 보안 중지 데이터의 `<SessionID>B64 encoded session ID</SessionID>`는 기록된 모든 보안 중지 세션의 와일드 카드인 별표(\*)가 될 수 있습니다. 즉 **SessionID** 태그는 구체적인 세션이거나 모든 보안 중지 세션을 선택하도록 와일드 카드(\*)가 될 수 있습니다.

## <a name="programming-considerations-for-encrypted-media-extension"></a>암호화된 미디어 확장에 대한 프로그래밍 고려 사항

이 섹션에는 Windows10 용 PlayReady 사용 웹 앱을 만들 때 고려해 야 하는 프로그래밍 고려 사항이 나와 있습니다.

앱에서 만든 **MSMediaKeys** 및 **MSMediaKeySession** 개체는 앱을 닫을 때까지 활성화되어 있어야 합니다. 이러한 개체를 활성 상태로 유지하는 한 가지 방법은 해당 개체를 전역 변수로 할당하는 것입니다. 변수가 함수 내부의 로컬 변수로 선언된 경우에는 변수가 범위에서 벗어나게 되며 가비지 수집에 따라 달라질 수 있습니다. 예를 들어 다음 샘플에서는 *g\_msMediaKeys* 및 *g\_mediaKeySession* 변수를 전역 변수로 할당합니다. 그런 다음 해당 변수가 함수의 **MSMediaKeys** 및 **MSMediaKeySession** 개체에 할당됩니다.

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

자세한 내용은 [샘플 응용 프로그램](https://code.msdn.microsoft.com/windowsapps/PlayReady-samples-for-124a3738)을 참조하세요.

## <a name="see-also"></a>참고 항목
- [PlayReady DRM](playready-client-sdk.md)




