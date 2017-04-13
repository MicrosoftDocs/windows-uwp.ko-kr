---
title: "GitHub에서 UWP(유니버설 Windows 플랫폼) 코드 샘플 가져오기"
description: "GitHub에서 UWP 기능 샘플을 다운로드하는 방법을 알아봅니다."
author: JoshuaPartlow
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.openlocfilehash: e576150cd06000405fa4754e147fbc1546cdf703
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
#<a name="get-the-universal-windows-platform-uwp-samples-from-github"></a>GitHub에서 UWP(유니버설 Windows 플랫폼) 샘플 가져오기
UWP 앱 샘플은 GitHub 리포지토리를 통해 사용할 수 있습니다. 처음으로 UWP 작업을 하는 경우라면 [Microsoft/Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples "Universal Windows Platform app samples GitHub repository") 리포지토리부터 시작합니다. 여기에 UWP 기능 및 API 사용 패턴을 모두 보여 주는 샘플도 포함되어 있습니다.  
![GitHub UWP 샘플 리포지토리](images/GitHubUWPSamplesPage.png) 추가 샘플은 개발자 센터의 [샘플](https://developer.microsoft.com/windows/samples "Dev Center samples")을 사용하여 찾을 수 있습니다.  

##<a name="download-the-code"></a>코드 다운로드
샘플을 다운로드하려면 [리포지토리](https://github.com/Microsoft/Windows-universal-samples "유니버설 Windows 플랫폼 앱 샘플 GitHub 리포지토리")로 이동하여 **복제 또는 다운로드**, **ZIP 다운로드**를 선택합니다. 또는 [여기](https://github.com/Microsoft/Windows-universal-samples/archive/master.zip "유니버설 Windows 플랫폼 앱 샘플 zip 파일 다운로드")를 클릭합니다.

zip 파일은 항상 최신 샘플을 보관합니다. 다운로드하기 위해 GitHub 계정이 필요하지 않습니다. SDK 업데이트가 릴리스되거나 최근 변경/추가를 선택하려면 최신 zip 파일에 대해 다시 확인하면 됩니다.

![샘플 다운로드](images/SamplesDownloadButton.png)


> **참고**: UWP 샘플에서는 Visual Studio 2015와 Windows SDK를 열고, 작성하고, 실행해야 합니다. Visual Studio를 아직 설치하지 않은 경우 [여기](http://go.microsoft.com/fwlink/p/?LinkID=280676 "Windows 개발 도구 다운로드")에서 UWP 앱 빌드를 지원하는 Visual Studio 2015 Community Edition의 무료 복사본을 얻을 수 있습니다.  
>
> 또한 개별 샘플이 아니라 전체 보관 파일의 압축을 풀어야 합니다. 샘플은 모두 보관 파일의 SharedContent 폴더에 따라 달라집니다. UWP 기능 샘플에서는 Visual Studio에서 연결된 파일을 사용하여 샘플 템플릿 파일과 이미지 자산과 같은 공통 파일이 중복되지 않게 합니다. 이러한 공통 파일은 리포티토리의 루트에 있는 SharedContent 폴더에 저장되고 링크를 사용하여 프로젝트 파일에서 참조됩니다.

zip 파일을 다운로드한 후 Visual Studio에서 샘플을 엽니다.

1.  보관 파일의 압축을 풀고 마우스 오른쪽 단추로 클릭한 다음 **속성** > **차단 해제** > **적용**을 선택합니다. 그런 다음 컴퓨터에서 로컬 폴더에 보관 파일의 압축을 풉니다.

    ![압축을 푼 보관 파일](images/SamplesUnzip1.png)
2.  샘플 폴더 내에서 UWP 기능 샘플을 포함하는 각 폴더 수가 표시됩니다.

    ![샘플 폴더](images/SamplesUnzip2.png)

3.  Altimeter와 같은 샘플을 선택하면 지원되는 언어를 나타내는 여러 폴더가 표시됩니다.

    ![언어 폴더](images/SamplesUnzip3.png)

4.  C\#용 CS 등 사용할 언어를 선택하고 Visual Studio에서 열 수 있는 Visual Studio 솔루션 파일이 표시됩니다.

    ![VS 솔루션](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>피드백 제공, 질문하기, 문제 보고

문제가 발생하거나 질문이 있는 경우 리포지토리의 문제 탭을 사용하여 새 문제를 만들면 지원하겠습니다.

![피드백 이미지](images/GitHubUWPSamplesFeedback.png)
