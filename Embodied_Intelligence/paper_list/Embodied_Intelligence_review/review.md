# Embodied Intelligence Review：World Model

> [阅读笔记 PDF](./Embodied_Intelligence_review.pdf)

## 1. 定义与范围

World models connect perception and decision-making in embodied intelligence by maintaining hidden state, anticipating consequences, comparing interventions, and adapting when execution departs from expectations.

世界模型通过维护世界状态、预测行为结果、比较不同干预的影响，并在执行偏离预期时自适应调整策略，连接感知与决策。

**面向具身智能的定义：** World Model 是根据交互历史和候选动作，预测任务相关环境变化的模型；其核心是“预测未来，并支持决策”，而非完整重建世界。

$$
\text{World Model} = \text{Predict Future} + \text{Support Decision}
$$

### 形式化描述

令观测、动作、任务信号与目标分别为 $o_t$、$a_t$、$r_t$ 与 $g$：

$$
h_t = (o_{1:t}, a_{1:t-1}, r_{1:t-1}, g)
$$

动作条件预测可表示为：

$$
p_\theta(z_{t+1:t+H}, r_{t:t+H-1} \mid h_t, a_{t:t+H-1})
$$

其中 $z$ 可以是 pixels、visual tokens、latent states、objects、geometry、physical variables、symbolic states 或 reward/value。**状态表示不等同于世界模型定义**；只要模型能够预测未来并支持决策，即可构成世界模型。

### 功能与相关模型

世界模型按功能定义，而非按网络结构定义。预测结果可用于：

- **Planning**：预测未来，辅助动作选择；
- **Policy learning**：利用 imagined trajectories 训练策略；
- **Simulation**：作为可 rollout 的模拟器；
- **Action generation**：将预测与动作生成结合。

| 相关模型 | 与 World Model 的关系 |
| --- | --- |
| **Learned Simulator** | $\text{Learned Simulator} \subset \text{World Model}$。WM 强调预测能力；Simulator 强调可重复 rollout。 |
| **World Action Model（WAM）** | 将未来预测与动作生成紧密结合，可采用 cascade、shared representation 或 joint generation。 |
| **Reward / Value Model** | 当其评价 predicted transitions、imagined trajectories 或 uncertainty 时，可视为 WM 的一部分；仅评价当前 $V(o_t)$ 时不属于。 |

## 2. 与 VLA、视频模型的边界

Vision–language–action (VLA) models map multimodal observations and instructions to robot actions, with the goal of broad task coverage and flexible instruction following. However, a direct observation-to-action mapping provides no explicit predictive mechanism for maintaining task-relevant state, anticipating delayed consequences, or comparing alternative interventions. These capabilities matter most in partially observed, contact-rich, and long-horizon environments, where an agent may need to reason about unobserved state, predict the consequences of candidate actions, and revise its behavior when execution departs from expectation.

VLA 将多模态观测与语言命令作为输入，并输出机器人动作，实现广泛的任务覆盖与灵活的人机交互；但纯粹的观测到动作映射不能显式预测世界，因此需要 WM 预测行为后果并辅助规划。

> World models represent how an environment evolves and mediate between perception and decision making.

| 模型 | 核心特征 | 成为 World Model 的条件 |
| --- | --- | --- |
| **VLA** | $\text{Vision} + \text{Language} \rightarrow \text{Action}$，本质上是 policy model | 未来状态预测参与 action generation、policy learning 或 candidate selection |
| **Video Generation Model** | 预测未来视觉观测 | 能反映动作影响，并服务决策；仅视觉逼真 $\neq$ World Model |

## 3. 能力阶梯

$$
\text{Plausible} \;\rightarrow\; \text{Controllable} \;\rightarrow\; \text{Actionable}
$$

| 层级 | 含义 |
| --- | --- |
| **Plausible** | 保留与任务相关的时间、几何或物理结构，预测合理的未来。 |
| **Controllable** | 进一步预测干预如何改变结构，即预测动作后果。 |
| **Actionable** | 将预测转化为规划、行动、学习、评估、验证、恢复或数据选择中的可度量收益。 |

世界模型的目标不是生成真实观测：

$$
\text{Generate realistic observations}
\;\neq\;
\text{World Model goal}
$$

而是：

$$
\boxed{\text{Predict future} + \text{Improve embodied decisions}}
$$

## 4. World Model 发展路线

**核心目标：** 学习环境动态规律，使 agent 能预测未来，并利用预测辅助决策。

$$
\text{Prediction} \;\rightarrow\; \text{Control} \;\rightarrow\; \text{Decision}
$$

### 4.1 Latent Dynamics Models（隐状态动力学模型）

**核心思想：** 不直接预测像素，而是学习低维状态。

$$
z_{t+1} = f(z_t, a_t)
$$

**解决问题：**

- 图像空间过高维；
- 决策只需要任务相关信息。

**代表能力：** `search`、`planning`、`policy optimization`

**局限：** latent state 缺少明确物理含义，空间结构较弱。

### 4.2 Visual / Structured Predictors（视觉与结构预测）

**核心思想：** 预测更具结构化的世界表示，如 visual feature、occupancy、3D/4D scene state。

$$
\mathrm{Scene}_t \;\rightarrow\; \mathrm{Scene}_{t+1}
$$

**关注：** 空间关系、物体持续存在、动态场景变化。

#### 与 3DGS 的关联

3DGS 可以作为场景状态的表示：

$$
\mathrm{Scene}_t \equiv G_t = \{\mathrm{Gaussian}_i\}
$$

相应地，世界模型可预测：

$$
G_t \;\rightarrow\; G_{t+1}
$$

### 4.3 Physics-aware Models（物理感知模型）

**核心思想：** 加入物理约束，不仅预测“会发生什么”，还理解“为什么发生”。

**建模变量：** motion（运动）、contact（接触）、force（力）、material response（材料响应）。

**目标：** 提升 `manipulation`、`interaction`、`physical reasoning`。

### 4.4 Action-conditioned / World Action Models

**核心思想：** 未来状态依赖于动作。

$$
p(s_{t+1} \mid s_t)
\;\rightarrow\;
p(s_{t+1} \mid s_t, a_t)
$$

**关键能力：** action consequence prediction，即“如果执行动作 $A$，会发生什么？”

**对应能力层：** $\boxed{\text{Controllable}}$

### 4.5 Decision-facing World Models

**核心思想：** 让预测进入机器人闭环。

$$
\text{Observe} \;\rightarrow\; \text{Predict} \;\rightarrow\; \text{Act} \;\rightarrow\; \text{Feedback}
$$

**应用：** `planning`、`policy learning`、`evaluation`、`verification`、`recovery`

**对应能力层：** $\boxed{\text{Actionable}}$

### 阶段总结

| 阶段 | 核心问题 | 表示 | 主要能力 |
| --- | --- | --- | --- |
| Latent Dynamics | 未来状态是什么？ | latent state | Prediction |
| Visual / Structured | 世界结构如何变化？ | feature / occupancy / 3D scene | Spatial understanding |
| Physics-aware | 为什么这样变化？ | motion / contact / force | Physical reasoning |
| Action-conditioned | 动作导致什么变化？ | state + action | Control |
| Decision-facing | 预测如何帮助任务？ | planning loop | Actionable |

## 5. Grounding–Improvement Framework

能力阶梯描述 WM “能做到什么”；Grounding–Improvement Framework 则从两个正交维度分析其基础与收益：

1. **Grounding**：预测基于什么，即模型学到了怎样的世界结构；
2. **Improvement**：预测如何提升系统。

二者构成 $3 \times 4$ Grounding–Improvement Matrix。

### Grounding：World Model 学到了什么？

| 维度 | 表示内容 | 核心问题 | 示例 |
| --- | --- | --- | --- |
| **Geometry** | object position、spatial relationship、3D/4D scene state | 世界在哪里？ | $\mathrm{Scene}_t \rightarrow \mathrm{Scene}_{t+1}$ |
| **Physics** | motion、contact、force、material response | 世界为什么这样变化？ | 预测符合物理约束的未来 |
| **Action** | 动作对世界的影响 | 如果我执行动作，会发生什么？ | $p(s_{t+1}\mid s_t) \rightarrow p(s_{t+1}\mid s_t,a_t)$ |

### Improvement：World Model 改善什么？

| 闭环 | 预测用途 |
| --- | --- |
| **Data** | 数据生成、数据筛选、场景扩充，获得 Better Data |
| **Reward** | 评价结果、奖励设计，获得 Better Reward |
| **Policy** | planning、action selection，获得 Better Policy |
| **Model** | 利用预测误差更新模型：$\text{Predict} \rightarrow \text{Feedback} \rightarrow \text{Update}$ |

### 3 × 4 Matrix

| Grounding \ Improvement | Data | Reward | Policy | Model |
| --- | --- | --- | --- | --- |
| **Geometry** | 空间数据 | 空间评价 | 几何规划 | 几何更新 |
| **Physics** | 物理数据 | 物理约束 | 物理控制 | 动力学更新 |
| **Action** | 动作数据 | 动作评价 | 动作规划 | 动作模型更新 |

## 6. 核心结论

$$
\text{Geometry} + \text{Physics} + \text{Action}
$$

$$
\text{Data} + \text{Reward} + \text{Policy} + \text{Model}
$$

$$
\boxed{\text{Prediction} \;\rightarrow\; \text{Decision} \;\rightarrow\; \text{Improvement}}
$$

World Model 不只是预测未来，而是通过预测提升 embodied agent 能力。
