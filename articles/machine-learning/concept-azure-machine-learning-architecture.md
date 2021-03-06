---
title: 아키텍처 및 주요 개념
titleSuffix: Azure Machine Learning
description: Azure Machine Learning을 구성하는 아키텍처, 용어 및 워크플로에 대해 알아봅니다.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: larryfr
author: Blackmist
ms.date: 05/13/2020
ms.custom: seoapril2019, seodec18
ms.openlocfilehash: 749a2366438bd1abfef4ca0cf2a195f23529d6a5
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86536303"
---
# <a name="how-azure-machine-learning-works-architecture-and-concepts"></a>Azure Machine Learning 작동 방법: 아키텍처 및 개념

Azure Machine Learning의 아키텍처, 개념 및 워크플로에 대해 알아봅니다. 다음 다이어그램에서 서비스의 주요 구성 요소와 서비스를 사용하기 위한 일반적인 워크플로를 보여줍니다.

![Azure Machine Learning 아키텍처 및 워크플로](./media/concept-azure-machine-learning-architecture/workflow.png)

## <a name="workflow"></a>워크플로

기게 학습 모델 워크플로는 일반적으로 다음과 같은 순서를 따릅니다.

1. **학습**
    + **Python**, **R** 또는 비주얼 디자이너를 사용하여 기계 학습용 학습 스크립트를 개발합니다.
    + **컴퓨팅 대상**을 만들고 구성합니다.
    + 해당 환경에서 실행하도록 구성된 컴퓨팅 대상에 **스크립트를 제출**합니다. 학습 동안 **데이터 저장소**에서 스크립트를 읽거나 쓸 수 있습니다. 학습 중에 생성된 로그 및 출력은 **작업 영역**에서 **실행**으로 저장되고 **실험** 아래에 그룹화됩니다.

1. **패키지** - 만족스러운 실행이 발견되면 **모델 레지스트리**에 지속되는 모델을 등록합니다.

1. **Validate** - **실험을 쿼리**하여 현재 및 과거 실행에서 기록된 메트릭을 확인합니다. 메트릭이 원하는 결과를 표시하지 않으면 1단계로 돌아가 스크립트를 반복합니다.

1. **배포** - 모델을 사용하는 점수 매기기 스크립트를 개발하고 Azure에서 **웹 서비스**로서 또는 **IoT Edge 디바이스**에 **모델을 배포**합니다.

1. **모니터링** - 학습 데이터 세트 및 배포된 모델의 유추 데이터 간의 **데이터 드리프트**를 모니터링합니다. 필요한 경우 1단계를 반복하여 새 학습 데이터로 모델을 다시 학습합니다.

## <a name="tools-for-azure-machine-learning"></a>Azure Machine Learning 도구

다음과 같은 Azure Machine Learning 도구를 사용합니다.

> [!IMPORTANT]
> 아래 표시 된 (미리 보기) 도구는 현재 공개 미리 보기로 제공 됩니다.
> 미리 보기 버전은 서비스 수준 계약 없이 제공 되며 프로덕션 워크 로드에는 권장 되지 않습니다. 특정 기능이 지원되지 않거나 기능이 제한될 수 있습니다. 자세한 내용은 [Microsoft Azure Preview에 대한 추가 사용 약관](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)을 참조하세요.

+  [Python용 Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)를 사용하여 Python 환경에서 서비스와 상호 작용합니다.
+ R (미리 보기) [에 대 한 AZURE MACHINE LEARNING SDK](https://azure.github.io/azureml-sdk-for-r/reference/index.html) 를 사용 하 여 모든 r 환경에서 서비스와 상호 작용 합니다.
+ [Azure Machine Learning CLI](https://docs.microsoft.com/azure/machine-learning/reference-azure-machine-learning-cli)를 사용하여 기계 학습 활동을 자동화합니다.
+ [Azure Machine Learning 디자이너(미리 보기)](concept-designer.md)를 사용하여 코드를 작성하지 않고 워크플로 단계를 수행합니다. ( [엔터프라이즈 작업 영역](concept-workspace.md#upgrade))은 디자이너를 사용 하는 데 필요 합니다.
+ [많은 모델 솔루션 가속기](https://aka.ms/many-models)(미리 보기)는 Azure Machine Learning을 기준으로 하며 수백 또는 수천 개의 기계 학습 모델을 학습, 운영 및 관리할 수 있습니다.

> [!NOTE]
> 이 문서에서는 Azure Machine Learning에서 사용되는 용어와 개념을 정의하지만, Azure 플랫폼에 대한 용어와 개념은 다루지 않습니다. Azure 플랫폼 용어에 대한 자세한 내용은 [Microsoft Azure 용어집](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology)을 참조하세요.

## <a name="glossary"></a>용어

* [활동](#activities)
* [작업 영역](#workspaces)
    * [실험](#experiments)
        * [실행](#runs) 
            * [실행 구성](#run-configurations)
            * [스냅샷](#snapshots)
            * [Git 추적](#github-tracking-and-integration)
            * [Logging](#logging)
    * [ML 파이프라인](#ml-pipelines)
    * [모델](#models)
        * [환경](#environments)
        * [학습 스크립트](#training-scripts)
        * [예측 도구](#estimators)
    * [엔드포인트](#endpoints)
        * [웹 서비스](#web-service-endpoint)
        * [IoT 모듈](#iot-module-endpoints)
    * [데이터 세트 및 데이터 저장소](#datasets-and-datastores)
    * [컴퓨팅 대상](#compute-targets)

### <a name="activities"></a>활동

작업은 장기 실행 작업을 나타냅니다. 다음 조작은 작업의 예입니다.

* 컴퓨팅 대상 만들기 또는 삭제
* 컴퓨팅 대상에서 스크립트 실행

작업은 SDK 또는 웹 UI를 통해 알림을 제공할 수 있으므로, 사용자가 이러한 조작의 진행 상황을 쉽게 모니터링할 수 있습니다.

### <a name="workspaces"></a>작업 영역

[작업 영역](concept-workspace.md)은 Azure Machine Learning의 최상위 리소스입니다. 작업 영역은 Azure Machine Learning를 사용하는 경우 만드는 모든 아티팩트를 사용할 수 있는 중앙 집중식 위치를 제공합니다. 작업 영역을 다른 사용자와 공유할 수 있습니다. 작업 영역에 대한 자세한 설명은 [Azure Machine Learning 작업 영역이란?](concept-workspace.md)을 참조하세요.

### <a name="experiments"></a>실험

실험은 지정된 스크립트의 많은 실행을 그룹화한 것입니다. 실험은 항상 작업 영역에 속합니다. 실행을 제출할 때 실험 이름을 제공합니다. 실행에 대한 정보는 해당 실험 아래에 저장됩니다. 실행을 제출하고 존재하지 않는 실험 이름을 지정하면 새로 지정된 해당 이름을 가진 새 실험이 자동으로 생성됩니다.

실험을 사용하는 예제는 [자습서: 첫 번째 모델 학습](tutorial-1st-experiment-sdk-train.md)을 참조하세요.

### <a name="runs"></a>실행

실행은 학습 스크립트의 단일 실행을 말합니다. 실험은 일반적으로 여러 실행을 포함합니다.

Azure Machine Learning은 모든 실행을 기록하고 실험에 다음 정보를 저장합니다.

* 실행에 대한 메타데이터(타임스탬프, 기간 등)
* 스크립트를 통해 기록된 메트릭
* 실험을 통해 자동 수집되거나 사용자가 명시적으로 업로드한 출력 파일
* 실행 전의 스크립트를 포함하는 디렉터리의 스냅샷

모델을 학습시키기 위해 스크립트를 제출할 때 모델을 생성합니다. 실행에는 0개 이상의 자식 실행이 포함될 수 있습니다. 예를 들어 최상위 실행에는 두 개의 자식 실행이 포함될 수 있고, 각 자식 실행에는 자체 자식 실행이 포함될 수 있습니다.

### <a name="run-configurations"></a>실행 구성

실행 구성은 지정된 컴퓨팅 대상에서 스크립트를 실행해야 하는 방법을 정의하는 지침 세트를 보여줍니다. 구성에는 기존 Python 환경을 사용할지 또는 사양에서 빌드되는 Conda 환경을 사용할지 여부와 같은 광범위한 동작 정의 세트가 포함됩니다.

실행 구성은 학습 스크립트를 포함하는 디렉터리 내의 파일에 지속되거나, 메모리 내 개체로 구성되고 실행을 제출하는 데 사용될 수 있습니다.

실행 구성 예제는 [모델 학습을 위한 컴퓨팅 대상 선택 및 사용](how-to-set-up-training-targets.md)을 참조하세요.

### <a name="snapshots"></a>스냅샷

실행을 제출하면 Azure Machine Learning은 스크립트를 포함하는 디렉터리를 zip 파일로 압축하여 컴퓨팅 대상으로 보냅니다. 그런 다음, zip 파일이 추출되고 스크립트가 실행됩니다. 또한 Azure Machine Learning은 zip 파일을 실행 기록의 일부인 스냅샷으로 저장합니다. 작업 영역에 대한 액세스 권한이 있는 사용자는 실행 기록을 찾아보고 스냅샷을 다운로드할 수 있습니다.

> [!NOTE]
> [!INCLUDE [amlinclude-info](../../includes/machine-learning-amlignore-gitignore.md)]

### <a name="github-tracking-and-integration"></a>GitHub 추적 및 통합

원본 디렉터리가 로컬 Git 리포지토리인 학습 실행을 시작하면 리포지토리에 대한 정보가 실행 기록에 저장됩니다. 이 작업은 예측 도구, ML 파이프라인 또는 스크립트 실행을 사용하여 제출된 실행에 대해 진행됩니다. SDK 또는 Machine Learning CLI에서 제출된 실행에 대해서도 진행됩니다.

자세한 내용은 [Azure Machine Learning에 대한 Git 통합](concept-train-model-git-integration.md)을 참조하세요.

### <a name="logging"></a>로깅

솔루션을 개발할 때 Python 스크립트에서 Azure Machine Learning Python SDK를 사용하여 임의 메트릭을 기록합니다. 실행 후에 메트릭을 쿼리하여 실행에서 배포하려는 모델을 생성했는지 여부를 확인합니다.

### <a name="ml-pipelines"></a>ML 파이프라인

기계 학습 파이프라인을 사용하여 기계 학습 단계를 연결하는 워크플로를 만들고 관리합니다. 예를 들어, 파이프라인에는 데이터 준비, 모델 학습, 모델 배포 및 추론/점수 매기기 단계가 포함될 수 있습니다. 각 단계(phase)는 각각 다양한 컴퓨팅 대상에서 자동으로 실행될 수 있는 여러 단계(step)를 포함할 수 있습니다. 

파이프라인 단계는 다시 사용할 수 있으며 해당 단계의 출력이 변경되지 않은 경우 이전 단계를 다시 실행하지 않고 실행할 수 있습니다. 예를 들어, 데이터가 변경되지 않은 경우 비용이 많이 드는 데이터 준비 단계를 다시 실행하지 않고 모델을 다시 학습할 수 있습니다. 파이프라인을 사용하여 데이터 과학자들은 기계 학습 워크플로의 개별 영역에서 작업하면서 협력할 수 있습니다.

이 서비스를 사용한 기계 학습 파이프라인에 대한 자세한 내용은 [파이프라인 및 Azure Machine Learning](concept-ml-pipelines.md)를 참조하세요.

### <a name="models"></a>모델

간단하게 설명하면, 하나의 모델은 입력을 받아들이고 출력을 생성하는 코드의 한 조각을 말합니다. 기계 학습 모델을 만드는 동안 알고리즘을 선택하고, 데이터를 제공하고, 하이퍼 매개 변수를 조정합니다. 학습은 학습된 모델을 생성하는 반복 프로세스이며, 학습된 모듈에는 모델이 학습 프로세스 중에 습득한 내용이 캡슐화되어 있습니다.

모델은 Azure Machine Learning에서 실행을 통해 생성됩니다. Azure Machine Learning 외부에서 학습된 모델을 사용할 수도 있습니다. Azure Machine Learning 작업 영역에 모델을 등록할 수 있습니다.

Azure Machine Learning은 프레임워크에 제약이 없습니다. 모델을 만드는 경우 Scikit-learn, XGBoost, PyTorch, TensorFlow 및 Chainer와 같은 인기 있는 기계 학습 프레임워크를 사용할 수 있습니다.

Scikit 및 예측 도구를 사용하여 모델을 학습하는 방법에 대한 예제를 보려면 [자습서: Azure Machine Learning을 사용하여 이미지 분류 모델 학습](tutorial-train-models-with-aml.md)을 참조하세요.

**모델 레지스트리**는 Azure Machine Learning 작업 영역에서 모든 모델을 추적합니다.

모델은 이름 및 버전으로 식별됩니다. 기존 이름과 동일한 이름을 사용하여 모델을 등록할 때마다 레지스트리는 해당 모델을 새 버전으로 간주합니다. 버전 번호가 증가하면 동일 이름으로 새 모델을 등록합니다.

모델을 등록할 때 추가 메타데이터 태그를 제공하면 모델을 검색할 때 해당 태그를 사용할 수 있습니다.

> [!TIP]
> 등록된 모델은 모델을 구성하는 하나 이상의 파일에 대한 논리적 컨테이너입니다. 예를 들어, 여러 파일에 저장된 모델의 경우 Azure Machine Learning 작업 영역에서 단일 모델로 등록할 수 있습니다. 등록 후에 등록된 모델을 다운로드하거나 배포하고 등록된 모든 파일을 받을 수 있습니다.

활성 배포에서 사용 중인 등록된 모델은 삭제할 수 없습니다.

모델을 등록하는 예제는 [Azure Machine Learning을 사용하여 이미지 분류 모델 학습](tutorial-train-models-with-aml.md)을 참조하세요.

### <a name="environments"></a>환경

Azure ML 환경은 데이터 준비, 모델 학습 및 모델 제공을 위한 재현 가능한 환경을 만드는 데 사용되는 구성(Docker/Python/Spark 등)을 지정하는 데 사용됩니다. 이러한 환경은 Azure Machine Learning 작업 영역 내에서 관리 및 버전이 지정된 엔터티로, 다양한 컴퓨팅 대상에 걸쳐 재현 가능하고, 감사 가능하고, 이식 가능한 기계 학습 워크플로를 사용할 수 있습니다.

로컬 컴퓨텅에서 환경 개체를 사용하여 학습 스크립트를 개발하고, 대규모 모델 학습을 위해 동일한 환경을 Azure Machine Learning 컴퓨팅에서 재사용하고, 동일한 환경에서 모델을 배포할 수 있습니다. 

학습 및 유추에 대해서는 [재사용 가능한 ML 환경을 만들고 관리하는 방법](how-to-use-environments.md)을 알아봅니다.

### <a name="training-scripts"></a>교육 스크립트

모델을 학습시키려면 학습 스크립트 및 연결된 파일을 포함하는 디렉터리를 지정합니다. 학습 중에 수집된 정보를 저장하는 데 사용되는 실험 이름도 지정합니다. 학습 중에 전체 디렉터리가 학습 환경(컴퓨팅 대상)에 복사되고 실행 구성을 통해 지정된 스크립트가 시작됩니다. 또한 디렉터리의 스냅샷이 작업 영역의 실험 아래에 저장됩니다.

예제를 보려면 [ 자습서: Azure Machine Learning을 사용하여 이미지 분류 모델 학습](tutorial-train-models-with-aml.md)을 참조하세요.

### <a name="estimators"></a>예측 도구

인기 있는 프레임워크를 사용하여 모델 학습을 용이하게 하기 위해 예측 도구 클래스를 사용하여 실행 구성을 쉽게 만들 수 있습니다. 제네릭 [예측 도구](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py)를 만들어서 사용하면 원하는 학습 프레임워크(예: scikit)를 사용하는 학습 스크립트를 제출할 수 있습니다.

PyTorch, TensorFlow 및 Chainer 작업의 경우 Azure Machine Learning은 이러한 프레임워크 사용을 간소화할 수 있도록 각각의 [PyTorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py), [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) 및 [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) 예측 도구를 제공합니다.

자세한 내용은 다음 문서를 참조하세요.

* [예측 도구를 사용하여 ML 모델 학습](how-to-train-ml-models.md)
* [Azure Machine Learning을 사용하여 대규모로 Pytorch 딥 러닝 모델 학습](how-to-train-pytorch.md)
* [Azure Machine Learning을 사용하여 대규모로 TensorFlow 모델 학습 및 등록](how-to-train-tensorflow.md)
* [Azure Machine Learning을 사용하여 대규모로 Chainer 모델 학습 및 등록](how-to-train-ml-models.md)

### <a name="endpoints"></a>엔드포인트

엔드포인트는 클라우드에서 호스트될 수 있는 웹 서비스 또는 통합형 디바이스 배포를 위한 IoT 모듈을 사용해 모델을 인스턴스화하는 것입니다.

#### <a name="web-service-endpoint"></a>웹 서비스 엔드포인트

모델을 웹 서비스로 배포할 때 엔드포인트를 Azure Container Instances, Azure Kubernetes Service 또는 FPGA에 배포할 수 있습니다. 모델, 스크립트 및 연결된 파일에서 서비스를 만듭니다. 이러한 항목은 모델에 대한 실행 환경을 포함하는 기본 컨테이너 이미지에 배치됩니다. 이미지에는 웹 서비스에 전송된 점수 매기기 요청을 수신하는 부하 분산된 HTTP 엔드포인트가 있습니다.

Azure에서는 이 기능을 사용하도록 선택한 경우 Application Insight 원격 분석 또는 모델 원격 분석을 수집하여 웹 서비스를 모니터링할 수 있습니다. 원격 분석 데이터는 사용자만 액세스할 수 있으며 Application Insights 및 스토리지 계정 인스턴스에 저장됩니다.

자동 크기 조정을 사용하도록 설정한 경우 Azure에서 배포 크기를 자동으로 조정합니다.

모델을 웹 서비스로 배포하는 예제는 [Azure Container Instance에 이미지 분류 모델 배포](tutorial-deploy-models-with-aml.md)를 참조하세요.

#### <a name="iot-module-endpoints"></a>IoT 모듈 엔드포인트

배포된 IoT 모듈 엔드포인트는 모델 및 연결된 스크립트나 애플리케이션과 모든 추가 종속성을 포함하는 Docker 컨테이너입니다. Edge 디바이스에서 Azure IoT Edge를 사용하여 이러한 모듈을 배포합니다.

모니터링을 사용하도록 설정한 경우 Azure에서는 Azure IoT Edge 모듈 내의 모델에서 원격 분석 데이터를 수집합니다. 원격 분석 데이터는 사용자만 액세스할 수 있으며 스토리지 계정 인스턴스에 저장됩니다.

Azure IoT Edge는 모듈이 실행 중인지 확인하고 모듈을 호스트 중인 디바이스를 모니터링합니다.


### <a name="compute-instance"></a><a name="compute-instance"></a>컴퓨팅 인스턴스

**Azure Machine Learning 컴퓨팅 인스턴스**(이전의 Notebook VM)는 기계 학습용으로 설치된 여러 도구 및 환경을 포함하는 완전 관리형 클라우드 기반 워크스테이션입니다. 컴퓨팅 인스턴스는 학습 및 추론 작업에 대한 컴퓨팅 대상으로 사용할 수 있습니다. 대량 태스크의 경우 다중 노드 크기 조정 기능이 있는 [Azure Machine Learning 컴퓨팅 클러스터](how-to-set-up-training-targets.md#amlcompute)를 컴퓨팅 대상으로 선택하는 것이 더 바람직합니다.

[컴퓨팅 인스턴스](concept-compute-instance.md)에 대해 자세히 알아봅니다.

### <a name="datasets-and-datastores"></a>데이터 세트 및 데이터 저장소

**Azure Machine Learning 데이터 세트**(미리 보기)를 사용하면 데이터를 더 쉽게 액세스하고 사용할 수 있습니다. 데이터 세트는 모델 학습 및 파이프라인 생성과 같은 다양한 시나리오에서 데이터를 관리합니다. Azure Machine Learning SDK를 사용하여 기본 스토리지에 액세스하고, 데이터를 탐색하고, 다양한 데이터 세트 정의의 수명 주기를 관리할 수 있습니다.

데이터 세트는 `from_delimited_files()` 또는 `to_pandas_dataframe()`을 사용하는 것과 같이 널리 사용되는 형식으로 데이터를 사용하기 위한 방법을 제공합니다.

자세한 내용은 [Azure Machine Learning 데이터 세트 만들기 및 등록](how-to-create-register-datasets.md)을 참조하세요.  데이터 세트를 사용하는 추가 예제는 [샘플 Notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/work-with-data/datasets-tutorial)을 참조하세요.

**데이터 저장소**는 Azure Storage 계정을 통한 스토리지 추상화입니다. 데이터 저장소는 Azure Blob 컨테이너 또는 Azure 파일 공유 중 하나를 백 엔드 스토리지로 사용할 수 있습니다. 각 작업 영역에는 기본 데이터 저장소가 있으며 추가 데이터 저장소를 등록할 수 있습니다. Python SDK API 또는 Azure Machine Learning CLI를 사용하여 데이터 저장소의 파일을 저장하고 검색합니다.

### <a name="compute-targets"></a>컴퓨팅 대상

[컴퓨팅 대상](concept-compute-target.md)을 사용하여 학습 스크립트를 실행하거나 서비스 배포를 호스트하는 컴퓨팅 리소스를 지정할 수 있습니다. 이 위치는 로컬 컴퓨터 또는 클라우드 기반 컴퓨팅 리소스일 수 있습니다.

[학습 및 배포에 사용할 수 있는 컴퓨팅 대상](concept-compute-target.md)에 대해 자세히 알아보세요.

### <a name="next-steps"></a>다음 단계

Azure Machine Learning을 시작하려면 다음을 참조하세요.

* [Azure Machine Learning이란 무엇인가요?](overview-what-is-azure-ml.md)
* [Azure Machine Learning 작업 영역 만들기](how-to-manage-workspace.md)
* [자습서(1부): 모델 학습](tutorial-train-models-with-aml.md)
