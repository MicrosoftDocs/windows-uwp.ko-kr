---
title: XDK Xbox Live API 원본 컴파일
description: Xbox 개발자 키트 (XDK)와 함께 제공 되는 Xbox Live API 소스를 컴파일하는 방법에 알아봅니다.
ms.assetid: 78425e82-c132-4f6b-9db3-2536862f1ce5
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, xdk
ms.localizationpriority: medium
ms.openlocfilehash: 9a98af637c8c60449cd2005c4fc6f83f9b0719cf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660998"
---
# <a name="compile-the-xbox-developer-kit-xdk-xbox-live-api-source"></a>Xbox 개발자 키트 (XDK) Xbox Live API 원본 컴파일

Xbox 개발자 키트 (XDK) Microsoft.Xbox.Services.dll (XSAPI)를 빌드하기 위한 소스를 포함 합니다. 개발자가 DLL의 로컬 빌드를 사용 하도록 해당 프로젝트를 업데이트 하도록이 지침을 따를 수도 있습니다.

XSAPI을 직접 작성 하는 경우는 것이 좋습니다.
1. 여기서는 오류 코드를 이해 하는 문제를 디버그 하려는 경우는에서 제공 됩니다.
1. 문제를 해결 하기 위한 원본 코드 패치를 제공 하는 경우 QFE를 배포 합니까 전에 합니다.

## <a name="to-compile-the-xdk-c-xsapi-project-for-yourself"></a>직접 XDK c + + XSAPI 프로젝트를 컴파일하려면

<ol>
  <li> Microsoft.Xbox.Services 소스를 가져옵니다. 이렇게 하려면 모든 파일에서 추출 "%XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox 서비스 API\8.0\SourceDist\Xbox.Services.zip" "C:\Program Files (x86)" 하거나 외부에서 쓰기 가능한 폴더에 소스를 복제할 수 <a href ="https://github.com/Microsoft/xbox-live-api">https://github.com/Microsoft/xbox-live-api</a></li>
  <li> 참조를 제거 해야 하는 프로젝트 빌드 전 DLL에서 참조 하는 경우</li>
    <ul>
      <li> For Visual Studio 2012: "프로젝트-> 참조..." 선택 Visual studio 합니다. Xbox 서비스 API가 참조로 나열 하는 경우 선택한 "참조 제거"를 클릭 합니다. "확인"을 클릭 하 고 프로젝트 파일을 저장 합니다.</li>
      <li> Visual Studio 2015 또는 2017의 경우: 선택 "프로젝트-> 참조 추가..." 만들면 됩니다. Xbox 서비스 API 옵션을 선택 하 고 선택을 취소 합니다. "확인"을 클릭 하 고 프로젝트 파일을 저장 합니다.</li>
    </ul>
  <li> 선택 된 XDK을 작성 하는 경우 "파일-> 추가-> 기존 프로젝트..." Visual studio 응용 프로그램의 솔루션에 다음 두 프로젝트를 추가 합니다. Vcxproj 파일 소스를 추출한 폴더에 배치 됩니다.</li>
For Visual Studio 2017: <ul>
      <li>\Build\Microsoft.Xbox.Services.141.XDK.Cpp\Microsoft.Xbox.Services.141.XDK.Cpp.vcxproj</li>   <li>\External\cpprestsdk\Release\src\build\vs15.xbox\casablanca141.Xbox.vcxproj</li>
    </ul>
For Visual Studio 2015: <ul>
      <li>\Build\Microsoft.Xbox.Services.140.XDK.Cpp\Microsoft.Xbox.Services.140.XDK.Cpp.vcxproj</li> <li>\External\cpprestsdk\Release\src\build\vs14.xbox\casablanca140.Xbox.vcxproj</li>
    </ul>
For Visual Studio 2012: <ul>
      <li>\Build\Microsoft.Xbox.Services.110.XDK.Cpp\Microsoft.Xbox.Services.110.XDK.Cpp.vcxproj</li> <li>\External\cpprestsdk\Release\src\build\vs11.xbox\casablanca110.Xbox.vcxproj</li>
    </ul>
    <li> 소스 프로젝트에 프로젝트를 선택 하 여 참조로 참조-> 추가... 고 "참조 추가"를 선택 합니다. "솔루션 프로젝트->", 아래에서 위의 두 프로젝트에는 다음 확인을 클릭 한 항목을 확인 합니다.</li>
    <li> Props 파일을 클릭 하 여 프로젝트에 추가 "보기 만들기-> 다른 Windows 속성 관리자->" 프로젝트를 마우스 오른쪽 단추로 클릭, "기존 속성 시트 추가"를 선택한 다음 마지막 SDK sourch 루트에서 xsapi.staticlib.props 파일을 선택 합니다.  전처리기 정 및 %XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox.Services.API.Cpp\8.0\DesignTime\CommonConfiguration\Neutral\ 에서처럼 라이브러리를 수동으로 추가 해야 빌드 시스템 props 파일을 지원 하지 않으면 Xbox.Services.API.Cpp.props</li>
    <li> 추가 프로젝트를 마우스 오른쪽 단추로 클릭 하 여 앱 원본 services.h 파일 추가-> 기존 항목 및 {SDK 소스 root}\Include\xsapi\services.h 파일 선택</li>
    <li> 응용 프로그램 프로젝트와 Xbox 서비스 프로젝트의 "출력 폴더" 동일한 지 확인 합니다. 이 설정은 있습니다 속성-> Visual Studio 프로젝트에서 구성 속성-> 일반-> 출력 디렉터리입니다.</li>
    <li> Visual Studio 솔루션을 다시 작성</li>
</ol>

## <a name="to-compile-the-xdk-winrt-xsapi-project-for-yourself"></a>직접 XDK WinRT XSAPI 프로젝트를 컴파일하려면

<ol>
  <li> Microsoft.Xbox.Services 소스를 가져옵니다. 모든 파일을 추출 하면이 위해 "%XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox 서비스 API\8.0\SourceDist\Xbox.Services.zip" "C:\Program Files (x86)" 하거나 외부에서 쓰기 가능한 폴더에 소스를 복제할 수 <a href ="https://github.com/Microsoft/xbox-live-api">https://github.com/Microsoft/xbox-live-api</a></li>
  <li> 참조를 제거 해야 하는 프로젝트 빌드 전 DLL에서 참조 하는 경우</li>
    <ul>
      <li> For Visual Studio 2012: "프로젝트-> 참조..." 선택 Visual studio 합니다. Xbox 서비스 API가 참조로 나열 하는 경우 선택한 "참조 제거"를 클릭 합니다. "확인"을 클릭 하 고 프로젝트 파일을 저장 합니다.</li>
      <li> Visual Studio 2015 또는 2017의 경우: 선택 "프로젝트-> 참조 추가..." 만들면 됩니다. Xbox 서비스 API 옵션을 선택 하 고 선택을 취소 합니다. "확인"을 클릭 하 고 프로젝트 파일을 저장 합니다.</li>
    </ul>
  <li> 선택 된 XDK을 작성 하는 경우 "파일-> 추가-> 기존 프로젝트..." Visual studio 응용 프로그램의 솔루션에 다음 두 프로젝트를 추가 합니다. Vcxproj 파일 소스를 추출한 폴더에 배치 됩니다.  Visual Studio 2015에 대 한 프로젝트 VS2015 형식으로 자동 업그레이드 됩니다.</li>
    <ul>
      <li>\Build\Microsoft.Xbox.Services.110.XDK.WinRT\Microsoft.Xbox.Services.110.XDK.WinRT.vcxproj</li> <li>\External\cpprestsdk\Release\src\build\vs11.xbox\casablanca110.Xbox.vcxproj</li>
    </ul>
  <li> Visual Studio에 대 한 참조를 추가 합니다.</li>
    <ul>
      <li> For Visual Studio 2012: "프로젝트-> 참조..." 선택 및 Visual Studio에서 "추가 참조"를 선택 합니다. 솔루션에서 프로젝트의 경우 chceck 위의 두 프로젝트에 대 한 항목-> 하 고 확인을 클릭 합니다.</li>
      <li> Visual Studio 2015 또는 2017의 경우: 선택 "프로젝트-> 참조 추가..." 만들면 됩니다. 프로젝트에서 위의 두 프로젝트에 대 한 항목을 확인 하 고 확인을 클릭 합니다.</li>
    </ul>
  <li> 응용 프로그램 프로젝트와 Xbox 서비스 프로젝트의 "출력 폴더" 동일한 지 확인 합니다. 이 설정은 있습니다 속성-> Visual Studio 프로젝트에서 구성 속성-> 일반-> 출력 디렉터리입니다.</li>
  <li> Visual Studio 솔루션을 다시 작성</li>
</ol>
