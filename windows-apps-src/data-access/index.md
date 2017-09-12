---
author: normesta
ms.assetid: 76776b0f-3163-48c9-835b-3f4213968079
title: "데이터 액세스"
description: "이 섹션에서는 UWP(유니버설 Windows 플랫폼) 앱에서 개체 관계형 매핑을 사용하는 방법과 디바이스의 개인 데이터베이스에 데이터를 저장하는 방법에 대해 설명합니다."
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 데이터, 데이터베이스, 관계형, 표, SQLite"
ms.openlocfilehash: 19690b6877fb4304b7740e6098711ca0b097d567
ms.sourcegitcommit: 378382419f1fda4e4df76ffa9c8cea753d271e6a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2017
---
# <a name="data-access"></a>데이터 액세스

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 섹션에서는 UWP(유니버설 Windows 플랫폼) 앱에서 개체 관계형 매핑을 사용하는 방법과 디바이스의 개인 데이터베이스에 데이터를 저장하는 방법에 대해 설명합니다.

SQLite는 UWP SDK에 포함되어 있습니다. Entity Framework Core는 UWP 앱에서 SQLite와 함께 작동합니다. 이러한 기술을 사용하여 오프라인/일시적인 연결 시나리오에 대해 개발하고 앱 세션에서 데이터를 유지합니다.

| 항목 | 설명|
|-------|------------|
| [C# 앱용 SQLite를 사용하는 Entity Framework Core](entity-framework-7-with-sqlite-for-csharp-apps.md) | EF(Entity Framework)는 도메인별 개체를 사용하여 관계형 데이터로 작업할 수 있는 개체 관계형 매퍼입니다. 이 문서에서는 유니버설 Windows 앱에서 SQLite 데이터베이스가 포함된 Entity Framework Core를 사용하는 방법을 설명합니다. |
| [SQLite 데이터베이스](sqlite-databases.md) | SQLite는 서버 없는 포함된 데이터베이스 엔진입니다. 이 문서에서는 SDK에 포함된 SQLite 라이브러리를 사용하거나, 유니버설 Windows 앱에서 고유한 SQLite 라이브러리를 패키지하거나, 소스에서 빌드하는 방법을 설명합니다. |
