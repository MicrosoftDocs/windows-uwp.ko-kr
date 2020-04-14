---
Description: 호스트 앱의 실행 파일, 진입점 및 런타임 특성을 상속 하는 호스트 된 앱을 빌드하는 방법에 대해 알아봅니다.
title: 호스트되는 앱 만들기
ms.date: 01/28/2020
ms.topic: article
keywords: windows 10, 데스크톱, 패키지, id, MSIX, Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: a3017073b15ea18214e9c78263fb212bb192132b
ms.sourcegitcommit: 8b7b677c7da24d4f39e14465beec9c4a3779927d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81266931"
---
# <a name="create-hosted-apps"></a>호스트되는 앱 만들기

Windows 10 버전 2004부터 *호스트 된 앱*을 만들 수 있습니다. 호스팅된 앱은 부모 *호스트* 앱과 동일한 실행 파일 및 정의를 공유 하지만 시스템에서 별도의 앱 처럼 보이고 동작 합니다.

호스팅된 앱은 독립 실행형 Windows 10 앱 처럼 동작 하는 구성 요소 (예: 실행 파일 또는 스크립트 파일)를 원하는 경우에 유용 하지만, 구성 요소에는 실행을 위해 호스트 프로세스가 필요 합니다. 예를 들어 PowerShell 또는 Python 스크립트를 실행 하기 위해 호스트를 설치 해야 하는 호스트 된 앱으로 배달할 수 있습니다. 호스팅된 앱에는 백그라운드 작업, 알림, 타일 및 공유 대상과 같은 Windows 10 기능과의 고유한 시작 타일, id 및 심층 통합이 있을 수 있습니다.

호스팅된 앱 기능은 호스트 된 앱이 호스트 앱 패키지에서 실행 파일 및 정의를 사용할 수 있도록 하는 패키지 매니페스트의 여러 요소 및 특성에서 지원 됩니다. 사용자가 호스트 된 앱을 실행 하면 OS에서 호스트 된 앱의 id로 호스트 실행 파일을 자동으로 시작 합니다. 호스트는 시각적 자산, 콘텐츠 또는 호출 Api를 호스트 된 앱으로 로드할 수 있습니다. 호스트 된 앱은 호스트와 호스트 된 앱 간에 선언 된 기능의 교집합을 가져옵니다. 즉, 호스트 된 앱이 호스트에서 제공 하는 것 보다 많은 기능을 요청할 수 없습니다.

## <a name="define-a-host"></a>호스트 정의

*호스트는 호스트* 된 앱에 대 한 기본 실행 파일 또는 런타임 프로세스입니다. 현재 유일 하 게 지원 되는 호스트는 *패키지 id*가 C++있는 데스크톱 앱 (.net 또는/win32)입니다. 지금은 UWP 앱이 호스트로 지원 되지 않습니다. 데스크톱 앱에 패키지 id를 포함 하는 방법에는 여러 가지가 있습니다.

* 데스크톱 앱에 패키지 id를 부여 하는 가장 일반적인 방법은 [MSIX 패키지에서](https://docs.microsoft.com/windows/msix)패키지 id를 패키징하는 것입니다.
* 경우에 따라 [스파스 패키지](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps)를 만들어 패키지 id를 부여 하도록 선택할 수도 있습니다. 이 옵션은 데스크톱 앱을 배포 하기 위해 MSIX 패키징을 채택할 수 없는 경우에 유용 합니다.

호스트는 **uap10: HostRuntime** 확장을 통해 패키지 매니페스트에 선언 됩니다. 이 확장에는 호스트 된 앱에 대 한 패키지 매니페스트에서 참조 하는 값을 할당 해야 하는 **Id** 특성이 있습니다. 호스트 된 앱이 활성화 되 면 호스트는 호스트 된 앱의 id로 시작 되 고 호스트 된 앱 패키지에서 콘텐츠나 이진 파일을 로드할 수 있습니다.

다음 예제에서는 패키지 매니페스트에서 호스트를 정의 하는 방법을 보여 줍니다. **Uap10: HostRuntime** 확장은 패키지 전체에 해당 하므로 [package](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-package) 요소의 자식으로 선언 됩니다.

``` xml
<Package xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10">

  <Extensions>
    <uap10:Extension Category="windows.hostRuntime"  
        Executable="PyScriptEngine\PyScriptEngine.exe"  
        uap10:RuntimeBehavior="packagedClassicApp"  
        uap10:TrustLevel="mediumIL">
      <uap10:HostRuntime Id="PythonHost" />
    </uap10:Extension>
  </Extensions>

</Package>
```

다음 요소에 대 한 중요 한 세부 정보를 기록해 둡니다.

| 요소              | 세부 정보 |
|----------------------|-------|
| **uap10: 확장** | `windows.hostRuntime` 범주는 호스트 된 앱을 활성화할 때 사용할 런타임 정보를 정의 하는 패키지 전체 확장을 선언 합니다. 호스팅된 앱은 확장에 선언 된 정의를 사용 하 여 실행 됩니다. 이전 예제에서 선언 된 호스트 앱을 사용 하면 호스트 된 앱이 **Mediumil** 신뢰 수준에서 실행 **PyScriptEngine** 실행 됩니다.<br/><br/>**실행 파일**, **Uap10: runtimebehavior**및 **uap10: TrustLevel** 특성은 패키지의 호스트 프로세스 이진 이름과 호스트 된 앱이 실행 되는 방법을 지정 합니다. 예를 들어 이전 예제에서 특성을 사용 하는 호스트 된 앱은 mediumIL 신뢰 수준에서 실행 PyScriptEngine 실행 됩니다. |
| **uap10:HostRuntime** | **Id** 특성은 패키지의 특정 호스트 앱에 대 한 고유 식별자를 선언 합니다. 패키지에는 여러 호스트 앱이 있을 수 있으며, 각각 고유한 **Id**를 가진 **uap10: HostRuntime** 요소가 있어야 합니다.

## <a name="declare-a-hosted-app"></a>호스팅된 앱 선언

호스트 된 *앱* 은 *호스트*에 대 한 패키지 종속성을 선언 합니다. 호스팅된 앱은 자체 패키지에서 진입점 실행 파일을 지정 하는 대신 호스트의 ID (즉, 호스트 패키지에서 **uap10: HostRuntime** 확장의 **id** 특성)를 활성화 합니다. 호스팅된 응용 프로그램에는 일반적으로 호스트에서 액세스할 수 있는 콘텐츠, 시각적 자산, 스크립트 또는 이진 파일이 포함 되어 있습니다. 호스트 된 앱 패키지의 [Targetdevicefamily](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) 값은 호스트와 동일한 값을 대상으로 해야 합니다.

호스트 된 앱 패키지는 서명 되거나 서명 되지 않을 수 있습니다.

* 서명 된 패키지는 실행 파일을 포함할 수 있습니다. 이는 호스트가 호스트 된 앱 패키지에서 DLL 또는 등록 된 구성 요소를 로드할 수 있도록 하는 이진 확장 메커니즘이 있는 시나리오에서 유용 합니다.
* 서명 되지 않은 패키지는 실행 파일이 아닌 파일만 포함할 수 있습니다. 호스트에서 이미지, 자산, 콘텐츠 또는 스크립트 파일을 로드 하기만 하면 되는 시나리오에서 유용 합니다. 서명 되지 않은 패키지는 [id](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 요소에 특수 한 `OID` 값을 포함 해야 하며 그렇지 않으면 등록할 수 없습니다. 이렇게 하면 서명 되지 않은 패키지가 서명 된 패키지의 id와 충돌 하거나이를 스푸핑 하는 것을 방지 합니다.

호스팅된 앱을 정의 하려면 패키지 매니페스트에서 다음 항목을 선언 합니다.

* **Uap10: HostRuntimeDependency** 요소입니다. 이는 [종속성](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-dependencies) 요소의 자식입니다.
* [응용 프로그램](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) 요소 (앱의 경우) 또는 [extension](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) 요소 (활성화 된 확장의 경우)의 **uap10: HostId** 특성입니다.

다음 예제에서는 서명 되지 않은 호스트 된 앱에 대 한 패키지 매니페스트의 관련 섹션을 보여 줍니다.

``` xml
<Package xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10">

  <Identity Name="NumberGuesserManifest"
    Publisher="CN=AppModelSamples, OID.2.25.311729368913984317654407730594956997722=1"
    Version="1.0.0.0" />

  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.19041.0" MaxVersionTested="10.0.19041.0" />
    <uap10:HostRuntimeDependency Name="PyScriptEnginePackage" Publisher="CN=AppModelSamples" MinVersion="1.0.0.0"/>
  </Dependencies>

  <Applications>
    <Application Id="NumberGuesserApp"  
      uap10:HostId="PythonHost"  
      uap10:Parameters="-Script &quot;NumberGuesser.py&quot;">
    </Application>
  </Applications>

</Package>
```

다음 요소에 대 한 중요 한 세부 정보를 기록해 둡니다.

| 요소              | 세부 정보 |
|----------------------|-------|
| [**신분을**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) | 이 예제의 호스팅된 응용 프로그램 패키지는 서명 되지 않으므로 **게시자** 특성에 `OID.2.25.311729368913984317654407730594956997722=1` 문자열이 포함 되어야 합니다. 이렇게 하면 서명 되지 않은 패키지가 서명 된 패키지의 id를 스푸핑할 수 없습니다. |
| [**Y**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) | **MinVersion** 특성은 10.0.19041.0 이상 OS 버전을 지정 해야 합니다. |
| **uap10:HostRuntimeDependency** | 이 요소 요소는 호스트 응용 프로그램 패키지에 대 한 종속성을 선언 합니다. 이는 호스트 패키지의 **이름** 및 **게시자** 와 종속 된 **MinVersion** 로 구성 됩니다. 이러한 값은 호스트 패키지의 [Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 요소 아래에서 찾을 수 있습니다. |
| **응용 프로그램** | **Uap10: HostId** 특성은 호스트에 대 한 종속성을 나타냅니다. 호스팅된 앱 패키지는 [응용 프로그램](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) 또는 [확장](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) 요소에 대 한 일반적인 **실행 파일** 및 **EntryPoint** 특성 대신이 특성을 선언 해야 합니다. 결과적으로 호스트 된 앱은 호스트에서 해당 **HostId** 값을 사용 하 여 **실행 파일**, **EntryPoint** 및 런타임 특성을 상속 합니다.<br/><br/>**Uap10: parameters** 특성은 호스트 실행 파일의 진입점 함수에 전달 되는 매개 변수를 지정 합니다. 호스트는 이러한 매개 변수를 사용 하 여 수행할 작업을 알고 있어야 하므로 호스트와 호스트 된 앱 간의 묵시적 계약이 있습니다. |

## <a name="register-an-unsigned-hosted-app-package-at-run-time"></a>런타임에 서명 되지 않은 호스트 된 앱 패키지 등록

**Uap10: HostRuntime** 확장의 장점 중 하나는 호스트가 런타임에 호스트 된 앱 패키지를 동적으로 생성 하 고이를 서명 하지 않고도 [PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) API를 사용 하 여 등록할 수 있다는 것입니다. 이렇게 하면 호스트에서 호스트 된 앱 패키지에 대 한 콘텐츠 및 매니페스트를 동적으로 생성 하 고 등록할 수 있습니다.

[PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) 클래스의 다음 메서드를 사용 하 여 서명 되지 않은 호스트 된 앱 패키지를 등록 합니다. 이러한 메서드는 Windows 10 버전 2004부터 사용할 수 있습니다.

* **AddPackageByUriAsync**: *Options* 매개 변수의 **allowunsigned** 속성을 사용 하 여 unsigned msix 패키지를 등록 합니다.
* **RegisterPackageByUriAsync**: 느슨한 패키지 매니페스트 파일 등록을 수행 합니다. 패키지가 서명 된 경우 매니페스트를 포함 하는 폴더에는 [따라서 appxsignature.p7x 파일](https://docs.microsoft.com/windows/msix/overview#inside-an-msix-package) 및 카탈로그가 포함 되어야 합니다. 서명 되지 않은 경우 *options* 매개 변수의 **allowunsigned** 속성을 설정 해야 합니다.

### <a name="requirements-for-unsigned-hosted-apps"></a>서명 되지 않은 호스트 된 앱에 대 한 요구 사항

* 패키지 매니페스트에 있는 [응용 프로그램](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) 또는 [확장](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) 요소는 **실행 파일**, **EntryPoint**또는 **TrustLevel** 특성과 같은 활성화 데이터를 포함할 수 없습니다. 대신 이러한 요소에는 호스트 및 **uap10: Parameters** 특성에 대 한 종속성을 표현 하는 **uap10: HostId** 특성만 포함 될 수 있습니다.
* 패키지는 주 패키지 여야 합니다. 번들, 프레임 워크 패키지, 리소스 또는 선택적 패키지가 될 수 없습니다.

### <a name="requirements-for-a-host-that-installs-and-registers-an-unsigned-hosted-app-package"></a>서명 되지 않은 호스트 된 앱 패키지를 설치 하 고 등록 하는 호스트에 대 한 요구 사항

* 호스트에 [패키지 id](#define-a-host)가 있어야 합니다.
* 호스트에는 **packageManagement** [제한 기능이](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#restricted-capabilities)있어야 합니다.
    ```xml
    <rescap:Capability Name="packageManagement" />
    ```

<!--
* If the host runs in app container (for example, it is a UWP app), it must also have the unsigned package management [custom capability](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#custom-capabilities) and a [Signed Custom Capability Descriptor (SCCD) file](https://docs.microsoft.com/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#preparing-the-signed-custom-capability-descriptor-sccd-file).
    ```xml
    <uap4:CustomCapability Name="Microsoft.unsignedPackageManagement_cw5n1h2txyewy" />
    ```
-->

## <a name="sample"></a>샘플

자신을 호스트로 선언 하 고 런타임에 호스팅된 앱 패키지를 동적으로 등록 하는 완전 한 기능을 갖춘 샘플 앱은 [호스팅된 앱 샘플](https://aka.ms/hostedappsample)을 참조 하세요.

### <a name="the-host"></a>호스트

호스트 이름은 **PyScriptEngine**입니다. Python 스크립트를 실행 하는 C# 에 작성 된 래퍼입니다. `-Register` 매개 변수를 사용 하 여 실행 하면 스크립트 엔진은 python 스크립트를 포함 하는 호스트 된 앱을 설치 합니다. 사용자가 새로 설치한 호스팅된 앱을 시작 하려고 하면 호스트가 시작 되 고 **NumberGuesser** python 스크립트를 실행 합니다.

호스트 앱에 대 한 패키지 매니페스트 (PyScriptEnginePackage 폴더에 있는 appxmanifest.xml 파일)에는 앱을 ID **PythonHost** 및 executable **PyScriptEngine**를 사용 하 여 호스트로 선언 하는 **uap10: HostRuntime** 확장이 포함 되어 있습니다.  

> [!NOTE]
> 이 샘플에서는 패키지 매니페스트의 이름이 appxmanifest.xml이 고 [Windows 응용 프로그램 패키징 프로젝트](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)의 일부입니다. 이 프로젝트를 빌드할 때 [appxmanifest.xml 라는 매니페스트를 생성](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/generate-package-manifest) 하 고 호스트 앱에 대 한 msix 패키지를 빌드합니다.

### <a name="the-hosted-app"></a>호스팅된 앱

호스팅된 앱은 python 스크립트와 패키지 매니페스트 등의 패키지 아티팩트로 구성 됩니다. PE 파일은 포함 되지 않습니다.

호스팅된 앱에 대 한 패키지 매니페스트 (NumberGuesser/Appxmanifest.xml 파일)에는 다음 항목이 포함 되어 있습니다.

* [Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 요소의 **Publisher** 특성에는 서명 되지 않은 패키지에 필요한 `OID.2.25.311729368913984317654407730594956997722=1` 식별자가 포함 됩니다.
* [Application](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) 요소의 **uap10: HostId** 특성은 **PythonHost** 를 호스트로 식별 합니다.

### <a name="run-the-sample"></a>예제 실행

이 샘플을 사용 하려면 10.0.19041.0 이상 버전의 Windows 10 및 Windows SDK 필요 합니다.

1. 개발 컴퓨터의 폴더에 [샘플](https://aka.ms/hostedappsample) 을 다운로드 합니다.
2. Visual Studio에서 PyScriptEngine 솔루션을 열고 **PyScriptEnginePackage** 프로젝트를 시작 프로젝트로 설정 합니다.
3. **PyScriptEnginePackage** 프로젝트를 빌드합니다.
4. 솔루션 탐색기에서 **PyScriptEnginePackage** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **배포**를 선택 합니다.
5. 샘플 파일을 복사한 디렉터리에서 명령 프롬프트 창을 열고 다음 명령을 실행 하 여 샘플 **NumberGuesser** 앱 (호스팅된 앱)을 등록 합니다. `D:\repos\HostedApps`를 샘플 파일을 복사한 경로로 변경 합니다.

    ```CMD
    D:\repos\HostedApps>pyscriptengine -Register D:\repos\HostedApps\NumberGuesser\AppxManifest.xml
    ```

    > [!NOTE]
    > 샘플의 호스트가 [Appexecutionalias](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias)를 선언 하기 때문에 명령줄에서 `pyscriptengine`를 실행할 수 있습니다.

6. **시작** 메뉴를 열고 **NumberGuesser** 를 클릭 하 여 호스팅된 앱을 실행 합니다.
