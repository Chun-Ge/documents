# 中大春哥树洞文档仓库

## Github Pages

为了您更好的浏览体验，本仓库大部分文档已经托管在[github pages](https://chun-ge.github.io/documents/)上。

但少量图表的源文件可能无法在pages上找到。

## 仓库结构

- `docs/management-docs` 中存放一般性的迭代开发过程中用于项目管理产生的文档
- `docs/model-docs` 中存放用例或设计模型图等文档
- `docs/technical-docs` 中存放开发过程中的技术规范或报告文档
- `docs/meeting-mind-graph` 中存放了每次会议的会议记录思维导图
- `docs/report` 中存放了作为课程作业的小组成员技术报告
- `drafts` 中存放了开发过程中用于参考的临时文档

其它大多为github pages的jekyll配置文件。

## 仓库使用说明

- 所有文件名使用全英文小写，单词间用`-`分隔，如`some-important-doc.md`
- 所有文字文档均使用Markdown格式（`.md`）
- 其它包含图的文档可以
    + 使用PDF格式
    + 直接使用图片（`.png`）
    + 使用图片内嵌入Markdown文档中
- 文字文档中所有可能包含的图片附件存放在文档所在目录的`images`文件夹下，命名为`<文档名>-<图片名>.png`，例如文档`management-docs/about.md`需要引用图`some-pic.png`，则图片的路径为`management-docs/images/about-some-pic.png`

