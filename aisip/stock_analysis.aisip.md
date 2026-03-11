# AISIP：股票分析引擎

此 AISIP 定义了一个用于多维度股票分析的专业级执行流程。

> **协议版本**: AISIP V1.0.0
> **ID**: `soulbot.stock_analysis`

## 1. 逻辑流程

```
start -> parse -> load_data_fetcher -> data -> tech -> sentiment -> synthesis -> risk -> conclusion -> end
```

## 2. 实现 (JSON)

```json
[
  {
    "role": "system",
    "content": {
        "protocol": "AISIP V1.0.0",
        "id": "soulbot.stock_analysis",
        "verified_on": ["Cursor", "Gemini CLI"],
        "tools": ["google_search", "shell", "file_system", "web_browser"]
    }
  },
  {
    "role": "user",
    "content": {
        "instruction": "RUN aisip.stock_analysis",
        "aisip": {
            "main": {
                "start":             { "type": "process",  "next": ["parse"] },
                "parse":             { "type": "process",  "next": ["load_data_fetcher"] },
                "load_data_fetcher": { "type": "delegate", "delegate_to": "stock_data_fetcher.aisip.json", "next": ["data"] },
                "data":              { "type": "process",  "next": ["tech"] },
                "tech":              { "type": "process",  "next": ["sentiment"] },
                "sentiment":         { "type": "process",  "next": ["synthesis"] },
                "synthesis":         { "type": "process",  "next": ["risk"] },
                "risk":              { "type": "process",  "next": ["conclusion"] },
                "conclusion":        { "type": "process",  "next": ["end"] },
                "end":               { "type": "end" }
            }
        },
        "functions": {
            "data": { "step1": "Fetch P/E, Market Cap, 52W High/Low, and current price." },
            "tech": { "step1": "Look for MA50/200 crossover, RSI levels, and volume trends." },
            "sentiment": { "step1": "Scan news from Bloomberg, Reuters, or Yahoo Finance for recent catalysts." },
            "conclusion": { "step1": "Provide a summary assessment. CRITICAL: Include 'Not Financial Advice' disclaimer." }
        }
    }
  }
]
```

## 3. 使用方法

要激活此流程，请将你的 `.env` 指向 `../aisip/stock_analysis.aisip.json`。

---
*生成自 `stock_analysis.aisip.json`*
