# 手撕代码与 Python：历史生成文档归档

## 权威成品

| 历史交付主题 | 权威文件 | 状态 |
|---|---|---|
| Quickselect、回文、链表、LRU + TTL 与输入输出 | `../Algorithm_and_Data_Structure_Practice.md` | 权威成品 |
| 线程、进程、GIL、池、asyncio、Task、Future、Promise | `../Python_Concurrency_Async_and_IO.md` | 权威成品 |

## 历史请求来源

用户曾明确要求：

- 把算法题的思考、错误和正确实现整理下来。
- 给出 LRU Cache + TTL 的完整设计、测试和标准输入解析。
- 通过真实服务场景解释多线程、多进程和 asyncio 的组合。
- 解释 Python 异步与 TypeScript Promise 的关系。

## 成品边界

当前两份权威文档保存的是完整学习总结和工程解释，但不是每一次对话中代码片段的逐字版本。

历史中反复修改过的具体代码，如果需要保留最终可运行版本，应在后续单独归档，例如：

```text
LRU_TTL_Executable_FINAL.py
Linked_List_Reversal_Practice_FINAL.py
Quickselect_Practice_FINAL.py
```

当前仓库主要以 Markdown 为主，暂未把每段历史代码独立保存为源码文件。

## 尚未完成

- 所有算法题的最终可执行源码归档。
- 每道题对应的输入样例与自动测试。
- 用户历次错误版本与修正 diff。