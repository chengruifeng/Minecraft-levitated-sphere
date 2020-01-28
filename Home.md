### 首先，**非常感谢**你考虑为 UpgradeAll 贡献帮助。
### 正是你这样的人使 UpgradeAll 成为一个很棒的工具。通过该文档，你可以全面了解/学习 UpgradeAll 项目的规则组织/编写方式

## 如何阅读

推荐的阅读顺序：

1. [云端仓库的目录结构解析](云端仓库的目录结构解析)
2. [rules_list 的 JSON 格式解释](rules_list-的-JSON-格式解释)
3. [App(跟踪项)配置文件的格式解释](App(跟踪项)配置文件的格式解释)
4. [软件源配置文件的格式解释](软件源配置文件的格式解释)
5. [软件源 JavaScript 脚本编写指南](软件源-JavaScript-脚本编写指南)

## 贡献规则

无论你选择何种方式提交，我们都将及时处理

### issues

开启一个 issues，附上你的跟踪项 JSON 配置或者软件源 JSON 配置和软件源 JS 脚本,并明确告知我们你希望提交你的配置给云端仓库

### Pull requests（处理速度更快）

Fork 本仓库并修改相关文件，然后向 master 分支开启一个合并请求  
请确认：

- 如果你修改/新增跟踪项配置（你至少需要检查/修改 rules/rules_list.json 与 rules/apps 中的内容）
- 如果你修改/新增软件源配置（你至少需要检查/修改 rules/rules_list.json、rules/hub 与 rules/hub/js 中的内容）
