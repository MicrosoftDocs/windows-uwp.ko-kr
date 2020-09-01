---
title: 시작 화면
description: 이 섹션에서는 앱의 시작 화면을 설정 하 고 구성 하는 방법을 설명 합니다.
ms.assetid: 6b954bb3-e5b0-46d1-8afc-fb805536cf6d
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 490f5f70efcd21fe2be5c909bcc409daf1feeeea
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175127"
---
# <a name="splash-screens"></a>시작 화면

모든 UWP 앱에는 이미지의 복합 인 시작 화면 및 배경색 (둘 다 사용자 지정할 수 있음)이 있어야 합니다.

사용자가 앱을 시작 하면 시작 화면이 즉시 표시 됩니다. 앱 리소스가 초기화 되는 동안 사용자에 게 즉각적인 피드백을 제공 합니다. 앱이 상호 작용할 준비가 되 면 시작 화면이 해제 됩니다.

잘 디자인 된 시작 화면을 통해 앱을 더 쉽게 초대할 수 있습니다. 간단한 understated 시작 화면은 다음과 같습니다.

![시작 화면 샘플에서 시작 화면의 75% 배율이 조정 된 화면 캡처입니다.](images/regularsplashscreen.png)

이 시작 화면은 녹색 배경색과 투명 한 배경 PNG 이미지를 결합 하 여 만듭니다.

배경색이 있는 단순 이미지는 앱이 실행 되는 장치에 관계 없이 양호 합니다. 배경의 크기만 다양 한 화면 크기를 보정 하도록 변경 됩니다. 이미지는 항상 그대로 유지 됩니다.

또한 [**SplashScreen**](/uwp/api/Windows.ApplicationModel.Activation.SplashScreen) 클래스를 사용 하 여 앱의 시작 환경을 사용자 지정할 수 있습니다. 만든 확장 된 시작 화면을 배치 하 여 앱 UI 준비 또는 네트워킹 작업 완료와 같은 추가 작업을 완료 하는 데 더 많은 시간을 앱에 제공할 수 있습니다. 또한 **SplashScreen** 클래스를 사용 하 여 시작 화면이 해제 될 때 사용자에 게 알리고 애니메이션을 시작할 수 있습니다.

| 항목 | 설명 |
|-------|-------------|
| [시작 화면 추가](add-a-splash-screen.md) | 앱의 시작 화면 이미지와 배경색을 설정합니다. |
| [시작 화면을 더 오래 표시](create-a-customized-splash-screen.md) | 앱의 연장된 시작 화면을 만들어 시작 화면을 더 오랫동안 표시합니다. 이 연장된 화면은 앱을 시작할 때 표시되는 시작 화면을 모방하며 사용자 지정할 수 있습니다. |