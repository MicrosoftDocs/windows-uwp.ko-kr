---
author: laurenhughes
title: 웹 설치에 대해 AWS에서 UWP 앱 패키지 호스트
description: 앱 설치 관리자 앱을 통해 앱 설치의 유효성을 검사 하도록 AWS 웹 서버를 설정 하는 것에 대 한 자습서
ms.author: cdon
ms.date: 05/30/2018
ms.topic: article
keywords: windows 10, Windows 10, UWP, 앱 설치 관리자, AppInstaller, 사이드 로드, 관련 AWS 설정, 선택적 패키지
ms.localizationpriority: medium
ms.openlocfilehash: f24abac93e2444a3c9f454c8883902e5db4d31be
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/22/2018
ms.locfileid: "7579088"
---
# <a name="hosting-uwp-app-packages-on-aws-for-web-install"></a>웹 설치에 대해 AWS에서 UWP 앱 패키지 호스트

앱 설치 관리자 앱을 사용하여 개발자 및 IT 전문가가 자신의 CDN(콘텐츠 배달 네트워크)에서 Windows 10 앱을 호스트하여 배포할 수 있습니다. Microsoft Store에 자신의 앱을 게시하고 싶지 않거나 게시할 필요가 없지만 여전히 Windows 10 패키지 및 배포 플랫폼을 활용하고자 하는 기업에 유용합니다.

이 항목에서는 UWP 앱 패키지를 호스트 하는 Amazon 웹 서비스 (AWS) 웹 사이트 및 앱 설치 관리자 앱을 사용 하 여 앱 패키지를 설치 하는 방법을 구성 하는 단계를 설명 합니다.

## <a name="setup"></a>설정

이 자습서를 수행하려면 다음이 필요합니다.
 
1. AWS 구독 
2. 웹 페이지
3. UWP 앱 패키지 - 배포할 앱 패키지

옵션: GitHub의 [Starter 프로젝트](https://github.com/AppInstaller/MySampleWebApp) 작업할 앱 패키지 또는 웹 페이지가 없지만 여전히 이 기능을 사용하는 방법을 알아보고자 하는 경우 유용합니다.

이 자습서에서는 웹 페이지 및 AWS에서 호스트 패키지를 설치 하는 방법을 다룹니다. AWS 구독이 필요 합니다. 작업 규모에 따라이 자습서를 수행 하려면 무료 멤버십을 사용할 수 있습니다. 

## <a name="step-1---aws-membership"></a>단계 1-AWS 멤버십
AWS 멤버십을 가져오려면 [AWS 계정 세부 정보 페이지](https://aws.amazon.com/free/)를 방문 하세요. 이 자습서의 목적을 위해 무료 멤버십을 사용할 수 있습니다.

## <a name="step-2---create-an-amazon-s3-bucket"></a>2 단계-Amazon s 3 버킷 만들기

Amazon Simple Storage Service (s 3)를 수집 하 고 저장 데이터 분석을 제공 하는 AWS입니다. S 3 버킷으로 호스트 UWP 앱 패키지 및 배포에 대 한 웹 페이지는 편리한 방법입니다. 

에 로그인 한 후 AWS 자격 증명을 사용 하 여 아래에서 `Services` 찾기 `S3`. 

**만들기 버킷**선택한 웹 사이트에 대 한 **보관 함 이름** 입력 합니다. 속성 및 사용 권한을 설정 하기 위한 대화 지시를 따릅니다. 웹 사이트에서 UWP 앱을 배포할 수 있습니다 위해, **읽기** 및 **쓰기** 권한을 보관 함에 대 한 사용 하도록 설정 하 고 **이 버킷으로 공개 읽기 권한을 부여를**선택 합니다.

![보관 함 Amazon s 3에서 사용 권한을 설정합니다](images/aws-permissions.png) 

선택한 옵션에 따라 반영 되도록 요약을 검토 합니다. 이 단계를 완료 하려면 **만들기 통** 클릭 합니다. 

## <a name="step-3---upload-uwp-app-package-and-web-pages-to-an-s3-bucket"></a>3 단계-s 3 버킷으로 웹 페이지 및 UWP 앱 패키지 업로드

Amazon s 3 버킷 만들지 않은 한, Amazon s 3 보기에서 볼 수 있습니다. 우리의 데모 버킷 모습의 예는 다음과 같습니다.

![Amazon s 3 버킷 보기](images/aws-post-create.png)

이제 우리 Amazon s 3 보관 함에서 호스트 하 고 싶습니다 웹 페이지와 앱 패키지를 업로드할 준비가 되었습니다. 

새로 만든된 버킷 콘텐츠 업로드를 클릭 합니다. 아직 업로드 된 것 이므로 버킷을 현재 비어 있습니다. **업로드** 단추를 클릭 하 고 웹 페이지 파일을 업로드 하 고 앱 패키지를 선택 합니다.

> [!NOTE]
> 사용할 수 있는 앱 패키지가 없는 경우 GitHub에서 제공된 [Starter 프로젝트](https://github.com/AppInstaller/MySampleWebApp) 리포지토리의 일부인 앱 패키지를 사용할 수 있습니다. 패키지를 서명한 인증서(MySampleApp.cer)는 GitHub의 샘플도 서명합니다. 앱을 설치하기 전에 디바이스에 인증서가 설치되어 있어야 합니다.

![앱 패키지를 업로드 합니다.](images/aws-upload-package.png)

Amazon s 3 통을 만드는 데 사용 권한을 마찬가지로 보관 함에서 콘텐츠 있어야 **읽기**, **쓰기**및 사용 권한을 **이 개체에 공개 읽기 액세스 권한을 부여** 합니다.

테스트 웹 페이지를 업로드 하려면 하지만 계정이 없는 경우 [Starter 프로젝트](https://github.com/AppInstaller/MySampleWebApp/blob/master/MySampleWebApp/default.html)에서 샘플 html 페이지 (default.html)를 사용할 수 있습니다.

> [!IMPORTANT]
> 웹 페이지를 업로드 하기 전에 웹 페이지에서 앱 패키지 참조 올바른지 확인 합니다. 

앱 패키지 참조를 가져오려면 먼저 앱 패키지를 업로드 하 고 앱 패키지 URL을 복사 합니다. 올바른 앱 패키지 경로 반영 하기 위해 html 웹 페이지를 편집 합니다. 자세한 내용은 코드 예제를 참조 하세요. 

이 예제와 비슷한 것을 앱 패키지에 대 한 참조 링크를 가져오려면 업로드 된 앱 패키지 파일 선택:

![업로드 패키지 경로](images/aws-package-path.png)

앱에 대 한 링크 **복사** 패키지 및 웹 페이지에 참조를 추가 합니다. 

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
Amazon s 3 버킷으로 html 파일을 업로드 합니다. **읽기** 및 **쓰기** 액세스를 허용 하도록 사용 권한을 설정 해야 합니다.

## <a name="step-4---test"></a>4 단계-테스트

웹 페이지는 Amazon s 3 버킷에 업로드 되 면 업로드 된 html 파일을 선택 하 여 웹 페이지에 대 한 링크를 가져옵니다.

링크를 사용 하 여 웹 페이지를 엽니다. 앱 패키지 및 웹 페이지에 대 한 공용 액세스 권한을 부여 하는 권한을 설정 했으므로, 이후 액세스 하 여 앱 설치 관리자를 사용 하 여 UWP 앱 패키지를 설치할 수 웹 페이지 링크를 갖고 있는. Note 앱 설치 관리자가 Windows 10 플랫폼의 일부가 되도록 합니다. 개발자가 앱 설치 관리자를 사용 하 여 앱 모든 추가 코드 또는 기능을 추가할 필요가 없습니다. 

## <a name="troubleshooting"></a>문제 해결

### <a name="app-installer-fails-to-install"></a>앱 설치 관리자 설치에 실패 

앱 패키지 서명 된 인증서는 장치에 설치 하지 않은 경우 앱 설치가 실패 합니다. 이 문제를 해결하려면 앱을 설치하기 전에 인증서를 설치해야 합니다. 공용 배포에 대 한 앱 패키지를 호스팅할 경우 인증 기관에서 발급 한 인증서를 사용 하 여 앱 패키지에 서명 하는 것입니다. 


