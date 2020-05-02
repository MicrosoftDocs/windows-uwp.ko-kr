---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 0e144789531e43e3561e7b82830c2b78249c294b
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "72314877"
---
데이터베이스 시스템에서 두 가지 [많이 사용되는](https://insights.stackoverflow.com/survey/2019#technology-_-databases) 것은 [MongoDB](https://www.mongodb.com/what-is-mongodb)와 [PostgreSQL](https://www.postgresql.org/about/)입니다. 

MongoDB는 JSON을 사용하고 스키마 없는 데이터를 저장하도록 고안된 NoSQL 문서 데이터베이스입니다. 유연한 비정형 데이터, 실시간 분석 캐싱 및 수평적 크기 조정에 적합합니다. 

PostgreSQL(Postgres라고도 함)은 확장성 및 표준 준수에 중점을 둔 SQL 관계형 데이터베이스입니다. 이제 JSON도 처리할 수 있지만, 일반적으로 정형 데이터, 수평적 크기 조정 및 전자 상거래 및 금융 거래와 같은 ACID 호환 요구에 더 적합합니다.

스키마:

**PostgreSQL:** 테이블 | 열 | 값 | 레코드

**MongoDB(NoSQL):** 컬렉션 | 키 | 값 | 문서

선택한 데이터베이스의 종류는 데이터베이스와 함께 사용하는 애플리케이션의 유형에 따라 달라집니다. 정형 및 비정형 데이터베이스의 장점과 단점을 살펴보고 사용 사례에 따라 선택하는 것이 좋습니다. PostgreSQL 및 MongoDB 외에도 몇 가지 다른 데이터베이스 시스템을 고려해야 합니다.