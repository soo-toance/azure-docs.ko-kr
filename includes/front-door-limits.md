---
title: 파일 포함
description: 포함 파일
services: frontdoor
author: sharad4u
ms.service: frontdoor
ms.topic: include
ms.date: 05/09/2019
ms.author: sharadag
ms.custom: include file
ms.openlocfilehash: 148ec3eccce71ab7a4a6c1391c0fa4753c248bd8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80335087"
---
| 리소스 | 제한 |
| --- | --- |
| 구독 당 Azure Front 도어 리소스 | 100 |
| 리소스 당 사용자 지정 도메인을 포함 하는 프런트 엔드 호스트 | 500 |
| 리소스당 라우팅 규칙 | 500 |
| 리소스 당 백 엔드 풀 | 50 |
| 백 엔드 풀 당 백 엔드 | 100 |
| 라우팅 규칙에 일치하는 경로 패턴 | 25 |
| 단일 캐시 제거 호출의 Url | 100 |
| 정책당 사용자 지정 웹 애플리케이션 방화벽 규칙 | 100 |
| 구독 당 웹 응용 프로그램 방화벽 정책 | 100 |
| 사용자 지정 규칙에 따라 웹 응용 프로그램 방화벽 일치 조건 | 10 |
| 일치 조건 당 웹 응용 프로그램 방화벽 IP 주소 범위 | 600 |
| 웹 응용 프로그램 방화벽 문자열 일치 조건 별 값 일치 | 10 |
| 웹 응용 프로그램 방화벽 문자열이 값 길이와 일치 합니다. | 256 |
| 웹 응용 프로그램 방화벽 게시 본문 매개 변수 이름 길이 | 256 |
| 웹 응용 프로그램 방화벽 HTTP 헤더 이름 길이 | 256 |
| 웹 응용 프로그램 방화벽 쿠키 이름 길이 | 256 |
| 웹 응용 프로그램 방화벽 HTTP 요청 본문 크기 검사 | 128KB |
| 웹 응용 프로그램 방화벽 사용자 지정 응답 본문 길이 | 2KB |

### <a name="timeout-values"></a>시간 제한 값
#### <a name="client-to-front-door"></a>클라이언트-Front Door
* Front Door에는 유휴 TCP 연결 시간 제한(61초)이 있습니다.

#### <a name="front-door-to-application-back-end"></a>응용 프로그램 백 엔드에 대 한 전면 도어
* 응답이 청크 분할 응답이 면 첫 번째 청크를 받은 경우 200이 반환 됩니다.
* HTTP 요청이 백 엔드로 전달 된 후에는 프런트 문이 백 엔드에서 첫 번째 패킷에 대해 30 초 동안 대기 합니다. 그런 다음 클라이언트에 503 오류를 반환 합니다. 이 값은 API의 sendRecvTimeoutSeconds 필드를 통해 구성할 수 있습니다.
    * 캐싱 시나리오의 경우이 시간 제한을 구성할 수 없으므로 요청이 캐시 되 고 프런트 도어 또는 백 엔드에서 첫 번째 패킷에 대해 30 초 넘게 걸리는 경우 클라이언트에 504 오류가 반환 됩니다. 
* 백 엔드에서 첫 번째 패킷을 받은 후 앞 도어는 유휴 시간 제한에서 30 초 동안 대기 합니다. 그런 다음 클라이언트에 503 오류를 반환 합니다. 이 시간 제한 값은 구성할 수 없습니다.
* 백 엔드 TCP 세션 시간 제한의 앞 도어는 90 초입니다.

### <a name="upload-and-download-data-limit"></a>데이터 제한 업로드 및 다운로드

|  | CTE (청크 분할 전송 인코딩) 사용 | HTTP 청크 제외 |
| ---- | ------- | ------- |
| **다운로드** | 다운로드 크기에 대 한 제한은 없습니다. | 다운로드 크기에 대 한 제한은 없습니다. |
| **업로드** |    각 CTE 업로드가 2gb 미만인 경우에는 제한이 없습니다. | 크기는 2gb 보다 클 수 없습니다. |

### <a name="other-limits"></a>기타 제한
* 최대 URL 크기-8192 바이트-원시 URL의 최대 길이 (체계 + 호스트 이름 + 포트 + 경로 + URL의 쿼리 문자열)를 지정 합니다.
* 최대 쿼리 문자열 크기-4096 바이트-쿼리 문자열의 최대 길이 (바이트)를 지정 합니다.
* 상태 프로브 URL-4096 바이트의 최대 HTTP 응답 헤더 크기-상태 프로브의 모든 응답 헤더에 대 한 최대 길이를 지정 했습니다. 
