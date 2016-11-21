---
author: TylerMSFT
Description: "Microsoft 시험 응시 앱을 위한 JavaScript API를 사용하면 평가의 보안을 유지할 수 있습니다. 시험 응시에서는 테스트 중 학생이 다른 컴퓨터나 인터넷 리소스를 사용할 수 없도록 보안 브라우저를 제공합니다."
title: "JavaScript API 시험 응시."
translationtype: Human Translation
ms.sourcegitcommit: 7f578d73a9a625b0ac7d9c10f6dc8118c36b07d0
ms.openlocfilehash: c2e1832489d36f4ccbeae4e2f67e18caf941a68f

---

# JavaScript API 시험 응시

[시험 응시](https://technet.microsoft.com/edu/windows/take-tests-in-windows-10)는 고위험 테스트를 위해 잠긴 온라인 평가를 렌더링하는 브라우저 기반 앱입니다. 고위험 공통 코어 테스트를 위한 SBAC 브라우저 API 표준을 지원하고 Windows를 잠그는 방법 대신 평가 콘텐츠에 초점을 맞출 수 있습니다.

Microsoft Edge 브라우저를 기반으로 하는 시험 응시에서는 웹 응용 프로그램에서 시험을 위해 잠금 환경을 제공하는 데 사용할 수 있는 JavaScript API를 제공합니다.

이 API([공통 코어 SBAC API](http://www.smarterapp.org/documents/SecureBrowserRequirementsSpecifications_0-3.pdf) 기반)는 텍스트 음성 변환 기능은 물론 디바이스가 잠겼는지 여부, 실행 중인 사용자와 프로세스 등을 쿼리하는 기능도 제공합니다.

앱 자체에 대한 자세한 내용은 [Take a Test app technical reference](https://technet.microsoft.com/en-us/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396)(시험 응시 앱 기술 참조)를 참조하세요.

> [!Important]
> 이 API는 원격 세션에서 작동하지 않습니다.  

문제 해결 도움말은 [이벤트 뷰어를 사용하여 Microsoft 시험 응시 문제 해결](troubleshooting.md)을 참조하세요.

## 참조 설명서
시험 응시 API는 다음 네임스페이스로 구성됩니다. 

| 네임스페이스 | 설명 |
|-----------|-------------|
|[보안 네임스페이스](#security-namespace)|디바이스를 잠글 수 있습니다.|
|[tts 네임스페이스](#tts-namespace)|텍스트 음성 변환 기능|


 ### 보안 네임스페이스

디바이스를 잠그고, 사용자 및 시스템 프로세스 목록을 확인하고, MAC 및 IP 주소를 가져오고, 캐시된 웹 리소스를 지울 수 있습니다.

| 메서드 | 설명   |
|--------|---------------|
|[clearCache](#clearCache) | 캐시된 웹 리소스를 지웁니다. |
|[닫기](#close) | 브라우저를 닫고 디바이스의 잠금을 해제합니다. |
|[enableLockDown](#enableLockDown) | 디바이스를 잠급니다. 디바이스의 잠금을 해제하는 데도 사용됩니다. |
|[getIPAddressList](#getIPAddressList) | 디바이스의 IP 주소 목록을 가져옵니다. |
|[getMACAddress](#getMACAddress)|디바이스의 MAC 주소 목록을 가져옵니다.|
|[getProcessList](#getProcessList)|실행 중인 사용자 및 시스템 프로세스의 목록을 가져옵니다.|
|[isEnvironmentSecure](#isEnvironmentSecure)|잠금 컨텍스트가 디바이스에 적용되는지 결정합니다.|  

---
<span id="clearCache"/>
### void clearCache()
캐시된 웹 리소스를 지웁니다.

**구문**  
`browser.security.clearCache();`

**매개 변수**  
`None`

**반환 값**  
`None`

**요구 사항**  
Windows10 버전 1607

---

<span id="close"/>
### close(boolean restart)
브라우저를 닫고 디바이스의 잠금을 해제합니다.

**구문**  
`browser.security.close(false);`

**매개 변수**  
`restart` - 이 매개 변수는 무시되지만 제공해야 합니다.

**반환 값**  
`None`

**요구 사항**  
Windows10 버전 1607

---

<span id="enableLockDown"/>
### enableLockdown(boolean lockdown)
디바이스를 잠급니다. 디바이스의 잠금을 해제하는 데도 사용됩니다.

**구문**  
`browser.security.enableLockDown(true|false);`

**매개 변수**  
`lockdown` - `true` 잠금 화면 위에서 시험 응시 앱을 실행하고 이 [문서](https://technet.microsoft.com/en-us/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396)에 설명된 정책을 적용합니다. `False` 앱이 잠겨 있지 않으면 잠금 화면 위에서 시험 응시의 실행을 중지하고 닫습니다. 이 경우 영향을 주지 않습니다.

**반환 값**  
`None`

**요구 사항**  
Windows10 버전 1607

---

<span id="getIPAddressList"/>
### string[] getIPAddressList()
디바이스의 IP 주소 목록을 가져옵니다.

**구문**  
`browser.security.getIPAddressList();`

**매개 변수**  
`None`

**반환 값**  
`An array of IP addresses.`

---

<span id="getMACAddress" />
### string[] getMACAddress()
디바이스의 MAC 주소 목록을 가져옵니다.

**구문**  
`browser.security.getMACAddress();`

**매개 변수**  
`None`

**반환 값**  
`An array of MAC addresses.`

**요구 사항**  
Windows10 버전 1607

---

<span id="getProcessList" />
### string[] getProcessList()
사용자가 실행 중인 프로세스 목록을 가져옵니다.

**구문**  
`browser.security.getProcessList();`

**매개 변수**  
`None`

**반환 값**  
`An array of running process names.`

**설명** 시스템 프로세스는 이 목록에 포함되지 않습니다.

**요구 사항**  
Windows10 버전 1607

---

<span id="isEnvironmentSecure" />
### boolean isEnvironmentSecure()
잠금 컨텍스트가 디바이스에 적용되는지 결정합니다.

**구문**  
`browser.security.isEnvironmentSecure();`

**매개 변수**  
`None`

**반환 값**  
`True indicates that the lockdown context is applied to the device; otherwise false.`

**요구 사항**  
Windows10 버전 1607

---

### tts 네임스페이스

tts 네임스페이스는 앱의 텍스트 음성 변환 기능을 처리합니다.

| 메서드 | 설명 |
|--------|-------------|
|[getStatus](#getStatus) | 음성 재생 상태를 가져옵니다.|
|[getVoices](#getVoices) | 사용 가능한 음성 팩 목록을 가져옵니다.|
|[pause](#pause)|음성 합성을 일시 중지합니다.|
|[resume](#resume)|일시 중지된 음성 합성을 다시 시작합니다.|
|[speak](#speak)|클라이언트 쪽 텍스트 음성 변환 합성|
|[stop](#stop)|음성 합성을 중지합니다.|

> [!Tip]
> [Microsoft Edge 음성 합성 API](https://blogs.windows.com/msedgedev/2016/06/01/introducing-speech-synthesis-api/)는 [W3C Speech API](https://dvcs.w3.org/hg/speech-api/raw-file/tip/webspeechapi.html)의 구현이며 가능한 경우 개발자는 이 API를 사용하는 것이 좋습니다.

---

<span id="getStatus" />
### string getStatus()
음성 재생 상태를 가져옵니다.

**구문**  
`browser.tts.getStatus();`

**매개 변수**  
`None`

**반환 값**  
`The speech playback status. Possible values are: “available”, “idle”, “paused”, and “speaking”.`

**요구 사항**  
Windows10 버전 1607

---

<span id="getVoices" />
### string[] getVoices()
사용 가능한 음성 팩 목록을 가져옵니다.

**구문**  
`browser.tts.getVoices();`

**매개 변수**  
`None`

**반환 값**  
`The available voice packs. For example: “Microsoft Zira Mobile”, “Microsoft Mark Mobile”`

**요구 사항**  
Windows10 버전 1607

---

<span id="pause" />
### void pause()

음성 합성을 일시 중지합니다.

**구문**  
`browser.tts.pause();`

**매개 변수**

`None`

**반환 값**

`None`

**요구 사항**  
Windows10 버전 1607

---

<span id="resume" />
### void resume()
일시 중지된 음성 합성을 다시 시작합니다.

**구문**  
`browser.tts.resume();`

**매개 변수**
`None`

**반환 값**
`None`

**요구 사항**  
Windows10 버전 1607

---

<span id="speak" />
### void speak(string text, object options, function callback)
클라이언트 쪽 텍스트 음성 변환 합성을 시작합니다.

**구문**  
`void browser.tts.speak(“Hello world”, options, callback);`

**매개 변수**  
`Speech options such as gender, pitch, rate, volume. For example:`  
```
var options = {
   'gender': this.currentGender,
   'language': this.currentLanguage,
   'pitch': 1,
   'rate': 1,
   'voice': this.currentVoice,
   'volume': 1
};
```

**반환 값**  
`None`

**설명** 옵션 변수는 소문자여야 합니다. gender, language, voice 매개 변수는 문자열을 사용합니다.
volume, pitch, rate은 옵션 개체가 아닌 SSML(Speech Synthesis Markup Language) 파일 내에 표시되어야 합니다.
옵션 개체는 위의 예제에 표시된 순서, 이름 지정 및 대/소문자 표기를 따라야 합니다.

**요구 사항**  
Windows10 버전 1607

---

<span id="stop" />
### void stop()
음성 합성을 중지합니다.

**구문**  
`void browser.tts.speak(“Hello world”, options, callback);`

**매개 변수**  
`None`

**반환 값**  
`None`

**요구 사항**  
Windows10 버전 1607



<!--HONumber=Nov16_HO1-->


