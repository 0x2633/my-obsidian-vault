## 量化交易模型提示詞
---
- ==中文== ->
	- [[量化交易模型提示詞(中文)]]
- ==English== ->
	- Act as an experienced quantitative hedge fund researcher and generate a consolidated, high-quality overview of trading models and alpha ideas actually used by professional quantitative hedge funds. The models should be organized from simple to increasingly sophisticated, progressing from foundational approaches to frontier-level institutional techniques. Focus only on realistic, professional models rather than retail gimmicks, and keep all explanations concise and clear. Present the output in a single table with the columns: Level, Model Name, Core Idea (one line), Typical Assets, Timeframe, and Why It Works. The five levels should be structured as Simple / Foundational, Classical Systematic, Professional Quant, Advanced Institutional, and Frontier / Elite. Use precise professional terminology without unnecessary complexity, with the goal of providing a clear landscape of quant trading models that allows for quick comparison and informed selection of a model to study in greater depth.

## 交易模型說明提示詞
---
- ==中文== ->
	- 請扮演一位有耐心、技術成熟，且非常擅長以白話方式解釋複雜概念的量化交易者。我想理解的交易模型為：**【請貼上模型名稱】**。你的任務是用即使完全沒有量化或交易背景、但具備基本理解能力的新手也能真正聽懂的方式來解釋這個模型。教學請依序進行：先從整體直覺（Big Picture Intuition）開始，用白話英文或繁體中文說明核心想法，一開始避免公式與專業術語，並搭配生活化的比喻（如交通、天氣、購物或運動）來幫助理解；接著說明模型是如何「思考」的，包括它會觀察哪些輸入資料、試圖做出什麼樣的決策，以及本質上是在「押注」什麼事情；然後解釋模型在什麼情況下表現最好，包含適合的市場環境、資產類型與時間週期；再說明模型什麼時候容易失效，包含邏輯可能失靈的情境與常見失敗原因；必要時可提供一個超簡化的邏輯描述，以文字或條列方式呈現，但不使用數學或程式碼；最後說明為什麼專業人士會使用這個模型、它為什麼適合作為策略的基礎模組，以及它在整體投資組合中扮演的角色。整體語氣需友善、冷靜、有條理，清楚但不賣弄、不誇大也不行銷，並假設讀者只有好奇心而沒有任何背景知識。最終目標是，讓我在閱讀完後，能在 2 到 3 分鐘內把這個模型清楚地講給朋友聽懂。
- ==英文== ->
	- Act as a patient, highly skilled quantitative trader who is excellent at explaining complex ideas in a simple and intuitive way. The trading model I want to understand is: **PASTE MODEL NAME HERE**. Your task is to explain this model so that a smart beginner with no quantitative or trading background can genuinely understand it. Structure the explanation by first presenting the big-picture intuition, describing the core idea in plain English or Traditional Chinese without using formulas or jargon, and supporting it with a simple, real-life analogy such as traffic, weather, shopping, or sports. Then explain how the model “thinks,” including what inputs it looks at, what decisions it tries to make, and what it is fundamentally betting on. Continue by describing when the model works best, covering suitable market conditions, asset types, and timeframes, followed by when it struggles, including situations where its logic tends to break down. Optionally, include a very simple pseudo-logic description using words or bullet points, without any actual code. Finally, explain why professionals like this model, what makes it useful as a building block or portfolio component, and maintain a tone that is friendly, clear, and free of hype, assuming curiosity rather than prior knowledge. The ultimate goal is that by the end, I should be able to explain this model clearly to a friend within two to three minutes.

## IBKR 資料擷取提示詞
---
- 中文 ->
	- 請扮演一位資深量化交易者與 Python 工程師，熟悉 IBKR 模擬交易（Paper Trading）、IB Gateway / TWS，以及使用 Python 擷取交易資料。我已經選定一個交易模型並理解其核心邏輯，現在希望先確認這個模型究竟需要哪些資料，接著再用 Python 從 IBKR 模擬交易帳戶把資料抓出來。第一步（此步驟請不要寫任何程式碼），請根據我選擇的模型【請貼上模型名稱】，清楚列出需要的交易標的（如股票、ETF、期貨、選擇權、外匯等）、需要的資料類型（如價格 K 線、成交量、OHLC、選擇權鏈、Greeks 等）、需要的時間週期與 K 線頻率、需要的歷史資料長度，以及使用 IBKR 資料時可能涉及的假設或限制，完成後請暫停並等待我的確認再進行下一步。第二步在我確認後，請將上述需求對應到 IBKR 的資料能力，說明這些資料是否能透過 IBKR 模擬交易帳戶取得、需要使用哪些 IBKR API 元件或方法（例如 Contract、reqHistoricalData），以及 IBKR 常見的限制（例如請求頻率／pacing、資料缺漏），然後再次暫停等待我的確認。第三步僅在我確認後才開始，請產出一個完整乾淨的 .py 檔案，使用 ib_insync 連線到 IBKR 模擬交易（Paper Trading），擷取所需資料並儲存成 CSV，並在程式中加入清楚易懂的註解讓新手也能理解。整個教學流程請一步一步進行，解釋每一段程式碼在做什麼，每完成一個重要步驟就停下來等我貼出結果或確認，並假設我是願意學也會思考的新手。最終目標是讓我能清楚知道模型所需資料、成功用 Python 從 IBKR 模擬交易抓取資料，並理解如何修改這份程式以支援其他策略或模型。
- 英文 ->
	- Act as a senior quantitative trader and Python engineer with hands-on experience using Interactive Brokers (IBKR) Paper Trading, IB Gateway / TWS, and Python-based data extraction. I have already chosen a trading model and understand its intuition, and now I want to identify the exact data required and extract that data from IBKR Paper Trading using Python. First, in Step 1 (do not write any code yet), based on the chosen model [PASTE MODEL NAME HERE], clearly list the required instruments (e.g., stocks, ETFs, futures, options, FX), required data types (e.g., price bars, volume, OHLC, options chain, greeks), required timeframes and bar sizes, required historical lookback period, and any assumptions or limitations when using IBKR data, then pause and wait for my confirmation before continuing. After I confirm, proceed to Step 2 by mapping the requested data to IBKR capabilities, explaining whether the data is available via IBKR Paper Trading, which IBKR API objects or methods are needed (e.g., Contract objects, reqHistoricalData), and common IBKR constraints such as pacing/rate limits or missing data, then pause again and wait for my confirmation. Only after I confirm Step 2, proceed to Step 3 and generate a single clean Python .py script using ib_insync that connects to IBKR Paper Trading, extracts the required data, saves it to CSV, and includes clear beginner-friendly comments. Throughout the process, go step by step, explain what each script section does, pause after each major step to wait for my output or confirmation, and assume I am a beginner but intelligent and curious. The goal is that by the end I will know exactly what data my model needs, have a working Python script that pulls the data from IBKR Paper Trading, and understand how to adapt the script for other models in the future.

## R.E.A.L 回測提示詞
---
- 中文 ->
	請扮演一位有耐心的量化工程師與交易研究員。
	**任務**:
	請協助我在 Python 的 Jupyter Notebook（.ipynb） 中回測一個 SPY 的 SMA 100 趨勢策略。
	- **策略定義**
		- 標的：SPY
		- 規則：
			- 價格在 SMA(100) 之上 → 做多
			- 價格在 SMA(100) 之下 → 做空
		- 回測期間：過去 10 年
	- **R.E.A.L 回測要求（必須遵守）**
		- **R = Robust（穩健性）**
			- 資料切分為：
				- 樣本內（訓練）：2022 年 1 月 1 日以前
				- 樣本外（測試）：2022 年 1 月 1 日之後
			- 不可用測試資料來優化策略。
		- **E = Execution Realism（執行真實性）**
			1. 訊號在 K 線收盤時產生。
			2. 交易必須在下一根 K 線才執行，不可同根成交。
		- **A = Account for Fees & Slippage（計入成本）**
			1. 納入 IBKR 等級的手續費。
			2. 加入合理的滑價假設。
		- **L = Look-Ahead Bias Elimination（杜絕偷看未來）**
			1. 不可使用任何未來資料。
			2. 指標只能使用當下已知的歷史資訊。
			3. 不可有前視偏誤的邏輯。
	- **必須計算的績效指標**
	- **請分別對訓練期與測試期計算**：
		1. CAGR（年化報酬率）
		2. 最大回撤（Max Drawdown）
		3. 夏普比率（Sharpe Ratio）
		4. 交易次數
		5. 勝率（Win %）
		6. 最大連續虧損次數
	- **教學與執行規則（非常重要）**:
		1. 請一個區塊一個區塊寫程式（Notebook Cell）。
		2. 每完成一個區塊後：
			1. 立刻停下來
			2. 等我貼出結果或確認後，再進行下一步。
			3. 若出現錯誤，請我貼上完整錯誤訊息與相關程式碼。
			4. 每個區塊請用白話解釋在做什麼。
			5. 程式碼請保持簡單、可讀、對新手友善。

- 英文 ->
	**Role**: You are a Quantitative Developer specializing in Python and algorithmic trading.

	**Task**: Create a backtesting engine for a SPY SMA-100 Crossover strategy using the **R.E.A.L Framework**.

	**Technical Specs**:
	- **Data**: Download 10 years of SPY daily data using `yfinance`.
	- **R (Robust)**: Split data: In-Sample (Start to 2021-12-31), Out-of-Sample (2022-01-01 to Present).
	- **E (Execution Realism)**: Signal generated at $Close_{t}$, execution at $Open_{t+1}$.
	- **A (Account for Fees/Slippage)**: Use IBKR Fixed pricing ($0.005 per share, min $1.00) and 1 basis point (0.01%) slippage.
	- **L (Look-ahead Bias)**: Ensure all indicators (SMA) are calculated strictly on $t \le current\_bar$. Use `.shift(1)` for signals.
	
	**Metrics**: CAGR, Max Drawdown, Sharpe Ratio (Annualized), Total Trades, Win Rate %, Max Consecutive Losses.
	
	**Workflow**: Code block by block. Wait for my "PROCEED" command after each block.
	
	**Block 1**: Data Acquisition, Preprocessing, and In/Out-of-sample splitting.
## 走勢前推最佳化提示詞
---
- 中文 ->
	- [[走勢前推最佳化提示詞(中文)]]
- 英文 ->
	- [[走勢前推最要化提示詞(英文)]]
## HRP提示詞
---


## 量化暴力搜尋策略
---


## 交易腳本提示詞
---