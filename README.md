# 国家医疗指南信息提取系统

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
![Python](https://img.shields.io/badge/python-3.8%2B-blue)

## 项目简介

本项目旨在从国家医疗指南PDF文档中自动提取结构化医学信息，特别是临床常用的生化检验项目参考区间。通过结合PDF处理、图像识别和大语言模型技术，实现对医疗文档的自动化信息抽取。

## 功能特点

- **PDF批量处理**: 将PDF文档转换为图像文件以便后续处理
- **视觉识别**: 利用视觉语言模型(VLM)从图像中提取文本信息
- **结构化信息抽取**: 使用大语言模型(LLM)从非结构化文本中提取标准化的医学指标信息
- **多语言支持**: 支持中文医疗文档处理
- **可扩展架构**: 模块化设计，易于添加新的信息抽取规则

## 项目结构

```
national_healthcare_guideline_info_extraction/
├── med_guidelines/                     # 医疗指南PDF文件目录
│   ├── 临床常用生化检验项目参考区间第1部分.pdf
│   ├── 临床常用生化检验项目参考区间第2部分.pdf
│   ├── 临床常用生化检验项目参考区间第3部分.pdf
│   └── 临床常用生化检验项目参考区间第4部分.pdf
├── batch_pdf_to_images.ipynb          # PDF转图像处理脚本
├── configs.py                         # 配置文件
└── README.md                          # 项目说明文档
```

## 环境依赖

- Python 3.8+
- PyMuPDF (fitz)
- LlamaIndex
- OpenAI-like API 兼容的大语言模型服务
- json-repair

## 安装指南

1. 克隆项目仓库:
   ```bash
   git clone git@github.com:gwzz/national_healthcare_guideline_info_extraction.git
   cd national_healthcare_guideline_info_extraction
   ```

2. 安装依赖包:
   ```bash
   pip install pymupdf llama-index-core json-repair
   ```

3. 配置模型API:
   在 `configs.py` 中设置您的视觉语言模型和语言模型API端点:
   ```python
   VL_LLM_API_BASE = "http://your-vl-model-api-endpoint/v1"
   VL_MODEL_NAME = "your-vl-model-name"
   LLM_API_BASE = "http://your-llm-api-endpoint/v1/"
   MODEL_NAME = "your-llm-model-name"
   ```

## 使用方法

### 1. PDF转图像处理

运行 `batch_pdf_to_images.ipynb` 笔记本来将PDF文件转换为图像:

```python
# 设置输入输出路径并执行转换
batch_pdf_to_images("med_guidelines", "med_guidelines_images", "jpeg", 1.0)
```

### 2. 图像信息提取

使用 `MedDocProcessor` 类处理图像并提取文本信息:

```python
processor = MedDocProcessor()
# 处理单个图像文件
result = processor.process_image(image_base64)
```

### 3. 结构化信息抽取

使用 `ChatQuery` 类从提取的文本中获取结构化的医学指标信息:

```python
chat = ChatQuery()
# 从文本中提取结构化信息
structured_data = chat.chat_query_no_think(extracted_text)
```

## 配置说明

在 `configs.py` 文件中可以配置以下参数:

- `VL_LLM_API_BASE`: 视觉语言模型API基础URL
- `VL_MODEL_NAME`: 视觉语言模型名称
- `VL_MAX_TOKENS`: 最大token数限制
- `TIME_OUT`: API请求超时时间
- `LLM_API_BASE`: 语言模型API基础URL
- `MODEL_NAME`: 语言模型名称

## 输出格式

系统最终会输出符合以下JSON格式的结构化医学指标信息:

```json
{
  "指标名": "血清钾 (K)",
  "适用人群": "(男/女)",
  "参考值范围": "3.5～5.3",
  "单位": "mmol/L"
}
```

## 贡献指南

欢迎提交 Issue 和 Pull Request 来改进这个项目。

## 许可证

本项目采用 MIT 许可证。详情请见 [LICENSE](LICENSE) 文件。

## 联系方式

如有问题或建议，请通过 GitHub Issues 提交。
