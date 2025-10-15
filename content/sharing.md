# 노트북 공유

```{objectives}
- 동료 및 커뮤니티와 노트북을 공유하는 방법을 배웁니다.
```

```{instructor-note}
- 5분 강의
- 20분 연습
```

```{discussion} 뇌를 깨우세요: 코드를 공유한 적이 있습니까?
- 두 대의 컴퓨터 사이에서 자신에게?
- 웹페이지와 같은 대규모 청중에게?
- 동료와 함께 코딩?
```

---

## 노트북을 공유하는 다양한 방법

- [nbviewer](https://nbviewer.jupyter.org/)에 URL, GitHub 리포지토리 또는 사용자 이름, 또는 GIST ID를 입력하고 렌더링된 Jupyter 노트북을 볼 수 있습니다.
- Read the Docs는 [nbsphinx 패키지](https://nbsphinx.readthedocs.io/)를 통해 Jupyter 노트북을 렌더링할 수 있습니다.
- [Binder](https://mybinder.org/)는 GitHub 리포지토리를 기반으로 라이브 노트북을 만듭니다.
- [EGI 노트북](https://notebooks.egi.eu) (https://egi-notebooks.readthedocs.io 참조)
- [JupyterLab](https://github.com/jupyterlab/jupyterlab)은 Google 드라이브를 통해 노트북의 공유 및 공동 편집을 지원합니다. 최근에는 [공동 노트북 모델을 사용한 공유 편집](https://github.com/jupyterlab/jupyterlab/pull/10118) 지원도 추가했습니다.
- [JupyterLite](https://jupyterlite.readthedocs.io/en/latest/)는 브라우저에서 Jupyterlab 환경을 만들고 GitHub 페이지로 호스팅할 수 있습니다.
- [Notedown](https://github.com/aaren/notedown), [Jupinx](https://github.com/QuantEcon/sphinxcontrib-jupyter) 및 [DocOnce](https://github.com/hplgit/doconce)는 마크다운 또는 스핑크스 파일을 가져와 Jupyter 노트북을 생성할 수 있습니다.
- [Voilà](https://voila.readthedocs.io/en/stable/)를 사용하면 Jupyter 노트북을 대화형 대시보드로 변환할 수 있습니다.
- `jupyter nbconvert` 도구는 (`.ipynb`) 노트북 파일을 다음으로 변환할 수 있습니다.
    - 파이썬 코드 (`.py` 파일)
    - HTML 파일
    - LaTeX 파일
    - PDF 파일
    - 브라우저의 슬라이드 쇼

```{figure} img/JupyterDownload.png
:alt: Jupyter 노트북을 다양한 형식으로 내보낼 수 있습니다. 일부는 추가 설치가 필요할 수 있습니다.
:width: 100%

Jupyter 노트북을 다양한 형식으로 내보낼 수 있습니다. 일부는 추가 설치가 필요할 수 있습니다.
```


### 무료 플랜이 있는 상업용 서비스

이러한 플랫폼은 무료로 사용할 수 있지만 클라우드 리소스에 더 빠르게 액세스하려면 유료 구독이 있습니다.

- [CoCalc](https://cocalc.com/) (이전의 SageMathCloud)는 클라우드에서 노트북의 공동 편집을 허용합니다.
- Google의 [Colaboratory](https://colab.research.google.com/)를 사용하면 클라우드에서 노트북 작업을 할 수 있으며 [드라이브의 노트북 파일을 읽고 쓸 수 있습니다](https://colab.research.google.com/notebooks/io.ipynb).
- [Microsoft Azure Notebooks](https://notebooks.azure.com/)도 클라우드에서 무료 노트북을 제공합니다.
- [Deepnote](https://deepnote.com/)는 실시간 공동 작업을 허용합니다.

---

(reproducible-via-binder)=

## [Binder](https://mybinder.org)에서 동적 노트북 공유하기

`````{exercise} 연습 (20분): Binder를 통해 누구나 노트북을 재현할 수 있도록 만들기
  - 새 GitHub 리포지토리를 만들고 **"README 파일 추가"를 클릭합니다**: <https://github.com/new>

  - 이 연습은 GitHub 웹 인터페이스를 통해 전적으로 수행할 수 있습니다(물론 터미널을 사용하는 것도 괜찮습니다). "파일 추가" 버튼을 사용하여 파일을 업로드할 수 있습니다.
    ```{figure} img/github-upload.png
    :alt: Binder 웹 인터페이스 스크린샷

    Binder 웹 인터페이스 스크린샷.
    ```

  ````{tabs}
    ```{group-tab} Jupyter 노트북
    - 이전에 만든 노트북을 이 리포지토리에 업로드합니다. 이전에 막혔다면 [이 노트북](https://raw.githubusercontent.com/coderefinery/jupyter/main/example/darts.ipynb)을 다운로드할 수 있습니다(마우스 오른쪽 버튼 클릭, "다른 이름으로 저장..."). 다른 노트북으로도 시도해 볼 수 있습니다.

    - 또한 다음을 포함하는 `requirements.txt` 파일을 추가합니다(노트북에 다른 종속성이 있는 경우 이를 조정하십시오).
      ```
      matplotlib==3.4.1
      ```
    ```
    ```{group-tab} R 마크다운/R 스튜디오 프로젝트
    이 연습은 Jupyter 노트북 대신 Rmd 파일을 사용하는 사람들을 위한 것입니다.

    - Rmd 파일을 이 GitHub 리포지토리에 업로드하거나 푸시합니다.

    - 사용하려는 R 버전을 지정하는 `runtime.txt` 파일을 추가합니다.
      ```
      r-3.6-2020-10-13
      ```

    - 예를 들어 종속성을 나열하는 `install.R` 파일을 추가합니다.
      ```
      install.packages(c("readr", "ggplot2"))
      ```

    - 그 후에 https://mybinder.org/v2/gh/**USER**/**REPOSITORY**/**BRANCH**?urlpath=rstudio 를 방문하십시오("USER", "REPOSITORY" 및 "BRANCH"를 조정하십시오).

    - 자세한 내용은 [이 가이드](https://github.com/alan-turing-institute/the-turing-way/blob/master/workshops/boost-research-reproducibility-binder/workshop-presentations/zero-to-binder-r.md)를 참조하십시오.
    ```
  ````

  - [https://mybinder.org](https://mybinder.org) 방문:
    ```{figure} img/binder.jpg
    :alt: Binder 웹 인터페이스 스크린샷

    Binder 웹 인터페이스 스크린샷.
    ```
  - mybinder 배지에 대한 마크다운 텍스트를 노트북 리포지토리의 README.md 파일에 복사하여 붙여넣습니다.

  - 노트북 리포지토리에 이제 GitHub의 `README.md` 파일에 "launch binder" 배지가 있는지 확인합니다.

  - 버튼을 클릭하고 리포지토리가 Binder에서 실행되는 것을 확인하십시오(1~2분 정도 걸릴 수 있습니다). 이제 노트북을 클라우드에서 탐색하고 실행할 수 있습니다.

  - 완전히 재현 가능해진 것을 즐기십시오! 더 좋은 방법은 노트북에 DOI를 받고 Binder가 DOI를 가리키도록 하는 것입니다.
`````

```{keypoints} Binder를 사용한 더 많은 예제:
- [Binder 문서](https://mybinder.readthedocs.io/en/latest/introduction.html)
- [예제 리포지토리 모음](https://github.com/binder-examples)
```

---

## 선택적 연습

(without-requirements)=

### 종속성 추적의 중요성

````{exercise} (선택 사항) 연습: requirements.txt가 없으면 어떻게 될까요?
동일한 [활동 불평등 리포지토리](https://github.com/timalthoff/activityinequality)를 살펴보겠습니다.

- [이 링크](https://mybinder.org/v2/gh/timalthoff/activityinequality/master)를 사용하여 Binder에서 리포지토리를 시작합니다.
- `fig3/fig3bc.ipynb`는 파이썬 노트북이므로 Binder에서 작동합니다. 대부분의 다른 노트북은 R로 되어 있으며, 이 역시 Binder에서 작동합니다. [하지만 어떻게?](https://mybinder.readthedocs.io/en/latest/howto/languages.html)
- 노트북을 실행해 보십시오. 어떻게 되나요?
- 대부분의 경우 첫 번째 셀에서 즉시 실행이 중단됩니다.
  ```python
  %matplotlib inline
  import pandas as pd
  import matplotlib.pyplot as plt
  import seaborn as sns
  sns.set(style="whitegrid")
  from itertools import cycle
  ```
  `ModuleNotFoundError` 메시지의 긴 목록이 표시됩니다. 이는 필요한 파이썬 패키지가 설치되지 않았고 가져올 수 없기 때문입니다. 누락된 패키지에는 최소한 오류 메시지에 언급된 `pandas` 및 `matplotlib`이 포함됩니다.
- 누락된 요구 사항을 설치하려면 노트북 시작 부분에 내용이 포함된 새 코드 셀을 추가하십시오.
  ```
  !python3 -m pip install pandas matplotlib
  ```
  그리고 노트북을 다시 실행하십시오. 이제 어떻게 되나요?
- 다시, 누락된 패키지로 인해 실행이 중단됩니다. 이번에는 `seaborn` 패키지가 원인입니다. 첫 번째 셀을 수정하여 다음으로 설치하십시오.
  ```
  !python3 -m pip install pandas matplotlib seaborn
  ```
  그리고 노트북을 세 번째로 실행해 보십시오. 마침내 작동합니까? 개발자가 다르게 할 수 있었던 것은 무엇입니까?
- 노트북을 더 유용하게 만드는 좋은 방법은 노트북을 실행하는 데 필요한 패키지가 포함된 `requirements.txt` 파일을 만들고 리포지토리의 노트북 옆에 추가하는 것입니다.
- 이 경우 `requirements.txt`는 다음과 같을 수 있습니다.
  ```
  pandas
  matplotlib
  seaborn
  ```
  그리고 패키지가 설치되었는지 확인하기 위해 원래 노트북 시작 부분에 다음 줄이 포함된 코드 셀을 추가할 수 있습니다.
  ```
  !python3 -m pip install -r requirements.txt
  ```
  노트북이 몇 달 후에도 계속 작동하도록 하려면 `requirements.txt` 파일에 버전도 지정하는 것이 좋습니다.
````

(share-widget)=

### Binder에서 대화형 노트북 공유

````{exercise} (선택 사항) 연습: Binder를 통해 대화형(ipywidgets) 노트북 공유
- {doc}`examples` 에피소드의 "대화형 데이터 피팅을 위한 위젯" 연습의 솔루션을 가져와 노트북에 붙여넣습니다.
- 노트북을 GitHub/GitLab 리포지토리로 푸시합니다.
- 노트북 리포지토리에 `requirements.txt` 파일을 만듭니다. 예:
  ```
  ipywidgets==7.4.2
  numpy==1.16.4
  matplotlib==3.1.0
  ```
- 위 연습과 동일한 방식으로 Binder를 통해 이 예제를 배포해 보십시오.
````