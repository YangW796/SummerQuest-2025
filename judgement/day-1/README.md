# Day-1 作业检查脚本

## 功能说明

`rule_lark_checker.py` 是用于检查 Day-1 作业的自动化脚本，通过规则检查学生提交的日志文件内容。

### 检查项目

1. **hw1.log 检查**
   - 检查文件是否存在
   - 验证是否包含关键字：`刘智耿`
   - 确认飞书自动化查询结果的正确性

2. **hw2_1.log 检查**
   - 检查 `nvidia-smi` 输出结果
   - 验证是否包含关键字：`NVIDIA`, `Version`
   - 确认显卡信息显示正确

3. **hw2_2.log 检查**
   - 检查环境配置脚本运行结果
   - 验证是否包含关键字：`PyTorch`, `CUDA`, `Transformers`
   - 确认Python环境配置正确

4. **hw2_3.log 检查**
   - 检查 vLLM 测试脚本运行结果
   - 验证是否包含关键字：`vLLM`, `初始化`, `AI回复`
   - 确认 vLLM 环境配置和测试正确

### 评分规则

- **Day-1-hw1**: 0分或4分（hw1.log 包含正确的查询结果）
- **Day-1-hw2**: 0-6分（hw2_*.log 文件的检查结果累计，每个文件2分）
  - hw2_1.log: 2分（显卡信息）
  - hw2_2.log: 2分（环境配置）
  - hw2_3.log: 2分（vLLM测试）

## 使用方法

### 环境准备

1. 设置飞书应用环境变量：
```bash
export FEISHU_APP_ID="your_app_id"
export FEISHU_APP_SECRET="your_app_secret"
```

2. 确保学生作业目录结构正确：
```
../../submission/
├── 学生姓名1/
│   └── day-1/
│       ├── README.md
│       ├── doc_viewer.py
│       ├── hw1.log
│       ├── hw2_1.log
│       ├── hw2_2.log
│       └── hw2_3.log
├── 学生姓名2/
│   └── day-1/
│       ├── README.md
│       ├── doc_viewer.py
│       ├── hw1.log
│       ├── hw2_1.log
│       ├── hw2_2.log
│       └── hw2_3.log
└── ...
```

### 运行脚本

```bash
cd /path/to/judgement
python3 day-1/rule_lark_checker.py
```

## 功能特性

- **规则检查**: 基于关键字匹配进行精确的内容验证
- **文件编码支持**: 自动处理 UTF-8 和 UTF-16 编码的日志文件
- **批量处理**: 自动扫描所有学生目录并批量检查
- **批量更新**: 使用飞书 API 批量更新表格数据
- **详细统计**: 提供完整的检查结果和更新统计
- **错误处理**: 完善的文件读取和API调用错误处理

## 检查逻辑详解

### hw1.log 检查
期望内容应包含飞书自动化查询结果，特别是包含"刘智耿"的记录：
```
筛选记录 1:
主讲: 刘智耿
助教: 怀天宇, 方世成, 宋悦荣
日期: Day-5（7.11）
课程: 常见框架介绍( llamafactory, verl, vllm)

筛选记录 2:
主讲: 郑逸宁
助教: 陈敬麒, 刘智耿
日期: Day-1（7.7）
课程: Slurm 使用方法（组内新生用 slurm）
```

### hw2_1.log 检查
期望内容应包含 `nvidia-smi` 命令的输出，显示显卡信息：
```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 525.60.13    Driver Version: 525.60.13    CUDA Version: 12.0  |
|-------------------------------+----------------------+----------------------+
...
```

### hw2_2.log 检查
期望内容应包含环境检查脚本的运行结果：
```
✅ PyTorch 版本: 2.0.1+cu118
✅ CUDA 可用: True
✅ Transformers 版本: 4.30.2
...
```

### hw2_3.log 检查
期望内容应包含 vLLM 测试脚本的运行结果：
```
✅ vLLM 初始化成功
🤖 AI回复: 你好！我是一个AI助手...
...
```

## 输出示例

```
✅ 成功获取 45 条记录

🔍 检查学生 张三 的作业文件...
✅ hw1.log 检查通过 (4分)
✅ hw2_1.log 检查通过 (2分)
✅ hw2_2.log 检查通过 (2分)
✅ hw2_3.log 检查通过 (2分)
📊 学生 张三 总分: Day-1-hw1=4/4, Day-1-hw2=6/6

🔍 检查学生 李四 的作业文件...
✅ hw1.log 检查通过 (4分)
✅ hw2_1.log 检查通过 (2分)
❌ hw2_2.log 缺少关键字: PyTorch (0分)
✅ hw2_3.log 检查通过 (2分)
📊 学生 李四 总分: Day-1-hw1=4/4, Day-1-hw2=4/6

✅ 成功批量更新 42 条记录

⚠️ 以下学生在飞书表格中不存在:
   - 王五
   - 赵六
```

## 注意事项

1. 确保飞书应用有足够的权限访问多维表格
2. 网络连接稳定，避免 API 调用失败
3. 学生姓名必须与飞书表格中的姓名完全一致
4. 日志文件必须包含完整的执行结果和关键信息
5. 支持多种文件编码，但建议使用 UTF-8 编码
6. 关键字匹配区分大小写，请确保日志内容格式正确

## 技术特点

- **精确匹配**: 基于关键字的精确内容验证
- **多编码支持**: 自动处理不同编码格式的文件
- **容错处理**: 对文件不存在、读取失败等情况有完善的处理
- **批量操作**: 高效的批量检查和更新机制
- **详细日志**: 提供完整的检查过程和结果反馈