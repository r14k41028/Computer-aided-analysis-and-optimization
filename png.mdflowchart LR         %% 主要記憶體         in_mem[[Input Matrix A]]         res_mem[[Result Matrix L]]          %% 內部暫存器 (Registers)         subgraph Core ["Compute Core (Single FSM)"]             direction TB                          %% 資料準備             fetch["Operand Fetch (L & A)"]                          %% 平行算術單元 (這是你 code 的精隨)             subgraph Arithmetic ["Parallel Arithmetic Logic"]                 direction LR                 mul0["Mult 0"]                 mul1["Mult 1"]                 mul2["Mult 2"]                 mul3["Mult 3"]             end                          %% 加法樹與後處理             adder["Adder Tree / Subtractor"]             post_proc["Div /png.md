flowchart LR
        %% 主要記憶體
        in_mem[[Input Matrix A]]
        res_mem[[Result Matrix L]]

        %% 內部暫存器 (Registers)
        subgraph Core ["Compute Core (Single FSM)"]
            direction TB
            
            %% 資料準備
            fetch["Operand Fetch (L & A)"]
            
            %% 平行算術單元 (這是你 code 的精隨)
            subgraph Arithmetic ["Parallel Arithmetic Logic"]
                direction LR
                mul0["Mult 0"]
                mul1["Mult 1"]
                mul2["Mult 2"]
                mul3["Mult 3"]
            end
            
            %% 加法樹與後處理
            adder["Adder Tree / Subtractor"]
            post_proc["Div / Sqrt Unit"]
            
            %% 連接關係
            fetch --> mul0 & mul1 & mul2 & mul3
            mul0 & mul1 & mul2 & mul3 --> adder
            adder --> post_proc
        end

        %% 外部連接
        in_mem --> fetch
        res_mem -.-> fetch
        post_proc --> res_mem

        %% 樣式設定
        style in_mem fill:#FFF2CC,stroke:#D8A45A,stroke-width:2px
        style res_mem fill:#FFF2CC,stroke:#D8A45A,stroke-width:2px
        style Core fill:#F9F9F9,stroke:#666,stroke-width:2px,stroke-dasharray: 5 5
        
        %% 算術單元強調色 (綠色代表你的平行化部分)
        style mul0 fill:#CBEFD6,stroke:#7FAF8A
        style mul1 fill:#CBEFD6,stroke:#7FAF8A
        style mul2 fill:#CBEFD6,stroke:#7FAF8A
        style mul3 fill:#CBEFD6,stroke:#7FAF8A
        style adder fill:#CBEFD6,stroke:#7FAF8A
        style post_proc fill:#E1D5E7,stroke:#9673A6
