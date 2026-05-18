<div align="center">
<h1 align="center"> 👉 SkillX 👈 </h1>
<b>SkillX：自动为智能体构建技能知识库</b>

[![Awesome](https://awesome.re/badge.svg)](https://github.com/zjunlp/SKillX) 
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
![](https://img.shields.io/github/last-commit/zjunlp/SKillX?color=green) 

<!-- <p align="center">
  <a href="https://arxiv.org/abs/2502.15589">📄arXiv</a> •
  <a href="https://x.com/zxlzr/status/1894729164609208338">𝕏 Blog</a> •
  <a href="https://huggingface.co/collections/zjunlp/SKillX-67f9faaaa518f2e00b17386b">🤗 Huggingface</a>
</p> -->

</div>

## 目录

- 👀[概述](#概述)
- 🔧[安装](#安装)
- 🏃[快速上手](#快速上手)
- 🎁[致谢](#致谢)
- 🚩[引用](#引用)

## 📖 概述

**SkillX** 是一个全自动框架，能够从智能体的历史经验中构建出**可复用、即插即用的技能知识库**，供大语言模型（LLM）智能体使用。

与直接存储原始轨迹、工作流或松散反思不同，SkillX 将智能体经验提炼为**三级技能层次体系**：

- **规划技能（Planning Skills）**：用于高层次任务分解与组织
- **功能技能（Functional Skills）**：可复用的多步工具调用子程序
- **原子技能（Atomic Skills）**：面向执行的具体工具使用模式

SkillX 以强大的骨干智能体为基础，生成可迁移的技能库，可直接注入到较弱的基础智能体和新环境中。在具有挑战性的长视野、用户交互基准测试（如 **AppWorld**、**BFCL-v3** 和 **τ2-Bench**）中，SkillX 持续提升**任务成功率**与**执行效率**。

<div align="center" style="width: 100%;">
  <img src="assets/overview.png" alt="概览图" style="width: 100%; max-width: 100%; height: auto;">
</div>

---

## 数据格式

### 轨迹输入（JSONL）

SkillX 期望的轨迹数据格式如下：

```json
{
  "trajectory_id": "traj_001",
  "task_id": "task_001",
  "user_task": "我的 Spotify 音乐库中有多少首歌曲？",
  "task_history": [
    {"role": "system", "content": "你是一个有用的助手..."},
    {"role": "assistant", "content": "我来帮你统计..."},
    {"role": "user", "content": "输出：\n```\n{\"songs\": 150}\n```"}
  ],
  "reward": 1.0,
  "metadata": {}
}
```

## 🤖 核心特性

### 层次化多级技能设计
SkillX 将原始轨迹转化为结构化的三层技能空间：
- **规划技能**：捕捉高层次的任务分解与执行顺序
- **功能技能**：表示可复用的多步工具调用子程序
- **原子技能**：编码实际工具使用约束与模式

### 全自动技能知识库构建
SkillX 提供端到端的自动化流水线，包括：
- 在训练任务上执行智能体推理，
- 从成功轨迹中提取可复用技能，
- 整合并过滤低质量技能，
- 构建可复用的**即插即用技能知识库**。

### 迭代式技能精炼
SkillX 通过以下方式持续优化技能库：
- **技能合并**：整合冗余行为，
- **质量过滤**：移除脆弱或幻觉技能，
- **迭代更新**：根据执行反馈对技能进行增、改、保留。

### 探索式技能扩展
除了种子示范，SkillX 还主动发现新技能，方法包括：
- 识别使用率低或易失败的工具，
- 引导环境探索，
- 从探索轨迹中合成新任务，
- 扩展技能覆盖范围，超越原始训练分布。

### 跨智能体即插即用迁移
生成的技能库可直接注入不同的基础智能体，实现**强到弱的知识迁移**，无需重新训练底层模型。

### 更好的性能与效率
SkillX 持续提升：
- **任务成功率**（在具有挑战性的基准测试上），
- **执行效率**（减少不必要的探索与工具误用），
- **泛化能力**（通过结构化、可复用的经验抽象）。

---

## 📊 核心亮点

- 在多个基准测试中，对较弱基础智能体带来约 **~10% 的绝对性能提升**
- 在 **AppWorld**、**BFCL-v3** 和 **τ2-Bench** 上稳定提升
- 迁移性强于基于轨迹、工作流和记忆的基线方法
- 减少冗余步骤，显著提升**执行效率**
- 即使技能库由更强的模型构建、被较弱的模型使用，效果依然显著

---

## 🧠 为什么选择 SkillX？

现有的经验学习方法通常存在以下问题：

- **孤立学习**：智能体反复重新发现相似的行为
- **迁移性弱**：原始轨迹和反思内容往往难以泛化
- **能力瓶颈**：自提取的经验受限于智能体自身的能力上限

SkillX 通过构建**结构化技能知识库**来解决这些问题，该知识库具有以下特点：

- **跨任务可复用**
- **跨智能体可迁移**
- **检索开销轻量**
- **易于注入提示词**
- **比长上下文渐进式技能格式更鲁棒**

---

## 🏗️ 方法概览

SkillX 由三个核心组件构成：

### 1. 多级技能提取
从成功的轨迹中，SkillX 自动提取：
- **规划技能**：简洁、可复用的任务计划
- **功能技能**：可复用的工具组合流程
- **原子技能**：特定工具的使用指南、约束与失败记录

### 2. 迭代式技能精炼
SkillX 通过以下方式提升技能库质量：
- **技能合并**：聚类并整合相似技能
- **技能过滤**：移除不可迁移、幻觉或无效技能
- **技能更新**：跨迭代对技能进行增加、修改或保留

### 3. 探索式技能扩展
SkillX 超越已观察的示范进行扩展：
- 引导探索朝向覆盖不足的工具和失败模式，
- 从探索中合成新任务，
- 重新运行提取与精炼流程以扩大技能库。

---

## 📈 主要实验结果

SkillX 在多种 LLM 骨干和基准测试中均提升了智能体性能。

### 代表性增益
- 在 **Qwen3-32B** 上，SkillX 在多个基准测试中带来约 **10 分的提升**
- 在 **Kimi-K2-Instruct-0905** 上，SkillX 在 **AppWorld** 上取得显著提升
- 在 **GLM-4.6** 上，即使模型本身已较强，SkillX 仍能提升性能与执行效率

### 评测基准
- **AppWorld**
- **BFCL-v3**
- **τ2-Bench**

### 核心结论
SkillX 优于以下强经验学习基线：
- **A-Mem**
- **AWM**
- **ExpeL**
- **无记忆（No-memory）**

这表明**经验的表示方式**与经验的来源同样重要，甚至更为关键。

---

## 🔍 SkillX 有何不同？

与已有经验格式相比：

- **原始轨迹**：冗长且难以迁移
- **洞察/反思**：往往过于抽象
- **工作流**：可能遗漏低级工具约束
- **Claude 风格技能**：依赖长上下文渐进式披露，需要复杂的环境支持

相比之下，SkillX 提供：
- **层次化、条目式、可复用的技能**
- **一次性提示词注入**
- **轻量级检索**
- **跨智能体和环境的强迁移性**

---

## 🚀 适用场景

SkillX 尤其适用于：

- **使用工具的 LLM 智能体**
- **长视野任务执行**
- **交互式应用环境**
- **跨智能体知识迁移**
- **从经验中构建可复用的智能体技能库**

---

## 🧪 所用评测基准

### AppWorld
一个包含真实应用程序和 API 的生态系统，用于长视野智能体执行测试。

### BFCL-v3
一个针对多轮函数调用和工具使用的挑战性基准测试。

### τ2-Bench
一个以用户交互为核心、面向对话式工具使用智能体的评测基准。

---

## 📦 计划发布内容

我们将公开发布：

- **SkillX 代码库**
- **自动构建的技能知识库**
- 以及支持技能提取、精炼与检索的配套资源

---

## 🙏 致谢
我们衷心感谢所有开发者、用户和行业合作伙伴的宝贵贡献与大力支持。

- [蚂蚁数科（蚂蚁集团）](https://intl.antdigital.com/en)

本项目在 ReMe 和 AgentEvolver 的代码基础上构建。基线实现改编自 AMEM、AWM 和 Expel。在此向所有贡献者的杰出工作致以诚挚感谢！

## 📚 引用

如果您觉得本工作对您有帮助，欢迎引用：

```bibtex
@article{wang2026skillx,
  author     = {Chenxi Wang and 
                Zhuoyun Yu and 
                Xin Xie and 
                Wuguannan Yao and 
                Runnan Fang and 
                Shuofei Qiao and 
                Kexin Cao and 
                Guozhou Zheng and 
                Xiang Qi and 
                Peng Zhang and 
                Shumin Deng},
  title      = {SkillX: Automatically Constructing Skill Knowledge Bases for Agents},
  year       = {2026},
  eprint     = {2604.04804},
  archivePrefix = {arXiv},
  primaryClass = {cs.CL},
  url        = {https://arxiv.org/abs/2604.04804}
}
```
