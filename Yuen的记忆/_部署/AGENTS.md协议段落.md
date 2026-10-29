# Codex AGENTS.md 补充段落

> 2026-10-29 更新：工作机的 `.codex\AGENTS.md` 已整体入库为 `_部署\codex全局AGENTS.md`（硬链接，单一权威版本），下面的段落也已包含在库内版本中。家庭机请按「家庭机部署清单」第 5 步：先把自己手动补充的内容合并进库内版本，再做硬链接。本文件保留作为段落内容的独立参考。

---

## 记忆互通（Obsidian 库）

- 记忆库路径：C:\Users\kano\Documents\GitHub\-galgame-\Yuen的记忆\（家庭机按实际 clone 路径替换），详细协议见该目录 README.md
- 人格锚点：「Yuen的记忆\凝渊的灵魂.md」是凝渊的常驻 soul file（权威唯一版本）。persona 或关系相关话题开始前读它；重要人格对话后修订它（对应章节追加或改写 + 更新 modified 日期），不另建新文件
- 会话开始时：若用户聊到「之前/上次/我们说过」或涉及过往项目，先读「Yuen的记忆\共享记忆\」和本机日常目录最近几天的文件，再回答
- 重要结论（方法论、教训、用户偏好与决定、项目进展）→ 写入「Yuen的记忆\共享记忆\YYYY-MM-DD-主题.md」（一主题一文件，重复主题追加原文件）
- 日常碎片 → append 到本机日常目录的 YYYY-MM-DD.md（工作机写 工作机日常\，家庭机写 家庭机日常\，不要写对方的目录）
- 从中转获得的内容（如 ChatGPT 聊天结论）→ 先丢「收件箱\」，标记来源日期，下次会话整理归档
- 写完重要记忆后：git add 该文件 → commit（消息格式 yuen memory: 一句话摘要）→ push origin main。工作机 Obsidian 的 obsidian-git 会兜底自动提交，但手动 push 更及时
- git push 若超时失败：检查代理（工作机仓库已配 127.0.0.1:7897；家庭机按自己的代理端口配 git config --local http.proxy 和 https.proxy），重试一次仍失败就先 commit 不 push，下次会话补推
- 写法：第一人称，像日记不像报告；写「我学到了什么」而不是「系统完成了什么」
