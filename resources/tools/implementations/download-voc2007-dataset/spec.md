---
id: download-voc2007-dataset
name: 下载VOC2007数据集
version: 0.1.0
type: script
language: python
status: active
created: 2026-09-12
---

# 下载VOC2007数据集

## 1. 功能概述

下载 VOC2007 数据集到本地目录，并调用【数据集注册API】将该目录注册为指定名称的数据集，最终返回下载与注册是否成功。

## 2. 输入规范

| 参数名 | 类型 | 必填 | 默认值 | 说明 |
|--------|------|------|--------|------|
| dataset | string | 是 | 无 | 保存的数据集名称，同时作为注册时的数据集标识 |

## 3. 输出规范

### 3.1 标准输出字段
| 字段 | 类型 | 说明 |
|------|------|------|
| status | string | success / failed |
| message | string | 结果说明，如：下载并注册成功 |
| output_format | string | text / image / table / file |
| data | dict/list | 输出数据，包含数据集名称、本地目录、注册结果等 |

`data` 字段示例：
```json
{
  "dataset": "VOC2007",
  "local_dir": "/data/datasets/VOC2007",
  "registered": true
}
```

### 3.2 可视化输出格式
| output_format | data 格式 | 界面渲染方式 |
|---------------|----------|-------------|
| `text` | `{"text":"..."}` | 纯文本 |
| `image` | `{"image_path":"/path/to/file.png"}` | 直接绘制图片 |
| `table` | `{"columns":[...], "rows":[[...]]}` | 渲染表格 |
| `file` | `{"file_path":"/path/to/result.csv"}` | 下载链接 |

本工具默认输出 `output_format=text`，`data.text` 示例：
```
VOC2007 数据集下载成功，已注册为 dataset_name。
```

## 4. 依赖环境

| 依赖 | 版本 | 用途 |
|------|------|------|
| requests | >=2.31.0 | 下载数据集文件 |
| torchvision | >=0.16.0 | 提供 VOC2007 数据集下载与解析能力 |
| tqdm | >=4.66.0 | 下载进度显示 |

## 5. 运行机制

### 5.1 执行流程
1. 读取输入数据，获取数据集名称 `dataset`
2. 校验参数：`dataset` 不能为空，且符合目录命名规范
3. 执行核心逻辑：
   - 下载 VOC2007 数据集到本地目录
   - 调用【数据集注册API】将该目录注册为数据集 `dataset`
4. 返回结果

### 5.2 错误处理
- 参数无效 → 返回验证错误
- 网络异常或下载失败 → 捕获异常并返回详细错误
- 本地存储空间不足 → 返回存储错误
- 调用【数据集注册API】失败 → 返回注册失败原因
- 处理异常 → 捕获并返回详细错误

## 6. 版本历史
| 版本 | 日期 | 变更 |
|------|------|------|
| 0.1.0 | 2026-09-12 | 初始版本 |