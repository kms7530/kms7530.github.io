---
title: Natural Language Processing
date: 2023-03-29T10:44:00Z
draft: false
katex: true
---

# Natural Language Processing

- NLP는 기계가 사람의 언어를 알아듣고 의미 파악을 하는 것을 말한다.
- 분야별로 작업이 나누어져 있는데, 다음과 같이 나뉜다. 보면 된다.
    - Low-level parsing
        - 이는 문장을 단위별로 쪼게는 것을 말한다.
        - Tokenization: 문장을 단어 단위로 쪼개는것이다.
        - Stemming: 어미를 제거한 어근만 추출하는것이다.
    - Word and pharase level
        - Named Entity Recognition(NER): 단순한 단어별 인식이 아닌 고유명사를 인식하는것이다.
        - Part-of-Speech(POS) tagging: 문장 내의 단어를 인식하는것이다.
    - Sentence level
        - 감정분석
        - 기계번역
    - Multi-sentence and paragraph level
        - Entailment prediction: 문장간의 모순 혹은 포함관계를 예측하는 것이다.
        - Question answering: 문서의 독해를 통해 답변하는것이다.
        - Dialog systems: 흔히 chatbot과 같은 것을 말한다.
- 유명한 학회로는 ACL, EMNLP. NAACL이 있다.

# Text mining

- Text 혹은 document에서 유용한 정보를 뽑아내는것을 말한다.
- 이렇게 뽑힌 정보를 이용하여 어떠한 특정 문서들을 군집화 시키거나 사회과학과 연관지어 사용하게 된다.
- 대표적인 학회로 KDD, The WebConf, WSDM, CIKM, ICWSM이 있다.

# 현재의 NLP 트렌드

- 초기에는 Word2Vec과 같은 word-embeding이 주류였다.
- 하지만 RNN군의 기술이 발전하게 되면서, 현재는 transformer model을 이용하는 쪽으로 발전되었다.
- 가장 좋은 예시가 기계번역 분야이다.
    - 이전에는 문장을 문법적으로 번역하였으나 RNN기반으로 바꾸면서 성능이 월등히 좋아져 딥러닝을 사용하게 되었다.
    - 이후 transformer를 기계번역에 사용하게 위해 만들었으며 transformer가 주를 이루게 된다.
- 최근에는 transformer를 이용하여 기본적인 module을 쌓는 방식으로 진행하게 되었고, 방대한 양의 데이터를 통해 self-supervised learning을 하게 되었다.