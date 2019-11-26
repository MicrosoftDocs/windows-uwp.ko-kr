---
title: UWP 앱 샘플 다운로드
description: GitHub에서 UWP 코드 샘플을 다운로드하는 방법을 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 샘플 코드, 코드 샘플
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.localizationpriority: medium
ms.openlocfilehash: ac3c99bc364e81386a362f1d1b5530bee9d462c4
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259520"
---
# <a name="get-uwp-app-samples"></a>UWP 앱 샘플 다운로드

UWP(유니버설 Windows 플랫폼) 앱 샘플은 GitHub 리포지토리에서 사용할 수 있습니다. 검색 가능한 항목별 목록은 [샘플](https://developer.microsoft.com/windows/samples)을 참조하거나, [Microsoft/Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples "유니버설 Windows 플랫폼 앱 샘플 GitHub 리포지토리") 리포지토리를 찾아볼 수 있습니다. Windows-universal-samples 리포지토리에는 모든 UWP 기능 및 해당 API 사용 패턴을 보여 주는 샘플이 포함되어 있습니다.

![GitHub UWP 샘플 리포지토리](images/GitHubUWPSamplesPage.png)

## <a name="download-the-code"></a>코드 다운로드

샘플을 다운로드하려면 [리포지토리](https://github.com/Microsoft/Windows-universal-samples "유니버설 Windows 플랫폼 앱 샘플 GitHub 리포지토리")로 이동합니다. **복제 또는 다운로드**, **ZIP 다운로드**를 차례로 선택합니다. 

![샘플 다운로드](images/SamplesDownloadButton.png)

이 문서에서 [샘플을 다운로드](https://github.com/Microsoft/Windows-universal-samples/archive/master.zip "유니버설 Windows 플랫폼 앱 샘플 zip 파일 다운로드")할 수도 있습니다.

샘플 다운로드 .zip 파일에는 항상 최신 샘플이 있습니다. 파일을 다운로드하기 위해 GitHub 계정이 필요하지 않습니다. SDK 업데이트가 릴리스되거나 최신 변경 내용과 추가 사항을 선택하려면 최신 zip 파일을 다운로드하기만 하면 됩니다.

> [!NOTE]
> UWP 샘플을 열고, 빌드하고, 실행하려면 Visual Studio 2015 이상 및 Windows SDK가 있어야 합니다. [Visual Studio Community의 체험판 복사본](https://www.microsoft.com/?ref=go)을 가져올 수 있습니다. Visual Studio Community는 UWP 앱 빌드를 지원합니다.  
>
> 샘플이 제대로 작동하려면 개별 샘플이 아니라 전체 보관 파일의 압축을 풀어야 합니다. 샘플은 모두 보관 파일의 SharedContent 폴더에 따라 달라집니다. UWP 기능 샘플에서는 Visual Studio에서 연결된 파일을 사용하여 샘플 템플릿 파일과 이미지 자산과 같은 공통 파일이 중복되지 않게 합니다. 공용 파일은 리포지토리의 루트에 있는 SharedContent 폴더에 저장됩니다. 링크는 프로젝트 파일에서 공용 파일을 참조하는 데 사용됩니다.
> 

## <a name="open-the-samples"></a>샘플 열기

.zip 파일이 다운로드되면 Visual Studio에서 샘플을 엽니다.

1.  보관 파일의 압축을 풀려면 먼저 마우스 오른쪽 단추로 해당 파일을 클릭한 다음, **속성** > **차단 해제** > **적용**을 차례로 선택합니다. 그러면 보관 파일의 압축이 컴퓨터의 로컬 폴더에서 풀립니다.

    ![압축을 푼 보관 파일](images/SamplesUnzip1.png)
2.  Samples 폴더의 각 폴더에는 UWP 기능 샘플이 포함되어 있습니다.

    ![샘플 폴더](images/SamplesUnzip2.png)
3.  Altimeter(고도계)와 같은 샘플을 선택합니다. 지원되는 언어가 하위 폴더에 표시됩니다.

    ![언어 폴더](images/SamplesUnzip3.png)
4.  사용하려는 언어의 폴더를 선택합니다. 폴더 콘텐츠에는 Visual Studio에서 열 수 있는 Visual Studio 솔루션(.sln) 파일이 표시됩니다. 예를 들어 *Altimeter.sln*입니다.

    ![VS 솔루션](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>피드백 제공, 질문하기, 문제 보고

문제가 발생하거나 질문이 있는 경우 리포지토리의 **Issues** 탭을 사용하여 새 문제를 작성하세요. 지원 가능한 모든 도움을 드릴 수 있습니다.

![피드백 이미지](images/GitHubUWPSamplesFeedback.png)
