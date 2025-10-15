# 첫 번째 계산 노트북

```{objectives}
- 분석용 노트북 시작하기.
- 실행 순서의 중요성에 대한 감각 익히기.
```

```{instructor-note}
- 5분 강의
- 20분 연습
```

## 계산적 서사 만들기

Jupyter 노트북에서 우리의 첫 번째 실제 계산적 서사를 만들어 봅시다
(Aalto Science IT의 [Python 및 R 데이터 분석 과정](https://github.com/AaltoScienceIT/python-r-data-analysis-course)에서 각색).

```{figure} img/pi_with_darts.png
:alt: 다트 던지기 그림
:width: 30%

파이를 계산하기 위해 다트 던지기.
```

당신이 무인도에 있고 파이를 계산하고 싶다고 상상해 보세요.
당신에게는 파이썬이 설치된 컴퓨터가 있지만 수학 라이브러리도 없고
위키피디아도 없습니다.

여기 한 가지 방법이 있습니다 - 정사각형 영역 내에 임의의 점을 생성하고
그 점들이 단위 원 안에 들어가는지 확인하여 "다트를 던지는 것"입니다.

---

## JupyterLab 노트북 실행하기

터미널에서 **먼저 새 노트북이 나타나게 할 폴더를 만들거나 해당 폴더로 이동하십시오**.

**새 폴더를 만들고 그곳으로 이동한 후**, JupyterLab을 실행하십시오:
```console
$ jupyter-lab
```

이렇게 하면 브라우저에서 JupyterLab이 열립니다. Python 3 타일을 클릭하십시오.

```{figure} img/launching-python.png
:alt: 브라우저에서 JupyterLab 열림
:width: 100%

브라우저에서 JupyterLab이 열렸습니다. Python 3 타일을 클릭하십시오.
```

JupyterLab을 열 브라우저를 선택하려면 다음을 사용하십시오:
```console
$ jupyter-lab --no-browser
```

---

(first-notebook)=

## 계산 노트북 예제

````{discussion} 힌트: JupyterLab 내에서 웹페이지 열기
이 웹페이지의 내용을 Jupyter 노트북에 복사하여 붙여넣고 싶다면,
이 페이지를 IFrame 내에서 여는 멋진 방법이 있습니다:
```python
from IPython.display import IFrame
IFrame(src="https://coderefinery.github.io/jupyter/first-notebook/", width='100%', height='500px')
```
````

````{exercise} 연습/시연: 몬테카를로 방법을 사용하여 파이 계산하기
이것은 20분짜리 연습으로 하거나 따라하기 시연으로 할 수 있습니다.

각 번호가 매겨진 항목은 새 셀이 됩니다. SHIFT+ENTER를 눌러 셀을 실행하고 아래에 새 셀을 만듭니다. 셀이 선택된 상태에서 ESCAPE를 눌러 명령 모드로 들어갑니다. 단축키 `M`과 `Y`를 사용하여 셀을 각각 마크다운과 코드로 변경합니다.

1. 새 노트북을 만들고, 이름을 지정하고, 제목(마크다운 셀)을 추가하십시오.
   ```markdown
   # 몬테카를로 방법을 사용하여 파이 계산하기
   ```

2. 새 셀(마크다운 셀)에 관련 공식을 문서화하십시오:
   ```markdown
   ## 관련 공식

   - 정사각형 넓이: $s = (2 r)^2$
   - 원 넓이: $c = \pi r^2$
   - $c/s = (\pi r^2) / (4 r^2) = \pi / 4$
   - $\pi = 4 * c/s$
   ```

3. 개념을 설명하기 위해 이미지(마크다운 셀)를 추가하십시오:
   ```markdown
   ## 개념을 시각화하기 위한 이미지

   ![Darts](https://raw.githubusercontent.com/coderefinery/jupyter/main/example/darts.svg)
   ```

4. 필요한 두 개의 모듈을 가져옵니다(코드 셀):
   ```python
   # 필요한 모듈 가져오기

   import random
   import matplotlib.pyplot as plt
   ```

5. 포인트 수 초기화(코드 셀):
   ```python
   # "던지기" 횟수 초기화

   num_points = 1000
   ```

6. "다트 던지기" (코드 셀):
   ```python
   # 여기서 "다트를 던지고" 맞춘 횟수를 셉니다

   points = []
   hits = 0
   for _ in range(num_points):
       x, y = random.random(), random.random()
       if x*x + y*y < 1.0:
           hits += 1
           points.append((x, y, "red"))
       else:
           points.append((x, y, "blue"))
   ```

7. 결과 플로팅(코드 셀):
   ```python
   # 포인트를 3개의 리스트로 압축 해제
   x, y, colors = zip(*points)

   # 그림 크기 정의
   fig, ax = plt.subplots()
   fig.set_size_inches(6.0, 6.0)

   # 결과 플로팅
   ax.scatter(x, y, c=colors)
   ```

8. 파이 추정치 계산(코드 셀):
   ```python
   # 추정치 계산 및 인쇄

   fraction = hits / num_points
   4 * fraction
   ```
````

노트북은 다음과 같습니다: <https://github.com/coderefinery/jupyter/blob/main/example/darts.ipynb>
(정적 버전, 나중에는 동적이고 수정 가능한 노트북을 공유하는 방법을 배울 것입니다).

```{instructor-note}
순서에 맞지 않는 실행 문제를 시연하고 이를 피하는 방법:
- 노트북 끝에 `num_points`를 재정의하는 셀 추가
- 그런 다음 파이 추정치를 계산하는 셀 실행
- 그런 다음 "모든 셀 실행" 시연
```

---

(other-kernels)=

## 다른 언어의 노트북

파이썬 이외의 다른 프로그래밍 언어에 Jupyter를 사용할 수 있습니다
([지원되는 커널 목록](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels)).  그러나
R 또는 Julia 코드를 작성하는 경우 커널을 설치하는 대신 이러한 언어에 최적화된
해당 노트북 솔루션을 사용하는 것이 좋습니다:
  - R용 [R Markdown](https://rmarkdown.rstudio.com/)
  - Julia용 [Pluto.jl](https://plutojl.org/)

---

## 토론

이것으로부터 무엇을 얻을 수 있습니까?

- 코드가 다른 모든 것과 분리되어 있으면, 당신은 단지
  숫자 하나나 플롯 하나를 당신의 상사/협력자에게 확인을 위해 보낼 수 있습니다.
- 노트북을 서사로 사용하면, 모든 것을 **일관된
  이야기**로 보낼 수 있습니다.
- 독자는 여전히 서론과 결론만 읽을 수도 있지만,
  원한다면 더 많은 것을 쉽게 보고 *직접 변경해 볼 수 있습니다*.

```{discussion} 어디에 주석을 추가해야 할까요?
**마크다운 셀**이나 코드 셀의 **코드 주석**으로 코드에 주석을 달 수 있습니다.

마크다운 셀에 주석을 다는 것의 장점은 무엇이며 코드 셀에 코드 주석을 작성하는 것의 장점은 무엇이라고 생각하십니까?
```

---

```{keypoints}
- 노트북은 대화형 계산 작업을 수행하는 직관적인 방법을 제공합니다.
- 테스트-코드-리팩터링 루프에서 빠른 피드백을 허용합니다.
- 셀은 어떤 순서로든 실행될 수 있으므로 순서에 맞지 않는 실행 버그에 주의하십시오!
```