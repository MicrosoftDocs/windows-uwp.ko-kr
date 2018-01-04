---
author: laurenhughes
ms.assetid: ee51eae3-ed55-419e-ad74-6adf1e1fb8b9
title: "수동 앱 패키징"
description: "이 섹션에는 UWP(유니버설 Windows 플랫폼) 앱의 수동 패키징에 대한 문서 또는 링크가 포함되어 있습니다."
ms.author: lahugh
ms.date: 03/07/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 패키징"
ms.openlocfilehash: 8eca88588b2e444450daccd997aebb9e838a90d4
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: HT
ms.contentlocale: ko-KR
---
# <a name="manual-app-packaging"></a>수동 앱 패키징

앱 패키지를 만들어 서명하려 하지만 앱 개발에 Visual Studio를 사용하지 않은 경우, 수동 앱 패키징 도구를 사용해야 합니다.

> [!IMPORTANT] 
> Visual Studio를 사용하여 앱을 개발하는 경우 Visual Studio 마법사를 사용하여 앱 패키지를 만들고 서명하는 것이 좋습니다. 자세한 내용은 [Visual Studio를 사용하여 UWP 앱 패키징](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)을 참조하세요.

## <a name="purpose"></a>용도

이 섹션에는 UWP(유니버설 Windows 플랫폼) 앱의 수동 패키징에 대한 문서 또는 링크가 포함되어 있습니다.

| 항목 | 설명 |
|-------|-------------|
| [MakeAppx.exe 도구를 사용하여 앱 패키지 만들기](create-app-package-with-makeappx-tool.md) | MakeAppx.exe는 앱 패키지와 번들을 만들고, 암호화 및 암호 해독하고, 파일을 추출합니다. |
| [패키지 서명 인증서 만들기](create-certificate-package-signing.md) | PowerShell 도구를 사용하여 앱 패키지 서명 인증서를 만들고 내보냅니다. |
| [SignTool을 사용하여 앱 패키지에 서명](sign-app-package-using-signtool.md) | SignTool을 사용하여 인증서로 앱 패키지에 수동으로 서명합니다. |