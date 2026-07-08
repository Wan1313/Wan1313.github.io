# MaintainPositionRelativeTo

**合格名称：** `manim.animation.updaters.update.MaintainPositionRelativeTo`

::: currentmodule
manim.animation.updaters.update
:::

::::: {.autoclass show-inheritance="" members="" private-members=""}
MaintainPositionRelativeTo

## 描述

`MaintainPositionRelativeTo` 是一个用于**保持相对位置**的动画类，它使一个 `Mobject` 在动画过程中始终相对于另一个 `Mobject` 保持固定的偏移量。该动画常用于：
- 实现对象的跟随效果（如光标跟随文本）；
- 在变换过程中保持附属物的相对位置不变；
- 构建复合对象时确保子部件相对父部件的位置一致。

该动画本质上是一个 `UpdateFromFunc` 的封装，但专门针对位置相对性进行了优化，并提供了明确的插值控制。

---

## 参数

| 参数 | 类型 | 说明 |
| :--- | :--- | :--- |
| `mobject` | `Mobject` | 要移动或更新的对象（随动对象）。 |
| `target` | `Mobject` | 参照对象（即保持相对位置的目标）。 |
| `offset` | `np.ndarray` 或 `list` | 相对于 `target` 的位移向量（三维坐标）。默认为 `[0, 0, 0]`（即与 `target` 位置重合）。 |
| `run_time` | `float` | 动画持续时间（秒）。默认为 `1.0`。 |
| `rate_func` | `Callable[[float], float]` | 速率函数，控制位置更新的节奏，但通常此动画会立即更新位置，因此影响不大。默认使用线性函数。 |
| `remover` | `bool` | 动画结束后是否移除 `mobject`，默认为 `False`。 |
| `suspend_mobject_updating` | `bool` | 是否暂停 `mobject` 的更新程序，默认为 `True`。 |
| `name` | `str` | 动画名称，默认为 `"MaintainPositionRelativeTo"`。 |

> **注意：** 其他继承自 `Animation` 的参数（如 `lag_ratio`）也接受，但通常不影响该动画的行为。

---

## 方法

::: {.autosummary nosignatures=""}
~MaintainPositionRelativeTo.interpolate_mobject
:::

### 方法详情

- **`interpolate_mobject(alpha)`**  
  核心更新方法，根据进度 `alpha` 更新 `mobject` 的位置。  
  该方法在每一帧被调用，通过计算当前 `target` 的位置加上 `offset` 来设置 `mobject` 的位置，从而实现相对位置的持续跟踪。  
  *参数：* `alpha` – 0 到 1 的浮点数（动画进度）。  
  *返回类型：* `None`

---

## 属性

::: autosummary
~MaintainPositionRelativeTo.run_time
:::

- **`run_time`**：动画的总时长（秒）。可通过 `set_run_time()` 修改。

---

## 示例

```python
from manim import *

class RelativePositionExample(Scene):
    def construct(self):
        dot = Dot(color=BLUE)
        square = Square(color=RED, side_length=0.5)
        square.move_to(dot.get_center() + RIGHT * 2)  # 初始偏移

        self.add(dot, square)
        # 使 square 始终保持在 dot 右侧 2 个单位
        self.play(
            MaintainPositionRelativeTo(square, dot, offset=RIGHT * 2, run_time=2),
            dot.animate.shift(UP * 3)  # dot 移动时 square 会跟随
        )
        self.wait(1)
