# AISIP：天气预报引擎

此 AISIP 定义了一个详尽的天气查询与分析执行流程。

> **协议版本**: AISIP V1.0.0
> **ID**: `soulbot.weather`

## 1. 逻辑流程

```
start -> validate_location --(valid)----> geocode -> unit_pref -> fetch -> analyze -> enhanced_report -> end
                           --(invalid)--> error_handler -> end
                                          fetch --(error)--> error_handler -> end
```

## 2. 实现 (JSON)

```json
[
  {
    "role": "system",
    "content": {
        "protocol": "AISIP V1.0.0",
        "id": "soulbot.weather",
        "verified_on": ["Cursor", "Gemini CLI"],
        "tools": ["google_search", "web_browser"]
    }
  },
  {
    "role": "user",
    "content": {
        "instruction": "RUN aisip.weather",
        "aisip": {
            "main": {
                "start":             { "type": "process",  "next": ["validate_location"] },
                "validate_location": { "type": "decision", "branches": { "valid": "geocode", "invalid": "error_handler" } },
                "geocode":           { "type": "process",  "next": ["unit_pref"] },
                "unit_pref":         { "type": "process",  "next": ["fetch"] },
                "fetch":             { "type": "process",  "next": ["analyze"], "error": "error_handler" },
                "analyze":           { "type": "process",  "next": ["enhanced_report"] },
                "enhanced_report":   { "type": "process",  "next": ["end"] },
                "error_handler":     { "type": "process",  "next": ["end"] },
                "end":               { "type": "end" }
            }
        },
        "functions": {
            "geocode": { "step1": "使用搜索工具验证城市名或获取坐标。" },
            "fetch": { "step1": "访问天气服务获取实时数据与预报。" },
            "analyze": { "step1": "检查是否存在预警或显著气温变化。" },
            "enhanced_report": { "step1": "将结果格式化为带有表情符号的友好报告。" }
        }
    }
  }
]
```

## 3. 使用方法

此 AISIP 由 `main.aisip.json` 自动路由激活，也可手动指向使用。

---
*生成自 `weather.aisip.json`*
