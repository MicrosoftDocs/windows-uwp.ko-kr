---
title: 시작 화면
description: 이 섹션에서는 앱의 시작 화면을 설정 및 구성하는 방법을 설명합니다.
ms.assetid: 6b954bb3-e5b0-46d1-8afc-fb805536cf6d
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: df3fc8f54a4174006fd28f319d7cab09142a81fd
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8333661"
---
# <a name="splash-screens"></a>시작 화면

모든 UWP 앱에는 사용자 지정할 수 있는 이미지와 배경색이 조합된 시작 화면이 있어야 합니다.

사용자가 앱을 실행하는 즉시 시작 화면이 표시됩니다. 이 화면에서는 앱 리소스가 초기화되는 동안 즉각적인 피드백을 제공합니다. 앱을 조작할 수 있게 되면 시작 화면이 해제됩니다.

앱의 시작 화면을 잘 디자인할수록 더 많은 사용자가 방문하게 됩니다. 다음은 간단한 절제된 시작 화면입니다.

![시작 화면 샘플의 시작 화면에 대한 75% 배율 화면 캡처입니다.](images/regularsplashscreen.png)

이 시작 화면은 녹색 배경색과 투명 배경 PNG 이미지를 결합하여 만든 것입니다.

배경색이 있는 간단한 이미지는 앱을 실행 중인 디바이스에 관계없이 제대로 표시됩니다. 다양한 화면 크기에 맞게 배경의 크기만 변경됩니다. 이미지는 항상 원래대로 유지됩니다.

또한 [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763) 클래스를 사용하여 앱의 시작 환경을 사용자 지정할 수 있습니다. 사용자가 만든 연장된 시작 화면을 배치하여 앱에 앱 UI 준비나 네트워킹 작업 완료와 같은 추가 작업을 완료할 수 있는 시간을 제공할 수 있습니다. **SplashScreen** 클래스를 사용하여 시작 화면이 해제될 때 사용자에게 알려 개시 애니메이션을 시작하도록 할 수도 있습니다.

| 항목 | 설명 |
|-------|-------------|
| [시작 화면 추가](add-a-splash-screen.md) | 앱의 시작 화면 이미지와 배경색을 설정합니다. |
| [시작 화면을 더 오래 표시](create-a-customized-splash-screen.md) | 앱의 연장된 시작 화면을 만들어 시작 화면을 더 오랫동안 표시합니다. 이 연장된 화면은 앱을 시작할 때 표시되는 시작 화면을 모방하며 사용자 지정할 수 있습니다. |