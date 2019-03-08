---
title: 웹 설치에 대해 AWS에서 UWP 앱 패키지 호스트
description: 앱 설치 관리자를 통해 앱 설치의 유효성을 검사 하려면 AWS 웹 서버 설정에 대 한 자습서
ms.date: 05/30/2018
ms.topic: article
keywords: windows 10, Windows 10 UWP 앱 설치 관리자를 AppInstaller, 테스트용으로 로드 관련 설정 (선택 사항) 패키지를 AWS
ms.localizationpriority: medium
ms.openlocfilehash: 53fe01a1c1a825377e886e042b4eef3868cbf5eb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628058"
---
# <a name="hosting-uwp-app-packages-on-aws-for-web-install"></a>웹 설치에 대해 AWS에서 UWP 앱 패키지 호스트

앱 설치 관리자 앱을 사용하여 개발자 및 IT 전문가가 자신의 CDN(콘텐츠 배달 네트워크)에서 Windows 10 앱을 호스트하여 배포할 수 있습니다. Microsoft Store에 자신의 앱을 게시하고 싶지 않거나 게시할 필요가 없지만 여전히 Windows 10 패키지 및 배포 플랫폼을 활용하고자 하는 기업에 유용합니다.

이 항목에서는 앱 설치 관리자 앱을 사용 하 여 앱 패키지를 설치 하는 방법과 호스트 UWP 앱 패키지에 Amazon Web Services (AWS) 웹 사이트를 구성 하는 단계를 간략하게 설명 합니다.

## <a name="setup"></a>설치

이 자습서를 수행하려면 다음이 필요합니다.
 
1. AWS 구독 
2. 웹 페이지
3. UWP 앱 패키지 - 배포할 앱 패키지

선택 사항: [시작 프로젝트](https://github.com/AppInstaller/MySampleWebApp) github입니다. 작업할 앱 패키지 또는 웹 페이지가 없지만 여전히 이 기능을 사용하는 방법을 알아보고자 하는 경우 유용합니다.

이 자습서에서는 웹 페이지 및 AWS에서 호스트 패키지를 설치 하는 방법을 살펴봅니다. AWS 구독을 해야 합니다. 작업의 규모에 따라이 자습서를 수행 하려면 사용 가능한 멤버 자격은 사용할 수 있습니다. 

## <a name="step-1---aws-membership"></a>1 단계-AWS 멤버 자격
AWS 회원 자격을 받으려면를 방문 합니다 [AWS 계정 세부 정보 페이지](https://aws.amazon.com/free/)합니다. 이 자습서의 목적을 위해 무료 멤버십을 사용할 수 있습니다.

## <a name="step-2---create-an-amazon-s3-bucket"></a>2 단계-Amazon S3 버킷 만들기

Amazon 단순 Storage 서비스 (S3의 경우)는 AWS 수집, 저장 및 분석 데이터에 대 한 제공 합니다. S3 버킷은 호스트 UWP 앱 패키지 및 배포에 대 한 웹 페이지에는 편리한 방법입니다. 

로그인 한 후 AWS 자격 증명을 사용 하 여 아래 `Services` 찾을 `S3`합니다. 

선택 **버킷 만들기**를 입력 하 고는 **버킷 이름** 웹 사이트에 대 한 합니다. 속성 및 사용 권한 설정에 대 한 대화를 지시 합니다. 웹 사이트에서 UWP 앱을 배포할 수 있는지를 확인 하려면 사용 하도록 설정 **읽기** 하 고 **작성** 버킷 고 선택에 대 한 권한을 **이 집합에 대 한 공용 읽기 액세스 권한을 부여** .

![Amazon S3 버킷의 권한 설정](images/aws-permissions.png) 

선택한 옵션에 반영 되 고 있는지 확인 하기 위해 요약을 검토 합니다. 클릭 **버킷 만들기** 이 단계를 완료 합니다. 

## <a name="step-3---upload-uwp-app-package-and-web-pages-to-an-s3-bucket"></a>3 단계-S3 버킷에 웹 페이지 및 UWP 앱 패키지 업로드

하나 만든를 Amazon S3 버킷의 Amazon S3 보기에 표시할 수 있습니다. 이 데모 버킷 모양은의 예는 다음과 같습니다.

![Amazon S3 버킷 보기](images/aws-post-create.png)

이제는 Amazon S3 버킷에 호스트 하고자 하는 웹 페이지 확인 하 고 앱 패키지를 업로드할 준비가 되었습니다. 

콘텐츠를 업로드 하려면 새로 만든된 버킷을 클릭 합니다. 버킷 아직 업로드 된 것 이므로 현재 비어 있습니다. 클릭 합니다 **업로드** 단추를 선택 하 고 앱 패키지를 업로드 하려면 웹 페이지 파일입니다.

> [!NOTE]
> 사용할 수 있는 앱 패키지가 없는 경우 GitHub에서 제공된 [Starter 프로젝트](https://github.com/AppInstaller/MySampleWebApp) 리포지토리의 일부인 앱 패키지를 사용할 수 있습니다. 패키지를 서명한 인증서(MySampleApp.cer)는 GitHub의 샘플도 서명합니다. 앱을 설치하기 전에 디바이스에 인증서가 설치되어 있어야 합니다.

![앱 패키지 업로드](images/aws-upload-package.png)

Amazon S3 버킷 생성에 대 한 사용 권한을 마찬가지로 버킷에 콘텐츠 있어야 **읽을**를 **작성**, 및 **이 개체에 대 한 공용 읽기 액세스 권한을 부여** 사용 권한입니다.

샘플 html 페이지 (default.html) 웹 페이지 업로드를 테스트 하려고 해도 계정이 없는 경우 사용할 수 있습니다 합니다 [시작 프로젝트](https://github.com/AppInstaller/MySampleWebApp/blob/master/MySampleWebApp/default.html)합니다.

> [!IMPORTANT]
> 웹 페이지에 업로드 하기 전에 웹 페이지의 앱 패키지 참조가 올바른지 확인 합니다. 

앱 패키지 참조를 가져오려면 앱 패키지를 먼저 업로드 하 고 앱 패키지 URL을 복사 합니다. 올바른 응용 프로그램 패키지 경로 반영 하도록 html 웹 페이지를 편집 합니다. 자세한 코드 예제를 참조 하세요. 

앱 패키지에 참조 링크를 가져오는 업로드 된 앱 패키지 파일 선택,이 예제와 유사 해야 합니다.

![업로드 된 패키지 경로](images/aws-package-path.png)

**복사** 는 앱의 링크를 패키지 하 고 웹 페이지에 대 한 참조를 추가 합니다. 

```html
<html>
    <head>
        <meta charset="utf-8" />
        <title> Install My Sample App</title>
    </head>
    <body>
        <a href="ms-appinstaller:?source=https://s3-us-west-2.amazonaws.com/appinstaller-aws-demo/MySampleApp.appxbundle"> Install My Sample App</a>
    </body>
</html>
```
Amazon S3 버킷의를 html 파일을 업로드 합니다. 수 있도록 사용 권한을 설정 해야 **읽을** 하 고 **작성** 액세스 합니다.

## <a name="step-4---test"></a>4-테스트 단계

웹 페이지에 Amazon S3 버킷에 업로드 되 면 업로드 된 html 파일을 선택 하 여 웹 페이지에 대 한 링크를 가져옵니다.

웹 페이지를 열려면 링크를 사용 합니다. 앱 패키지 및 웹 페이지에 대 한 공용 액세스를 부여할 사용 권한을 설정 하는 것 이므로 웹 페이지에 대 한 링크를 사용 하 여 모든 사용자가 액세스 하 고 앱 설치 관리자를 사용 하 여 UWP 앱 패키지를 설치할 수 있게 됩니다. 앱 설치 관리자는 Windows 10 플랫폼의 일부인 것 note 합니다. 개발자 앱 설치 관리자를 사용 하도록 설정 하려면 앱에는 추가 코드나 기능을 추가할 필요가 없습니다. 

## <a name="troubleshooting"></a>문제 해결

### <a name="app-installer-fails-to-install"></a>앱 설치 관리자가 설치 되지 않습니다. 

앱 패키지를 사용 하 여 서명 된 인증서를 장치에 설치 되지 않은 경우 앱을 설치 하지 못합니다. 이 문제를 해결하려면 앱을 설치하기 전에 인증서를 설치해야 합니다. 공용 배포에 대 한 앱 패키지를 호스트 하는 경우는 것이 좋습니다에 인증 기관에서 인증서를 사용 하 여 앱 패키지를 서명 합니다. 


