---
title: FedRAMP High 청사진 개요
description: FedRAMP High 청사진 샘플의 개요입니다. 이 청사진 샘플은 고객이 특정 FedRAMP High 컨트롤을 평가하는 데 도움이 됩니다.
ms.date: 07/31/2020
ms.topic: sample
ms.openlocfilehash: a8761d02d089013bccd31a9968367b26c3d22f87
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87495865"
---
# <a name="overview-of-the-fedramp-high-blueprint-sample"></a>FedRAMP High 청사진 샘플의 개요

FedRAMP High 청사진 샘플은 특정 FedRAMP High 컨트롤을 평가하는 데 도움이 되는 [Azure Policy](../../../policy/overview.md)를 사용하여 거버넌스 가드 레일을 제공합니다. 이 청사진은 고객이 FedRAMP High 컨트롤을 구현해야 하는 Azure 배포 아키텍처에 대한 핵심 정책 세트를 배포하는 데 도움이 됩니다.

## <a name="control-mapping"></a>컨트롤 매핑

컨트롤 매핑 섹션에서는 이 청사진에 포함된 정책과 이러한 정책이 FedRAMP High의 다양한 컨트롤을 처리하는 방법에 대해 자세히 설명합니다. 아키텍처에 할당된 리소스는 할당된 정책을 준수하지 않는지 Azure 정책에서 평가됩니다. 자세한 내용은 [Azure Policy](../../../policy/overview.md)를 참조하세요.

## <a name="next-steps"></a>다음 단계

FedRAMP High의 개요와 그 청사진 샘플을 살펴봤습니다. 이제 다음 문서를 참조하여 컨트롤 매핑 및 이 샘플을 배포하는 방법에 대해 알아보세요.

> [!div class="nextstepaction"]
> [FedRAMP High 청사진 - 컨트롤 매핑](./control-mapping.md)
> [FedRamp High 청사진 - 배포 단계](./deploy.md)

청사진 및 사용 방법에 대한 추가 문서:

- [청사진 수명 주기](../../concepts/lifecycle.md)에 대해 알아봅니다.
- [정적 및 동적 매개 변수](../../concepts/parameters.md) 사용 방법 이해
- [청사진 시퀀싱 순서](../../concepts/sequencing-order.md)를 사용자 지정하는 방법 알아보기
- [청사진 리소스 잠금](../../concepts/resource-locking.md)을 활용하는 방법 알아보기
- [기존 할당을 업데이트](../../how-to/update-existing-assignments.md)하는 방법 알아보기
