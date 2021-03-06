---
ms.assetid: 6AA037C0-35ED-4B9C-80A3-5E144D7EE94B
title: WinAppDeployCmd.exe 도구를 사용하여 앱 설치
description: Windows 응용 프로그램 배포(WinAppDeployCmd.exe)는 Windows 10 PC에서 Windows 10 디바이스로 UWP(유니버설 Windows 플랫폼) 앱을 배포하는 데 사용할 수 있는 명령줄 도구입니다.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 265f505c8a1ad500417800774a5e175f777c9ff9
ms.sourcegitcommit: 2b7f6fdb3c393f19a6ad448773126a053b860953
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/12/2021
ms.locfileid: "100334933"
---
# <a name="install-apps-with-the-winappdeploycmdexe-tool"></a>WinAppDeployCmd.exe 도구를 사용하여 앱 설치

Windows 응용 프로그램 배포(WinAppDeployCmd.exe)는 Windows 10 PC에서 Windows 10 디바이스로 UWP(유니버설 Windows 플랫폼) 앱을 배포하는 데 사용할 수 있는 명령줄 도구입니다. Windows 10 디바이스가 Microsoft Visual Studio 또는 해당 앱의 솔루션 없이도 동일한 서브넷에서 사용 가능하거나 USB로 연결되면 이 도구를 사용하여 앱 패키지를 배포할 수 있습니다. 또한 원격 PC 또는 Xbox One에 먼저 패키징하지 않고 앱을 배포할 수도 있습니다. 이 문서는 이 도구를 사용하여 UWP 앱을 설치하는 방법을 설명합니다.

명령 프롬프트 또는 스크립트 파일에서 WinAppDeployCmd 도구를 실행하려면 Windows 10 SDK만 설치하면 됩니다. WinAppDeployCmd.exe로 앱을 설치하면 이 도구는 .appx/.msix 파일 또는 AppxManifest(느슨한 파일의 경우)를 사용하여 앱을 Windows 10 디바이스에 테스트용으로 로드합니다. 이 명령은 앱에 필요한 인증서를 설치하지 않습니다. 앱을 실행하려면 Windows 10 디바이스가 개발자 모드이거나 인증서가 이미 설치되어 있어야 합니다.

모바일 디바이스에 배포하려면 먼저 패키지를 만들어야 합니다. 자세한 내용은 [여기](/windows/msix/package/packaging-uwp-apps)를 참조하세요.

**WinAppDeployCmd.exe** 도구는 Windows 10 PC의 **C:\\Program Files (x86)\\Windows Kits\\10\\bin\\&lt;SDK Version&gt;\\x86\\WinAppDeployCmd.exe**(SDK 설치 경로에 따라 다름) 위치에 설치되어 있습니다.

> [!NOTE]
> SDK 버전 15063 이상에서는 SDK가 버전별 폴더에 나란히 설치됩니다. 이전 SDK(14393 및 이전 버전)는 부모 폴더에 직접 작성되었습니다.

먼저 Windows 10 디바이스를 동일한 서브넷에 연결하거나 USB 연결로 Windows 10 컴퓨터에 직접 연결합니다. 그런 후 다음 구문이나 이 문서의 뒷부분에 있는 이 명령의 예를 사용하여 UWP 앱을 배포합니다.

## <a name="winappdeploycmd-syntax-and-options"></a>WinAppDeployCmd 구문 및 옵션

**WinAppDeployCmd.exe** 에 사용되는 일반 구문은 다음과 같습니다.

```CMD
WinAppDeployCmd command -option <argument>
```

다음은 다양한 명령을 사용하는 몇 가지 추가 구문 예입니다.

```CMD
WinAppDeployCmd devices
WinAppDeployCmd devices <x>
WinAppDeployCmd install -file <path> -ip <address>
WinAppDeployCmd install -file <path> -guid <address> -pin <p>
WinAppDeployCmd install -file <path> -ip <address> -dependency <a> <b> 
WinAppDeployCmd install -file <path> -guid <address> -dependency <a> <b>
WinAppDeployCmd uninstall -file <path>
WinAppDeployCmd uninstall -package <name>
WinAppDeployCmd update -file <path>
WinAppDeployCmd list -ip <address>
WinAppDeployCmd list -guid <address>
WinAppDeployCmd deployfiles -file <path> -remotedeploydir <remoterelativepath> -ip <address>
WinAppDeployCmd registerfiles -remotedeploydir <remoterelativepath> -ip <address>
WinAppDeployCmd addcreds -credserver <server> -credusername <username> -credpassword <password> -ip <address>
WinAppDeployCmd getcreds -credserver <server> -ip <address>
WinAppDeployCmd deletecreds -credserver <server> -ip <address>
```

대상 장치에 앱을 설치 또는 제거하거나 이미 설치된 응용 프로그램을 업데이트할 수 있습니다. 이미 설치된 앱으로 저장된 데이터나 설정을 유지하려면 **install** 옵션 대신 **update** 옵션을 사용합니다.

다음 표에서는 **WinAppDeployCmd.exe** 에 대한 명령에 대해 설명합니다.

| **Command**  | **설명**                                                     |
|--------------|---------------------------------------------------------------------|
| 디바이스      | 사용 가능한 네트워크 장치의 목록을 표시합니다.                         |
| 설치      | UWP 앱 패키지를 대상 디바이스에 설치합니다.                     |
| update       | 대상 디바이스에 이미 설치되어 있는 UWP 앱을 업데이트합니다.    |
| list         | 지정된 대상 디바이스에 설치된 UWP 앱 목록을 표시합니다. |
| 제거    | 대상 장치에서 지정된 앱 패키지를 제거합니다.         |
| deployfiles  | 대상 경로의 느슨한 파일 앱을 디바이스의 원격 상대 경로로 복사합니다.|
| registerfiles| 원격 배포 디렉터리에서 느슨한 파일 앱을 등록합니다.         |
| addcreds     | Xbox에 자격 증명을 추가하여 앱 등록을 위한 네트워크 위치에 액세스할 수 있게 합니다.|
| getcreds     | 네트워크 공유에서 애플리케이션을 실행할 때 대상 용도로 네트워크 자격 증명을 가져옵니다.|
| deletecreds  | 네트워크 공유에서 애플리케이션을 실행할 때 대상이 사용하는 네트워크 자격 증명을 삭제합니다.|

다음 표에서는 **WinAppDeployCmd.exe** 에 대한 옵션에 대해 설명합니다.

| **Command**  | **설명**  |
|--------------|------------------|
| -h(-help)       | 명령, 옵션 및 인수를 표시합니다. |
| -ip              | 대상 장치의 IP 주소입니다. |
| -g(-guid)       | 대상 장치의 고유 식별자입니다.|
| -d(-dependency) | (선택 사항) 각 패키지 종속성에 대한 종속성 경로를 지정합니다. 경로가 지정되지 않으면 도구에서 앱 패키지 및 SDK 디렉터리에 대한 루트 디렉터리의 종속성을 검색합니다.|
| -f(-file)       | 설치, 업데이트 또는 제거할 앱 패키지에 대한 파일 경로입니다.|
| -p(-package)    | 제거할 앱 패키지의 전체 패키지 이름입니다. list 명령을 사용하여 이미 디바이스에 설치된 패키지의 전체 이름을 확인할 수 있습니다. |
| -pin             | 대상 장치에 연결하는 데 필요한 경우 PIN입니다. 인증이 필요한 경우 -pin 옵션을 사용하여 다시 시도하라는 메시지가 표시됩니다. |
| -credserver      | 대상에서 사용할 네트워크 자격 증명의 서버 이름입니다. |
| -credusername    | 대상에서 사용할 네트워크 자격 증명의 사용자 이름입니다. |
| -credpassword    | 대상에서 사용할 네트워크 자격 증명의 암호입니다. |
| -connecttimeout  | 디바이스에 연결할 때 사용되는 시간 제한(초)입니다. |
| -remotedeploydir | 원격 디바이스에서 파일을 복사할 상대 디렉터리 경로/이름입니다. 잘 알려진, 자동으로 결정된 원격 배포 폴더입니다. |
| -deleteextrafile | 원본 디렉터리에 맞게 원격 디렉터리의 기존 파일을 제거해야 하는지 여부를 나타내는 스위치입니다. |

다음 표에서는 **WinAppDeployCmd.exe** 에 대한 옵션에 대해 설명합니다.

| **Argument**           | **설명**                                                              |
|------------------------|------------------------------------------------------------------------------|
| &lt;x&gt;              | 제한 시간(초)입니다. (기본값은 10)                                          |
| &lt;address&gt;        | 대상 장치의 IP 주소 또는 고유 식별자입니다.                        |
| &lt;a&gt;&lt;b&gt; ... | 각 앱 패키지 종속성에 대한 종속성 경로입니다.                    |
| &lt;p&gt;              | 연결을 설정하는 장치 설정에 표시되는 영숫자 PIN입니다. |
| &lt;path&gt;           | 파일 시스템 경로입니다.                                                            |
| &lt;name&gt;           | 제거할 앱 패키지의 전체 패키지 이름입니다.                          |
| &lt;server&gt;         | 파일 네트워크의 서버입니다.                                                  |
| &lt;username&gt;       | 파일 네트워크의 서버에 액세스할 수 있는 자격 증명의 사용자입니다.      |
| &lt;password&gt;       | 파일 네트워크의 서버에 액세스할 수 있는 자격 증명의 암호입니다. |
| &lt;remotedeploydir&gt;| 배포 위치에 상대적인 디바이스의 디렉터리                      |

## <a name="winappdeploycmdexe-examples"></a>WinAppDeployCmd.exe 예제

다음은 **WinAppDeployCmd.exe** 의 구문을 사용하여 명령줄에서 배포하는 방법을 보여 주는 몇 가지 예입니다.

배포에 사용할 수 있는 장치를 보여줍니다. 명령의 제한 시간은 3초입니다.

``` CMD
WinAppDeployCmd devices 3
```

PC의 Downloads 디렉터리에 있는 MyApp.appx 패키지의 앱을 A1B2C3 핀과 192.168.0.1 IP 주소를 사용하는 Windows 10 디바이스에 설치하여 디바이스와의 연결을 설정합니다.

``` CMD
WinAppDeployCmd install -file "Downloads\MyApp.appx" -ip 192.168.0.1 -pin A1B2C3
```

192.168.0.1 IP 주소를 사용하는 Windows 10 디바이스에서 지정된 패키지(전체 이름 기반)를 제거합니다. 명령 목록을 사용하여 장치에 설치된 모든 패키지의 전체 이름을 볼 수 있습니다.

``` CMD
WinAppDeployCmd uninstall -package Company.MyApp_1.0.0.1_x64__qwertyuiop -ip 192.168.0.1
```

지정된 앱 패키지를 사용하여 IP 주소가 192.168.0.1인 Windows 10 디바이스에 이미 설치된 앱을 업데이트합니다.

``` CMD
WinAppDeployCmd update -file "Downloads\MyApp.appx" -ip 192.168.0.1
```

192.168.0.1 IP 주소를 사용하는 PC 또는 Xbox에서 AppxManifest와 동일한 폴더의 앱 파일을 디바이스 배포 경로 아래의 app1_F5 디렉터리로 배포합니다.

``` CMD
WinAppDeployCmd deployfiles -file "C:\apps\App1\AppxManifest.xml" -remotedeploydir app1_F5 -ip 192.168.0.1
```

192.168.0.1을 사용하는 PC 또는 Xbox 배포 경로 아래의 app1_F5 디렉터리에서 앱을 등록합니다.

``` CMD
WinAppDeployCmd registerfiles -remotedeploydir app1_F5 -ip 192.168.0.1
```

## <a name="using-winappdeploycmd-to-set-up-run-from-pc-deployment-on-xbox-one"></a>WinAppDeployCmd를 사용하여 Xbox One에 PC에서 실행 배포 설정

PC에서 실행을 통해 이진 파일을 복사하지 않고 Xbox One에 UWP 애플리케이션을 배포할 수 있습니다. 대신 이진 파일은 Xbox와 동일한 네트워크의 네트워크 공유에 호스트됩니다.  이렇게 하려면 개발자가 Xbox One을 잠금 해제해야 하고 Xbox가 액세스할 수 있는 네트워크 드라이브의 느슨한 파일 UWP 애플리케이션이 필요합니다.

앱을 등록하려면 다음을 실행합니다.

``` CMD
WinAppDeployCmd registerfiles -ip <Xbox One IP> -remotedeploydir <location of app> -username <user for network> -password <password for user>

ex. WinAppDeployCmd register files -ip 192.168.0.1 -remotedeploydir \\driveA\myAppLocation -username admin -password A1B2C3
```
