# Claude Code 專案設定

## 語言偏好
- 請使用繁體中文回覆所有對話
- 程式碼註解保持英文

## 專案規範
- Python 3.x
- 使用虛擬環境 (venv)
- 遵循 PEP 8 程式碼風格

## 重要指示
- 安裝套件時務必使用：`/Users/jimmy/Projects/JimmyPlaud/venv/bin/pip install`
- 執行 Python 時使用：`/Users/jimmy/Projects/JimmyPlaud/venv/bin/python`
- 不要使用系統的 pip 或 python

## 專案文件結構
專案文件存放於 `/docs` 資料夾，包含以下檔案：

### 匯入文件指令
```bash
# 匯入需求規格書
cat /Users/jimmy/Projects/JimmyPlaud/docs/requirements.md

# 匯入待辦事項清單
cat /Users/jimmy/Projects/JimmyPlaud/docs/todo.md

# 匯入 API 規格書
cat /Users/jimmy/Projects/JimmyPlaud/docs/api-spec.md

# 一次匯入所有文件
cat /Users/jimmy/Projects/JimmyPlaud/docs/*.md
```

### 文件說明
- **requirements.md**: 需求規格書模板，用於記錄專案的功能與非功能需求
- **todo.md**: 待辦事項追蹤，使用四象限法則管理任務優先順序
- **api-spec.md**: API 規格文件，定義 RESTful API 的端點、請求與回應格式