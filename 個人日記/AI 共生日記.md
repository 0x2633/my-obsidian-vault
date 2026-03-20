---
tags:
  - claude
  - flutter
  - gemini
---
# 20260305
---
與 AI 聊天的過程比結果重要，過程決定結果。我的流程是從 BRD -> PRD -> SPEC -> TASK，但我在具備 SPEC 的時候，從  Gemini 3 Pro 轉換到 Claude Opus 4.6 開始進行開發。

AI 透過 SPEC 生成了 4 個 Sprint 進行開發，在過程中 AI 還是常會忽略一些規則，並不會完全如我們的願，比如: 我規定他使用 TDD ，在一開始的階段有，但後來不知為何就忘記要這麼做，導致Sprint 1 - Sprint 2 沒有 TDD 的實作，但在了解 TDD 之後，==在小專案中是沒有必要所有功能都進行實作，反而僅需要針對重要功能實作測試即可==

# 20260318
---
今天從 AI 身上學到一件事 -- 當指令越來越複雜的時候，就要利用 [Makefile](https://makefiletutorial.com/#getting-started)來減少打指令的次數