---
title: 포함 파일
description: 포함 파일
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: include
ms.custom: include file
ms.date: 07/30/2020
ms.openlocfilehash: e17316d1a17ff36c0a0adf38148ef87c9714e355
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87461131"
---
[참조 설명서](https://docs.microsoft.com/python/api/azure-cognitiveservices-personalizer/azure.cognitiveservices.personalizer?view=azure-python) | [라이브러리 소스 코드](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-personalizer) | [패키지(pypi)](https://pypi.org/project/azure-cognitiveservices-personalizer/) | [샘플](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/python/Personalizer)

## <a name="prerequisites"></a>필수 구성 요소

* Azure 구독 - [체험 구독 만들기](https://azure.microsoft.com/free/)
* [Python 3.x](https://www.python.org/)

## <a name="using-this-quickstart"></a>이 빠른 시작 사용


이 빠른 시작을 사용하는 몇 가지 단계가 있습니다.

* Azure Portal에서 Personalizer 리소스 만들기
* Azure Portal의 Personalizer 리소스에 대한 **구성** 페이지에서 모델 업데이트 빈도를 매우 짧은 간격으로 변경합니다.
* 코드 편집기에서 코드 파일을 만들고 코드 파일을 편집합니다.
* 명령줄 또는 터미널의 명령줄에서 SDK 설치
* 명령줄 또는 터미널에서 코드 파일 실행

[!INCLUDE [Create Azure resource for Personalizer](create-personalizer-resource.md)]

[!INCLUDE [Change model frequency](change-model-frequency.md)]

## <a name="install-the-python-library-for-personalizer"></a>Personalizer용 Python 라이브러리 설치

다음 명령을 사용하여 Python용 Personalizer 클라이언트 라이브러리를 설치합니다.

```console
pip install azure-cognitiveservices-personalizer
```

## <a name="object-model"></a>개체 모델

Personalizer 클라이언트는 키가 포함된 Microsoft.Rest.ServiceClientCredentials를 사용하여 Azure에 인증하는 [PersonalizerClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-personalizer/azure.cognitiveservices.personalizer.personalizer_client.personalizerclient?view=azure-python) 개체입니다.

콘텐츠의 가장 좋은 단일 항목을 요청하려면 [RankRequest](https://docs.microsoft.com/python/api/azure-cognitiveservices-personalizer/azure.cognitiveservices.personalizer.models.rankrequest?view=azure-python)를 만든 다음, client.Rank 메서드에 전달합니다. Rank 메서드는 RankResponse를 반환합니다.

Personalizer에 보상 점수를 보내려면 이벤트 ID와 보상 점수(값)를 EventOperations 클래스의 [보상](https://docs.microsoft.com/python/api/azure-cognitiveservices-personalizer/azure.cognitiveservices.personalizer.operations.events_operations.eventsoperations?view=azure-python#reward-event-id--value--custom-headers-none--raw-false----operation-config-) 메서드로 보내도록 설정합니다.

이 빠른 시작에서 보상을 결정하는 것은 간단합니다. 프로덕션 시스템에서 [보상 점수](../concept-rewards.md)에 영향을 주는 요소와 크기를 결정하는 것은 복잡한 프로세스일 수 있으며, 시간이 지남에 따라 변경될 수 있습니다. 이는 Personalizer 아키텍처에서 기본 설계 결정 중 하나여야 합니다.

## <a name="code-examples"></a>코드 예제

여기에 나와 있는 코드 조각에서는 Python용 Personalizer 클라이언트 라이브러리를 사용하여 다음 작업을 수행하는 방법을 보여 줍니다.

* [Personalizer 클라이언트 만들기](#create-a-personalizer-client)
* [순위 API](#request-the-best-action)
* [보상 API](#send-a-reward)

## <a name="create-a-new-python-application"></a>새 Python 애플리케이션 만들기

선호하는 편집기 또는 `sample.py`라는 IDE에서 Python 애플리케이션을 새로 만듭니다.

## <a name="add-the-dependencies"></a>종속성 추가

프로젝트 디렉터리의 원하는 편집기나 IDE에서 **sample.py** 파일을 엽니다. 다음을 추가합니다.

[!code-python[Add module dependencies](~/cognitive-services-quickstart-code/python/Personalizer/sample.py?name=Dependencies)]

## <a name="add-personalizer-resource-information"></a>Personalizer 리소스 정보 추가

리소스의 Azure 키 및 엔드포인트에 대한 코드 파일의 위쪽에서 키 및 엔드포인트 변수를 편집합니다. 

[!code-python[Create variables to hold the Personalizer resource key and endpoint values found in the Azure portal.](~/cognitive-services-quickstart-code/python/Personalizer/sample.py?name=AuthorizationVariables)]

## <a name="create-a-personalizer-client"></a>Personalizer 클라이언트 만들기

다음으로, Personalizer 클라이언트를 반환하는 메서드를 만듭니다. 메서드의 매개 변수는 `PERSONALIZER_RESOURCE_ENDPOINT`이고, ApiKey는 `PERSONALIZER_RESOURCE_KEY`입니다.

[!code-python[Create the Personalizer client](~/cognitive-services-quickstart-code/python/Personalizer/sample.py?name=Client)]

## <a name="get-content-choices-represented-as-actions"></a>작업으로 표현되는 콘텐츠 선택 항목 가져오기

작업은 Personalizer에서 최상의 콘텐츠 항목을 선택할 수 있는 콘텐츠 선택 항목을 나타냅니다. Program 클래스에 다음 메서드를 추가하여 작업 세트와 해당 기능을 표시합니다.

[!code-python[Present time out day preference to the user](~/cognitive-services-quickstart-code/python/Personalizer/sample.py?name=getActions)]

[!code-python[Present time out day preference to the user](~/cognitive-services-quickstart-code/python/Personalizer/sample.py?name=createUserFeatureTimeOfDay)]

[!code-python[Present food taste preference to the user](~/cognitive-services-quickstart-code/python/Personalizer/sample.py?name=createUserFeatureTastePreference)]

## <a name="create-the-learning-loop"></a>학습 루프 만들기

Personalizer 학습 루프는 [순위](#request-the-best-action) 및 [보상](#send-a-reward) 호출의 주기입니다. 이 빠른 시작에서는 각 순위 호출을 수행하여 콘텐츠를 맞춤 설정한 다음, 보상 호출을 수행하여 서비스에서 얼마나 잘 수행되었는지 Personalizer에 알려줍니다.

다음 코드는 명령줄에서 사용자에게 해당 기본 설정을 요청하고, 해당 정보를 Personalizer로 보내어 최상의 작업을 선택하고, 고객에게 해당 선택 항목을 제시하여 목록에서 선택한 다음, Personalizer에 대한 보상을 서비스에서 얼마나 잘 지정했는지 알려주는 주기를 반복합니다.

[!code-python[The Personalizer learning loop ranks the request.](~/cognitive-services-quickstart-code/python/Personalizer/sample.py?name=mainLoop&highlight=9,10,29)]

코드 파일을 실행하기 전에 [콘텐츠 선택을 가져오는](#get-content-choices-represented-as-actions) 다음 메서드를 추가합니다.

* `get_user_preference`
* `get_user_timeofday`
* `get_actions`

## <a name="request-the-best-action"></a>최상의 작업 요청


프로그램에서는 순위 요청을 수행하기 위해 사용자의 기본 설정에서 `currentContent` 콘텐츠 선택 항목을 만들도록 요청합니다. 이 프로세스에서는 작업에서 제외할 콘텐츠(`excludeActions`로 표시됨)를 만들 수 있습니다. 순위 요청에는 응답을 받을 수 있는 작업과 해당 기능, currentContext 기능, excludeActions 및 고유한 순위 이벤트 ID가 필요합니다.

이 빠른 시작에는 시간 및 사용자 음식 기본 설정에 대한 간단한 컨텍스트 기능이 있습니다. 프로덕션 시스템에서 [작업 및 기능](../concepts-features.md)을 결정하고 [평가](../concept-feature-evaluation.md)하는 것은 간단한 문제가 아닐 수 있습니다.

[!code-python[The Personalizer learning loop ranks the request.](~/cognitive-services-quickstart-code/python/Personalizer/sample.py?name=rank)]

## <a name="send-a-reward"></a>보상 보내기


보상 요청에서 보상 점수를 가져오기 위해 프로그램은 명령줄에서 사용자의 선택 사항을 가져와서 숫자 값을 각 선택 항목에 할당한 다음, 고유한 이벤트 ID와 보상 점수를 숫자 값으로 보상 API에 보냅니다.

이 빠른 시작에서는 0 또는 1의 간단한 숫자를 보상 점수로 할당합니다. 프로덕션 시스템에서 특정 요구 사항에 따라 [보상](../concept-rewards.md) 호출에 보내는 시기와 대상을 결정하는 것은 간단한 문제가 아닐 수 있습니다.

[!code-python[The Personalizer learning loop sends a reward.](~/cognitive-services-quickstart-code/python/Personalizer/sample.py?name=reward&highlight=9)]

## <a name="run-the-program"></a>프로그램 실행

애플리케이션 디렉터리에서 python을 사용하여 애플리케이션을 실행합니다.

```console
python sample.py
```

![빠른 시작 프로그램은 기능으로 알려진 사용자 기본 설정을 수집하기 위해 몇 가지 질문을 합니다.](../media/csharp-quickstart-commandline-feedback-loop/quickstart-program-feedback-loop-example.png)