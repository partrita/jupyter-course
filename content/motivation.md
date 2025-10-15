# Jupyter 노트북

```{objectives}
- Jupyter의 목적에 대한 아이디어를 얻습니다.
- 영감을 주는 Jupyter 노트북 몇 가지를 봅니다.
```

```{instructor-note}
- 10분 강의
- 0분 연습
```

## Jupyter 노트북의 동기

Jupyter 노트북은 라이브 코드, 방정식, 시각화 및 서술 텍스트를 포함하는 문서를 만들고 공유하기 위한 도구입니다.

```{figure} img/‎Jupyter_Pandas_demo.png
:alt: 순수 파이썬 코드와 Jupyter 노트북의 비교는 Jupyter 노트북이 일반 코드 파일보다 훨씬 더 정교한 서사를 포함할 수 있음을 강조합니다.
:width: 100%

Jupyter 노트북은 일반 코드 이상의 것을 포함할 수 있습니다. (CC-BY).
```

- **코드, 텍스트, 방정식, 그림, 플롯** 등이 혼합되어 *계산적 서사*를 만듭니다.
- [*"사용자가 코드를 실행하고, 무슨 일이 일어나는지 보고, 연구원과 데이터 간의 일종의 반복적인 대화에서 수정하고 반복하는 환경"*](https://www.nature.com/articles/d41586-018-07196-1)
- "Jupyter"라는 이름은 Julia+Python+R에서 유래했지만, 오늘날 Jupyter 커널은 [수십 개의 프로그래밍 언어](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels)에 대해 존재합니다.


### Jupyter 노트북과 유사한 기능은 널리 사용됩니다.

많은 도구에서 코드 및 마크다운 셀과 같은 Jupyter와 유사한 동작을 가질 수 있습니다.
- VSCode
- Google Colab
- GitHub Codespaces

---

## 사례 연구

Jupyter 노트북을 사용하면 코드를 공유하는 것이 가능해집니다. 아이디어를 설명하고 누구나 [Binder](https://mybinder.org/)와 같은 환경에서 코드를 실행할 수 있습니다.

### [중력파 발견](https://www.gw-openscience.org/about/)

```{figure} img/gravity.jpg
:alt: 중력파 발견
:width: 50%

중력파 발견.
```

사례 연구로, 중력파 발견과 함께 발표된 분석을 살펴보겠습니다. [이 페이지](https://losc.ligo.org/tutorials/)에는 사용 가능한 분석 목록이 있으며 이를 탐색할 수 있는 몇 가지 옵션이 제공됩니다.

- 짧은 데이터 세그먼트에 대한 간략한 보기는 [https://github.com/losc-tutorial/quickview](https://github.com/losc-tutorial/quickview)에서 찾을 수 있습니다.
- "Binder 실행" 버튼을 클릭하여 Binder를 사용하여 노트북을 열고 대화식으로 탐색할 수 있습니다.
- Binder 인스턴스는 어떤 파이썬 패키지를 로드해야 하는지 어떻게 알 수 있습니까? `requirements.txt`(파이썬) 또는 `environment.yml` 또는 `runtime.txt`(R)와 같은 파일에서 정보를 가져옵니다.


### [활동 불평등](http://activityinequality.stanford.edu/) 연구

```{figure} img/activity_inequality.png
:alt: 스탠포드 활동 불평등 연구
:width: 80%

스탠포드 활동 불평등 연구.
```

스탠포드 활동 불평등 연구의 연구원들은 전 세계 여러 국가의 700,000명 이상의 사용자에 대한 휴대폰 추적 데이터에서 일일 활동을 측정했습니다.
- 모든 데이터와 노트북은 [https://github.com/timalthoff/activityinequality](https://github.com/timalthoff/activityinequality)에서 사용할 수 있습니다.
- "Binder 실행" 버튼이 없어도 노트북은 여전히 [Binder에서 실행](https://mybinder.org/v2/gh/timalthoff/activityinequality/master)할 수 있습니다(나중에 자세히 설명하겠지만 `runtime.txt` 파일이 없기 때문에 "R 커널 누락" 오류가 표시될 수 있습니다).
- 예를 들어 [fig3bc](https://github.com/timalthoff/activityinequality/blob/master/fig3/fig3bc.ipynb)를 재현하는 데 잠재적인 문제가 있습니까?


### 더 많은 예시

더 많은 영감을 얻으려면 [흥미로운 Jupyter 노트북 갤러리](https://github.com/jupyter/jupyter/wiki)로 이동하십시오.

---

## 사용 사례

- **선형 워크플로우**에 정말 좋습니다(예: 데이터 읽기, 데이터 필터링, 일부 통계 수행, 결과 플로팅).
- 새로운 아이디어 실험, 새로운 라이브러리/데이터베이스 테스트
- 코드, 데이터 분석 및 시각화를 위한 *대화형* 개발 환경으로
- HPC 클러스터에서의 대화형 작업
- 동료와 코드 공유 및 설명
- 교육(프로그래밍, 실험/이론 과학)
- 다른 노트북에서 배우기
- **디지털 실험실 노트북**과 같은 대화형 세션 추적
- **출판된 기사와 함께 보충 정보**
- [Reveal.js](https://github.com/damianavila/RISE)를 사용한 슬라이드 프레젠테이션


## 함정

- 비선형 코드 흐름이 있는 프로그램
- 대규모 코드베이스(그러나 Jupyter를 대규모 코드베이스에 대한 인터페이스로 사용하고 코드베이스를 모듈로 가져오는 것은 의미가 있을 수 있습니다).
- 텍스트 편집기에서 직접 노트북을 쉽게 작성할 수 없습니다([R Markdown](https://rmarkdown.rstudio.com/)으로는 가능합니다).
- 노트북은 **버전 제어**가 가능합니다([nbdime](https://nbdime.readthedocs.io/)이 도움이 되지만), **여전히 제한 사항이 있습니다**.
- 노트북은 **기본적으로 이름이 지정되지 않고** **관련 없는 것들을 많이** 가지는 경향이 있습니다. 정리에 주의하십시오!
- <https://scicomp.aalto.fi/scicomp/jupyter-pitfalls/>도 참조하십시오.


## 좋은 습관

- "Untitled.ipynb"에서 노트북 이름을 바꿉니다.
- 공유/저장하기 전에 모든 셀을 실행하여 컴퓨터에서 보는 결과가 셀이 순서대로 실행되지 않았기 때문이 아닌지 확인하십시오(나중에 시도해 볼 것입니다).