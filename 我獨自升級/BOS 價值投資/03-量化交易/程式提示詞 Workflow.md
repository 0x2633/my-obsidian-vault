## 量化交易模型提示詞
- ==中文== -> [[01-量化交易模型提示詞(中文)]]
- ==English== -> [[01-量化交易模型提示詞(English)]]

---
## 交易模型說明提示詞
- ==中文== -> [[02-交易模型說明提示詞(中文)]]
- ==英文== -> [[02-交易模型說明提示詞(English)]]

---
## IBKR 資料擷取提示詞
- ==中文== ->
	- 請扮演一位資深量化交易者與 Python 工程師，熟悉 IBKR 模擬交易（Paper Trading）、IB Gateway / TWS，以及使用 Python 擷取交易資料。我已經選定一個交易模型並理解其核心邏輯，現在希望先確認這個模型究竟需要哪些資料，接著再用 Python 從 IBKR 模擬交易帳戶把資料抓出來。第一步（此步驟請不要寫任何程式碼），請根據我選擇的模型【請貼上模型名稱】，清楚列出需要的交易標的（如股票、ETF、期貨、選擇權、外匯等）、需要的資料類型（如價格 K 線、成交量、OHLC、選擇權鏈、Greeks 等）、需要的時間週期與 K 線頻率、需要的歷史資料長度，以及使用 IBKR 資料時可能涉及的假設或限制，完成後請暫停並等待我的確認再進行下一步。第二步在我確認後，請將上述需求對應到 IBKR 的資料能力，說明這些資料是否能透過 IBKR 模擬交易帳戶取得、需要使用哪些 IBKR API 元件或方法（例如 Contract、reqHistoricalData），以及 IBKR 常見的限制（例如請求頻率／pacing、資料缺漏），然後再次暫停等待我的確認。第三步僅在我確認後才開始，請產出一個完整乾淨的 .py 檔案，使用 ib_insync 連線到 IBKR 模擬交易（Paper Trading），擷取所需資料並儲存成 CSV，並在程式中加入清楚易懂的註解讓新手也能理解。整個教學流程請一步一步進行，解釋每一段程式碼在做什麼，每完成一個重要步驟就停下來等我貼出結果或確認，並假設我是願意學也會思考的新手。最終目標是讓我能清楚知道模型所需資料、成功用 Python 從 IBKR 模擬交易抓取資料，並理解如何修改這份程式以支援其他策略或模型。
- ==英文== ->
	- Act as a senior quantitative trader and Python engineer with hands-on experience using Interactive Brokers (IBKR) Paper Trading, IB Gateway / TWS, and Python-based data extraction. I have already chosen a trading model and understand its intuition, and now I want to identify the exact data required and extract that data from IBKR Paper Trading using Python. First, in Step 1 (do not write any code yet), based on the chosen model [PASTE MODEL NAME HERE], clearly list the required instruments (e.g., stocks, ETFs, futures, options, FX), required data types (e.g., price bars, volume, OHLC, options chain, greeks), required timeframes and bar sizes, required historical lookback period, and any assumptions or limitations when using IBKR data, then pause and wait for my confirmation before continuing. After I confirm, proceed to Step 2 by mapping the requested data to IBKR capabilities, explaining whether the data is available via IBKR Paper Trading, which IBKR API objects or methods are needed (e.g., Contract objects, reqHistoricalData), and common IBKR constraints such as pacing/rate limits or missing data, then pause again and wait for my confirmation. Only after I confirm Step 2, proceed to Step 3 and generate a single clean Python .py script using ib_insync that connects to IBKR Paper Trading, extracts the required data, saves it to CSV, and includes clear beginner-friendly comments. Throughout the process, go step by step, explain what each script section does, pause after each major step to wait for my output or confirmation, and assume I am a beginner but intelligent and curious. The goal is that by the end I will know exactly what data my model needs, have a working Python script that pulls the data from IBKR Paper Trading, and understand how to adapt the script for other models in the future.

---
## R.E.A.L 回測提示詞
- 中文 -> [[04-R.E.A.L 回測提示詞(中文)]]
- English -> [[04-R.E.A.L 回測提示詞(English)]]
---
## 走勢前推最佳化提示詞
- ==中文== -> [[05-走勢前推最佳化提示詞(中文)]]
- ==英文== -> [[05-走勢前推最要化提示詞(English)]]

---
## HRP 提示詞
- ==中文== -> [[06-HRP 提示詞(中文)]]
- ==英文== -> [[06-HRP 提示詞(English)]]

---
## 量化暴力搜尋策略


---
## 交易腳本提示詞