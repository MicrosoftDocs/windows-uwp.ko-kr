---
author: DBirtolo
ms.assetid: ECE848C2-33DE-46B0-BAE7-647DB62779BB
title: "센서 보정"
description: "나침반, 경사계 방향 센서 같이 자력계를 기반으로 한 장치의 센서는 환경 요인으로 인해 보정이 필요해질 수 있습니다."
ms.author: dbirtolo
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 8c02d9ed6e0612e0b89236b3fdb387e8ac3a5488
ms.lasthandoff: 02/07/2017

---
# <a name="calibrate-sensors"></a>센서 보정

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

**중요 API**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Windows.Devices.Sensors.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn895032)

나침반, 경사계 방향 센서 같이 자력계를 기반으로 한 장치의 센서는 환경 요인으로 인해 보정이 필요해질 수 있습니다. [**MagnetometerAccuracy**](https://msdn.microsoft.com/library/windows/apps/Dn297552) 열거형은 장치에 보정이 필요한 경우 작업 과정을 결정하는 데 도움이 될 수 있습니다.

## <a name="when-to-calibrate-the-magnetometer"></a>자력계를 보정하는 경우

[**MagnetometerAccuracy**](https://msdn.microsoft.com/library/windows/apps/Dn297552) 열거형에는 앱이 실행 중인 장치를 보정해야 할지 결정하는 데 도움이 되는 값 네 개가 있습니다. 장치를 보정해야 하는 경우 사용자에게 보정이 필요하다는 것을 알려야 합니다. 그러나 사용자에게 너무 자주 보정하도록 메시지를 표시하지 않아야 합니다. 10분마다 한 번을 넘지 않는 것이 좋습니다.

| 값           | 설명                                                                                                                                                      |-----------------|-------------------|                                                                                                                                              | **알 수 없음**     | 센서 드라이버가 현재 정확도를 보고하지 못했습니다. 이 문제가 반드시 장치가 보정 범위를 벗어났음을 의미하는 것은 아닙니다. **Unknown**이 반환될 경우 앱이 가장 적합한 작업 과정을 결정합니다. 앱이 정확한 센서 판독에 의존하는 경우 사용자에게 장치를 보정하라는 메시지를 표시할 수 있습니다. | | **불안정**  | 현재 자력계의 부정확도가 높습니다. 이 값이 처음 반환될 경우 앱은 항상 사용자에게 보정을 요청해야 합니다. | | **대략 일치** | 데이터가 일부 응용 프로그램에 대해 충분히 정확합니다. 사용자가 장치를 위/아래 또는 왼쪽/오른쪽으로 이동했는지 여부만을 인식해야 하는 가상 현실 앱은 보정 없이 계속 사용할 수 있습니다. 방향을 제공하기 위해 주행하고 있는 방향을 인식해야 하는 내비게이션 앱 같은 절대 진행 방향이 필요한 앱은 보정을 요청해야 합니다. | | **높음**        | 데이터가 정확합니다. 증강 현실이나 내비게이션 앱 같이 절대 진행 방향을 인식해야 하는 앱의 경우에도 보정이 필요하지 않습니다. |

## <a name="how-to-calibrate-the-magnetometer"></a>자력계를 보정하는 방법

이 짧은 동영상에서는 자력계를 보정하는 방법을 간략하게 설명합니다.<iframe src="https://hubs-video.ssl.catalog.video.msn.com/embed/727bd0e3-9116-49c3-8af6-0b4339324b71/IA?csid=ux-en-us&MsnPlayerLeadsWith=html&PlaybackMode=Inline&MsnPlayerDisplayShareBar=false&MsnPlayerDisplayInfoButton=false&iframe=true&QualityOverride=HD" width="720" height="405" allowFullScreen="true" frameBorder="0" scrolling="no">One Dev Minute - 센서 보정</iframe>

