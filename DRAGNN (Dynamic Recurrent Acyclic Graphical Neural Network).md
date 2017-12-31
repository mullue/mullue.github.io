#DRAGNN (Dynamic Recurrent Acyclic Graphical Neural Network)

원본

[Tensorflow DRAGNN readme](https://github.com/tensorflow/models/blob/master/research/syntaxnet/g3doc/DRAGNN.md)



##개념

텐서플로우를 이용하여 동적 신경망 그래프를 작성하도록 하는 도구이다. 전통적인 RNN(Recurrent Neural Network)과 달리 Recurrent Edge를 그래프에 추가할 때 학습된 정책을 이용한다.

왜  fully dynamic computation graph가 필요할가? 왜냐하면 모델들이 재사용가능한 콤포넌트로 구성되기 때문이다. 예를들어 NLU(Natural Language Understanding)은 다음과 같은 파이프라인으로 이루어진다.

- 입력을 단어 단위로 분리시키고 이들 단어를 표현하는 워드벡터 생성
- 각 단어들에 대한 태깅처리 (PoS tagging, morphological tagging, 외부지식 연결 등)
- 파싱을 통해 구(Phrase)에 대한 벡터표현
- 문장단위 해석(Sentence-level reasoning) 모듈이 구(Phrases)의 의미해석 (감성분석, 의미분석, SRL 등)

DRGNN은 이런 파이프라인들을 텐서플로우 그래프로 정의하고 학습시킬 수 있도록 하는 프레임워크를 제공한다. 이 때 그래프는 파싱트리, 구(phrase), 스팬(span) 등 중단단계 결과를 동적으로 구성할 수 있어야 한다. 그리고 라이브러리는 멀티태스크 프레임워크를 통해 병렬로 학습된 모델을 연결할 수 있어야 하며, 특정 중간 단계에 대한 어노테이션이 없는 경우도 지원해야 한다.



##작동원리

DRGNN은 텐서플로우 그래프와 별개로 dragnn::ComputeSession 이라는 C++ API를 추가하였다. 이런 구성을 위해 dragnn::ComputeSession 라는 API 를 제공한다.

dragnn.Spec 을 통해 정의된 마스터 설정을 입력으로 하여 텐서플로우 그래프와 GRAGNN 처리가 만들어진다.

![DRAGNN](image/DRAGNN.png)





##DRAGNN 그래프의 개념

DRAGNN은 모듈화된 프레임워크이다. 순수 텐서플로우 RNN과 달리DRAGNN은 재사용가능하도록 구성되어야 한다. 

Syntaxnet 사례 : SyntaxNet PoS 태거는 syntaxnet::Sentence 프로토콜 버퍼를 배치로 입력받아서 각 토큰들을 순회하며 자질을 도출하고 Feed forward NN에 입력하고 PoS 태깅을 처리하는 단일 콤포넌트로 구성된다. 

DRAGNN은 여러개의 콤포넌트를 하나의 텐서플로우 그래프로 연결되므로 여러개의 Annotation 작업은 하나의 파이프라인으로 연결될 수 있다. 

Syntaxnet 사례 : Parsy McParseface는 PoS태깅 후 의존관계 분석을 하게되는데 이는 하나의 syntaxnet::Sentence 프로토콜 버퍼를 사용하는 두개의 콤포넌트로 모델링된다. 이 때 첫번째 콤포넌트는 PoS태깅을 실행하고 두번째 콤포넌트에서 PoS태깅 결과를 자질로 하여 의존관계 트리를 예측하게 된다.

이 때 DRAGNN의 진정한 가치는 상위 콤포넌트가 하위 콤포넌트의 표현을 학습할 수 있다는데 있다. 하나의 콤포넌트가 입력데이터를 순환(iteration)할 때 중간결과가 tf.TensorArray 오프젝트에 저장되고 하위 콤포넌트가 이를 읽는다. 그리고 학습단계에서 하위 콤포넌트에서 상위 컴포넌터로 오류가 역전파되어 상위 파라미터값을 튜닝할 수 있으며 이를 stack propagation 이라고 부른다. 

콤포넌트의 설정사항은 다음과 같다.

- A global C++ backend configuration
- Task-specific configuration for the Transition System
- 자질추출 설정 (Configuration for feature extraction)
- Configuration for dynamic edge creation
- Configuration for the Python GraphBuilder to construct the network cell inside a tf.while_loop.





##DRAGNN 입력(feed)

DRAGNN 그래프는 문자열을 입력받아서 이를 어떻게 해석할지 결정한다. 

 