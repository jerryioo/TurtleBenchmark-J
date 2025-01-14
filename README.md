# 海龟 Benchmark

[English](./README_en.md)

海龟 Benchmark 是一个新颖的、无法作弊的基准测试，专注于评估 LLM 的逻辑推理和上下文理解能力。评测数据集全部来自几千名真实用户在[海龟汤](https://www.tanghenre.com)游戏中的输入数据。

初步测评结果见：[用 2 万条真人AI海龟汤数据评估大模型推理能力](https://mazzzystar.github.io/2024/08/09/turtle-benchmark-zh/)

### 特点

- **无需背景知识**：不依赖背景知识和模型记忆能力，可以从 200 字以内的故事中获得作出判断所需的全部信息，让模型评测专注于推理能力。
- **客观且无偏见**：衡量猜测的正确性，结果是客观的、和人的感受无关。
- **可量化的结果**：明确、可测量的结果（正确/错误/未知），便于比较。
- **无法作弊**：使用真实用户提出的问题，并且随着线上游戏的进行，新数据会动态产生，使得作弊变得不可能。

### 数据集

- 32 个独特的"海龟汤"故事。
- 1537 个来自用户问题的人工标注标签: 对(T)、错(F)、不相关(N)
- 我们的评估日志。

> [!IMPORTANT]  
> 我们在标注时发现：存在部分样本既可以标注为错(F)，也可以标注为不相关(N)，因此我们将**错**(F)和**不相关**(N)进行了合并、不做区分，在计算准确率时也在代码中做了合并——这会降低难度，因此我们可能在未来，将类别标注重新变为三类：对、错、不相关，并在 [4448](https://github.com/mazzzystar/TurtleBenchmark/blob/dev/evaluation/chinese/data/sorted_cases.list) 条样本上重新标注、测试模型表现。

### 使用方法

```bash
cd evaluation

mv .env.example .env
# 添加API密钥。

# Evaluate Chinese or English.
cd chinese

# zero-shot，评估更快更省钱，默认 2-shot
python evaluate.py --shot 0
```

### 结果

#### 1. 总体准确率

每个模型在所有测试案例中的总体准确率。

![总体基准测试结果](/evaluation/chinese/imgs/Turtle-Benchmark-result.png)

#### 2. 故事的平均准确率

为了减轻模型在某个具有大量测试样本的故事上表现不佳带来的偏差，我们分别计算了每个模型在所有 32 个故事中的准确率，并除以 32。

![32个故事的结果](/evaluation/chinese/imgs/Turtle-Benchmark-over-32stories.png)

#### 3. 性能图表

这个散点图比较了 2-shot 学习场景中每个模型的总体准确率（x 轴）和平均故事准确率（y 轴）。

![2-Shot学习性能](/evaluation/chinese/imgs/average_model_accuracy_over_stories_2-shot.png)

### 评测

根据这些结果，我们可以清楚地看到各种模型之间的性能差异：

1. **第一梯队**：Claude 3.5 Sonnet 作为无可争议的领导者脱颖而出，明显优于所有其他模型。

2. **第二梯队**：GPT-4o、Qwen-2（通义千问）、Moonshot AI（月之暗面）、LLama3.1 405B 和 Minimax 构成第二梯队。虽然我们避免了进一步的细分，但在这个组内，按照所列顺序，性能明显下降。

3. **第三梯队**：豆包（Douban）、DeepSeek 和 LLama3.1 70B 构成第三梯队。

4. **第四梯队**：GPT-4o-mini 独自位于第四梯队。

5. **过时**：GPT-3.5 的性能表明它在这个背景下不再具有竞争力。

需要注意的是，这项评估只针对**模型的中文语言理解和推理能力**。未来，视资源和资金情况，我们计划将所有故事和测试问题翻译成英文，并使用英文提示重新运行测试。这将有助于消除可能归因于语言差异的任何性能差异。

## TODO

- [ ] 将标注类别重新改为三类：T/F/N，并在[4448](https://github.com/mazzzystar/TurtleBenchmark/blob/dev/evaluation/chinese/data/sorted_cases.list) 条样本上重新标注、测试模型表现。
- [x] 将数据集和测试样例翻译成英文。
- [ ] 人工逐条二次确认翻译后的标注准确率。
- [ ] 使用英语 prompt，在英文模型上评测并给出结果。

### 致谢

衷心感谢：

- 五源资本 的**石允丰**（Steven Shi）为这项研究所需的 token 提供慷慨的财务支持。
- 实习生**赵乾之**（Jerry Zhao）和我一起手工标注了 26,000 条数据。
