# clinical-med-study-coach · 临床医学本科学习教练（Claude Skill）

一个让 Claude 化身「绩点前 5% / 4.4+ 临床医学学长学姐」的 **Agent Skill**。它不讲知识点，而是手把手教你**怎么学、怎么复习、怎么排时间、怎么扛过考前**——给的是踩过坑、能直接落地的具体打法，而不是"要努力、要预习"这类正确的废话。

> 语言：内容为简体中文，面向中国临床医学本科生。
> 类型：Agent Skill（遵循 `SKILL.md` 开放标准，可用于 Claude.ai / Claude Code / Claude API）。

---

## 它能做什么

向 Claude 提问时，凡是涉及**学习策略**的，这个 skill 会自动接管并给出针对性建议：

- 多门课怎么排优先级、水课怎么应付、平时分怎么全拿
- 不同学科用不同打法（逻辑链型 / 搭积木型 / 情境淘金型 / 记忆量大型 / 水课型）
- 记不住、看不进书、时间不够怎么破
- 考前的**三轮复习法**全流程（提前 3–4 周如何排日历）
- 一天怎么安排、"一直在学却没效果"的真实原因
- 精神累 / 身体累的分型恢复，以及考前坐不住时的应急办法
- 错题怎么系统化、基础医学怎么衔接到临床、保研视角的长期规划

**它不做什么**：不替代讲解具体医学概念。如果你要的是"讲讲补体激活通路 / 臂丛神经走行"，那是知识讲解，直接问 Claude 即可，不归这个 skill 管。

---

## 目录结构

仓库根目录**就是**这个 skill 文件夹，方便直接克隆使用。

```
clinical-med-study-coach/          # 仓库根 = 技能文件夹（名字须和 SKILL.md 里的 name 一致）
├── SKILL.md                       # 主文件：教练身份 + 核心信念 + 学科分型路由 + 高频打法（始终加载）
├── references/                    # 详细内容，Claude 按需调取，平时不占用上下文
│   ├── study-methods.md           #   学科分型详解、记忆术、看书法、三轮复习法、各科经验
│   ├── habits-and-energy.md       #   平时习惯、时间安排、能量恢复、心态
│   └── supplements.md             #   【教练补充】错题系统、基础→临床衔接、科研入门、长期规划
├── README.md                      # 本文件（仓库说明，Claude 不读）
├── LICENSE                        # 许可证
├── .gitignore
└── dist/
    └── clinical-med-study-coach.zip   # 预打包好的上传文件（用于 Claude.ai；可选）
```

**为什么这样放（设计取舍）**
- **`SKILL.md` 放在根目录**：这样仓库本身就是一个合法的技能文件夹，克隆下来即可直接用于 Claude Code，无需再挪动。
- **`references/` 分文件**：Skill 用"渐进式加载"——名字+描述常驻、`SKILL.md` 触发时加载、reference 用到才读。把详细内容拆进 `references/` 能让主文件保持精简、触发更准。
- **`README` / `LICENSE` / `dist/` 与技能文件并存无害**：Claude 只读 `SKILL.md` 及它引用的文件，仓库说明类文件会被忽略。
- **`dist/` 里的 zip 是构建产物**：方便直接上传到 Claude.ai。若不想把产物提交进仓库，可改为放到 GitHub Releases（见下）。

> 如果将来你要做**多个 skill**，建议改成"合集仓库"结构：根目录放 `README`/`LICENSE`，所有技能放进 `skills/<技能名>/`。本仓库目前是单技能，用根目录布局最简单。

---

## 安装与使用

> ⚠️ 三个平台**互不同步**：在 Claude.ai 上传的技能不会自动出现在 API 或 Claude Code，反之亦然。需要哪个就在哪个平台单独装。

### 1) Claude.ai（最简单，适合直接聊天用）
需要 Pro / Max / Team / Enterprise 等付费方案。
1. 右上角头像 → **Settings（设置）** → **Features（功能）** → **Skills（技能）**
2. 选择**上传自定义技能**，上传一个 **ZIP** 文件
3. ZIP 内必须包含一个**含有 `SKILL.md` 的文件夹**——直接用 `dist/clinical-med-study-coach.zip` 即可；或把 `clinical-med-study-coach/` 文件夹自己压成 zip 上传
4. 之后正常聊天，问到学习相关问题时 Claude 会自动调用

> 注：仓库里若提供的是 `.skill` 文件，它本质就是 zip，把后缀改成 `.zip` 再上传即可。

### 2) Claude Code（命令行 / 跟项目走）
技能是文件系统里的文件夹，放进对应目录即可：

```bash
# 全局可用（你自己的所有项目都能用）
git clone https://github.com/<你的用户名>/clinical-med-study-coach \
  ~/.claude/skills/clinical-med-study-coach

# 或只在某个项目里共享（提交进该项目仓库，队友克隆即得）
#   <项目>/.claude/skills/clinical-med-study-coach/
```
重启 Claude Code 后用 `/skills` 确认已加载。

### 3) Claude API
通过 Skills API 上传使用。具体见官方文档：<https://docs.claude.com>（搜索 "Agent Skills" / "Skills API"）。

---

## 用起来长什么样（示例）

直接像跟学长学姐聊天那样问就行，比如：

- "下学期六门专业课加两门水课，时间根本不够，怎么排？"
- "还有三周考病理生理，完全没头绪，帮我排个复习计划"
- "生化大题每次都答不到点上，怎么破？"
- "天天泡图书馆但分数上不去，问题出在哪？"
- "考前两天背不进去、坐不住，怎么办？"

Claude 会先快速判断你的**课型 / 距考试多远 / 卡在哪个症状**，再给针对性的下一步，而不是甩一堆通用建议。

---

## 内容来源与透明度

- **核心方法**复刻自一位绩点前 5% / 4.4+ 临床医学生公开分享的学习经验，尽量保留其原话与具体打法。
- **`references/supplements.md` 中的内容是在原方法基础上补充的**（错题系统、基础→临床衔接、科研入门、长期规划），文件内已逐条标注"教练补充，原文未覆盖"，以区别于原作者的经验。
- **题库 / 笔记 / 公众号 / UP 主等具体名称是按某一所学校的情况举例**（如某些题库、"天马"大题资料、相关公众号与 B 站 UP 主）。换到别的学校，请替换成本校对应资源——**原则不变**：跟着老师定位重点、用解析最全的资料、以真题为靶、有策略地重复。


---

## 适配你自己的学校 / 个人情况

最值得改的两处：
1. `references/study-methods.md` 末尾的**资源清单**——换成你们学校真实在用的题库、笔记、经验号、老师推荐的网课。
2. `SKILL.md` 的**学科分型表**——按你们专业的课程补充（如解剖、药理、诊断学各归哪一型）。

改完后若用于 Claude.ai，记得重新打包上传（见下）。

---

## 修改与重新打包

技能就是纯文本文件，直接用任意编辑器改 `SKILL.md` 或 `references/*.md` 即可。改完重新生成上传用的 zip：

```bash
# 在仓库根目录执行：把技能文件夹（不含 README/LICENSE/dist）打包
zip -r dist/clinical-med-study-coach.zip clinical-med-study-coach \
  -x "clinical-med-study-coach/README.md" \
     "clinical-med-study-coach/LICENSE" \
     "clinical-med-study-coach/.gitignore" \
     "clinical-med-study-coach/dist/*"
```
> 更干净的做法：不提交 zip，改用 **GitHub Releases** 上传打好的包，给用户提供版本化下载。

---

## 免责声明

本仓库提供的是**学习方法与习惯建议**，基于个人经验，效果因人、因校、因课而异，不构成任何保证；也**不涉及临床诊疗建议**。请结合自身情况和本校实际取用。

---

## 许可证

见 [LICENSE](./LICENSE)。

## 致谢

- 学习方法核心来自一位临床医学生的公开经验分享小红书@全球美食品鉴家
