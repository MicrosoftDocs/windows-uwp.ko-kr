---
author: normesta
Description: Run your packaged app and see how it looks without having to sign it. Then, set breakpoints and step through code. When you're ready to test your app in a production environment, sign your app and then install it.
Search.Product: eADQiWindows 10XVcnh
title: 패키지 데스크톱 앱 실행, 디버그, 테스트(데스크톱 브리지)
ms.author: normesta
ms.date: 08/31/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.localizationpriority: medium
ms.openlocfilehash: 0f8d443bc201d0e60387673edb7f9b73e2e47490
ms.sourcegitcommit: 1773bec0f46906d7b4d71451ba03f47017a87fec
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/17/2018
ms.locfileid: "1662683"
---
# <a name="run-debug-and-test-a-packaged-desktop-app-desktop-bridge"></a>패키지 데스크톱 앱 실행, 디버그, 테스트(데스크톱 브리지)

패키지로 만든 앱을 실행하고 로그인할 필요 없이 어떻게 표시가 되는지 확인합니다. 그런 다음, 중단점을 설정하고 코드를 단계별로 실행합니다. 프로덕션 환경에서 앱을 이미 테스트했다면 앱에 로그인한 다음 설치합니다. 이 항목에서는 이러한 작업을 수행하는 방법을 보여 줍니다.

<a id="run-app" />

## <a name="run-your-app"></a>앱 실행

인증서를 얻어 로그인할 필요 없이 앱을 실행하여 로컬 테스트를 수행할 수 있습니다. 앱을 실행하는 방법은 패키지를 만드는 데 사용한 도구에 따라 달라집니다.

### <a name="you-created-the-package-by-using-visual-studio"></a>Visual Studio를 사용하여 패키지를 만든 경우

패키징 프로젝트를 시작 프로젝트로 설정하고 나서 Ctrl+F5 키를 눌러 앱을 시작합니다.

### <a name="you-created-the-package-manually-or-by-using-the-desktop-app-converter"></a>수동으로 또는 Desktop App Converter를 사용하여 패키지를 작성한 경우

Windows PowerShell 명령 프롬프트를 열고 출력 폴더의 하위 폴더 **PackageFiles**에서 다음 cmdlet을 실행합니다.

```
Add-AppxPackage –Register AppxManifest.xml
```
앱을 시작 하려면 Windows 시작 메뉴에서 앱을 찾습니다.

![시작 메뉴의 패키지 앱](images/desktop-to-uwp/converted-app-installed.png)

> [!NOTE]
> 패키지로 만든 앱은 항상 대화형 사용자로서 실행이 되며, 패키지 앱이 설치될 모든 드라이브는 NTFS 형식으로 포맷해야 합니다.

## <a name="debug-your-app"></a>앱 디버그

앱을 디버그하는 방법은 패키지를 만드는 데 사용한 도구에 따라 달라집니다.

Visual Studio 2017 15.4 릴리스에서 사용할 수 있는 [새 패키징 프로젝트](desktop-to-uwp-packaging-dot-net.md#new-packaging-project)를 사용하여 패키지를 만든 경우 패키징 프로젝트를 시작 프로젝트로 설정한 후 F5 키를 눌러 앱을 디버그합니다.

기타 도구를 사용하여 패키지를 만든 경우 다음 단계를 따르세요.

1. 패키지 앱을 한 번 이상 시작, 로컬 컴퓨터에 설치되어 있는지 확인합니다.

   위의 [앱 실행](#run-app) 섹션을 참조하세요.

2. Visual Studio를 시작합니다.

   승격된 권한으로 앱을 디버그하려는 경우에는 **관리자 권한으로 실행** 옵션을 사용해 Visual Studio를 시작합니다.

3. Visual Studio에서 **디버그**->**기타 디버그 대상**->**설치 된 앱 패키지 디버그**를 선택합니다.

4. **설치된 앱 패키지** 목록에서 앱 패키지를 선택한 다음 **첨부** 버튼을 선택합니다.

#### <a name="modify-your-app-in-between-debug-sessions"></a>디버그 세션 간에 앱을 수정

버그를 수정하여 앱의 내용을 변경할 경우, MakeAppx 도구를 사용하여 다시 패키징합니다. [MakeAppx 도구 실행](desktop-to-uwp-manual-conversion.md#make-appx)을 참조하세요.

### <a name="debug-the-entire-app-lifecycle"></a>전체 앱 수명 주기 디버깅

그러나 경우에 따라 앱이 시작되기 전에 디버깅하는 기능을 포함하여 디버깅 프로세스에서 세부적으로 제어할 수 있습니다.

[PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx)를 사용해 일시 중지, 다시 시작, 종료를 포함해 앱 수명 주기를 완전히 제어할 수 있습니다.

[PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx)는 Windows SDK에 포함되어 있습니다.

## <a name="test-your-app"></a>앱 테스트

앱 배포를 준비할 때 현실적인 설정에서 앱을 테스트하려면 앱에 로그인한 다음 설치하는 것이 가장 좋습니다.

### <a name="test-an-app-that-you-packaged-by-using-visual-studio"></a>Visual Studio를 사용하여 패키징한 앱 테스트

Visual Studio는 테스트 인증서를 사용하여 앱에 서명합니다. **앱 패키지 만들기** 마법사가 생성하는 출력 폴더에서 해당 인증서를 찾을 수 있습니다. 인증서 파일에는 *.cer* 확장이 있으며 해당 인증서를 앱을 테스트하려는 PC의 **신뢰할 수 있는 루트 인증 기관** 저장소에 설치해야 합니다. [패키지 사이드로드](../packaging/packaging-uwp-apps.md#sideload-your-app-package)를 참조하세요.

### <a name="test-an-app-that-you-packaged-by-using-the-desktop-app-converter-dac"></a>Desktop App Converter(DAC)를 사용하여 패키징한 앱 테스트

Desktop App Converter를 사용하여 앱을 패키징한 경우에는 ``sign``매개 변수를 사용하면 생성된 인증서를 통해 앱에 자동으로 로그인할 수 있습니다. 해당 인증서를 설치한 다음 앱을 설치해야 합니다. [패키징된 앱 실행](desktop-to-uwp-run-desktop-app-converter.md#run-app)을 참조하세요.   


### <a name="manually-sign-apps-optional"></a>수동으로 앱 서명(선택 사항)

또한 수동으로 앱에 서명할 수 있습니다. 방법

1. 인증서를 만듭니다. [인증서 만들기](../packaging/create-certificate-package-signing.md)를 참조하세요.

2. 해당 인증서를 시스템의 **신뢰할 수 있는 루트** 또는 **신뢰할 수 있는 사용자** 인증서 저장소에 설치합니다.

3. 인증서를 사용해 앱에 로그인하는 방법은 [SignTool을 사용하여 앱 패키지 로그인](../packaging/sign-app-package-using-signtool.md)을 참조하세요.

  > [!IMPORTANT]
  > 인증서의 게시자 이름이 앱 게시자 이름과 일치하는지 확인합니다.

    **관련 샘플**

    [SigningCerts](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SigningCerts)


### <a name="test-your-app-for-windows-10-s"></a>Windows 10 S용 앱을 테스트합니다.

앱을 게시하기 전에, Windows 10 S가 실행되는 장치에서 올바르게 작동하는지 확인하세요. 앱을 Microsoft Store에 게시할 계획을 갖고 있다면, 반드시 확인을 해야 합니다. Microsoft Store의 요구 사항이기 때문입니다. Windows 10 S를 실행하는 장치에서 올바르게 작동하지 않는 앱은 인증이 되지 않습니다.

[Windows 10 S용 Windows 앱 테스트](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-test-windows-s)를 참조하세요.

### <a name="run-another-process-inside-the-full-trust-container"></a>완전 신뢰 컨테이너 내 다른 프로세스 실행

지정한 앱 패키지의 컨테이너 내에서 사용자 지정 프로세스를 호출할 수 있습니다. 이는 시나리오 테스트에 유용할 수 있습니다(예: 사용자 지정 테스트 도구가 있어 앱의 출력을 테스트할 경우). 이렇게 하려면 ```Invoke-CommandInDesktopPackage``` PowerShell cmdlet을 사용합니다.

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## <a name="next-steps"></a>다음 단계

**질문에 대한 답변 찾기**

질문이 있으세요? Stack Overflow에서 질문해 주세요. 저희 팀은 이러한 [태그](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다. [여기](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)에서 Microsoft에 문의할 수도 있습니다.

**피드백 제공 또는 기능 제안**

[UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)를 참조하세요.
