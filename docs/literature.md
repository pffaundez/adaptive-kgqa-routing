# Literature Map

## Purpose

This document tracks the papers that motivate, constrain, or contextualize the project. It is a working research index, not a claim that every listed paper is directly comparable to the proposed method.

The list is divided into:

- **Core literature:** papers that directly motivate the current KGQA workflow-routing formulation or provide the main technical components from which the research gap is derived.
- **General bibliography:** complementary papers on routing, adaptive graph reasoning, multi-agent systems, datasets, evaluation, and related methods. Inclusion here does not make a paper a direct baseline.

Publication status and links should be rechecked before paper submission.

## Core literature

### RouterKGQA

**Citation:** Bo Yuan, Hexuan Deng, Xuebo Liu, and Min Zhang. 2026. *RouterKGQA: Specialized–General Model Routing for Constraint-Aware Knowledge Graph Question Answering.*

**Status:** arXiv preprint, arXiv:2603.20017. No peer-reviewed venue verified as of 2026-09-30.

**Primary source:** https://arxiv.org/abs/2603.20017

**Code:** https://github.com/Oldcircle/RouterKGQA

**Role in this project:** Closest KGQA routing reference and direct motivation for selecting between cheaper specialized reasoning and more expensive general-model repair.

**Relevant insight or limitation:**

- establishes a quality-cost motivation for conditional use of a general model;
- uses reachability as a central routing signal;
- supports only a limited flat constraint representation;
- a reachable path may still be semantically incorrect;
- evaluation is restricted to English Freebase benchmarks;
- motivates routing based on predicted workflow suitability rather than reachability alone.

### EffiQA

**Citation:** Zixuan Dong, Baoyun Peng, Yufei Wang, Jia Fu, Xiaodong Wang, Xin Zhou, Yongxue Shan, Kangchen Zhu, and Weiguo Chen. 2025. *EffiQA: Efficient Question-Answering with Strategic Multi-Model Collaboration on Knowledge Graphs.* COLING 2025, pages 7180–7194.

**Status:** Peer-reviewed conference paper.

**Primary source:** https://aclanthology.org/2025.coling-main.479/

**Role in this project:** Main peer-reviewed anchor for efficient collaboration between large and small models in KGQA.

**Relevant insight or limitation:**

- validates that collaborative KGQA can balance reasoning performance and computational cost;
- combines global planning, efficient KG exploration, and self-reflection;
- motivates heterogeneous reasoning capabilities;
- follows a designed iterative collaboration pattern rather than learning a one-shot pre-execution workflow choice for each query.

### Plan-on-Graph

**Citation:** Liyi Chen, Panrong Tong, Zhongming Jin, Ying Sun, Jieping Ye, and Hui Xiong. 2024. *Plan-on-Graph: Self-Correcting Adaptive Planning of Large Language Model on Knowledge Graphs.* NeurIPS 2024.

**Status:** Peer-reviewed conference paper.

**Primary source:** https://openreview.net/forum?id=CwCUEr6wO5

**Code:** https://github.com/liyichen-cly/PoG

**Role in this project:** Main reference for adaptive exploration, reflection, and self-correction over knowledge graphs.

**Relevant insight or limitation:**

- demonstrates the value of adapting graph exploration to query semantics;
- introduces iterative memory, reflection, and path correction;
- motivates the bounded verification-and-repair workflow;
- makes sequential decisions during execution and consumes intermediate outcomes, whereas the proposed router selects a workflow before candidate execution.

### Graph-constrained Reasoning

**Citation:** Linhao Luo, Zicheng Zhao, Gholamreza Haffari, Yuan-Fang Li, Chen Gong, and Shirui Pan. 2025. *Graph-constrained Reasoning: Faithful Reasoning on Knowledge Graphs with Large Language Models.* ICML 2025, PMLR 267:41540–41565.

**Status:** Peer-reviewed conference paper.

**Primary source:** https://proceedings.mlr.press/v267/luo25t.html

**Role in this project:** Main reference for faithful graph-constrained decoding and collaboration between a KG-specialized model and a general LLM.

**Relevant insight or limitation:**

- constrains decoding to KG-grounded reasoning paths through a KG-Trie;
- validates the usefulness of specialized and general model capabilities;
- motivates the graph-constrained workflow;
- graph validity addresses hallucinated paths but does not by itself establish semantic adequacy for every query;
- uses a designed collaboration architecture rather than query-dependent routing among alternative workflows.

### ChatKBQA

**Citation:** Haoran Luo, Haihong E, Zichen Tang, Shiyao Peng, Yikai Guo, Wentai Zhang, Chenghao Ma, Guanting Dong, Meina Song, Wei Lin, Yifan Zhu, and Anh Tuan Luu. 2024. *ChatKBQA: A Generate-then-Retrieve Framework for Knowledge Base Question Answering with Fine-tuned Large Language Models.* Findings of ACL 2024, pages 2039–2056.

**Status:** Peer-reviewed Findings paper.

**Primary source:** https://aclanthology.org/2024.findings-acl.122/

**Code:** https://github.com/LHRLAB/ChatKBQA

**Role in this project:** Main reference for explicit logical-form generation and compositional query representation.

**Relevant insight or limitation:**

- shows the value of generating a logical form before retrieving and replacing entities and relations;
- motivates a workflow capable of filters, joins, comparisons, aggregation, and other compositional operations;
- provides an operational contrast to simple path-based reasoning;
- does not address cost-aware pre-execution selection among multiple KGQA workflows.

### KG-Agent

**Citation:** Jinhao Jiang, Kun Zhou, Wayne Xin Zhao, Yang Song, Chen Zhu, Hengshu Zhu, and Ji-Rong Wen. 2025. *KG-Agent: An Efficient Autonomous Agent Framework for Complex Reasoning over Knowledge Graph.* ACL 2025, pages 9505–9523.

**Status:** Peer-reviewed ACL main-conference paper.

**Primary source:** https://aclanthology.org/2025.acl-long.468/

**Role in this project:** Main reference for autonomous tool use and sequential reasoning over a knowledge graph.

**Relevant insight or limitation:**

- demonstrates iterative tool selection and memory updates for complex KG reasoning;
- motivates the expensive adaptive workflow and explicit stopping conditions;
- makes decisions during execution rather than selecting one workflow before execution;
- its sequential autonomy has a different information and cost budget from the proposed one-shot router.

## General bibliography

### AgentRouter

**Citation:** Zheyuan Zhang, Kaiwen Shi, Zhengqing Yuan, Zehong Wang, Tianyi Ma, Keerthiram Murugesan, Vincent Galassi, Chuxu Zhang, and Yanfang Ye. 2026. *AgentRouter: A Knowledge-Graph-Guided LLM Router for Collaborative Multi-Agent Question Answering.* ACL 2026, pages 788–809.

**Primary source:** https://aclanthology.org/2026.acl-long.33/

**Relevance:** Important methodological neighbor for query-dependent agent routing, complementarity, soft supervision, and empirical performance labels.

**Scope distinction:** It uses a knowledge graph internally to represent queries, entities, and agents, but its evaluated QA setting is not direct KGQA over an external knowledge base. It should not be presented as a KGQA predecessor.

### RouteLLM

**Citation:** Isaac Ong, Amjad Almahairi, Vincent Wu, Wei-Lin Chiang, Tianhao Wu, Joseph E. Gonzalez, M. Waleed Kadous, and Ion Stoica. 2025. *RouteLLM: Learning to Route LLMs from Preference Data.* ICLR 2025.

**Primary source:** https://openreview.net/forum?id=8sSqNntaMr

**Relevance:** Establishes learned cost-performance routing between stronger and weaker LLMs and motivates simple deployable routing baselines.

**Scope distinction:** Selects a model rather than a functionally distinct KGQA workflow.

### Graph Counselor

**Citation:** Junqi Gao, Xiang Zou, Ying Ai, Dong Li, Yichen Niu, Biqing Qi, and Jianxing Liu. 2025. *Graph Counselor: Adaptive Graph Exploration via Multi-Agent Synergy to Enhance LLM Reasoning.* ACL 2025, pages 24650–24668.

**Primary source:** https://aclanthology.org/2025.acl-long.1202/

**Code:** https://github.com/gjq100/Graph-Counselor

**Relevance:** Provides evidence for adaptive graph information extraction, dynamic reasoning depth, multi-agent collaboration, and semantic self-correction.

**Scope distinction:** Uses a designed multi-agent reasoning process during execution rather than selecting one KGQA workflow before execution.

## Maintenance rules

When adding a paper:

1. verify the exact title, authors, year, venue, and current publication status from a primary source;
2. use the peer-reviewed version as the primary citation when one exists;
3. distinguish direct KGQA work from methodological neighbors;
4. state the paper's concrete role in this project;
5. record the limitation or open question relevant to the current research gap;
6. do not promote a paper from general bibliography to core literature without an explicit methodological reason;
7. do not infer novelty merely because no paper uses the same terminology.
