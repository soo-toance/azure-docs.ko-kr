---
title: 작업 API 취소-Microsoft 상업적 marketplace
description: 제안에서 현재 진행 중인 작업을 취소 하는 API입니다.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
author: mingshen-ms
ms.author: mingshen
ms.date: 07/14/2020
ms.openlocfilehash: 095d9cf68374ee81cafdb6e7a32df29a57d122cb
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87287931"
---
# <a name="cancel-operation"></a>작업 취소

> [!NOTE]
> Cloud 파트너 포털 Api는와 통합 되며 파트너 센터에서 계속 작업 합니다. 전환에는 작은 변화가 도입 됩니다. [CLOUD 파트너 포털 API 참조](./cloud-partner-portal-api-overview.md) 에 나열 된 변경 내용을 검토 하 여 파트너 센터로 전환 된 후 코드가 계속 작동 하는지 확인 합니다. CPP Api는 파트너 센터로 전환 하기 전에 이미 통합 된 기존 제품에만 사용 해야 합니다. 새 제품은 파트너 센터 제출 Api를 사용 해야 합니다.

이 API는 제안에 대해 현재 진행 중인 작업을 취소합니다. [작업 검색 API](./cloud-partner-portal-api-retrieve-operations.md)를 사용하여 이 API에 전달할 `operationId`를 가져옵니다. 일반적으로 취소는 동기 작업이지만, 일부 복잡한 시나리오에서는 기존 작업을 취소하기 위해 새 작업이 필요할 수 있습니다. 이 경우 HTTP 응답 본문에는 상태 쿼리에 사용되는 작업 위치가 포함됩니다.

  `POST https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/cancel?api-version=2017-10-31`

## <a name="uri-parameters"></a>URI 매개 변수

--------------

|  **Name**    |      **설명**                                  |    **데이터 형식**  |
| ------------ |     ----------------                                  |     -----------   |
| publisherId  |  게시자 식별자(예: `contoso`)입니다.         |   문자열          |
| offerId      |  제안 식별자입니다.                                     |   문자열          |
| api-version  |  API의 현재 버전입니다.                               |    Date           |
|  |  |  |

## <a name="header"></a>헤더
------

|  **이름**              |  **값**         |
|  ---------             |  ----------        |
|  콘텐츠 형식          |  application/json  |
|  권한 부여         |  전달자 사용자 토큰 |
|  |  |

## <a name="body-example"></a>본문 예제
------------

### <a name="request"></a>요청

``` json
{
   "metadata": {
     "notification-emails": "jondoe@contoso.com"
    }
}     
```

### <a name="request-body-properties"></a>요청 본문 속성

|  **Name**                |  **설명**                                               |
|  --------                |  ---------------                                               |
|  notification-emails     | 게시 작업의 진행률 알림을 받을 이메일 ID의 쉼표로 구분된 목록입니다. |
|  |  |

### <a name="response"></a>응답

#### <a name="migrated-offers"></a>마이그레이션된 제품

`Location: /api/publishers/contoso/offers/contoso-offer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8?api-version=2017-10-31`

#### <a name="non-migrated-offers"></a>마이그레이션되지 않은 제품

`Location: /api/operations/contoso$contoso-offer$2$preview?api-version=2017-10-31`

### <a name="response-header"></a>응답 헤더

|  **이름**             |    **값**                       |
|  ---------            |    ----------                      |
| 위치    | 이 작업의 상태를 검색할 상대 경로입니다. |
|  |  |

### <a name="response-status-codes"></a>응답 상태 코드

| **코드**  |  **설명**                                                                       |
|  ------   |  ------------------------------------------------------------------------               |
|  200      | 정상. 요청이 성공적으로 처리되었으며 작업이 동기적으로 취소됩니다. |
|  202      | 수락됨 요청이 성공적으로 처리되었으며 작업 처리가 진행 중입니다. 취소 작업의 위치가 응답 헤더에 반환됩니다. |
|  400      | 잘못된 형식의 요청입니다. 오류 응답 본문이 자세한 정보를 제공할 수 있습니다.  |
|  403      | 액세스 권한 없음 클라이언트는 요청에 지정된 네임스페이스에 대해 액세스 권한이 없습니다. |
|  404      | 찾을 수 없음 지정된 엔터티가 존재하지 않습니다. |
|  |  |
