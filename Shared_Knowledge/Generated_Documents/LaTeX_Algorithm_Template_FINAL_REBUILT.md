# LaTeX 算法排版完整模板

> 状态：历史重建版。根据过去关于 Overleaf 算法环境不显示、重复加载宏包和中文排版的讨论重建。

## 1. 推荐完整导言区

```latex
\documentclass[UTF8]{article}

% 中文支持
\usepackage[UTF8]{ctex}

% 数学
\usepackage{amsmath,amssymb}
\allowdisplaybreaks

% 页面
\usepackage{geometry}
\geometry{margin=1in}

% 图片和浮动体
\usepackage{graphicx}
\usepackage{float}
\usepackage{caption}

% 表格
\usepackage{booktabs}
\usepackage{tabularx}
\usepackage{array}

% 颜色
\usepackage{xcolor}

% 算法环境
\usepackage{algorithm}
\usepackage[noend]{algpseudocode}

% 强制算法放在当前位置
\floatplacement{algorithm}{H}
```

关键原则：

- `ctex` 只加载一次。
- `algpseudocode` 只加载一次。
- 不同时混用多个定义相同算法命令的包。
- 一般不需要同时加载 `algcompatible`。
- 算法浮动体使用 `algorithm`，伪代码命令使用 `algpseudocode`。

## 2. 最小可编译算法示例

```latex
\documentclass[UTF8]{article}
\usepackage[UTF8]{ctex}
\usepackage{amsmath,amssymb}
\usepackage{float}
\usepackage{algorithm}
\usepackage[noend]{algpseudocode}

\floatplacement{algorithm}{H}

\begin{document}

\begin{algorithm}[H]
\caption{基于规则发现的奖励塑形流程}
\label{alg:rule-discovery-shaping}
\begin{algorithmic}[1]
\Require 环境 $\mathcal{E}$，初始策略 $\pi_0$，训练轮数 $T$
\Ensure 最终策略 $\pi_T$ 与规则集合 $\mathcal{R}$

\State 使用 $\pi_0$ 与环境交互，收集轨迹集合 $\mathcal{D}$
\State $\mathcal{R} \gets \Call{DiscoverRules}{\mathcal{D}}$
\State $\mathcal{R} \gets \Call{FilterRules}{\mathcal{R}}$

\For{$t = 1$ to $T$}
    \State 获取当前状态 $s_t$
    \State 根据策略选择动作 $a_t \sim \pi_{t-1}(\cdot \mid s_t)$
    \State 执行动作并获得环境奖励 $r_t^{env}$
    \State $r_t^{rule} \gets \Call{RuleReward}{s_t, a_t, \mathcal{R}}$
    \State $r_t \gets r_t^{env} + \lambda r_t^{rule}$
    \State 使用 $(s_t, a_t, r_t)$ 更新策略 $\pi_t$
\EndFor

\State \Return $\pi_T, \mathcal{R}$
\end{algorithmic}
\end{algorithm}

\end{document}
```

## 3. 常用命令

```latex
\State
\If ... \EndIf
\For ... \EndFor
\While ... \EndWhile
\Require
\Ensure
\Return
\Call{FunctionName}{arguments}
```

即使使用 `[noend]`，源码中仍建议保留 `\EndIf`、`\EndFor` 和 `\EndWhile`，只是最终排版不显示结束文字。

## 4. 常见不显示原因

### 重复加载

下面这种写法容易造成冲突：

```latex
\usepackage[noend]{algpseudocode}
\usepackage{algcompatible}
\usepackage[noend]{algpseudocode}
```

应该删到只保留：

```latex
\usepackage{algorithm}
\usepackage[noend]{algpseudocode}
```

### 环境不配对

正确嵌套：

```latex
\begin{algorithm}
  \begin{algorithmic}
  ...
  \end{algorithmic}
\end{algorithm}
```

### 使用了错误命令大小写

`algpseudocode` 使用：

```latex
\State
\If
\For
\While
```

不是旧包中的全大写命令。

### 浮动位置

需要固定位置时：

```latex
\begin{algorithm}[H]
```

并确保加载：

```latex
\usepackage{float}
```

## 5. 调试顺序

1. 先复制最小可编译示例。
2. 确认算法能显示。
3. 再逐个加入原论文宏包。
4. 每加入一组就编译一次。
5. 出现冲突时检查重复包、同名环境和命令重定义。

不要一开始保留几十个重复宏包再猜是哪一个导致问题。