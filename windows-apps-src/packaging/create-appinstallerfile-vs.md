---
author: ridomin
title: Visual Studio를 사용하여 앱 설치 관리자 파일 만들기
description: Visual Studio를 사용하여 .appinstaller 파일로 자동 업데이트를 사용하는 방법을 알아보세요.
ms.author: rmpablos
ms.date: 5/2/2018
ms.topic: article
keywords: Windows 10, uwp 앱 설치 관리자, AppInstaller, 사이드로드
ms.localizationpriority: medium
ms.openlocfilehash: 8d4740c132f274cb3eeee5776790d077d2cbec47
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5711140"
---
# <a name="create-an-app-installer-file-with-visual-studio"></a>Visual Studio를 사용하여 앱 설치 관리자 파일 만들기

Windows 10 버전 1804, and Visual Studio 2017 업데이트 15.7부터 사이드로드된 앱을 구성하여 `.appinstaller` 파일을 사용하여 자동 업데이트를 받을 수 있습니다. Visual Studio는 이러한 업데이트를 사용할 수 있도록 지원합니다.

## <a name="app-installer-file-location"></a>앱 설치 관리자 파일 위치
`.appinstaller` 파일은 HTTP 끝점 또는 UNC 공유 폴더와 같은 공유 위치에 호스팅할 수 있으며 설치할 앱 패키지 찾기 위해 경로를 포함합니다. 사용자는 공유 위치에서 앱을 설치하고 새 업데이트에 대한 주기적 검사를 사용하도록 설정합니다. 


### <a name="configure-the-project-to-target-the-correct-windows-version"></a>올바른 Windows 버전을 대상으로 하도록 프로젝트 구성

프로젝트를 만들 때 `TargetPlatformMinVersion` 속성을 구성하거나 나중에 프로젝트 속성에서 이를 변경할 수 있습니다. 

>[!IMPORTANT]
> 앱 설치 관리자 파일은 `TargetPlatformMinVersion`이 Windows 10 버전 1804 이상일 때만 생성됩니다.


### <a name="create-packages"></a>패키지 만들기

테스트용 로드를 통해 앱을 배포 하려면 앱 패키지 (.appx/.msix) 또는 앱 번들 (.appxbundle/.msixbundle) 만들기 하 고 공유 위치에 게시 해야 합니다.

이를 위해 다음 단계를 따라 Visual Studio에서 **앱 패키지 만들기** 마법사를 사용합니다.

1. 프로젝트를 마우스 오른쪽 단추로 클릭하고 **Microsoft Store** -> **앱 패키지 만들기**를 선택합니다.  

![앱 패키지 만들기로 이동할 수 있는 컨텍스트 메뉴](images/packaging-screen2.jpg)   

**앱 패키지 만들기** 마법사가 나타납니다.

2. **사이드로드에 대한 패키지를 만듭니다.** 및 **자동 업데이트 사용**을 선택합니다.  

![표시되는 패키지 만들기 대화 창](images/select-sideloading.png)  

**자동 업데이트 사용** 프로젝트의 `TargetPlatformMinVersion`이 올바른 Windows 10 버전으로 설정된 경우에만 활성화됩니다.

3. **패키지 선택 및 구성** 대화 상자에서 지원되는 아키텍처 구성을 선택할 수 있습니다. 번들을 선택하면 단일 설치 관리자가 생성되지만 번들을 만들지 않고 아키텍처마다 하나의 패키지를 원하는 경우 아키텍처마다 하나의 설치 관리자가 생성됩니다.  어떤 아키텍처를 선택할지 잘 모르겠거나 다양한 디바이스에서 사용되는 아키텍처에 대한 자세한 내용을 보려면 [앱 패키지 아키텍처](device-architecture.md)를 참조하세요.

4. 버전 번호 또는 패키지 출력 위치 등 추가 세부 정보를 구성합니다.

![패키지 구성이 표시된 앱 패키지 만들기 창](images/packaging-screen5.jpg)  

5. 2단계에서 **자동 업데이트 사용**을 체크한 경우 **업데이트 설정 구성** 대화 상자가 나타납니다. 여기에서 **설치 URL** 및 업데이트 확인 빈도를 지정할 수 있습니다.

![위치 구성 게시가 표시된 업데이트 설정 구성 창](images/sideloading-screen.png)  

6. 앱이 성공적으로 패키지되면 앱 패키지를 포함하는 출력 폴더의 위치를 표시하는 대화 상자가 나타납니다. 출력 폴더는 앱 홍보에 사용할 수 있는 HTML 페이지를 포함하여 앱을 사이드로드하는 데 필요한 모든 파일을 포함합니다.

### <a name="publish-packages"></a>패키지 게시

응용 프로그램을 사용할 수 있도록 하려면 생성된 파일을 지정 위치에 게시해야 합니다.

#### <a name="publish-to-shared-folders-unc"></a>공유 폴더에 게시(UNC)

범용 명명 규칙(UNC) 공유 폴더를 통해 패키지를 게시하려면 앱 패키지 출력 폴더 및 설치 URL을 동일한 경로에 구성합니다(자세한 내용은 6단계 참조). 마법사가 정확한 위치에 파일을 생성하며 사용자는 동일한 경로에서 앱과 향후 업데이트를 받게 됩니다.

#### <a name="publish-to-a-web-location-http"></a>웹 위치에 게시(HTTP)

웹 위치에 게시하려면 웹 서버에 대한 게시 콘텐츠에 액세스하여 최종 URL이 마법사에서 정의한 설치 URL과 일치하는지 확인할 수 있어야 합니다(자세한 내용은 6단계 참조). 일반적으로 파일 전송 프로토콜(FTP) 또는 SSH 파일 전송 프로토콜(SFTP)은 파일을 업로드하는 데 사용되지만 웹 공급자에 따라 MSDeploy, SSH, 또는 Blob 저장소와 같은 다른 게시 방법이 있습니다.

웹 서버를 구성하려면 사용 중인 파일 형식에 대해 사용되는 MIME 형식을 확인 해야 합니다. 이 예는 인터넷 정보 서비스(IIS)에 대한 `web.config`입니다.

```xml
<configuration>
  <system.webServer>
    <staticContent>
      <mimeMap fileExtension=".appx" mimeType="application/vns.ms-appx" />
      <mimeMap fileExtension=".appxbundle" mimeType="application/vns.ms-appx" />
      <mimeMap fileExtension=".appinstaller" mimeType="application/xml" />
    </staticContent>  
  </system.webServer>  
</configuration>
```




















