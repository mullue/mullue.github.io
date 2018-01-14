---
layout: post
title: 프레임시맨틱(Frame Semantics)과 프레임넷(FrameNet)
categories: NLP
---

### 프레임 시맨틱 (Frame Semantics)

참조 : [위키피디아](https://en.wikipedia.org/wiki/Frame_semantics_(linguistics))

언어적 의미(linguistic semantics)를 사전식 지식(encyclopedic knowledge)으로 연결시키는 이론
예를 들어 팔다(sell)라는 언어는 구매자, 판매자, 상품, 금액 등 맥락없이는 이해할 수 없다. 이런 맥락을 프레임으로 구성하여 해석하려는 시도이다.
최초에는 lexeme(의미의 최소단위)로만 적용하였으나 현재에는 핵심 의미분석요소로 문법구조(grammatical construction), 보다 복잡한 언어단위(linguistic units), 구성문법(construction grammer) 등에 적용되고 있다.

<br>
### 프레임넷 (FrameNet)

참조 : [위키피디아](https://en.wikipedia.org/wiki/FrameNet)

Frame Semantics 이론에 기반한 지식프레임 자산을 확보하고자 하는 프로젝트
예) "John sold a car to Mary"와 "Mary bought a car from John"이 동일한 내용임을 이해한다. 
이벤트와 관계, 참여자와 대상 등의 의미적 구조를 설명하는 프레임으로 이해할 수 있다.
FrameNet 어휘 DB는 1,200개의 Semantic Frame과 13,000 개의 lexical unit (단어와 의미의 매핑, 동음이의어는 여러개의 lexical unit과 대응됨), 20만개의 샘플문장으로 구성된다. 
1997년 프레임시맨틱 이론을 개발하고 초기 프로젝트 리더였던 Charles J. Fillmore에 의해 주로 만들어졌다. 
자동화된 Semantic Role Labeling을 가능하게 할 것으로 기대하면서 자연어처리와 언어학 모두에 영향을 주고 있다. 

<br>
#### Frames

하나의 상황을 의미적으로 표현하는 것. 참가자(participants), props(소품?) 등 개념적 역할을 포함한다. 
- Frame 이름 사례 : Being_born and Locative_relation (정의와 역할을 설명)

<br>
#### Frame elements(FE)

문장의 의미구조에 부가적인 정보를 제공한다. 
- 예1 : Being_born의 core element는 Child, non-core element는 Time, Place, Relatives 등임.
- 예2 : Commerce_goods-transfer의 core element는 Seller, Buyer, Goods 등이고 non-core element는 Place, Purpose 등임.
- 문장사례 : <u>She</u>**[Child FE]** was born <u>about AD 460</u>**[Time FE]**

<br>
#### Lexical units(LU)

Lexical Units는 문장내에서 품사와 함께 특정 프레임으로 생성될 수 있는 레마(Lemma)이다. 
문장에서 LU가 고유할 경우 특정 프레임으로 연결될 수 있다. 
- 예 : Complaining이라는 frame은 동사 complain, grouse, lament 등의 Lexical units로부터 생성(evoke)될 수 있다.

<br>
#### Example sentences

예1) Being_born
> She was born about AD 460
>

- Frame : Being_born
- She : Child
- about AD 460 : Time  

예2) Revenge 의 FE
- Frame Definition: Because of some injury to something-or-someone important to an avenger (maybe himself), the avenger inflicts a punishment on the offender. The offender is the person responsible for the injury. 
- FE List: 
  + avenger
  + offender
  + injury
  + injured_party
  + punishment.

프레임 REVENGE는 프레임엘리먼트(FE)로 Avenger, Punishment, Offender, Injury, Injured party 를 가짐  
REVENGE는 다음의 Lexical Unit으로부터 evoke됨
- avenge.v, avenger.n, get even.v, retaliate.v, retaliation.n, retribution.n, retributive.a, retributory.a, revenge.v, revenge.n, revengeful.a, revenger.n, vengeance.n, vengeful.a, and vin-dictive.a.

<br>
#### Valences

별도 링크 참조 - https://en.wikipedia.org/wiki/Valency_(linguistics)  
술어(predicate)에 의해 통제되는 요소(arguments)의 수 

Statistics on the valences of the frames, that is the number and the position of the frame elements within example sentences.
sample : 
https://framenet2.icsi.berkeley.edu/fnReports/data/lu/lu9791.xml

<br>
#### 다른 프레임과의 관계
- Inheritance: 보다 구체적인 자식 프레임과 추상적인 부모 프레임과의 관계. 부모 프레임으로 True인 사실은 상속되고 부모, 자식 프레임요소는 매핑된다. 
- Perspectivized_in: 중립적 프레임과 구체적인 프레임의 관계(?)
- perspective of the same scenario (e.g. the Commerce_sell frame, which assumes the perspective of the seller or the Commerce_buy frame, which assumes the perspective of the buyer)
- Subframe: Some frames like the Criminal_process frame refer to complex scenarios that consist of several individual states or events that can be described by separate frames like Arrest, Trial, and so on.
- Precedes: The Precedes relation captures a temporal order that holds between subframes of a complex scenario.
- Causative_of and Inchoative_of: There is a fairly systematic relationship between stative descriptions (e.g. the Position_on_a_scale frame, "She had a high salary") and causative descriptions (Cause_change_of_scalar_position, "She raised his salary") or inchoative descriptions (Change_position_on_a_scale, e.g. "Her salary increased").
- Using: A relationship that holds between a frame that in some way involves another frame. For instance, the Judgment_communication frame uses both the Judgment frame and the Statement frame, but does not inherit from either of them because there is no clear correspondence of the frame elements.
- See_also: Connects frames that bear some resemblance but need to be distinguished carefully.

