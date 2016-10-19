---
author: mcleblanc
ms.assetid: BC7E8130-A28A-443C-8D7E-353E7DA33AE3
description: "EF(Entity Framework)는 도메인별 개체를 사용하여 관계형 데이터로 작업할 수 있는 개체 관계형 매퍼입니다."
title: "C# 앱용 SQLite를 사용하는 Entity Framework 7"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: b453a2a6c3ab0b9418122ae27bf6a3a1c56e5873

---

# C# 앱용 SQLite를 사용하는 Entity Framework 7

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

EF(Entity Framework)는 도메인별 개체를 사용하여 관계형 데이터로 작업할 수 있는 개체 관계형 매퍼입니다. 이 문서에서는 유니버설 Windows 앱에서 SQLite 데이터베이스가 포함된 Entity Framework 7을 사용하는 방법을 설명합니다.

원래 .Net 개발자용인 Entity Framework 7을 UWP(유니버설 Windows 플랫폼)에서 SQLite와 함께 사용하면 도메인별 개체를 사용하는 관계형 데이터를 저장하고 조작할 수 있습니다. EF 코드를 .NET 앱에서 UWP 앱으로 마이그레이션할 수 있으며 연결 문자열을 적절하게 변경하여 사용할 수 있습니다.

현재 EF는 UWP의 SQLite에만 지원됩니다. Entity Framework 7 설치 및 모델 만들기에 대한 자세한 연습은 [유니버설 Windows 플랫폼 시작 페이지](http://go.microsoft.com/fwlink/p/?LinkId=735013)에서 확인할 수 있습니다. 다음 항목에 대해 설명합니다.

-   사전 요구 사항
-   새 프로젝트 만들기
-   Entity Framework 설치
-   모델 만들기
-   데이터베이스 만들기
-   모델 사용




<!--HONumber=Aug16_HO3-->


