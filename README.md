# Chaoxing Assistant

> AI Agent 技能：让 Claude 帮你自动刷学习通网课

[![skills.sh](https://img.shields.io/badge/skills.sh-install-blue)](https://skills.sh)
[![license](https://img.shields.io/badge/license-MIT-green)](./LICENSE)

一个 Claude Code Skill，通过 opencli 浏览器驱动，让 AI Agent 自动完成超星学习通（Chaoxing/Xuexitong）的在线课程任务。

## 它能做什么

- 自动登录检测 + 课程列表扫描
- 自动播放课程视频，处理弹窗验证（"继续观看"、人机验证）
- 自动回答视频中途弹出的测验题目
- 追踪每节课的完成进度（绿勾/百分比）
- 检查待完成的作业与考试列表

## 前置条件

| 依赖 | 说明 |
|------|------|
| [OpenCLI](https://github.com/jackwener/OpenCLI) | 浏览器驱动工具，需安装 Chrome 扩展 |
| Chrome/Chromium | 已登录学习通账号的浏览器 |
| Claude Code | AI Agent 运行环境 |

## 安装

```bash
npx skills add mk42langzi/chaoxing-assistant@chaoxing-assistant -g -y
```

## 使用

安装后在 Claude Code 里直接说：

```
帮我刷学习通的课
自动看超星视频
帮我把这学期的网课刷完
```

Agent 会自动触发 `chaoxing-assistant` 技能，按五个阶段执行：

```
Phase 1: 检查环境    → opencli doctor
Phase 2: 扫描课程    → opencli browser open 课程中心
Phase 3: 检查作业    → opencli chaoxing assignments
Phase 4: 播放视频    → opencli browser 控制播放 + 处理弹窗
Phase 5: 追踪进度    → opencli browser snapshot
```

## 工作原理

```
用户: "帮我刷课"
  │
  ▼
Claude + chaoxing-assistant skill
  │
  ├─→ opencli chaoxing assignments/exams   (数据查询)
  ├─→ opencli browser open/navigate/click  (浏览器操控)
  ├─→ opencli browser snapshot             (页面状态读取)
  └─→ opencli browser find/click           (弹窗处理)
```

## 注意事项

- **需要提前在 Chrome 里登录学习通**，Skill 不会帮你输入密码
- 视频播放期间学习通会不定时弹窗验证，Agent 会定时轮询处理
- 操作间隔模拟人类速度，降低被检测风险
- 本项目仅供学习 AI Agent 开发技术，请遵守学校相关规定

## 文件结构

```
chaoxing-assistant/
├── SKILL.md         # 技能指令文件（Agent 读取的核心）
├── README.md        # 项目说明
└── scripts/         # 辅助脚本（待扩展）
```

## License

MIT
