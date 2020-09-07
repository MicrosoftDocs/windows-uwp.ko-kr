---
title: Windows 앱 샘플 가져오기
description: 대부분의 Windows 기능과 해당 API 사용 패턴을 보여주는 GitHub 코드 샘플을 찾아보고 다운로드하고 여는 방법을 알아봅니다.
ms.date: 06/30/2020
ms.topic: article
keywords: windows 10, 샘플 코드, 코드 샘플
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.localizationpriority: medium
ms.openlocfilehash: e08a81ac6f9bca2e4fe4b7451df830e20c714893
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162947"
---
# <a name="get-windows-app-samples"></a>Windows 앱 샘플 가져오기

다양한 GitHub 리포지토리에서 [UWP(유니버설 Windows 플랫폼) 앱 샘플](https://github.com/microsoft/Windows-universal-samples), [Windows 클래식 샘플](https://github.com/microsoft/Windows-classic-samples), [Windows 개발자 설명서 샘플](#windows-developer-documentation-samples) 컬렉션을 비롯한 여러 공식 Windows 코드 샘플을 얻을 수 있습니다. 이러한 샘플은 대부분의 Windows 기능 및 해당 API 사용 패턴을 보여줍니다.

:::image type="content" source="images/github-windows-samples-page.png" alt-text="GitHub Windows 유니버설 샘플 리포지토리":::

[샘플 브라우저](/samples/browse/)를 통해 다양한 Microsoft 개발자 도구 및 기술에 대한 범주별 코드 샘플 컬렉션을 찾아서 검색하면 특정 샘플을 좀 더 쉽게 찾을 수 있습니다.

:::image type="content" source="images/samples-browser-windows.png" alt-text="Microsoft 샘플 브라우저":::

### <a name="windows-developer-documentation-samples"></a>Windows 개발자 설명서 샘플

다음은 Windows 개발자 설명서를 지원하기 위해 특별히 작성된 미니 앱 샘플 목록입니다. 별도의 언급이 없는 한, 다음 샘플은 모두 최신 [WinUI 2.4](/windows/apps/winui/winui2/release-notes/winui-2.4) 컨트롤을 사용하도록 업데이트된 UWP(유니버설 Windows 플랫폼) 앱입니다.

- [Rss 판독기](https://github.com/Microsoft/Windows-appsample-rssreader) - RSS 피드를 검색하고 문서 보기
- [가족 메모](https://github.com/Microsoft/Windows-appsample-familynotes) - 다양한 입력 형식 및 사용자 인식 시나리오 살펴보기
- [고객 주문](https://github.com/Microsoft/Windows-appsample-customers-orders-database) - AAD(Azure Active Directory) 인증, UI 컨트롤(데이터 그리드 포함), Sqlite 및 SQL Azure 데이터베이스 통합, Entity Framework, 클라우드 API 서비스 등 엔터프라이즈 개발자에게 유용한 기능
- [점심 식사 스케줄러](https://github.com/Microsoft/Windows-appsample-lunch-scheduler) - 친구나 동료와의 점심 약속을 예약
- [컬러링 북](https://github.com/Microsoft/Windows-appsample-coloringbook) Windows Ink(Windows Ink 도구 모음 포함) 및 방사형 컨트롤러(Surface Dial 같은 휠 디바이스용) 기능
- [네트워크 도우미(퀴즈 게임)](https://github.com/Microsoft/Windows-appsample-networkhelper) - 네트워크 검색 및 통신
- [HUE 조명 컨트롤러](https://github.com/Microsoft/Windows-appsample-huelightcontroller) - Cortana 및 Bluetooth LE(Bluetooth Low Energy)를 사용하는 인텔리전트 홈 자동화
- [Marble Maze](https://github.com/Microsoft/Windows-appsample-marble-maze) - DirectX를 사용하는 기본 3D 게임
- [PhotoLab](https://github.com/Microsoft/Windows-appsample-photo-lab) - 이미지 파일 보기 및 편집

## <a name="download-the-code"></a>코드 다운로드

샘플을 다운로드하려면 [UWP(유니버설 Windows 플랫폼) 앱 샘플](https://github.com/microsoft/Windows-universal-samples) 같은 Microsoft 리포지토리 중 하나로 이동합니다. **복제 또는 다운로드**, **ZIP 다운로드**를 차례로 선택합니다.

![샘플 다운로드](images/SamplesDownloadButton.png)

샘플 다운로드 .zip 파일에는 항상 최신 샘플이 있습니다. 파일을 다운로드하기 위해 GitHub 계정이 필요하지 않습니다. SDK 업데이트가 릴리스되거나 최신 변경 내용과 추가 사항을 선택하려면 최신 zip 파일을 다운로드하기만 하면 됩니다.

> [!NOTE]
> Windows 샘플을 열고, 빌드하고, 실행하려면 Visual Studio 및 Windows SDK가 있어야 합니다. [Visual Studio Community](https://www.microsoft.com/?ref=go)의 체험판 복사본을 받을 수 있습니다.  
>
> 샘플이 제대로 작동하려면 개별 샘플이 아니라 전체 보관 파일의 압축을 풀어야 합니다. 많은 샘플이 중복을 줄이기 위해 샘플 템플릿 파일 및 이미지 자산을 비롯한 *SharedContent* 폴더의 공통 파일과 연결된 파일을 사용합니다.

## <a name="open-the-samples"></a>샘플 열기

.zip 파일이 다운로드되면 Visual Studio에서 샘플을 엽니다.

1. 보관 파일의 압축을 풀려면 먼저 마우스 오른쪽 단추로 파일을 클릭한 다음, **속성** > **차단 해제** > **적용**을 차례로 선택합니다. 그러면 보관 파일의 압축이 컴퓨터의 로컬 폴더에서 풀립니다.

    ![압축을 푼 보관 파일](images/SamplesUnzip1.png)

2. Samples 폴더의 각 폴더에는 Windows 기능 샘플이 포함되어 있습니다.

    ![샘플 폴더](images/SamplesUnzip2.png)

3. 샘플을 선택합니다. 지원되는 언어는 언어별 하위 폴더로 표시됩니다.

    ![언어 폴더](images/SamplesUnzip3.png)

4. 사용하려는 언어의 폴더를 선택합니다. 폴더 콘텐츠에는 Visual Studio에서 열 수 있는 Visual Studio 솔루션(.sln) 파일이 표시됩니다.

    ![VS 솔루션](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>피드백 제공, 질문하기, 문제 보고

문제가 발생하거나 질문이 있는 경우 리포지토리의 **Issues** 탭을 사용하여 새 문제를 작성하세요.

![피드백 이미지](images/GitHubUWPSamplesFeedback.png)