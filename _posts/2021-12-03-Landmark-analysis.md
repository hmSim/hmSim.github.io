---
layout: single
title: "[Survival analysis] Landmark analysis"
categories:
- Survival analysis
---

#### 1. Motivation
The landmark analysis has been used extensively in medical research to correct for the bias inherent in the analysis of time-to-event outcome between groups determined during study follow-up.

Such a situation was identified inthe context of heart transplant data in which the survivla of patients receiving heart transplant is compared with that of control subjects. The patients receiving a heart transplant must have at least survived from time of diagnosis to time of treatment, whereas no such requirement is necessary for the control subjects. This 'time-to-treatment' bias is identical to the 'time-to-response' bias in the comparison of overall survival between 'responders' and 'nonresponders'

In general, the landmark analysis is applied to the classification to any groups formed during follow-up on the basis of "a risk elevating or diminishing event". But, the method is not attempting to address the lack of randomization on the basis of outcome.

#### 2. Method
Only patients alive at the landmark time are included in the analysis, separated into 2 response categories according to whether they have responded up to that time, that is, the landmark method ignores all responses after the landmark time and all deaths before that time.

#### 3. Limitations
##### 1) Choice of Landmark
- Data-Driven Results: the landmark should be selected a priori, based on some clinically significant natural time before the start of data anlaysis.
- Conditional Hazard Ratio Estimates: In such cases that the earlier than the landmark point survival history is different, a fact that cannot generally be verified, the omission could lead to possible misrepresentation of the overall survival time model.
- Missclassification and Loss of Power: The landmark method is clearly most powerful when the 'risk altering intervening event' occurs comparatively early and the outcomes of interest are not particularly common at this early study period. In this case, when using an early landmark time point, both the misclassification with longer follow-up and the loss of power would be unimportant. Thus, for example, the landmark analysis is a recognized useful tool for removing guarantee-time bias in the cases that response to treatment occurs early after starting treatment, whereas in cases in which response to treatment occurs over an extended period of time, use of the landmark method is not indicated.
- Alternative method: Time-varying Cox model

##### 2) Lack of the Randomization Property
- The group membership property is confounded with the patient characteristics.
- In the case of “responders,” response is possibly just a marker that selects the good prognosis patients.1,9 Because of the breakdown of the randomization property, the significantly longer survival in one group could be attributed to the better prognosis characteristics of the members of this group instead of the property of “group membership.” It should be recognized that in cases that landmark analysis results to a significant differential effect between groups as defined by the time of landmark, it can only claim “association” and not a “cause-effect” relationship between benefit and group membership. This is consistent with inference within the usual “observational study” framework with the group membership effect corresponding to the “treatment effect.” In this context, it is generally close to impossible to disentangle the effect of a “treatment” from the possibly confounding effects of the underlying factors that lead to “treatment allocation.” Nevertheless, this limitation of the landmark analysis is well known and it is shared by the ad hoc method suggested by Mantel and Byar or the time-varying covariate Cox model.
- Alternative method: inverse probability weighted method

#### 4. Connection between time-varying Cox model and landmark method

#### 5. References
[1]
[2]
