---
title: Azure 전면 도어 캐싱 | Microsoft Docs
description: 이 문서는 Azure Front 도어가 백 엔드의 상태를 모니터링 하는 방법을 이해 하는 데 도움이 됩니다.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: d4fed878e2c0b1430e963f43743fd772493d3270
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "79471747"
---
# <a name="caching-with-azure-front-door"></a>Azure Front 도어를 사용 하 여 캐싱
다음 문서는 캐싱을 허용하고 회람 규칙을 사용하는 Front Door의 동작을 설명합니다. 프런트 도어는 CDN (최신 Content Delivery Network) 이며 동적 사이트 가속 및 부하 분산과 더불어 다른 CDN과 마찬가지로 캐싱 동작도 지원 합니다.

## <a name="delivery-of-large-files"></a>대용량 파일 전달
Azure 전면 도어는 파일 크기에 캡이 없는 큰 파일을 제공 합니다. Front Door는 개체 청크라는 기술을 사용합니다. 대용량 파일이 요청되면 Front Door는 백엔드에서 더 작은 파일 조각을 검색합니다. 전체 또는 바이트 범위의 파일을 요청 받은 후 Front Door 환경은 백엔드에서 8MB 청크의 파일을 요청합니다.

</br>청크가 Front Door 환경에 도착하면 캐시되고 사용자에게 즉시 제공됩니다. 그런 다음 Front Door가 다음 청크를 동시에 프리페치합니다. 프리페치 과정 덕분에 콘텐츠가 사용자보다 한 청크 앞서 진행되므로 대기 시간이 줄어듭니다. 이 프로세스는 전체 파일을 다운로드하거나(요청된 경우), 모든 바이트 범위를 사용할 수 있거나(요청된 경우), 클라이언트에서 연결을 종료할 때까지 계속됩니다.

</br>바이트 범위 요청에 대한 자세한 내용은 [RFC 7233](https://web.archive.org/web/20171009165003/http://www.rfc-base.org/rfc-7233.html)을 읽어보세요.
Front Door가 청크를 받는 즉시 모두 캐시하기 때문에 전체 파일을 Front Door에서 캐시할 필요가 없시보드에서 유료 구독 키를 사용할 수도 있습니다(Cognitive Services API 계정 참조).습니다. 파일 또는 바이트 범위에 대한 후속 요청은 캐시에서 처리됩니다. 일부 청크가 캐시되지 않은 경우 프리페치를 사용하여 백엔드에서 청크를 요청합니다. 이 최적화는 바이트 범위 요청을 지원하기 위해 백엔드 기능에 의존합니다. 즉, 백엔드가 바이트 범위 요청을 지원하지 않으면 이 최적화가 수행되지 않습니다.

## <a name="file-compression"></a>파일 압축
Front Door는 콘텐츠를 에지에서 동적으로 압축하므로 클라이언트에 보다 작고 빠른 응답을 제공할 수 있습니다. 모든 파일이 압축에 적합합니다. 그러나 파일은 압축 목록에 적합한 MIME 형식이어야 합니다. 현재 Front Door에서는 이 목록의 변경을 허용하지 않습니다. 현재 목록:</br>
- "application/eot"
- "application/font"
- "application/font-sfnt"
- "application/javascript"
- "application/json"
- "application/opentype"
- "application/otf"
- "application/pkcs7-mime"
- "application/truetype"
- "application/ttf",
- "application/vnd.ms-fontobject"
- "application/xhtml+xml"
- "application/xml"
- "application/xml+rss"
- "application/x-font-opentype"
- "application/x-font-truetype"
- "application/x-font-ttf"
- "application/x-httpd-cgi"
- "application/x-mpegurl"
- "application/x-opentype"
- "application/x-otf"
- "application/x-perl"
- "application/x-ttf"
- "application/x-javascript"
- "font/eot"
- "font/ttf"
- "font/otf"
- "font/opentype"
- "image/svg+xml"
- "text/css"
- "text/csv"
- "text/html"
- "text/javascript"
- "text/js", "text/plain"
- "text/richtext"
- "text/tab-separated-values"
- "text/xml"
- "text/x-script"
- "text/x-component"
- "text/x-java-source"

또한 파일의 크기는 1KB 이상 8MB 이하여야 합니다.

이러한 프로필은 다음과 같은 압축 인코딩을 지원합니다.
- [Gzip (GNU zip)](https://en.wikipedia.org/wiki/Gzip)
- [Brotli](https://en.wikipedia.org/wiki/Brotli)

요청이 gzip 및 Brotli 압축을 지원하는 경우 Brotli 압축이 우선적으로 적용됩니다.</br>
자산에 대한 요청이 압축을 지정하고 캐시의 요청 결과가 누락된 경우 Front Door는 POP 서버에서 직접 자산의 압축을 수행합니다. 이후 압축된 파일은 캐시에서 제공됩니다. 결과 항목은 transfer-encoding: chunked를 사용하여 반환됩니다.

## <a name="query-string-behavior"></a>쿼리 문자열 동작
Front Door를 사용하면 쿼리 문자열이 포함된 웹 요청에 대해 파일이 캐시되는 방식을 제어할 수 있습니다. 쿼리 문자열이 있는 웹 요청에서 쿼리 문자열은 물음표(?) 다음에 나오는 요청 부분입니다. 쿼리 문자열은 필드 이름 및 해당 값이 등호(=)로 구분된 하나 이상의 키-값 쌍을 포함할 수 있습니다. 각 키-값 쌍은 앰퍼샌드(&)로 구분됩니다. 예: `http://www.contoso.com/content.mov?field1=value1&field2=value2`. 요청의 쿼리 문자열에 둘 이상의 키-값 쌍이 있는 경우 해당 순서는 중요하지 않습니다.
- **쿼리 문자열 무시**: 기본 모드입니다. 이 모드에서는 Front Door가 첫 번째 요청에서 요청자의 쿼리 문자열을 백엔드로 전달하고 자산을 캐시합니다. 캐시된 자산이 만료될 때까지 Front Door 환경에서 제공되는 해당 자산의 모든 후속 요청은 쿼리 문자열을 무시합니다.

- **각 고유한 URL 캐시**: 이 모드에서는 쿼리 문자열을 포함하여 고유한 URL이 있는 각 요청이 고유 캐시가 있는 고유 자산으로 처리됩니다. 예를 들어 `www.example.ashx?q=test1`에 대한 요청의 경우 백엔드에서의 응답이 Front Door 환경에서 캐시되고 그와 동일한 쿼리 문자열을 가진 후속 캐시에 대해 반환됩니다. `www.example.ashx?q=test2`에 대한 요청은 자체 TTL(Time To Live) 설정과 함께 별도의 자산으로 캐시됩니다.

## <a name="cache-purge"></a>캐시 제거
Front Door는 자산의 TTL(Time-to-Live)이 만료될 때 자산을 캐시합니다. 자산의 TTL이 만료된 후에 클라이언트가 자산을 요청하는 경우 Front Door는 새로 업데이트된 자산 복사본을 검색하여 클라이언트 요청을 처리하고 캐시를 새로 고칩니다.
</br>사용자가 항상 자산의 최신 복사본을 받도록 보장하는 가장 좋은 방법은 각 업데이트에 대해 자산 버전 관리를 수행하여 자산을 새 URL로 게시하는 것입니다. Front Door는 다음 클라이언트 요청에 대한 새 자산을 즉시 검색합니다. 경우에 따라 모든 가장자리 노드에서 캐시된 콘텐츠를 제거하고 이러한 콘텐츠가 모두 새로 업데이트된 자산을 검색하도록 강제하려 할 수 있습니다. 웹 애플리케이션에 대한 업데이트 또는 잘못된 정보가 포함된 신속한 업데이트 자산 때문일 수 있습니다.

</br>가장자리 노드에서 제거하려는 자산을 선택합니다. 모든 자산을 지우려면 모두 제거 확인란을 클릭합니다. 그렇지 않으면, 제거하려는 각 자산의 경로를 경로 텍스트 상자에 입력합니다. 경로에 지원되는 형식은 아래와 같습니다.
1. **단일 경로 제거**:/pictures/strasbourg.png 같은 파일 확장명을 사용 하 여 자산의 전체 경로 (프로토콜 및 도메인 제외)를 지정 하 여 개별 자산을 제거 합니다.
2. **와일드 카드 제거**: 별표(\*)를 와일드 카드로 사용할 수 있습니다. 경로에서/를 사용 하 여 끝점 아래의 모든 폴더, 하위 폴더 및 파일을 제거 \* 하거나, 폴더에/(예:/pictures/)를 지정 하 여 특정 폴더에 있는 모든 하위 폴더 및 파일을 제거 \* \* 합니다.
3. **루트 도메인 제거**: 경로에 "/"가 포함된 엔드포인트의 루트를 제거합니다.

Front Door의 캐시 제거에서는 대/소문자를 구분하지 않습니다. 또한 쿼리 문자열을 알 수 없으므로 URL을 제거하면 URL의 모든 쿼리 문자열 변형이 제거됩니다. 

## <a name="cache-expiration"></a>캐시 만료
항목이 캐시에 저장될 기간을 결정하기 위해 다음 순서의 헤더가 사용됩니다.</br>
1. Cache-Control: s-maxage=\<seconds>
2. Cache-control: max-age =\<seconds>
3. 기간이\<http-date>

캐시 제어 응답 헤더는 캐시 제어: 전용, Cache-control: 캐시 없음 및 Cache-control: 저장소가 적용 되지 않음을 나타내는 응답을 캐시 하지 않습니다. 그러나 여러 요청이 동일한 URL에 대해 POP에서 이동 중인 경우 해당 응답을 공유할 수도 있습니다. 캐시가 제어 되지 않는 경우 기본 동작은 AFD가 1 ~ 3 일 사이에 X를 임의로 선택 하는 X 시간 동안 리소스를 캐시 하는 것입니다.

## <a name="request-headers"></a>요청 헤더

캐싱을 사용하는 경우 다음 요청 헤더는 백엔드로 전달되지 않습니다.
- Content-Length
- Transfer-Encoding

## <a name="next-steps"></a>다음 단계

- [Front Door를 만드는](quickstart-create-front-door.md) 방법을 알아봅니다.
- [Front Door의 작동 원리](front-door-routing-architecture.md)를 알아봅니다.
