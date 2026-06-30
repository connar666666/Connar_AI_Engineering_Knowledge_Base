# 上传到 GitHub 的操作说明

目标仓库：

```text
https://github.com/connar666666/Connar_AI_Engineering_Knowledge_Base
```

## 方式一：网页上传

适合不想用命令行时使用。

1. 打开仓库页面。
2. 点击 `Add file`。
3. 选择 `Upload files`。
4. 把本地解压后的整个 `Connar_AI_Engineering_Knowledge_Base` 文件夹里的内容拖进去。
5. 填写提交信息：

```text
Initialize project-based knowledge base
```

6. 点击 `Commit changes`。

## 方式二：命令行上传

如果本地已经 clone 了仓库：

```bash
cd Connar_AI_Engineering_Knowledge_Base
# 把本次生成的文件复制到这个仓库目录内

git status
git add .
git commit -m "Initialize project-based AI engineering knowledge base"
git push origin main
```

如果还没有 clone：

```bash
git clone https://github.com/connar666666/Connar_AI_Engineering_Knowledge_Base.git
cd Connar_AI_Engineering_Knowledge_Base
# 把本次生成的文件复制到这个目录内

git add .
git commit -m "Initialize project-based AI engineering knowledge base"
git push origin main
```

## 推荐后续维护方式

每次和 ChatGPT 讨论出一个成熟结论后，不要只保存聊天。建议整理成对应项目目录下的一个 `.md` 文件。

例如：

```text
02_ComfyUI_Bench/Database_Design.md
03_OpenAI_Agent_Attack/Search_Strategy.md
04_AgentOS_OSAgent/Comparison_and_Future.md
```

