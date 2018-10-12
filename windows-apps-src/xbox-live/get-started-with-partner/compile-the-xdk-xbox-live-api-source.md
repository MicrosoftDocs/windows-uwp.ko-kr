---
title: XDK Xbox Live API 소스 컴파일
author: KevinAsgari
description: Xbox 개발자 키트 (XDK)와 함께 제공 되는 Xbox Live API 소스 컴파일 하는 방법을 알아봅니다.
ms.assetid: 78425e82-c132-4f6b-9db3-2536862f1ce5
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox xdk
ms.localizationpriority: medium
ms.openlocfilehash: 301d646b421e77044f613244ccab987f1cb40dc6
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4563541"
---
# <a name="compile-the-xbox-developer-kit-xdk-xbox-live-api-source"></a>Xbox 개발자 키트 (XDK) Xbox Live API 소스 컴파일

(XDK (Xbox 개발자 키트) Microsoft.Xbox.Services.dll (XSAPI)를 구축 하기 위한 소스를 포함 합니다. 개발자는 DLL의 로컬 빌드를 사용 하는 프로젝트를 업데이트 하려면 다음이 지침에 따라 수 있습니다.

경우 XSAPI 직접 빌드 하려는 경우:
1. 오류 코드에서 제공 되는 위치를 이해 하는 문제를 디버깅 해야 합니다.
1. 전에 수 있는 문제를 해결 하려면 소스 코드 패치를 제공 QFE를 배포할 수 했습니다.

## <a name="to-compile-the-xdk-c-xsapi-project-for-yourself"></a>C + + XSAPI XDK 프로젝트를 직접

<ol>
  <li> Microsoft.Xbox.Services 소스를 가져옵니다. 이렇게 하려면 모든 파일에서 추출 "%XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox 서비스 API\8.0\SourceDist\Xbox.Services.zip" 하거나 "C:\Program Files (x86)" 외부 쓰기 가능한 폴더에서 소스를 복제할 수<a href ="https://github.com/Microsoft/xbox-live-api">https://github.com/Microsoft/xbox-live-api</a></li>
  <li> 참조를 제거 해야 하는 프로젝트 빌드 전 DLL에서 참조 하는 경우</li>
    <ul>
      <li> Visual Studio 2012: 선택 "프로젝트-> 참조..." Visual Studio에서. Xbox 서비스 API에 대 한 참조로 나열 되는 경우 선택 하 고 "참조 제거"를 클릭 합니다. "확인"를 클릭 하 고 프로젝트 파일을 저장 합니다.</li>
      <li> Visual Studio 2015 또는 2017: 선택 "프로젝트-> 참조... 추가" 만들면 됩니다. Xbox 서비스 API 확인란이 선택 취소 합니다. "확인"를 클릭 하 고 프로젝트 파일을 저장 합니다.</li>
    </ul>
  <li> XDK를 빌드하는 경우 선택 "파일에 추가-> 기존 프로젝트->" Visual studio 프로젝트에서 다음 두 가지 응용 프로그램의 솔루션에 추가 합니다. Vcxproj 파일을 소스 압축을 푼 폴더에 배치 됩니다.</li>
Visual Studio 2017: <ul>
      <li>\Build\Microsoft.Xbox.Services.141.XDK.Cpp\Microsoft.Xbox.Services.141.XDK.Cpp.vcxproj</li>   <li>\External\cpprestsdk\Release\src\build\vs15.xbox\casablanca141 합니다. Xbox.vcxproj</li>
    </ul>
Visual Studio 2015. <ul>
      <li>\Build\Microsoft.Xbox.Services.140.XDK.Cpp\Microsoft.Xbox.Services.140.XDK.Cpp.vcxproj</li> <li>\External\cpprestsdk\Release\src\build\vs14.xbox\casablanca140 합니다. Xbox.vcxproj</li>
    </ul>
Visual Studio 2012: <ul>
      <li>\Build\Microsoft.Xbox.Services.110.XDK.Cpp\Microsoft.Xbox.Services.110.XDK.Cpp.vcxproj</li> <li>\External\cpprestsdk\Release\src\build\vs11.xbox\casablanca110 합니다. Xbox.vcxproj</li>
    </ul>
    <li> 원본 프로젝트에서 프로젝트를 선택 하 여 참조로 참조를...-> "참조 추가 선택"을 추가 합니다. "솔루션 프로젝트->", 아래에서 위의 두 프로젝트 확인을 클릭 한 다음에 대 한 항목을 확인 합니다.</li>
    <li> 속성 파일을 클릭 하 여 프로젝트에 추가 "보기를-> 다른 Windows 속성 관리자->" 프로젝트를 마우스 오른쪽 단추로 클릭, "기존 속성 시트 추가"를 선택한 다음 마지막 SDK sourch 루트에 xsapi.staticlib.props 파일을 선택 합니다.  전처리기 정 및 라이브러리 %XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox.Services.API.Cpp\8.0\DesignTime\CommonConfiguration\Neutral\에 표시 된 대로 수동으로 추가 해야 빌드 시스템 속성 파일을 지원 하지 않는 경우 Xbox.Services.API.Cpp.props</li>
    <li> {SDK 소스 root}\Include\xsapi\services.h 파일을 선택 하 고 기존 항목 추가 추가 프로젝트를 마우스 오른쪽 단추로 클릭 하 여 앱 원본 services.h 파일-></li>
    <li> 응용 프로그램 프로젝트와 Xbox 서비스 프로젝트의 "출력 폴더" 동일한 지 확인 합니다. 이 설정을 구성 속성-> 일반 하는 속성-> Visual Studio 프로젝트에서 찾을 수 있습니다 출력 디렉터리-> 합니다.</li>
    <li> Visual Studio 솔루션을 다시 작성</li>
</ol>

## <a name="to-compile-the-xdk-winrt-xsapi-project-for-yourself"></a>XDK WinRT XSAPI 프로젝트를 직접

<ol>
  <li> Microsoft.Xbox.Services 소스를 가져옵니다. 모든 파일에서이 작업을 수행 하려면 추출 "%XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox 서비스 API\8.0\SourceDist\Xbox.Services.zip" 하거나 "C:\Program Files (x86)" 외부 쓰기 가능한 폴더에서 소스를 복제할 수<a href ="https://github.com/Microsoft/xbox-live-api">https://github.com/Microsoft/xbox-live-api</a></li>
  <li> 참조를 제거 해야 하는 프로젝트 빌드 전 DLL에서 참조 하는 경우</li>
    <ul>
      <li> Visual Studio 2012: 선택 "프로젝트-> 참조..." Visual Studio에서. Xbox 서비스 API에 대 한 참조로 나열 되는 경우 선택 하 고 "참조 제거"를 클릭 합니다. "확인"를 클릭 하 고 프로젝트 파일을 저장 합니다.</li>
      <li> Visual Studio 2015 또는 2017: 선택 "프로젝트-> 참조... 추가" 만들면 됩니다. Xbox 서비스 API 확인란이 선택 취소 합니다. "확인"를 클릭 하 고 프로젝트 파일을 저장 합니다.</li>
    </ul>
  <li> XDK를 빌드하는 경우 선택 "파일에 추가-> 기존 프로젝트->" Visual studio 프로젝트에서 다음 두 가지 응용 프로그램의 솔루션에 추가 합니다. Vcxproj 파일을 소스 압축을 푼 폴더에 배치 됩니다.  VS2015 형식으로 프로젝트에서 Visual Studio 2015에 대 한 자동 업그레이드 됩니다.</li>
    <ul>
      <li>\Build\Microsoft.Xbox.Services.110.XDK.WinRT\Microsoft.Xbox.Services.110.XDK.WinRT.vcxproj</li> <li>\External\cpprestsdk\Release\src\build\vs11.xbox\casablanca110 합니다. Xbox.vcxproj</li>
    </ul>
  <li> Visual Studio에서 참조를 추가 합니다.</li>
    <ul>
      <li> Visual Studio 2012: "프로젝트-> 참조..."를 선택 하 고 Visual Studio에서 "참조 추가"를 선택 합니다. 솔루션에서 프로젝트를 chceck 위의 두 프로젝트에 대 한 항목-> 하 고 확인을 클릭 합니다.</li>
      <li> Visual Studio 2015 또는 2017: 선택 "프로젝트-> 참조... 추가" 만들면 됩니다. 프로젝트에서 위의 두 프로젝트에 대 한 항목을 확인 하 고 확인을 클릭 합니다.</li>
    </ul>
  <li> 응용 프로그램 프로젝트와 Xbox 서비스 프로젝트의 "출력 폴더" 동일한 지 확인 합니다. 이 설정을 구성 속성-> 일반 하는 속성-> Visual Studio 프로젝트에서 찾을 수 있습니다 출력 디렉터리-> 합니다.</li>
  <li> Visual Studio 솔루션을 다시 작성</li>
</ol>
