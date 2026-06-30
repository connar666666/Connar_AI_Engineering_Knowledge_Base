# Engineering：Windows 硬件与内存升级记录

## 1. 当前电脑配置

型号：

```text
ASUSPRO P3540FB_PX574FB
```

CPU：

```text
Intel Core i5-8265U
```

内存：

```text
4GB Samsung DDR4-2400
4GB SK Hynix DDR4-2667
总计 8GB
```

硬盘：

```text
WDC PC SN520 256GB NVMe SSD
ST1000LM035 1TB HDD
```

## 2. 当前瓶颈判断

任务管理器中 CPU 占用不高，但内存占用达到 80% 以上，说明当前主要瓶颈是 RAM，而不是 CPU。

8GB 内存在现代 Windows + 浏览器 + 微信 + VS Code + WSL 的场景下明显偏小。

## 3. 为什么 Chrome / Edge 会有很多进程

Chrome 和 Edge 都是 Chromium 内核，采用多进程架构。

即使只开几个网页，也会有：

- Browser Process。
- Renderer Process。
- GPU Process。
- Network Process。
- Extension Process。
- Utility Process。
- Service Worker。

所以任务管理器中显示 Chrome(12)、Edge(16) 是正常的。

## 4. 推荐升级方案

当前电脑是两根 4GB 内存，两个插槽已满。

推荐：

```text
8GB + 8GB DDR4 SO-DIMM
总计 16GB
```

这是性价比最高的方案。

不优先推荐 32GB，因为 i5-8265U 最终会成为新的瓶颈。除非计划继续使用 3 年以上，或者经常跑 Docker、多虚拟机、本地 LLM，否则 16GB 更划算。

## 5. 购买建议

规格：

```text
DDR4 SO-DIMM
8GB x 2
2666 或 3200 均可
1.2V
```

推荐品牌：

- Samsung
- SK Hynix
- Micron / Crucial
- Kingston
- Lexar

拆机条可以考虑，但要满足：

- 原厂品牌。
- 支持退换。
- 有质保。
- 店铺可靠。
- 不买价格离谱的杂牌。

## 6. Windows 优化建议

优先处理：

- 任务管理器启动项。
- Edge/Chrome 二选一。
- 不用 WSL 时执行 `wsl --shutdown`。
- 关闭不必要的后台程序。
- 用 Autoruns 查看隐藏启动项。
- 用 WizTree 查看大文件。

不要依赖所谓“一键清理内存”软件。

