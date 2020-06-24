---
title: 프로세스 간 통신(IPC)
description: 이 항목에서는 유니버설 Windows 플랫폼 (UWP) 응용 프로그램 및 Win32 응용 프로그램 간에 IPC (프로세스 간 통신)를 수행 하는 다양 한 방법에 대해 설명 합니다.
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp
ms.openlocfilehash: 5db029db3ffb538802f39aa616c96dbe75601eac
ms.sourcegitcommit: bf7d4f6739aeeaac735aae3dd0dcbda63a8c5e69
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85256383"
---
# <a name="interprocess-communication-ipc"></a>프로세스 간 통신(IPC)

이 항목에서는 유니버설 Windows 플랫폼 (UWP) 응용 프로그램 및 Win32 응용 프로그램 간에 IPC (프로세스 간 통신)를 수행 하는 다양 한 방법에 대해 설명 합니다.

## <a name="app-services"></a>앱 서비스

앱 서비스를 사용 하면 응용 프로그램에서 기본 ([**Valueset**](/uwp/api/Windows.Foundation.Collections.ValueSet))의 속성 모음을 허용 하 고 반환 하는 서비스를 백그라운드에서 노출할 수 있습니다. [Serialize](https://stackoverflow.com/questions/46367985/how-to-make-a-class-that-can-be-added-to-the-windows-foundation-collections-valu)된 경우에는 다양 한 개체가 전달 될 수 있습니다.

앱 서비스는 백그라운드 작업으로 [프로세스](/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service) 를 진행 하거나 포그라운드 응용 프로그램 내에서 [프로세스](/windows/uwp/launch-resume/convert-app-service-in-process) 를 실행할 수 있습니다.

앱 서비스는 거의 실시간 대기 시간이 필요 하지 않은 소량의 데이터를 공유 하는 데 가장 적합 합니다.

## <a name="com"></a>COM

[COM](/windows/win32/com/component-object-model--com--portal) 은 상호 작용 하 고 통신할 수 있는 이진 소프트웨어 구성 요소를 만들기 위한 분산 개체 지향 시스템입니다. 개발자는 COM을 사용 하 여 응용 프로그램에 대 한 재사용 가능한 소프트웨어 구성 요소 및 자동화 계층을 만듭니다. COM 구성 요소는 프로세스 또는 프로세스를 처리할 수 있으며 [클라이언트 및 서버](/windows/win32/com/com-clients-and-servers) 모델을 통해 통신할 수 있습니다. Out-of-process COM 서버는 [개체 간 통신](/windows/win32/com/inter-object-communication)을 위한 수단으로 오래 사용 되었습니다.

[RunFullTrust](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities) 기능이 포함 된 패키지 된 응용 프로그램은 [패키지 매니페스트](/uwp/schemas/appxpackage/uapmanifestschema/element-com-extension)를 통해 IPC 용 out-of-process COM 서버를 등록할 수 있습니다. 이를 패키지 된 [COM](https://blogs.windows.com/windowsdeveloper/2017/04/13/com-server-ole-document-support-desktop-bridge/)이라고 합니다.

## <a name="filesystem"></a>파일 시스템

### <a name="broadfilesystemaccess"></a>BroadFileSystemAccess

패키지 된 응용 프로그램은 [broadFileSystemAccess](/windows/uwp/files/file-access-permissions#accessing-additional-locations) 제한 된 기능을 선언 하 여 광범위 한 파일 시스템을 통해 IPC를 수행할 수 있습니다. 이 기능은 [Windows 저장소](/uwp/api/Windows.Storage) api를 부여 하 고 광범위 한 파일 시스템에 대 한 Win32 api 액세스를 [xxxFromApp](/previous-versions/windows/desktop/legacy/mt846585(v=vs.85)) 합니다.

기본적으로 패키지 된 응용 프로그램에 대 한 파일 시스템을 통한 IPC는이 섹션에 설명 된 다른 메커니즘으로 제한 됩니다.

### <a name="publishercachefolder"></a>PublisherCacheFolder

[PublisherCacheFolder](/uwp/api/windows.storage.applicationdata.getpublishercachefolder) 를 사용 하면 패키지 된 응용 프로그램에서 동일한 게시자가 다른 패키지와 공유할 수 있는 폴더를 해당 매니페스트에 선언할 수 있습니다.

공유 저장소 폴더에는 다음과 같은 요구 사항과 제한 사항이 있습니다.

* 공유 저장소 폴더의 데이터는 백업 되거나 로밍 되지 않습니다.
* 사용자가 공유 저장소 폴더의 내용을 지울 수 있습니다.
* 공유 저장소 폴더를 사용 하 여 서로 다른 게시자의 응용 프로그램 간에 데이터를 공유할 수 없습니다.
* 공유 저장소 폴더를 사용 하 여 여러 사용자 간에 데이터를 공유할 수는 없습니다.
* 공유 저장소 폴더에는 버전 관리가 없습니다.

여러 응용 프로그램을 게시 하 고 두 응용 프로그램 간에 데이터를 공유 하는 간단한 메커니즘을 원하는 경우 PublisherCacheFolder는 간단한 파일 시스템 기반 옵션입니다.

### <a name="sharedaccessstoragemanager"></a>SharedAccessStorageManager

[Sharedaccessstoragemanager](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) 는 토큰을 통해 storagefiles를 공유 하기 위해 App services, 프로토콜 활성화 (예: LaunchUriForResultsAsync) 등의와 함께 사용 됩니다.

## <a name="fulltrustprocesslauncher"></a>FullTrustProcessLauncher 관리자

[RunFullTrust](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities) 기능을 사용 하면 패키지 된 응용 프로그램이 동일한 패키지 내에서 [완전 신뢰 프로세스를 시작할](/uwp/api/Windows.ApplicationModel.FullTrustProcessLauncher) 수 있습니다.

패키지 제한이 부담이 되는 시나리오 또는 IPC 옵션을 사용 하지 않는 시나리오의 경우 응용 프로그램은 시스템을 사용 하 여 인터페이스 하는 프록시로 완전 신뢰 프로세스를 사용 하 고 App services 또는 기타 잘 지원 되는 다른 IPC 메커니즘을 통해 완전 신뢰 프로세스 자체로 IPC를 사용할 수 있습니다.

## <a name="launchuriforresultsasync"></a>LaunchUriForResultsAsync

[LaunchUriForResultsAsync](/windows/uwp/launch-resume/how-to-launch-an-app-for-results) 은 [protocolforresults](/windows/uwp/launch-resume/how-to-launch-an-app-for-results#step-2-override-applicationonactivated-in-the-app-that-youll-launch-for-results) 활성화 계약을 구현 하는 다른 패키지 된 응용 프로그램과 단순 ([valueset](/uwp/api/Windows.Foundation.Collections.ValueSet)) 데이터 교환에 사용 됩니다. 일반적으로 백그라운드에서 실행 되는 App services와는 달리 대상 응용 프로그램이 포그라운드에서 시작 됩니다.

ValueSet를 통해 응용 프로그램에 [SharedStorageAccessManager](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) 토큰을 전달 하 여 파일을 공유할 수 있습니다.

## <a name="loopback"></a>루프백

루프백은 localhost (루프백 주소)에서 수신 대기 하는 네트워크 서버와 통신 하는 프로세스입니다.

보안 및 네트워크 격리를 유지 하기 위해 IPC에 대 한 루프백 연결은 기본적으로 패키지 된 응용 프로그램에 대해 차단 됩니다. [기능](/previous-versions/windows/apps/hh770532(v=win.10)) 및 [매니페스트 속성](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules)을 사용 하 여 신뢰할 수 있는 패키지 응용 프로그램 간에 루프백 연결을 사용 하도록 설정할 수 있습니다.

* 루프백 연결에 참여 하는 패키지 된 응용 프로그램은 모두 `privateNetworkClientServer` [패키지 매니페스트에서](/uwp/schemas/appxpackage/uapmanifestschema/element-capability)기능을 선언 해야 합니다.
* 패키지 매니페스트 내에서 [LoopbackAccessRules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules) 를 선언 하 여 두 개의 패키지 된 응용 프로그램이 루프백을 통해 통신할 수 있습니다.
    * 각 응용 프로그램은 [LoopbackAccessRules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules)에 다른 응용 프로그램을 나열 해야 합니다. 클라이언트는 서버에 대 한 "출력" 규칙을 선언 하 고 서버는 지원 되는 클라이언트에 대 한 "in" 규칙을 선언 합니다.

> [!NOTE]
> 이러한 규칙에서 응용 프로그램을 식별 하는 데 필요한 패키지 패밀리 이름은 개발 시간 동안 Visual Studio의 패키지 매니페스트 편집기를 통해, Microsoft Store을 통해 게시 된 응용 프로그램의 경우 [파트너 센터](/windows/uwp/publish/view-app-identity-details) 를 통해, 이미 설치 된 응용 프로그램의 경우 [add-appxpackage](/powershell/module/appx/get-appxpackage?view=win10-ps) PowerShell 명령을 통해 찾을 수 있습니다.

패키지 되지 않은 응용 프로그램 및 서비스에는 패키지 id가 없으므로 [LoopbackAccessRules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules)에서 선언할 수 없습니다. [CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10))를 통해 루프백을 통해 패키지 되지 않은 응용 프로그램 및 서비스와 연결 하도록 패키지 된 응용 프로그램을 구성할 수 있지만이는 컴퓨터에 로컬로 액세스 하 고 관리자 권한이 있는 테스트용으로 로드 또는 디버깅 시나리오 에서만 가능 합니다.

* 루프백 연결에 참여 하는 패키지 된 응용 프로그램은 모두 `privateNetworkClientServer` [패키지 매니페스트에서](/uwp/schemas/appxpackage/uapmanifestschema/element-capability)기능을 선언 해야 합니다.
* 패키지 응용 프로그램이 패키지 되지 않은 응용 프로그램 또는 서비스에 연결 하는 경우를 실행 `CheckNetIsolation.exe LoopbackExempt -a -n=<PACKAGEFAMILYNAME>` 하 여 패키지 된 응용 프로그램에 대 한 루프백 예외를 추가 합니다.
* 패키지 되지 않은 응용 프로그램 또는 서비스가 패키지 된 응용 프로그램에 연결 되는 경우 `CheckNetIsolation.exe LoopbackExempt -is -n=<PACKAGEFAMILYNAME>` 를 실행 하 여 패키지 된 응용 프로그램이 인바운드 루프백 연결을 수신할 수 있도록 합니다.
    * 패키지 된 응용 프로그램이 연결을 수신 대기 하는 동안에는 [CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10)) 를 계속 실행 해야 합니다.
    * `-is`플래그가 Windows 10, 버전 1607 (10.0;)에서 도입 되었습니다. 빌드 14393).

> [!NOTE]
> CheckNetIsolation.exe플래그에 필요한 패키지 패밀리 이름은 `-n` 개발 시간 [CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10)) 동안 Visual Studio의 패키지 매니페스트 편집기를 통해, Microsoft Store를 통해 게시 된 응용 프로그램의 경우 [파트너 센터](/windows/uwp/publish/view-app-identity-details) 를 통해, 이미 설치 된 응용 프로그램의 경우 [add-appxpackage](/powershell/module/appx/get-appxpackage?view=win10-ps) PowerShell 명령을 통해 찾을 수 있습니다.

[CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10)) 는 [네트워크 격리 문제를 디버깅](/previous-versions/windows/apps/hh780593(v=win.10)#debug-network-isolation-issues)하는 데에도 유용 합니다.

## <a name="pipes"></a>파이프

[파이프는](/windows/win32/ipc/pipes) 파이프 서버와 하나 이상의 파이프 클라이언트 간의 간단한 통신을 가능 하 게 합니다.

[익명 파이프](/windows/win32/ipc/anonymous-pipes) 및 [명명 된 파이프](/windows/win32/ipc/named-pipes) 는 다음과 같은 제약 조건에서 지원 됩니다.

* 기본적으로 패키지 응용 프로그램의 명명 된 파이프는 프로세스가 완전 신뢰 되지 않는 한 동일한 패키지 내의 프로세스 간에만 지원 됩니다.
* 명명 된 파이프는 [명명 된 개체를 공유](/windows/uwp/communication/sharing-named-objects)하는 지침에 따라 패키지에서 공유할 수 있습니다.
* 패키지 응용 프로그램의 명명 된 파이프는 파이프 이름에 구문을 사용 해야 합니다 `\\.\pipe\LOCAL\` .

## <a name="registry"></a>레지스트리

IPC에 대 한 [레지스트리](/windows/win32/sysinfo/registry-functions) 사용은 일반적으로 권장 되지 않지만 기존 코드에 대해 지원 됩니다. 패키지 된 응용 프로그램은 액세스 권한이 있는 레지스트리 키에만 액세스할 수 있습니다.

[Msix으로 패키지 된 데스크톱 응용 프로그램](/windows/msix/desktop/desktop-to-uwp-root) 은 msix 패키지 내의 개인 hive에 글로벌 레지스트리 쓰기가 포함 되도록 [레지스트리 가상화](/windows/msix/desktop/desktop-to-uwp-behind-the-scenes#registry) 를 활용 합니다. 이렇게 하면 전역 레지스트리 영향을 최소화 하는 동시에 소스 코드 호환성이 가능 하며 동일한 패키지의 프로세스 간 IPC에 사용할 수 있습니다. 레지스트리를 사용 해야 하는 경우에는 전역 레지스트리를 조작 하는 것과이 모델을 사용 하는 것이 좋습니다.

## <a name="rpc"></a>RPC

패키지 응용 프로그램이 RPC 끝점의 Acl과 일치 하는 올바른 기능을 제공 하는 경우 [rpc](/windows/win32/rpc/rpc-start-page) 를 사용 하 여 패키지 된 응용 프로그램을 Win32 RPC 끝점에 연결할 수 있습니다.

사용자 지정 기능을 통해 Oem 및 Ihv는 [임의의 기능을 정의](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#reserving-a-custom-capability)하 고, 해당 [RPC 끝점을 사용 하 여 ACL](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#allowing-access-to-an-rpc-endpoint-to-a-uwp-app-using-the-custom-capability)을 만든 다음, [권한 있는 클라이언트 응용 프로그램에 이러한 기능을 부여할](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#preparing-the-signed-custom-capability-descriptor-sccd-file)수 전체 샘플 응용 프로그램은 [Customcapability](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomCapability) 샘플을 참조 하세요.

또한 RPC 끝점은 특정 패키지 응용 프로그램에 대 한 액세스를 제한 하 여 끝점에 대 한 액세스를 사용자 지정 기능의 관리 오버 헤드로 요구 하지 않고 해당 응용 프로그램 으로만 제한할 수 있습니다. [DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername) API를 사용 하 여 패키지 제품군 이름에서 sid를 파생 시킨 다음 [CUSTOMCAPABILITY](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/CustomCapability/Service/Server/RpcServer.cpp) 샘플과 같이 sid를 사용 하 여 RPC 끝점에 ACL을 지정할 수 있습니다.

## <a name="shared-memory"></a>공유 메모리

[파일 매핑은](/windows/win32/memory/sharing-files-and-memory) 다음 제약 조건을 사용 하 여 두 개 이상의 프로세스 간에 파일 또는 메모리를 공유 하는 데 사용할 수 있습니다.

* 기본적으로 패키지 응용 프로그램의 파일 매핑은 프로세스가 완전 신뢰를 제외 하 고는 동일한 패키지 내의 프로세스 간에만 지원 됩니다.
* [명명 된 개체를 공유](/windows/uwp/communication/sharing-named-objects)하는 방법에 대 한 지침에 따라 패키지 간에 파일 매핑을 공유할 수 있습니다.

많은 양의 데이터를 효율적으로 공유 하 고 조작 하기 위해 공유 메모리를 권장 합니다.
