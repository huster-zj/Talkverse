# Talkverse MVP — 附录（技术 / 竞品 / 选型参考）

> 本附录承载 PRD 正文放不下的技术细节与"方案对比"类信息。它不是需求，是给后续 UX / 架构 / 调研用的背景。

## 1. 市场与竞品扫描（2026-11 快扫，未逐一核实）

- **Character.AI** — 品类龙头（2000万+ MAU），2024-06 加了双向"Character Calls"语音；名人/历史人物由用户创建并声明为虚构。→ 参考产品 + 其法律定位（"角色说的都是虚构"，部分名人角色因 ToS 被下架）。
- **Call Annie（Animato）** — 实时视频/语音 AI 陪伴，验证了低延迟语音通话的体验与订阅模式（~$9.99/月）。
- **BanterAI** — 与名人/专家的 AI 克隆实时语音通话（Musk、Obama、Swift 等），sub-2.5s，用户可自建克隆。→ 直接竞品，几乎就是"给名人克隆打电话"。
- **CallCeleb / Famous Friends** — 免注册名人语音通话 App，用"真实克隆音色"（40+ 名人/历史人物）。→ 进一步验证品类需求。
- **Meta AI 名人语音** — Meta 2024 付费授权真实名人（John Cena、Judi Dench 等）做助手语音；更早未付费的"AI 网红"机器人被砍。→ 关键法律信号：合规路径是"付费授权"，不是"擅自克隆"。

## 2. 开源参考项目（蒸馏 + 语音克隆）

- **SillyTavern** — 最成熟的开源 LLM 角色扮演前端，支持角色卡 + 用户人设 + 多 TTS 后端逐角色绑定音色。→ "人物卡 + 音色绑定"的参考架构。
- **YeJe-cpu/talk-to-fengge** — 实时(<1s)语音对话，克隆任意人的音色+人格；栈 = LiveKit + STT + LLM + 克隆 TTS（VoxCPM/Cartesia）。→ 几乎就是 Talkverse 的管线（语音进 → 人物 LLM → 克隆音色出）。
- **MatheusSimonaci/clone-talking** — 手机优先"和任意人物 AI 克隆对话"，人物为 `.md` + YAML，OpenRouter LLM + Whisper STT + VoiSpark 克隆。→ 极简 MVP 参考。
- **vogtsw/boss-skills / sickn33/agentic-awesome-skills / xixu-me/awesome-persona-distill-skills** — 一批"蒸馏某个人"的提示词技能库（含 Musk/Jobs 现成人物）。→ 人物蒸馏的现成模板。
- **yzfly/Mind-Cloning-Engineering** — "认知数字孪生"技能，含预装名人（Jobs 等），模拟决策/偏见而非仅语气。→ "蒸馏思路"的进阶模型。
- **GPT-SoVITS / Coqui XTTS-v2 / Chatterbox** — 开源少样本语音克隆 TTS（GPT-SoVITS ~1 分钟音频、XTTS ~5s、Chatterbox ~10s 带情绪标签）。→ MVP 克隆音色的免费路线（注意：克隆真人声线有法律红线，PRD 已约束用原创/授权音色）。

## 3. 语音克隆 API 短名单
- **ElevenLabs** — 质量最高（~30s 音频），约 3000 音色。
- **Cartesia Sonic** — 延迟最低（首音 ~90ms），适合实时语音。
- **MiniMax Speech** — 最便宜 / 多语言（~10s 克隆）。
- 三者都对"克隆名人"做同意/验证门槛。

## 4. 技术栈方向（README 候选 + 本次补充）
- 后端：Python / FastAPI；前端：Vue 3 / TS（网页）。
- ASR：faster-whisper 等；TTS：Qwen3-TTS / CosyVoice / GPT-SoVITS 等。
- 实时语音：LiveKit / Pipecat 可作为"低延迟 + 打断"的现成骨架参考。
- 人物配置：人物卡（Profile/Personality/Knowledge/Voice/Sources）分模块维护，与 LLM 解耦。
