---
author: jwmsft
Description: Learn how Fluent motion fundamentals come together in your app.
title: 동작 중인 움직임 - UWP 앱의 애니메이션
label: Motion in practice
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 87a17d3f73887c9b1b5029e2096c5b41c9444c4e
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843891"
---
# <a name="bringing-it-together"></a>통합

> [!IMPORTANT]
> 이 문서에서는 아직 출시되지 않아 상업적으로 출시하기 전에 크게 수정될 수 있는 기능에 대해 설명합니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.

타이밍, 감속, 방향 및 무게는 함께 작동하여 Fluent 동작의 기초를 형성합니다. 각각은 다른 것의 컨텍스트에서 간주되고 앱의 컨텍스트에서 적절하게 적용되어야 합니다.

앱에서 Fluent 움직임을 적용하는 3가지 방법은 다음과 같습니다.

:::row::: :::column::: **암시적 애니메이션**

        Automatic tween and timing between values in a parameter change to achieve very simple Fluent motion using the standardized values.
    :::column-end:::
    :::column:::
        **Built-in animation**

        System components, such as common controls and shared motion, are "Fluent by default". Fundamentals have been applied in a manner consistent with their implied usage.
    :::column-end:::
    :::column:::
        **Custom animation following guidance recommendations**

        There may be times when the system does not yet provide an exact motion solution for your scenario. In those cases, use the baseline fundamental recommendations as a starting point for your experiences.
    :::column-end:::
:::row-end:::

### **<a name="transition-example"></a>전환 예제**

![기능적 애니메이션](images/pageRefresh.gif)

:::row::: :::column::: <b>방향 앞-밖:</b><br>
        페이드 아웃: 150m; 감속: 기본 가속

        <b>Direction Forward In:</b><br>
        Slide up 150px: 300ms; Easing: Default Decelerate
    :::column-end:::
    :::column:::
         <b>Direction Backward Out:</b><br>
        Slide down 150px: 150ms; Easing: Default Accelerate
      
        <b>Direction Backward In:</b><br>
        Fade in: 300ms; Easing: Default Decelerate
    :::column-end:::
:::row-end:::

 ### **<a name="object-example"></a>개체 예제**

 ![300ms 움직임](images/control.gif)
 
:::row::: :::column::: <b>방향 확장:</b><br>
        증가: 300ms; 감속: 표준 :::column-end::: :::column::: <b>방향 축소:</b><br>
        증가: 150ms; 감속: 기본 가속 :::column-end::: :::row-end:::

## <a name="related-articles"></a>관련 문서

- [움직임 개요](index.md)
- [타이밍 및 감속](timing-and-easing.md)
- [방향 및 무게](directionality-and-gravity.md)