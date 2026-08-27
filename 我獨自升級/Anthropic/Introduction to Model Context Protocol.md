## Introduction
---
### 什麼是 MCP
- Model Context Protocol(MCP)，是一種通訊層的協定。旨在為大語言模型提供 **上下文(Context)** 與 **工具(Tools)** 之間的溝通橋樑，讓開發人員不用自行撰寫程式碼即可以直接調用外部工具。
- 實際專案中只會實作`Client`端或是`Server`端其中一方，而不會同時兩者都實現。

### MCP 架構
- **主從式架構(Client-Server)**
- 外部提供服務方為伺服器方(Server)，用戶方(Client)則為欲存取外部服務的端點。**Server Side**通常包含多個內部組件，如下圖。![[Pasted image 20260615143817.png]]
- 多數情況都是由**官方提供**MCP的功能。==但其實任何人或組織都是可以實作並提供其服務==
- MCP 與一般工具使用上的差別在**工具的使用方式**及**使用的對象**。我們不需要再自己寫程式碼，而且使用的對象從**人換成了AI**。

### MCP Clients
- 提供**本地**與**MCP Server**之間溝通的橋樑。
- 實現各種資料傳送方式(Transport Agnostic)，可支援 `Stdio`(本地) 或 `HTTP`及 `Websockets`(遠端)
- 一個完整的**MCP**調用如下圖(截取 [Anthropic 官方教材](https://anthropic.skilljar.com/introduction-to-model-context-protocol/296694))。![[Pasted image 20260615171414.png|606]]

## Hands-on with MCP Server
---
在建立一個**MCP Server**前，我們要先規劃，這個**MCP Server**要提供哪些工具，如:**Read**,**Update**之類的功能再搭配**SDK**搭建**MCP Server**，以**Read**功能來說，我們以**Python**搭配**SDK**建立讀取文件的工具，程式碼大致如下:(重點是`@`修飾子的內容)

```python
@mcp.tool(
    name="read_doc_contents",
    description="Read the contents of a document and return it as a string."
)
def read_document(
    doc_id: str = Field(description="Id of the document to read")
):
    if doc_id not in docs:
        raise ValueError(f"Doc with id {doc_id} not found")
    
    return docs[doc_id]
```

完成所需要的工具實作後，再搭配`Server Inspector`來進行測試，執行下列指令即可以開啟介面:
```bash
mcp dev mcp_server.py
```

開啟介面後點擊**Connect**即可以與開發中的**MCP Server**連線。

## Connecting with MCP Clients
---
### Implementing a client
**MCP Client**有兩個主要元件:
- **MCP Client**: 一個方便我們簡單使用**Session**的客制化**Class**
- **Client Session**: 實際與**Server**連線的介面(**SDK**的一部份)![[Pasted image 20260624110501.png]]
- **Client**不使用的時候要進行**Session**的清理
- **Client**主要做兩件事:
	- 列舉可用的工具(Toos) 
	- 當**Claude**請求工具的使用時可透過**Client**執行![[Pasted image 20260624133840.png]]

### Defining Resources


### Accessing Resources

### Defining Prompts

### Prompts In The Clients
