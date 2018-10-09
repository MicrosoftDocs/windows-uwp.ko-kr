---
title: Windows PC에서 Xbox Live 설정 문제 해결
author: KevinAsgari
description: Xbox Live 개발 환경 Windows PC에서 문제를 해결 하는 방법을 알아봅니다.
ms.assetid: 9cfebdcd-0c1c-4fc2-9457-e7e434b6374c
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 문제 해결
ms.localizationpriority: medium
ms.openlocfilehash: 3eabd83f9fc42f86fb1ec35bbce7d8b7b2359e0e
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4424668"
---
# <a name="troubleshooting-xbox-live-setup-on-windows-pc"></a>Windows PC에서 Xbox Live 설정 문제 해결

Windows 10 PC에서 사용자 컴퓨터는 다음이 단계를 사용 하 여 올바르게 설치 되도록 할 수 있습니다.

1. XDKS.1 샌드박스 샘플 실행 하도록 설계 된 위치를 가리키도록 컴퓨터를 변경 합니다.  이 스크립트를 실행 하 여이 작업을 수행 합니다.

        {*SDK source root*}\Tools\SwitchSandbox.cmd XDKS.1

1. "SourcesAndSamples.zip" 발견 SDK 내 zip 파일 콘텐츠의 압축을 풉니다.
1. 샘플 솔루션을 엽니다.
    1. {*SDK 소스 루트*} \Samples\Social\UWP\Cpp\Social.Cpp.140.sln c + + api:
    1. C#을 사용 하 여 WinRT API에 대 한: {*SDK 소스 루트*} \Samples\Social\UWP\CSharp\Social.CSharp.140.sln
    1. WinRT api C + + CX: {*SDK 소스 루트*} \Samples\TitleStorage\UWP\CppCx\TitleStorageUniversal.sln
1. 빌드 대상 플랫폼 "Win32" 또는 "x64"로 변경 합니다.
1. 솔루션을 마우스 오른쪽 단추로 클릭 하 고 모든 작업을 다시 빌드하십시오.
1. 디버거에서 응용 프로그램을 실행 합니다.
1. 또는에 로그인 [Xbox 개발자 포털](https://xdp.xboxlive.com)에서 만든 개발 계정을 사용 하 여 [Windows 개발자 센터](https://developer.microsoft.com/dashboard/windows/overview)에서 권한을 부여한 소매 개발자 계정을 사용 하 여 합니다.
1. Xbox Live 정보에 액세스 하려면 앱 권한을 부여 합니다.
1. 확인 하 고 앱 정보를 검색할 수 게이머 태그를 볼 수 있습니다.