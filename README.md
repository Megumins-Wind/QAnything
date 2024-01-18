![star_us](https://github.com/netease-youdao/QAnything/assets/29041332/ea92eee0-b2d1-4ec1-88f5-8531669cf6ca)<div align="center">

  <a href="https://github.com/netease-youdao/QAnything">
    <!-- Please provide path to your logo here -->
    <img src="docs/images/qanything_logo.png" alt="Logo" width="800">
  </a>

# **Q**uestion and **A**nswer based on **Anything**

<p align="center">
  <a href="./README.md">English</a> |
  <a href="./README_zh.md">简体中文</a>
</p>

</div>

<div align="center">

<a href="https://qanything.ai"><img src="https://img.shields.io/badge/try%20online-qanything.ai-purple"></a>
&nbsp;&nbsp;&nbsp;&nbsp;
<a href="https://read.youdao.com#/home"><img src="https://img.shields.io/badge/try%20online-read.youdao.com-purple"></a>
&nbsp;&nbsp;&nbsp;&nbsp;

<a href="./LICENSE"><img src="https://img.shields.io/badge/license-Apache--2.0-yellow"></a>
&nbsp;&nbsp;&nbsp;&nbsp;
<a href="https://github.com/netease-youdao/QAnything/pulls"><img src="https://img.shields.io/badge/PRs-welcome-red"></a>
&nbsp;&nbsp;&nbsp;&nbsp;
<a href="https://twitter.com/YDopensource"><img src="https://img.shields.io/badge/follow-%40YDOpenSource-1DA1F2?logo=twitter&style={style}"></a>
&nbsp;&nbsp;&nbsp;&nbsp;

</div>

<details open="open">
<summary>Table of Contents</summary>

- [What is QAnything](#What-is-QAnything)
  - [Key features](#Key-features)
  - [Architecture](#Architecture)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
- [Usage](#usage)
  - [API Document](#API-Document)
- [WeChat Group](#WeChat-Group)
- [Support](#support)
- [License](#license)
- [Acknowledgements](#Acknowledgments)

</details>

## What is QAnything?
**Q**uestion and **A**nswer based on **Anything** (`QAnything`) is a local knowledge base question-answering system designed to support a wide range of file formats and databases, allowing for offline installation and use.

With `QAnything`, you can simply drop any locally stored file of any format and receive accurate, fast, and reliable answers.

Currently supported formats include: **PDF, Word (doc/docx), PPT, Markdown, Eml, TXT, Images (jpg, png, etc.), Web links** and more formats coming soon…


### Key features

- **Data Security**, supports installation and usage with network cable unplugged throughout the process.
- **Cross-language QA support**, freely switch between Chinese and English QA, regardless of the language of the document.
- **Supports massive data QA**, two-stage retrieval ranking, solving the degradation problem of large-scale data retrieval; the more data, the better the performance.
- **High-performance production-grade system**, directly deployable for enterprise applications.
- **User-friendly**, no need for cumbersome configurations, one-click installation and deployment, ready to use.
- **Multi knowledge base QA** Support selecting multiple knowledge bases for Q&A

## Before You Start
**Star us on GitHub, and be instantly notified for new release!**
![star_us](https://github.com/netease-youdao/QAnything/assets/29041332/fd5e5926-b9b2-4675-9f60-6cdcaca18e14)


### Architecture
<div align="center">
<img src="docs/images/qanything_arch.png" width = "700" alt="qanything_system" align=center />
</div>

#### Why 2 stage retrieval?
In scenarios with a large volume of knowledge base data, the advantages of a two-stage approach are very clear. If only a first-stage embedding retrieval is used, there will be a problem of retrieval degradation as the data volume increases, as indicated by the green line in the following graph. However, after the second-stage reranking, there can be a stable increase in accuracy, **the more data, the better the performance**.
<div align="center">
<img src="docs/images/two_stage_retrieval.jpg" width = "500" alt="two stage retrievaal" align=center />
</div>

QAnything uses the retrieval component [BCEmbedding](https://github.com/netease-youdao/BCEmbedding), which is distinguished for its bilingual and crosslingual proficiency. BCEmbedding excels in bridging Chinese and English linguistic gaps, which achieves
- **A high performence on <a href="https://github.com/netease-youdao/BCEmbedding/tree/master?tab=readme-ov-file#evaluate-semantic-representation-by-mteb" target="_Self">Semantic Representation Evaluations in MTEB</a>**;
- **A new benchmark in the realm of <a href="https://github.com/netease-youdao/BCEmbedding/tree/master?tab=readme-ov-file#evaluate-rag-by-llamaindex" target="_Self">RAG Evaluations in LlamaIndex</a>**.


#### 1st Retrieval（embedding）
| Model | Retrieval | STS | PairClassification | Classification | Reranking | Clustering | Avg |  
|:-------------------------------|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|  
| bge-base-en-v1.5 | 37.14 | 55.06 | 75.45 | 59.73 | 43.05 | 37.74 | 47.20 |  
| bge-base-zh-v1.5 | 47.60 | 63.72 | 77.40 | 63.38 | 54.85 | 32.56 | 53.60 |  
| bge-large-en-v1.5 | 37.15 | 54.09 | 75.00 | 59.24 | 42.68 | 37.32 | 46.82 |  
| bge-large-zh-v1.5 | 47.54 | 64.73 | **79.14** | 64.19 | 55.88 | 33.26 | 54.21 |  
| jina-embeddings-v2-base-en | 31.58 | 54.28 | 74.84 | 58.42 | 41.16 | 34.67 | 44.29 |  
| m3e-base | 46.29 | 63.93 | 71.84 | 64.08 | 52.38 | 37.84 | 53.54 |  
| m3e-large | 34.85 | 59.74 | 67.69 | 60.07 | 48.99 | 31.62 | 46.78 |  
| ***bce-embedding-base_v1*** | **57.60** | **65.73** | 74.96 | **69.00** | **57.29** | **38.95** | ***59.43*** |  

- More evaluation details please check [Embedding Models Evaluation Summary](https://github.com/netease-youdao/BCEmbedding/blob/master/Docs/EvaluationSummary/embedding_eval_summary.md)。

#### 2nd Retrieval（rerank）
| Model | Reranking | Avg |  
|:-------------------------------|:--------:|:--------:|  
| bge-reranker-base | 57.78 | 57.78 |  
| bge-reranker-large | 59.69 | 59.69 |  
| ***bce-reranker-base_v1*** | **60.06** | ***60.06*** |  

- More evaluation details please check [Reranker Models Evaluation Summary](https://github.com/netease-youdao/BCEmbedding/blob/master/Docs/EvaluationSummary/reranker_eval_summary.md)

#### RAG Evaluations in LlamaIndex（embedding and rerank）

<img src="https://github.com/netease-youdao/BCEmbedding/blob/master/Docs/assets/rag_eval_multiple_domains_summary.jpg">

***NOTE:***

- In `WithoutReranker` setting, our `bce-embedding-base_v1` outperforms all the other embedding models.
- With fixing the embedding model, our `bce-reranker-base_v1` achieves the best performence.
- **The combination of `bce-embedding-base_v1` and `bce-reranker-base_v1` is SOTA**.
- If you want to use embedding and rerank separately, please refer to [BCEmbedding](https://github.com/netease-youdao/BCEmbedding)

#### LLM

The open source version of QAnything is based on QwenLM and has been fine-tuned on a large number of professional question-answering datasets. It greatly enhances the ability of question-answering.
If you need to use it for commercial purposes, please follow the license of QwenLM. For more details, please refer to: [QwenLM](https://github.com/QwenLM/Qwen)


## Getting Started
[:point_right: try QAnything online](https://qanything.ai)
### Prerequisites

|  **Required item**     | **Minimum Requirement** | **Note** |
| --------------         |------------------------| --------------------------------- |
| NVIDIA GPU Memory      | >= 16GB                | NVIDIA 3090 recommended |
| NVIDIA Driver Version  | >= 525.105.17          |                           |
| CUDA Version           | >= 12.0                |                           |
| docker compose  version| >= 2.12.1              | [docker compose install](https://docs.docker.com/compose/install/)|



### Installation
#### step1: pull qanything repository
```
git clone https://github.com/netease-youdao/QAnything.git
```
#### step2: download the model and unzip it to the root directory of the current project.
This project provides multiple model download platforms. Choose one of the methods for downloading.

[👉【WiseModel】](https://wisemodel.cn/models/Netease_Youdao/qanything)
[👉【ModelScope】](https://www.modelscope.cn/models/netease-youdao/QAnything)
[👉【HuggingFace】](https://huggingface.co/netease-youdao/QAnything)

<details>
<summary>Download method 1：WiseModel（recommend👍）</summary>

```
cd QAnything
# Make sure you have git-lfs installed (https://git-lfs.com)
git lfs install
git clone https://www.wisemodel.cn/Netease_Youdao/qanything.git
unzip qanything/models.zip   # in root directory of the current project
```
</details>
<details>
<summary>Download method 2：ModelScope</summary>

```
cd QAnything
# Make sure you have git-lfs installed (https://git-lfs.com)
git lfs install
git clone https://www.modelscope.cn/netease-youdao/QAnything.git
unzip QAnything/models.zip   # in root directory of the current project
```
</details>
<details>
<summary>Download method 3：HuggingFace</summary>

```
cd QAnything
# Make sure you have git-lfs installed (https://git-lfs.com)
git lfs install
git clone https://huggingface.co/netease-youdao/QAnything
unzip QAnything/models.zip   # in root directory of the current project
```
</details>


#### step3: change config
##### in the Windows system
```
vim docker-compose-windows.yaml # change CUDA_VISIBLE_DEVICES to your gpu device id
vim front_end/.env.production # set the excetly host.
```
##### in the Linux system
```
vim docker-compose-linux.yaml # change CUDA_VISIBLE_DEVICES to your gpu device id
vim front_end/.env.production # set the excetly host.
```
#### step4: start server
##### in the Windows system
<details>
<summary>Beginner's recommendation!</summary>

```shell
# Front desk startup, log prints to the screen in real time, press ctrl+c to stop.
docker-compose -f docker-compose-windows.yaml up qanything_local
```
</details>

<details>
<summary>Recommended for experienced players!</summary>

```shell
# Background startup, ctrl+c will not stop.
docker-compose -f docker-compose-windows.yaml up -d
# Execute the following command to view the log.
docker-compose -f docker-compose-windows.yaml logs qanything_local
# Stop service
docker-compose -f docker-compose-windows.yaml down
```
</details>

##### in the Linux system
<details>
<summary>Beginner's recommendation!</summary>

```shell
# Front desk startup, log prints to the screen in real time, press ctrl+c to stop.
docker-compose -f docker-compose-linux.yaml up qanything_local
```
</details>

<details>
<summary>Recommended for experienced players!</summary>

```shell
# Background startup, ctrl+c will not stop.
docker-compose -f docker-compose-linux.yaml up -d
# Execute the following command to view the log.
docker-compose -f docker-compose-linux.yaml logs qanything_local
# Stop service
docker-compose -f docker-compose-linux.yaml down
```
</details>

After successful installation, you can experience the application by entering the following addresses in your web browser.

- Frontend address: http://{your_host}:5052/qanything/

- API address: http://{your_host}:5052/api/

For detailed API documentation, please refer to [QAnything API 文档](docs/API.md)


## Usage

### Cross-lingual: Multiple English paper Q&A
[![](docs/videos/multi_paper_qa.mp4)](https://github.com/netease-youdao/QAnything/assets/141105427/8915277f-c136-42b8-9332-78f64bf5df22)
### Information extraction
[![](docs/videos/information_extraction.mp4)](https://github.com/netease-youdao/QAnything/assets/141105427/b9e3be94-183b-4143-ac49-12fa005a8a9a
)
### Various files
[![](docs/videos/various_files_qa.mp4)](https://github.com/netease-youdao/QAnything/assets/141105427/7ede63c1-4c7f-4557-bd2c-7c51a44c8e0b)
### Web Q&A
[![](docs/videos/web_qa.mp4)](https://github.com/netease-youdao/QAnything/assets/141105427/d30942f7-6dbd-4013-a4b6-82f7c2a5fbee)

### API Document
If you need to access the API, please refer to the [QAnything API documentation](docs/API.md).

## WeChat Group

Welcome to scan the QR code below and join the WeChat group.

<img src="docs/images/Wechat.jpg" width="20%" height="auto">

## Support
Reach out to the maintainer at one of the following places:

- [Github issues](https://github.com/netease-youdao/QAnything/issues)
- Contact options listed on [this GitHub profile](https://github.com/netease-youdao)


## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=netease-youdao/QAnything,netease-youdao/BCEmbedding&type=Date)](https://star-history.com/#netease-youdao/QAnything&netease-youdao/BCEmbedding&Date)



## License

`QAnything` is licensed under [Apache 2.0 License](./LICENSE)

## Acknowledgments
`QAnything` adopts dependencies from the following:
- Thanks to our [BCEmbedding](https://github.com/netease-youdao/BCEmbedding) for the excellent embedding and rerank model. 
- Thanks to [Qwen](https://github.com/QwenLM/Qwen) for strong base language models.
- Thanks to [Triton Inference Server](https://github.com/triton-inference-server/server) for providing great open source inference serving.
- Thanks to [FasterTransformer](https://github.com/NVIDIA/FasterTransformer) for highly optimized LLM inference backend.
- Thanks to [Langchain](https://github.com/langchain-ai/langchain) for the wonderful llm application framework. 
- Thanks to [Langchain-Chatchat](https://github.com/chatchat-space/Langchain-Chatchat) for the inspiration provided on local knowledge base Q&A.
- Thanks to [Milvus](https://github.com/milvus-io/milvus) for the excellent semantic search library.
- Thanks to [PaddleOCR](https://github.com/PaddlePaddle/PaddleOCR) for its ease-to-use OCR library.
- Thanks to [Sanic](https://github.com/sanic-org/sanic) for the powerful web service framework.

