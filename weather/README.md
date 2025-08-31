# Weather MCP Server
- weather.py：一個範例 MCP Server，可用於天氣預報和天氣警報，程式碼主要來自 MCP 官方範例。
- mcp_logger.py：用於擷取記錄 MCP Server 的輸入輸出，並將記錄內容寫入 mcp_io.log，該程式碼主要由 Gemini 2.5 Pro 編寫。


## 用 mcp_logger.py 擷取 MCP Server 的輸入輸出
概念上就是跑   
```bash
python mcp_logger.py uv run /path/weather.py 
```
![示意圖](../images/mcp_logger_作用示意圖.png)

mcp_io.log (留一個紀錄於 mcp_io_learn.txt) 
- 溝通為 json {} 形式
- 輸入: Cline -> MCP Server
- 輸出: MCP Server -> Cline

## MCP Server - Cline - LLM 互動
```mermaid
sequenceDiagram
    participant User as 用戶
    participant MCP as MCP Server (weather)
    participant Cline as Cline
    participant Model as 模型

    Note over User, Model: 初始化

    Cline->>MCP: 啟動 MCP Server
    Cline->>MCP: 你好，我是 Cline，版本與協定為...
    MCP->>Cline: 你好，我是 weather，版本與協定為...
    Cline->>MCP: 收到
    
    Note over User, Model: 確認工具

    Cline->>MCP: 你有哪些工具？
    MCP->>Cline: 我有 get_forecast 和 get_alerts

    Note over User, Model: 等待用戶輸入
    
    User->>Cline: 紐約明天的天氣怎麼樣？
    Cline->>Model: 紐約明天的天氣怎麼樣？(with 一堆工具...)
    Model->>Cline: 我要調用 get_forecast，參數是...
    Cline->>MCP: 我要調用 get_forecast，參數是...
    MCP->>Cline: 調用完畢，結果是...
    Cline->>Model: 調用完畢，結果是...
    Model->>Cline: 紐約明天的天氣是這樣的...
    Cline->>User: 紐約明天的天氣是這樣的...
```



# Reference： 
- Youtube:
    - [基礎篇：使用 MCP + 簡介流程](https://www.youtube.com/watch?v=yjBUnbRgiNs)
    - [進階篇：動手寫一個 MCP Server + 分析底層協議](https://www.youtube.com/watch?v=zrs_HWkZS5w)
- [Github Repo](https://github.com/MarkTechStation/VideoCode/tree/main/MCP%E7%BB%88%E6%9E%81%E6%8C%87%E5%8D%97-%E8%BF%9B%E9%98%B6%E7%AF%87/weather)
- [MCP Server 官方文件](https://modelcontextprotocol.io/quickstart/server)