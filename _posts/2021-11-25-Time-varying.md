---
layout: single
title: "[Causal Inference] Causal Inference with Complex Longitudinal Data"
categories:
- Causal Inference
---
Longitudinal setting에서 적용 가능한 causal inference method로 가장 잘 알려져 있는 방법은 g-method입니다. 이 g-method에 대해 공부한 것들을 하나씩 공유하고자 합니다. 저도 공부하는 입장으로서 최대한 오류 없는 내용을 기술하려 노력하겠지만 그럼에도 불구하고 틀린 내용을 기술할 수 도 있습니다. 그러한 내용에 대해 읽는 분들께서 아낌 없이 지적을 해주시면 감사하겠습니다. 또한, 이 블로그(?)의 존재의 이유가 제가 공부한 내용들을 정리하여 추후 다시 보기 위함이기 때문에 작성된 내용들이 읽는 분의 배경 지식 및 누적된 지식에 따라 쉽게 또는 어렵게 느껴지실 수 있을 것이며, 여기에 기술된 내용만이 전부는 절대 아니고 때로는 최소한의 내용이 될 수도 있습니다. 읽는 분들 중 작성된 내용이 어려운 분들께서는 어떤 내용이 어려운지, 쉬운 분들께는 어떤 내용이 더 자세하게 기술되었으면 하는지 그러한 바람 및 기대를 저에게 알려주신다면 반영하여 글을 업데이트하도록 하겠습니다. 하지만 업데이트 속도는 그리 빠르지 않을 수 있으니 너그러운 마음으로 지켜봐주셨으면 좋겠습니다. 또한, 전공 상 국외논문을 읽는 경우가 많고, 사용하는 용어에 대한 적절하면서 마음에 드는 한글 표기법을 찾는 것이 어려워 많은 표기에 있어 영어와 한글을 섞어서 사용하도록 하겠습니다. 아무쪼록 제 작은 지식이 산업보건, 직업환경의학 및 역학 관련 종사자 분들께 도움이 되어 우리나라의 보건 및 역학 연구에 기여될 수 있기를 바랍니다.

[G-method]
G-method에는 g-formula, inverse probability of treatment weighting(IPTW), g-estimation 3가지가 있습니다. G-method는 longitudinal data에 적용이 가능한 방법이지만 그 중 특히 treatment-confounder feedback을 포함하는 causal graph를 가지는 data에서 그 강점을 발휘합니다. 자료 분석을 함에 있어 longitudinal data는 시간도 중요한 정보 중 하나이기 때문에 고려하여 분석해야 합니다. causal inference에서 각 방법을 적용하기 이전에 causal graph를 가장 먼저 고려하게 되는데 causal graph에 시간에 대한 정보를 반영함으로써 longitudinal data에서 중요한 특징인 시간을 고려하여 자료를 분석합니다.

아래의 그림은 longitudinal data에서 흔히 볼 수 있는 causal graph입니다.

(그림. A_{t}는 시점 t에서의 time-varying treatment 또는 time-varying exposure. L_{t}는 시점 t에서의 time-varying confounder 그리고 Y_{t}는 시점 t에서의 time-varying outcome을 의미합니다.)

위의 그림에 볼 수 있는 특징은 A_{t-1}에서 L_{t}, L_{t}에서 A_{t}으로의 화살표입니다. time-fixed causal graph에서는 두 요인 사이의 화살표에 대해서 화살표의 방향이 양방이면 안 되고 일방이어야 한다고 알려져 있습니다. 하지만 현재 보시는 causal graph에서는 양방임을 확인할 수 있는데, 이러한 표현이 가능한 이유는 시간에 대한 정보를 그림이 가지고 있기 때문입니다. 그러므로 양방의 화살표처럼 보이지만 causal graph에서는 일방의 화살표로만 표현되어 있습니다. 위와 같이 time-varying treatment와 time-varying confounder가 서로 그 영향을 주고 받으면서 time-varying confounder을 보정해야하지만 보정하면 open path가 생기는 경우 그 causal graph는 treatment-confounder feedback을 가지고 있다고 합니다. treatment-confounder feedback을 포함하는 causal graph를 표현하는 자료에서는 전통적인 통계방법(예; glm)을 사용하여 분석하면 결과 변수에 대한 노출 변수의 causal effect에 bias가 발생합니다. 이러한 문제를 해결하기 위해 James M. Robins가 개발한 방법이 바로 g-method입니다. g-formula, IPTW 그리고 g-estimation 3 가지 중 g-formula에 대해서 먼저 설명드리도록 하겠습니다.

[G-formula] Parametric g-formula
g-formula는 time-fixed causal inference method에서 잘 알려진 방법인 standardization가 time-varying data으로 extension 된 방법 또는 g-formula의 time-fixed version이 standardization이라고 볼 수 있습니다. 이러한 이유로 time-fixed causal inference method에서 standardization을 이해하고 계신 독자 분들께서는 보다 쉽게 g-formula를 이해하시리라 생각합니다. g-formula에 대한 자세한 기술 이전에 time-varying causal inference에서 자주 사용되는 intervention에 대해서 먼저 소개를 하겠습니다. 아래에서 기술한 모둔 g-formula에 대한 설명은 James M. Robins(1986), Hernan MA, James M. Robins(2020), Lin VL et al.(2020) 또는 산업보건안전연구원 g-formula 가이드라인(2021)에 기술되어 있는 내용입니다. 보다 자세한 내용을 알고 싶으신 독자께서는 해당 논문을 참고해주시기 바랍니다.

Intervention이란 ?


Types of interventions
1. Deterministic intervention / Random, probabilistic or stochastic intervention
(1) Deterministic intervention

(2) Random, probabilistic or stochastic intervention

2. Static / Dynamic intervention
(1) Static intervention

(2) Dynamic intervention


Assumptions for g-method
1. Sequential (conditional) exchangeability or ignorability


2. SUTVA


3. Positivity

위의 3가지 가정이 모두 만족하면 g-formula를 통해 expectation of counterfactual outcome을 관찰된 자료를 통해 산출할 수 있습니다. 아래의 식은 expectation of counterfactual outcome와 관찰된 자료 사이의 연결성을 표현하고 있습니다.

(식)


g-formula를 통해 expectation of counterfactural outcome을 계산하는 과정은 아래와 같습니다.
[Step 1]

[Step 2]

[Step 3]



[Inverse probability of treatment weighting] Marginal Structural Models(MSMs)



[G-estimation] Structural Nested Models(SNMs)

[Reference]
1. James M. Robins(1986)
2. Hernan MA(2020), What if
3. Lin VL and Sean Mcgrath(2020)