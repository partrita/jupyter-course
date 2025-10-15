# 요약


## 긴 노트북에 대한 권장 사항

### 상단에 목차 만들기

마크다운을 사용하여 이 작업을 수행할 수 있습니다. 이것은 긴 노트북에 대한 멋진 개요를 생성합니다.
예: <https://stackoverflow.com/a/39817243>


### Jupyter Book

- <https://jupyterbook.org>
- 인용 및 상호 참조가 가능합니다.
- 셀 가시성을 토글할 수 있습니다.
- ... 그리고 훨씬 더 많습니다.

---

## 토론 포인트

```{discussion} 사용 사례 및 재현성
- 이미 Jupyter를 사용하고 있다면 어떤 작업에 사용하십니까?
- Jupyter를 처음 사용하는 경우 가능한 사용 사례가 있습니까?
- Jupyter 노트북이 재현 불가능한 결과 문제를 해결하는 데 도움이 될 수 있다고 생각하십니까?
```

```{discussion} Jupyter 노트북은 "워크플로우"인가요?
- 재현 가능한 워크플로우는 "파이프라인"을 문서화합니다.
- 또한 Jupyter 노트북은 데이터 처리 및 시각화 파이프라인이 될 수 있습니다.
- Jupyter 노트북은 "워크플로우"인가요?
```

---

## 흥미로운 블로그 게시물 및 기사

- [A. Guzharina, "Github에서 10,000,000개의 Jupyter 노트북을 다운로드했습니다 – 이것이 우리가 배운 것입니다"](https://blog.jetbrains.com/datalore/2020/12/17/we-downloaded-10-000-000-jupyter-notebooks-from-github-this-is-what-we-learned/)
- [J. M. Perkel, "반응성, 재현성, 협업: 계산 노트북의 진화", Nature 593, 156-157 (2021)](https://doi.org/10.1038/d41586-021-01174-w)

---

## 다른 언어를 위한 유사한 도구

- R: [R Shiny](https://shiny.rstudio.com/gallery/), [R Markdown](https://rmarkdown.rstudio.com/)
- JavaScript: [Observable](https://observablehq.com/)
- Julia: [Pluto](https://github.com/fonsp/Pluto.jl)

---

## 반복적인 코드 피하기

**모든 것은 짧고 간단한 노트북으로 시작되었습니다** 하지만 프로젝트와 노트북이 커짐에 따라 어떻게 구성해야 할까요?

육각형 2D 히스토그램 플롯을 위해 이 `fancy_plot` 함수를 작성했다고 상상해 봅시다(노트북에서 시도해 보십시오).
```{code-block} python
---
emphasize-lines: 7-12
---
import seaborn as sns

# 임의의 숫자를 얻기 위해
from numpy.random import default_rng


# 이것은 간단하지만 매우 긴 것을 상상해 봅시다.
def fancy_plot(x_values, y_values, color):
    """
    멋진 플롯을 만드는 멋진 함수.
    """
    sns.jointplot(x=x_values, y=y_values, kind="hex", color=color)


rng = default_rng()

x_values = rng.standard_normal(500)
y_values = rng.standard_normal(500)

# 우리 함수 호출
fancy_plot(x_values, y_values, "#4cb391")


other_x_values = rng.standard_normal(500)
other_y_values = rng.standard_normal(500)

# 이번에는 다른 데이터로 우리 함수를 다시 호출
fancy_plot(other_x_values, other_y_values, "#fc9272")
```

이제 모든 노트북에 이 함수를 복제하지 않고 다른 5개의 노트북에서 이 함수를 사용하고 싶습니다(함수가 매우 길다고 상상해 보십시오).

노트북과 동일한 폴더에 `myplotfunctions.py`라는 파이썬 파일을 만드는 것이 유용할 수 있습니다(이름은 변경할 수 있음).
이 코드를 `myplotfunctions.py`에 넣습니다.
```{code-block} python
import seaborn as sns


# 이것은 간단하지만 매우 긴 것을 상상해 봅시다.
def fancy_plot(x_values, y_values, color):
    """
    멋진 플롯을 만드는 멋진 함수.
    """
    sns.jointplot(x=x_values, y=y_values, kind="hex", color=color)
```

이제 노트북을 단순화할 수 있습니다.
```{code-block} python
---
emphasize-lines: 4, 13, 20
---
# 임의의 숫자를 얻기 위해
from numpy.random import default_rng

from myplotfunctions import fancy_plot


rng = default_rng()

x_values = rng.standard_normal(500)
y_values = rng.standard_normal(500)

# 우리 함수 호출
fancy_plot(x_values, y_values, "#4cb391")


other_x_values = rng.standard_normal(500)
other_y_values = rng.standard_normal(500)

# 이번에는 다른 데이터로 우리 함수를 다시 호출
fancy_plot(other_x_values, other_y_values, "#fc9272")
```