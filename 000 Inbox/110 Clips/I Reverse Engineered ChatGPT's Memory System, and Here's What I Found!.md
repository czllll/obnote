---
title: "I Reverse Engineered ChatGPT's Memory System, and Here's What I Found!"
source: "https://manthanguptaa.in/posts/chatgpt_memory/"
author:
  - "[[Manthan Gupta]]"
published: 2001-12-09
created: 2025-12-11
description: "When I asked ChatGPT what it remembered about me, it listed 33 facts from my name and career goals to my current fitness routine. But how does it actually store and retrieve this information? And why does it feel so seamless?After extensive experimentation, I discovered that ChatGPT’s memory system is far simpler than I expected. No vector databases. No RAG over conversation history. Instead, it uses four distinct layers: session metadata that adapts to your environment, explicit facts stored long-term, lightweight summaries of recent chats, and a sliding window of your current conversation."
tags:
  - "clippings"
---
When I asked ChatGPT what it remembered about me, it listed 33 facts from my name and career goals to my current fitness routine. But how does it actually store and retrieve this information? And why does it feel so seamless?

After extensive experimentation, I discovered that ChatGPT’s memory system is far simpler than I expected. No vector databases. No RAG over conversation history. Instead, it uses four distinct layers: session metadata that adapts to your environment, explicit facts stored long-term, lightweight summaries of recent chats, and a sliding window of your current conversation.

This blog breaks down exactly how each layer works and why this approach might be superior to traditional retrieval systems. Everything here comes from reverse engineering ChatGPT’s behavior through conversation. OpenAI did not publish these implementation details.这篇博客详细解析了每一层的工作原理，并阐释了为何这种方法可能优于传统检索系统。所有分析均源自通过对话对ChatGPT行为进行的逆向工程，OpenAI官方并未公布这些实现细节。

## ChatGPT’s Context StructureChatGPT的上下文结构

Before understanding memory, it’s important to understand the entire context ChatGPT receives for every message. The structure looks like this:在理解记忆机制之前，有必要先了解ChatGPT处理每条消息时所接收的完整上下文信息。其结构如下所示：

```
[0] System Instructions
[1] Developer Instructions
[2] Session Metadata (ephemeral)
[3] User Memory (long-term facts)
[4] Recent Conversations Summary (past chats, titles + snippets)
[5] Current Session Messages (this chat)
[6] Your latest message
```

The first two components define high-level behavior and safety rules. They aren’t the focus of this blog. The interesting pieces begin with Session Metadata.前两个组件定义了高层次的行为和安全规则。它们并非本文的重点。真正有趣的部分从会话元数据开始。

These details are injected once at the beginning of a session. They are not stored permanently and don’t become part of long-term memory. This block includes:这些数据仅在会话开始时注入一次。它们不会被永久存储，也不会成为长期记忆的一部分。该数据块包含：

- Device type (desktop/mobile) 设备类型（桌面/移动端）
- Browser + user agent 浏览器 + 用户代理
- Rough location/timezone 大致位置/时区
- Subscription level 订阅级别
- Usage patterns and activity frequency 使用模式与活动频率
- Recent model usage distribution 近期模型使用分布情况
- Screen size, dark mode status, JS enabled, etc.屏幕尺寸、深色模式状态、JavaScript启用状态等

An example of session metadata is:会话元数据的示例包括：

```
Session Metadata:
- User subscription: ChatGPT Go
- Device: Desktop browser
- Browser user-agent: Chrome on macOS (Intel)
- Approximate location: India (may be VPN)
- Local time: ~16:00
- Account age: ~157 weeks
- Recent activity:
    - Active 1 day in the last 1
    - Active 5 days in the last 7
    - Active 18 days in the last 30
- Conversation patterns:
    - Average conversation depth: ~14.8 messages
    - Average user message length: ~4057 characters
    - Model usage distribution:
        * 5% gpt-5.1
        * 49% gpt-5
        * 17% gpt-4o
        * 6% gpt-5-a-t-mini
        * etc.
- Device environment:
    - JS enabled
    - Dark mode enabled
    - Screen size: 900×1440
    - Page viewport: 812×1440
    - Device pixel ratio: 2.0
- Session duration so far: ~1100 seconds
```

This information helps the model tailor responses to your environment, but none of it persists after the session ends.这些信息能帮助模型根据你的使用场景定制回复，但所有数据都会在会话结束后清空。

## User Memory用户记忆

ChatGPT has a dedicated tool for storing and deleting stable, long-term facts about the user. These are the pieces that accumulate over weeks and months to form a persistent “profile.” ChatGPT配备了一个专门工具，用于存储和删除与用户相关的长期稳定信息。这些数据经过数周数月的积累，最终会形成一个持久化的"用户画像"。

In my case, the model had 33 stored facts — things like:就我而言，这个模型存储了33条固定信息，例如：

- My name, age 我的姓名、年龄
- Career goals 职业目标
- Background and past roles 背景与过往角色
- Current projects 当前项目
- Areas I am studying 我正在研究的领域
- Fitness routine 健身日常
- Personal preferences 个人偏好
- Long-term interests 长期兴趣

These are not guessed; they are explicitly stored only when:这些并非猜测所得；仅在以下情况才会被明确存储：

- The user says “remember this” or “store this in memory”, or 当用户说"记住这个"或"将此存入记忆"，或
- The model detects a fact that fits OpenAI’s criteria (like your name, job title, or stated preferences) and the user implicitly agrees through conversation 模型会检测符合OpenAI标准的事实（如你的姓名、职位或已声明的偏好），并通过对话获得用户的默许

These memories are injected into every future prompt as a separate block.这些记忆会作为独立模块注入到未来的每个提示中。

If you want to add or remove anything, you can simply say:如果你想添加或删除任何内容，只需说：

- “Store this in memory…” “将此信息存入记忆…”
- “Delete this from memory…” “从记忆中删除这个…”

Example:示例：

```
- User's name is Manthan Gupta.
- Previously worked at Merkle Science and Qoohoo (YC W23).
- Prefers learning through a mix of videos, papers, and hands-on work.
- Built TigerDB, CricLang, Load Balancer, FitMe.
- Studying modern IR systems (LDA, BM25, hybrid, dense embeddings, FAISS, RRF, LLM reranking).
```

This part surprised me the most, because I expected ChatGPT to use some kind of RAG across past conversations. Instead, it uses a lightweight digest.这部分最令我惊讶，因为我原以为ChatGPT会使用某种跨对话检索增强生成技术，结果它采用的是轻量级摘要方案。

ChatGPT keeps a list of recent conversation summaries in this format:ChatGPT采用以下格式保存最近对话摘要：

```ruby
1. <Timestamp>: <Chat Title>
|||| user message snippet ||||
|||| user message snippet ||||
```

Observations:观察结论：

- It only summarizes my messages, not the assistant’s.它仅汇总我的发言内容，不包括助手回复。
- There were around 15 summaries available.近期大约保存了15条摘要记录。
- They act as a loose map of my recent interests, not detailed context.这些摘要犹如我近期兴趣的简易地图，而非详尽语境。

This block gives ChatGPT a sense of continuity across chats without pulling in full transcripts.该功能让ChatGPT能够在聊天间保持连贯性，而无需载入完整对话记录。

Traditional RAG systems would require:传统RAG系统需要：

- Embedding every past message 嵌入所有历史消息
- Running similarity searches on each query 对每个查询执行相似性搜索
- Pulling in full message contexts 提取完整消息上下文
- Higher latency and token costs 更高的延迟和token开销

ChatGPT’s approach is simpler: pre-compute lightweight summaries and inject them directly. This trades detailed context for speed and efficiency.ChatGPT的做法更为简洁：预先计算轻量级摘要并直接注入。这种做法以牺牲详细上下文为代价，换取速度与效率。

## Current Session Messages当前会话消息

This is the normal sliding window of the present conversation. It contains the full history (not summarized) of all messages exchanged in this session.这是当前对话的标准滑动窗口机制。它完整记录了本次会话中所有消息的历史（未经摘要处理）。

I wasn’t able to get the exact token limit out of ChatGPT but it did confirm:虽然未能从ChatGPT处获取确切的令牌上限，但已确认：

- The cap is based on token count, not number of messages 限制基于令牌数量，而非消息条数
- Once the limit is reached, older messages in the current session roll off (but memory facts and conversation summaries remain) 当达到限制后，当前对话中的较早消息会被移除（但记忆事实和对话摘要仍会保留）
- Everything in this block is passed verbatim to the model, maintaining full conversational context 此区块内容将原样传递给模型，完整保留会话上下文

This is what allows the assistant to reason coherently within a session.这正是使助手能在会话中保持思维连贯性的关键

## How It All Works Together各环节如何协同运作

When you send a message to ChatGPT, here’s what happens:当你向ChatGPT发送消息时，会发生以下流程：

1. **Session starts**: Session metadata is injected once, giving ChatGPT context about your device, subscription, and usage patterns.会话启动：会话元数据一次性注入，为ChatGPT提供设备配置、订阅状态及使用习惯等背景信息
2. **Every message**: Your stored memory facts (33 in my case) are always included, ensuring responses align with your preferences and background.所有消息：您存储的记忆信息（如我的33条记忆）始终会被包含，确保回复符合您的偏好和背景。
3. **Cross-chat awareness**: The recent conversations summary provides a lightweight map of your interests without pulling in full transcripts.跨聊天感知：最近的对话摘要提供了您兴趣点的轻量级映射，无需调取完整对话记录。
4. **Current context**: The sliding window of current session messages maintains coherence within the conversation.当前上下文：当前会话的消息滑动窗口保持对话连贯性。
5. **Token budget**: As the session grows, older messages roll off, but your memory facts and conversation summaries remain, preserving continuity.令牌预算：随着对话的进行，较早的消息会被移除，但您的记忆要点和对话摘要会保留下来，确保对话的连贯性。

This layered approach means ChatGPT can feel personal and context-aware without the computational cost of searching through thousands of past messages.这种分层设计意味着ChatGPT能够提供个性化和上下文感知的体验，而无需承担检索数千条历史消息的计算成本。

## Conclusion结论

ChatGPT’s memory system is a multi-layered architecture that balances personalization, performance, and token efficiency. By combining ephemeral session metadata, explicit long-term facts, lightweight conversation summaries, and a sliding window of current messages, ChatGPT achieves something remarkable: it feels personal and context-aware without the computational overhead of traditional RAG systems.ChatGPT的记忆系统采用多层架构设计，在个性化体验、响应性能和标记效率之间实现了巧妙平衡。通过整合瞬时会话元数据、显式长期事实、轻量级对话摘要以及滑动窗口式的最新消息处理机制，该系统达成了一项突破性成就：既能提供富有个人特色且上下文感知的交互体验，又避免了传统RAG系统带来的巨大计算负担。

The key insight here is that not everything needs to be “memory” in the traditional sense. Session metadata adapts to your environment in real-time. Explicit facts persist across sessions. Conversation summaries provide continuity without detail. And the current session maintains coherence. Together, these dynamic components—each updated as the session progresses and your preferences evolve—create the illusion of a system that truly knows you.这里的关键洞察在于：并非所有内容都需要传统意义上的"记忆"。会话元数据会实时适应你的使用环境，显性事实能在不同会话间持续存在，对话摘要提供连续性而无需保留细节，当前会话则保持连贯性。这些动态组件随着会话推进和你的偏好演变而不断更新，共同营造出一个真正"了解"你的系统假象。

For users, this means ChatGPT can feel increasingly personal over time without requiring explicit knowledge base management. For developers, it’s a lesson in pragmatic engineering: sometimes simpler, more curated approaches outperform complex retrieval systems, especially when you control the entire pipeline.对于用户而言，这意味着ChatGPT无需专门的知识库管理就能随时间推移逐渐形成个性化体验。对于开发者，这揭示了务实工程的真谛：当能够掌控全流程时，经过精心设计的简洁方案往往能胜过复杂的检索系统。

The trade-off is clear: ChatGPT sacrifices detailed historical context for speed and efficiency. But for most conversations, that’s exactly the right balance. The system remembers what matters (your preferences, goals, and recent interests) while staying fast and responsive.这种权衡很明显：ChatGPT为了速度和效率牺牲了详细的历史背景。但对于大多数对话来说，这恰恰是恰到好处的平衡。系统在保持快速响应能力的同时，会记住重要信息（你的偏好、目标和近期兴趣）。

---

*This blog is based on experimentation and reverse engineering through conversation, not official documentation—so take it with a grain of salt. If you found this interesting, I’d love to hear your thoughts. Share it on [Twitter](https://twitter.com/manthanguptaa), [LinkedIn](https://www.linkedin.com/in/manthanguptaa/), or [Peerlist](https://peerlist.io/manthangupta), or reach out at guptaamanthan01\[at\]gmail\[dot\]com.本博客内容基于实验性探索及对话式逆向工程所得，并非源自官方文档——故请持保留态度审阅。若您觉得这些发现有趣，我很乐意聆听您的见解。欢迎通过Twitter、LinkedIn或Peerlist分享，亦可发送邮件至guptaamanthan01\[at\]gmail\[dot\]com联系。*