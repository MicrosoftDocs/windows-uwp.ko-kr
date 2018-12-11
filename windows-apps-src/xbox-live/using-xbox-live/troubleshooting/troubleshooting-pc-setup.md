---
title: Windows PC에서 Xbox Live 설정 문제 해결
description: Windows PC에서 Xbox Live 개발 환경 문제를 해결 하는 방법을 알아봅니다.
ms.assetid: 9cfebdcd-0c1c-4fc2-9457-e7e434b6374c
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 문제 해결
ms.localizationpriority: medium
ms.openlocfilehash: c1f055a49fe34be35335e50dc8b1efbfb7b9b922
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8887104"
---
# <a name="troubleshooting-xbox-live-setup-on-windows-pc"></a>Windows PC에서 Xbox Live 설정 문제 해결

Windows 10 PC에서 컴퓨터는 다음이 단계를 사용 하 여 올바르게 설치 되도록 할 수 있습니다.

1. XDKS.1 샌드박스 샘플 실행 하도록 설계 된 위치를 가리키도록 컴퓨터를 변경 합니다.  이 스크립트를 실행 하 여이 작업을 수행 합니다.

        {*SDK source root*}\Tools\SwitchSandbox.cmd XDKS.1

1. "SourcesAndSamples.zip" SDK 내에서 발견 zip 파일 콘텐츠의 압축을 풉니다.
1. 샘플 솔루션을 엽니다.
    1. {*SDK 소스 루트*} \Samples\Social\UWP\Cpp\Social.Cpp.140.sln c + + api:
    1. C#을 사용 하 여 WinRT API에 대 한: {*SDK 소스 루트*} \Samples\Social\UWP\CSharp\Social.CSharp.140.sln
    1. WinRT api C + + CX: {*SDK 소스 루트*} \Samples\TitleStorage\UWP\CppCx\TitleStorageUniversal.sln
1. "Win32" 또는 "x64" 빌드 대상 플랫폼을 변경 합니다.
1. 솔루션을 마우스 오른쪽 단추로 클릭 하 고 모든를 다시 빌드하십시오.
1. 디버거에서 응용 프로그램을 실행 합니다.
1. 로그인 [Xbox 개발자 포털](https://xdp.xboxlive.com)에서 만든 개발 계정 또는 정품 개발자 계정 [파트너 센터](https://partner.microsoft.com/dashboard)에서 승인 합니다.
1. Xbox Live 정보에 액세스 하려면 앱 권한을 부여 합니다.
1. 앱에 정보를 검색할 수 게이머 태그를 볼 수 있는지 확인 하십시오.