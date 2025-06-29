# Software Language Engineering 流程图详解

下面基于图示，总结 Software Language Engineering 的整体架构与关键环节。

## 1. 前端（Frontend: read）
- **控制脚本（control script）**：定义工具链执行顺序和参数
- **模型（models）**：语言实例文件，作为输入
- **模型加载器（model loader）**：读取模型并生成初始 AST（input AST）
- **工作流执行（workflow execution）**：根据控制脚本调度各环节
- **函数库（function library）**：为模板和引擎提供辅助操作

## 2. 中心（Center: transform）
- **上下文条件（Context Conditions, CoCo）**：对 AST 进行约束检查，保证模型的全局和上下文正确性
- **访问者模式（Visitors）**：使用 Traverser/Visitor 深度遍历 AST，实现检查、分析、转换等逻辑
- **符号管理（Symbols）**：构建符号表及作用域，管理定义、引用与解析，支持跨模型和跨语言的符号操作
- **输出 AST（output AST）**：经过验证和转换的 AST，作为后续生成阶段输入

## 3. 后端（Backend: generate）
- **模板文件（templates）**：FreeMarker 模板，定义输出格式和结构
- **模板引擎（template engine）**：将 output AST 与模板和函数库结合，生成目标产物
- **手写代码集成（Handcoding）**：在生成产物中插入手写基础实现或钩子，增强可扩展性
- **最终输出**：文档、报告、源代码等多种形式的生成产物



```mermaid
graph TD
    subgraph "Input"
        Model["Model (Text)"]
    end

    subgraph "Frontend: Syntax Analysis"
        Grammar["Grammar (.mc4)"] -- "generates" --> Parser
        Model -- "is parsed by" --> Parser
        Parser -- "produces" --> AST{"Abstract Syntax Tree (AST)"}
    end

    subgraph "Middle-End: Semantic Analysis"
        AST -- "is traversed to build" --> SymbolTable["Symbol Table"]
        AST -- "is checked by" --> CoCos["Context Conditions (CoCos)"]
        SymbolTable -- "is used by" --> CoCos
    end

    subgraph "Backend: Generation"
        AST -- "is input for" --> Generator
        Templates["Templates (.ftl)"] -- "are used by" --> Generator
        Generator -- "produces" --> GeneratedCode["Generated Code (e.g., Java)"]
    end

    subgraph "Final Product"
        GeneratedCode -- "is combined with" --> FinalApplication
        HandwrittenCode["Hand-written Code"] -- "is combined with" --> FinalApplication("Final Application")
    end

    classDef core-concept fill:#E6F3FF,stroke:#0066CC,stroke-width:2px;
    class AST,SymbolTable,Generator core-concept;
````
