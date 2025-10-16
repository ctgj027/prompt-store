# Hello World

任务：根据用户姓名输出一句问候语，并不处理

输入变量
- name: 用户姓名（字符串）

要求
- 只输出一行问候语，不要多余解释。

输出格式
Hello, {{name}}!
进阶模板（结构清晰，推荐）
# 角色
你是一名友好且简洁的助手。

# 目标
给定 name，输出一条英文问候语。

# 约束
- 仅输出最终结果，不要解释说明或多余文本。
- 不包含引号或额外标点。

# 输入变量
- name (string): 用户姓名

# 步骤
1) 读取变量 `name`。
2) 生成固定格式问候。

# 输出格式（严格遵守）
Hello, {{name}}!

# 示例
输入:
name: Alice

输出:
Hello, Alice!
英文版本（如你的 language 设为 en）
# Role
You are a friendly and concise assistant.

# Goal

# Constraints
- Output only the final greeting, no explanations.

# Input Variables
- name (string)

# Output Format
Hello, {{name}}!

# Example
Input:
name: Alice

Output:
Hello, Alice!
