

背景
使用 prm 进行错误检测。现有工作使用基于 Z-scores + 全局阈值 (`y_mean - y_std`) 的判别方法。
把长 CoT 切成 section，让 PRM 给每个 section 打过程奖励分，然后把“异常低分”的 section 判为错误 section。不用固定阈值，而用 Z-score 异常检测判错。统计 reward 分布的均值和标准差，低于阈值的 section 被预测为错误 section。使用模型是否认为存在错误片段与是否存在过程错误标准相结合

使用 llm as a judge进行检测


我构建了评测数据集。收集自检索 self search rl（SSRL）中rollout数据，使用deepseek v4 flash进行标注是否错误，进行分析，验证三部分。标注10k数据，8/2分为训练/测试集。生成反思验证数据集。

---

## 1. PRM 模型 (Qwen2.5-Math-PRM-7B)

基于 Z-scores + 全局阈值 (`y_mean - y_std`) 的判别方法。


| 指标 | 值 |
|---|---|
| TP (True Positive) | 314 |
| FP (False Positive) | 20 |
| TN (True Negative) | 442 |
| FN (False Negative) | 1214 |
| Accuracy | 0.3799 |
| Precision | 0.9401 |
| Recall | 0.2055 |
| F1-Score | 0.3373 |



---

## 2. vLLM 模型评估 (各模型最佳运行)

prompt如下，写入附录
```
VERIFIER_PROMPT_TEMPLATE = """You are a professional quality evaluator for a Q&A system. Your task is to analyze an AI model's "think-retrieve-answer" process, determine if any errors exist in any part of the process, and analyse ALL errors throughout the entire process. If the model thinks it lacks knowledge, it will search using <search>query</search> and then produce its own generated information <information>information</information>, and use information above to answer, which means it does not retrieve from any external data source but answers by itself.

Input Format
You will receive the following information:

Question: The user's original question
Model Output: The model's complete think-retrieve-answer process
Ground Truth: The correct answer

Your Task
Please analyze according to the following steps:

Step 1: Review
Review model's "think-retrieve-answer" process, judging whether process is correct.

Step 2: Error Explanation
For each error, briefly explain why the specific step is wrong and what the correct approach/information should be. Since the model does not retrieve from any external data source but answers by itself, you should not mention using other source information in the feedback, but instead guide it to generate the correct reasoning, query, and corresponding correct information.

Output Format (Strict JSON)
For error samples, output:

{{
  "step_by_step_analysis": "review the 'think-retrieve-answer' process and explain your reasoning",
  "has_error": true,
  "error_description": "describe each error content in detail.",
  "reflection_feedback": "guide how to fix errors above"
}}

For correct samples, output:

{{
  "step_by_step_analysis": "review the 'think-retrieve-answer' process and explain your reasoning",
  "has_error": false,
  "verification": "Briefly explain why each step is correct"
}}

Now please analyze the following sample:

Question: {question}

Model Output: {model_output}

Ground Truth: {ground_truth}

Please output your final analysis result strictly in the JSON format specified above.
"""
---

## 最终累计结果（三次结果取均值）

  | 模型                 | Accuracy | Precision | Recall | F1-Score |
  |----------------------|----------|-----------|--------|----------|
  | Qwen2.5-3B-refletion | 0.8318   | 0.8880    | 0.8937 | 0.8908   |
  | Qwen2.5-3B-Instruct  | 0.8203   | 0.8632    | 0.9101 | 0.8860   |
  | qwen2.5-7b-instruct  | 0.7824   | 0.8010    | 0.9535 | 0.8706   |
  | Qwen2.5-3B-SSRL      | 0.6625   | 0.9235    | 0.6111 | 0.7355   |

## 每轮按 Data Source 统计（放入附录）

| iter | data_source | total | EM=1 current | EM=1 cumulative | EM=1 new | verifier current | verifier cumulative | verifier new |
|---:|---|---:|---:|---:|---:|---:|---:|---:|
| 0 | bamboogle | 125 | 42 | 42 | 42 | 18 | 18 | 18 |
| 0 | hotpotqa | 500 | 136 | 136 | 136 | 55 | 55 | 55 |
| 0 | musique | 500 | 50 | 50 | 50 | 10 | 10 | 10 |
| 0 | nq | 500 | 147 | 147 | 147 | 61 | 61 | 61 |
| 0 | popqa | 500 | 171 | 171 | 171 | 10 | 10 | 10 |
| 1 | bamboogle | 125 | 97 | 101 | 59 | 58 | 59 | 41 |
| 1 | hotpotqa | 500 | 354 | 364 | 228 | 206 | 222 | 167 |
| 1 | musique | 500 | 353 | 359 | 309 | 150 | 155 | 145 |
| 1 | nq | 500 | 377 | 392 | 245 | 213 | 224 | 163 |
| 1 | popqa | 500 | 432 | 447 | 276 | 152 | 156 | 146 |
| 2 | bamboogle | 125 | 87 | 107 | 6 | 62 | 73 | 14 |
| 2 | hotpotqa | 500 | 364 | 405 | 41 | 229 | 282 | 60 |
| 2 | musique | 500 | 349 | 412 | 53 | 175 | 210 | 55 |
| 2 | nq | 500 | 386 | 428 | 36 | 235 | 265 | 41 |
| 2 | popqa | 500 | 410 | 477 | 30 | 171 | 207 | 51 |
| 3 | bamboogle | 125 | 97 | 116 | 9 | 68 | 83 | 10 |
| 3 | hotpotqa | 500 | 376 | 424 | 19 | 250 | 316 | 34 |
| 3 | musique | 500 | 344 | 430 | 18 | 201 | 263 | 53 |
| 3 | nq | 500 | 390 | 440 | 12 | 236 | 285 | 20 |
| 3 | popqa | 500 | 430 | 487 | 10 | 173 | 246 | 39 |
| 4 | bamboogle | 125 | 96 | 118 | 2 | 70 | 89 | 6 |
| 4 | hotpotqa | 500 | 380 | 432 | 8 | 274 | 336 | 20 |
| 4 | musique | 500 | 369 | 447 | 17 | 209 | 285 | 22 |
| 4 | nq | 500 | 389 | 446 | 6 | 254 | 302 | 17 |
| 4 | popqa | 500 | 429 | 489 | 2 | 185 | 277 | 31 |
| 5 | bamboogle | 125 | 99 | 120 | 2 | 72 | 92 | 3 |
| 5 | hotpotqa | 500 | 387 | 441 | 9 | 285 | 356 | 20 |
| 5 | musique | 500 | 348 | 452 | 5 | 217 | 308 | 23 |
| 5 | nq | 500 | 384 | 450 | 4 | 263 | 317 | 15 |
| 5 | popqa | 500 | 434 | 491 | 2 | 198 | 299 | 22 |
| 6 | bamboogle | 125 | 104 | 121 | 1 | 81 | 100 | 8 |
| 6 | hotpotqa | 500 | 378 | 443 | 2 | 286 | 368 | 12 |
| 6 | musique | 500 | 351 | 456 | 4 | 241 | 330 | 22 |
| 6 | nq | 500 | 386 | 450 | 0 | 264 | 326 | 9 |
| 6 | popqa | 500 | 432 | 493 | 2 | 216 | 320 | 21 |
| 7 | bamboogle | 125 | 107 | 121 | 0 | 73 | 101 | 1 |
| 7 | hotpotqa | 500 | 381 | 447 | 4 | 284 | 378 | 10 |
| 7 | musique | 500 | 358 | 462 | 6 | 249 | 348 | 18 |
| 7 | nq | 500 | 392 | 455 | 5 | 269 | 337 | 11 |
| 7 | popqa | 500 | 424 | 493 | 0 | 221 | 334 | 14 |





