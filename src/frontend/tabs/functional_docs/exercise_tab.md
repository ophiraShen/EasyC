# ExerciseTab 类设计文档

## 功能概述
`ExerciseTab` 类实现了习题练习的用户界面，提供了习题选择、代码编辑和运行等功能的完整交互界面。

## 类结构
```python
class ExerciseTab:
    def __init__(self, exercise_service: ExerciseService)
    def _handle_chapter_select(self, chapter_id: str) -> dict
    def _handle_exercise_select(self, evt: gr.SelectData) -> dict
    async def _handle_run_code(self, code: str) -> str
    def create() -> gr.Tab
```

## 界面组件

### 1. 左侧面板
- **章节选择器** (gr.Dropdown)
  - 显示所有可用章节
  - 格式：`(chapter_id, 第id章 (Y题))`
  - 触发章节内容加载

- **习题列表** (gr.Dataset)
  - 显示当前章节所有习题
  - 列表头：["题目", "难度"]
  - 支持习题选择

### 2. 右侧面板
- **习题描述** (gr.Markdown)
  - 显示习题标题和详细描述
  - 支持 Markdown 格式

- **代码编辑器** (gr.Code)
  - 语言：C
  - 提供代码模板
  - 支持语法高亮

- **运行按钮** (gr.Button)
  - 触发代码执行

- **输出框** (gr.Textbox)
  - 显示运行结果
  - 显示错误信息
  - 支持多行显示

## 事件处理

### 1. 章节选择处理
- **方法**: `_handle_chapter_select`
- **触发**: 当用户选择新的章节时
- **功能**:
  - 加载章节习题列表
  - 清空当前习题状态
  - 更新界面显示
- **返回数据**:
  ```python
  {
      "exercise_list": [[title, difficulty], ...],
      "exercise_description": "",
      "code_editor": "",
      "output_box": ""
  }
  ```

### 2. 习题选择处理
- **方法**: `_handle_exercise_select`
- **触发**: 当用户选择习题时
- **功能**:
  - 加载习题详情
  - 设置代码模板
  - 更新当前习题状态
- **返回数据**:
  ```python
  {
      "exercise_description": "### 标题\n描述",
      "code_editor": "代码模板",
      "output_box": ""
  }
  ```

### 3. 代码运行处理
- **方法**: `_handle_run_code`
- **触发**: 当用户点击运行按钮时
- **功能**:
  - 验证习题选择状态
  - 执行代码
  - 显示运行结果
- **错误处理**:
  - 未选择习题时的提示
  - 运行错误的展示

## 状态管理
- 使用 `current_exercise` 追踪当前选中的习题
- 保持界面状态的一致性
- 清理无效状态

## 使用示例
```python
# 创建标签页
exercise_service = ExerciseService(compiler_service)
exercise_tab = ExerciseTab(exercise_service)
tab = exercise_tab.create()

# 集成到主界面
with gr.Blocks() as app:
    exercise_tab.create()
```

## 界面交互流程
1. 用户选择章节
   - 显示章节习题列表
   - 清空右侧面板

2. 用户选择习题