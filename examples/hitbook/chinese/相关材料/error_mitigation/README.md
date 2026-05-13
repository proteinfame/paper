# Selected test-set accuracy figures

Datasets: PopQA, MuSiQue, HotpotQA, BamboogleQA, NQ. Values are TensorBoard `reward mean@1`, plotted with y-axis label `Accuracy`.

| Dataset | SSRL final | Ours final | SSRL best | Ours best |
|---|---:|---:|---:|---:|
| PopQA | 0.3124 | 0.3322 | 0.3268 | 0.3466 |
| MuSiQue | 0.1576 | 0.1646 | 0.1646 | 0.1702 |
| HotpotQA | 0.2872 | 0.2908 | 0.3032 | 0.2962 |
| BamboogleQA | 0.2728 | 0.2656 | 0.3337 | 0.3440 |
| NQ | 0.3142 | 0.2926 | 0.3142 | 0.2980 |

用于第三章图示，表明本文方法能改进模型性能。
其中best_checkpoint遵循相关工作方法展示表现最优的checkpoint，final_checkpoint是训练完成的checkpoint
examples/hitbook/chinese/相关材料/final_and_best_selected_accuracy/best_checkpoint.png
examples/hitbook/chinese/相关材料/final_and_best_selected_accuracy/final_checkpoint.png

用于附录，展示相关数据集随着训练表现变化
examples/hitbook/chinese/相关材料/final_and_best_selected_accuracy/bamboogle_curve.png
examples/hitbook/chinese/相关材料/final_and_best_selected_accuracy/hotpotqa_curve.png
examples/hitbook/chinese/相关材料/final_and_best_selected_accuracy/musique_curve.png
examples/hitbook/chinese/相关材料/final_and_best_selected_accuracy/nq_curve.png
examples/hitbook/chinese/相关材料/final_and_best_selected_accuracy/popqa_curve.png


用于第三章和附录三线图图表绘制
examples/hitbook/chinese/相关材料/final_and_best_selected_accuracy/summary.csv

第三章 面向错误缓解的策略研究与实现 
使用 grpo 进行模型推理改进手段
grpo 一般关注结果 orm
现有prm在知识调用上不能提供足够稳定的监督信号
我进行ssrl强化学习，收集自检索 self search rl（SSRL）中rollout数据，使用deepseek v4 flash进行标注是否错误，进行分析，验证三部分。标注10k数据，8/2分为训练/测试集。生成反思验证数据集
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
```

使用反思验证数据集对qwen2.5 3b instruct 进行sft
对微调后模型进行强化学习（Ours）。
结果表明反思训练对强化学习效果有促进作用
