# Autonova-AI-Folder-Organizer
Autonova-AI-Folder-Organizer


## 1. 專案目標 (Project Goal)
開發一個運行於 **Windows 10** 的本機桌面應用程式，擁有**高品質、現代化 (Premium/Beautiful)** 的 GUI。
主要功能是協助使用者管理與分類雜亂的資料夾，結合 **AI 多模態 (Multimodal)** 能力來分析檔案內容，並提供強大的預覽功能。
**重點要求：此程式必須為 Portable (免安裝) 版本，單一 EXE 檔案即可運行，方便攜帶與分享。**

## 2. 核心介面佈局 (Layout)

應用程式分為三個主要區塊：

*   **左側面板 (Source Panel)**
    *   **功能**: 顯示來源目錄的檔案列表與結構。
    *   **操作**: 選擇檔案、批量選取、拖曳起始點。
    *   **狀態**: 顯示待處理檔案數量。

*   **中間面板 (AI & Preview Stage)**
    *   **核心功能**: 
        1.  **智能預覽 (Rich Preview)**: 
            *   支援圖片 (JPG, PNG, WebP 等)。
            *   支援專業格式 (PSD, PDF, DWG thumbnail)。
            *   支援壓縮檔 (ZIP, RAR, 7z) **顯示內部檔案清單 (Peeking)**，無需解壓。
        2.  **AI 助手 (AI Assistant)**:
            *   顯示 AI 分析結果 (檔案內容摘要、關鍵字、分類建議)。
            *   操作可視化：顯示檔案從左側「飛」入右側某個資料夾的路徑預覽。

*   **右側面板 (Destination Panel)**
    *   **功能**: 顯示使用者設定的目標資料夾 (Target Folders)。
    *   **配置**: 支援最多 9 個自定義資料夾，以 3×3 九宮格佈局顯示。
    *   **操作**: 
        *   拖曳放置區 (Drop Zone)，每個資料夾是一個「卡片」。
        *   鍵盤快捷鍵 1-9 快速分配檔案，0 取消分配。
        *   九宮格對應小鍵盤排列 (7-8-9 上排、4-5-6 中排、1-2-3 下排)。
    *   **預設組**: 可保存多組資料夾配置，透過下拉選單快速切換。

## 3. 關鍵技術功能 (Key Features)

### A. 檔案預覽 (Advanced File Preview)
*   **基本圖片**: (✅ 已完成) 直接渲染 (JPG, PNG, WebP, GIF)。
*   **PSD (Photoshop)**: (✅ 已完成) 使用 psd.js 解析合成圖層，支援縮放/平移，結果快取。
*   **PDF**: (✅ 已完成) 渲染首頁或提供完整預覽 (Browser Native)。
*   **DWG (AutoCAD)**: (✅ 已完成) 使用 LibreDWG 轉換 SVG 預覽，支援縮放、平移、反色。
*   **Office Documents**: (✅ 已完成) DOC/DOCX (mammoth + word-extractor), XLS/XLSX (xlsx 套件)。
*   **Text/Data**: (✅ 已完成) 支援 TXT, JSON, MD, Code。 (語法高亮、截斷顯示)
*   **Archives (壓縮檔)**: (✅ 已完成) 使用 7zip library 讀取 header，列出內部所有檔名。

### B. AI 多模態分類 (Multimodal AI Categorization)
*   **視覺分析 (Vision)**: (✅ 已完成) 讀取圖片/截圖內容，透過 Webhook 送往 AI 分析。
*   **PDF 分析**: (✅ 已完成) 自動擷取 PDF 第一頁圖片送往 AI 分析。
*   **文本分析 (Text)**: (✅ 已完成) 讀取 TXT/JSON/Code 文字內容進行語意分類。
*   **結構分析**: (✅ 已完成) 根據壓縮檔內的檔名結構判斷用途。
*   **AI 介面**: (✅ 已完成)
    *   透過 n8n Webhook 串接外部 AI 模型 (如 Gemini)。
    *   支援全域狀態管理，分析過程中可自由切換檔案。
    *   分析結果顯示建議名稱、關鍵字、描述。
    *   一鍵套用 AI 建議的新檔名。

### C. 互動體驗 (UX/Interaction)
*   **Drag & Drop**: (✅ 已完成) 流暢的拖曳體驗，從左側或預覽區直接拖拉到右側。
*   **Animations**: (✅ 已完成) 檔案移動時有 UI 反饋，資料夾卡片有 Hover 效果。
*   **Glassmorphism/Dark Mode**: (✅ 已完成) 採用現代化深色玻璃擬態 UI。

## 4. 建議技術棧 (Tech Stack)

為了達到「GUI 漂亮」且「本機運行」的要求，選擇 **Electron** 生態系：

*   **App Framework**: **Electron** (跨平台桌面應用核心，支援 Windows, macOS, Linux)。
    *   *註：雖然代碼通用，但若要產生 macOS 的 `.dmg` 或 `.app`，通常需要在 Mac 環境下進行編譯 (Build)，或使用 CI/CD (如 GitHub Actions) 進行跨平台打包。*
*   **Frontend**: **React** + **Vite** (快速、模組化)。
*   **Styling**: **Tailwind CSS** + **Framer Motion** (處理複雜動畫與漂亮的 UI)。
*   **Language**: **TypeScript** (確保代碼穩固)。
*   **Database (Optional)**: **SQLite** 或 **Lowdb** (本機儲存設定與歷史紀錄)。
*   **AI Model**: **Google Gemini API** (Multimodal 能力強) 或 Local LLM (若需完全離線，但預覽多模態較重)。
*   **Build System**: **Electron Builder** (configured for Portable target to generate single `.exe`).

### 特定格式處理庫 (Potential Libraries)
*   **Images**: `sharp` (高效能圖片處理/縮圖)。
*   **PSD**: `ag-psd` 或 `psd.js` (讀取 Photoshop 縮圖)。
*   **PDF**: `react-pdf` (預覽渲染), `pdf-parse` (抽取文字)。
*   **Office (Word)**: `mammoth` (轉 HTML 預覽), `textract` (抽取文字)。
*   **Office (Excel)**: `xlsx` (SheetJS) (讀取表格數據)。
*   **OCR (Text in Images/PDF)**: `tesseract.js` (本機 OCR 引擎) 或直接利用 **Gemini Vision API** (AI 端 OCR)。
*   **Archives**: `node-7z` (wrapper for 7zip-bin)。
*   **DWG**: (需評估 `libre-dwg` 或其他轉換工具)。

## 5. 已完成進度 (Completed)
1.  ✅ 初始化 Electron + React + Vite + TypeScript 專案結構。
2.  ✅ 建立基礎 UI 左中右三欄佈局 (Glassmorphism 深色主題)。
3.  ✅ 實作檔案樹瀏覽與選取。
4.  ✅ 實作多種檔案格式預覽 (圖片、PDF、DWG、文字、壓縮檔、PSD、Office)。
5.  ✅ 實作拖放檔案到目標資料夾功能。
6.  ✅ 整合 AI 分析 (透過 n8n Webhook)。
7.  ✅ AI 結果顯示與一鍵重新命名。
8.  ✅ AI 結果持久化儲存。
9.  ✅ 預覽區統一縮放/平移控制。
10. ✅ Blender (.blend) 檔案預覽 (嵌入縮圖 + HQ 背景渲染)。
11. ✅ 圖片旋轉功能 (順時針 90 度，高品質保存)。
12. ✅ 鍵盤快捷鍵分配 (1-9 分配檔案，0 取消分配)。
13. ✅ 九宮格目標資料夾佈局 (對應小鍵盤排列)。
14. ✅ 資料夾預設組 (保存、載入、切換配置)。
15. ✅ 檔案樹內拖放移動 (支援多選，資料夾懸停高亮)。
16. ✅ 檔案樹增強篩選 (支援篩選已展開子目錄內容)。
15. ✅ 智能選取建議 (偵測同名不同副檔名、連號檔案)，提供一鍵全選。
16. ✅ 左側檔案樹優化 (系統資料夾快捷鍵、內聯重命名、自動滾動)。
17. ✅ 背景預覽加載 (PDF, PSD, DWG, Blend) 與記憶體快取優化。
18. ✅ ADS 元數據讀取效能優化 (Native Node.js fs)。
19. ✅ 目標資料夾面板增強 (一鍵清空、未保存修改提示)。
20. ✅ 預覽快取自動管理 (旋轉/移動/重命名時自動清除過期快取)。
21. ✅ AI 體驗優化 (重命名後自動重置分析狀態)。
22. ✅ 目的地捷徑 (點擊九宮格圖示可開啟目標資料夾)。
23. ✅ 安全批量移動 (一鍵移動已標記檔案，包含 Hash 校驗與進度回饋)。
24. ✅ UI 優化 (簡化操作按鈕，提升空間利用率)。
25. ✅ 右側面板分區 (新增統計與工具儀表板)。
26. ✅ 圖片轉 PDF (多選圖片自動顯示轉檔面板，自動按檔名排序)。
27. ✅ 移除 GPL 依賴 (Poppler 改用 pdfjs-dist+canvas，LibreDWG 改為外挂模式)。
28. ✅ 正則表達式生成改用 OpenRouter API (支援用戶自訂 Prompt 設定)。
29. ✅ 資料夾雙擊重命名 (左側檔案樹中資料夾也可雙擊重命名)。
30. ✅ 自訂快捷資料夾 (1-4 快捷按鈕，左鍵儲存/切換，右鍵清除)。
31. ✅ 排序選單精簡 (移除標籤，簡化文字)。
32. ✅ **多語言支援 (i18n)** (繁體中文、英文、日文，340+ 翻譯鍵)。
33. ✅ **RAR 格式支援** (使用 node-unrar-js，MIT 授權可商用)。
34. ✅ **壓縮檔解壓縮** (ZIP/7Z/RAR，支援編碼設定，原檔移入解壓資料夾)。
35. ✅ **EXIF 資訊顯示** (拍攝日期、GPS、相機型號、曝光參數)。
36. ✅ **付費模型警告** (批量 AI 分析按鈕懸浮提示)。
37. ✅ **視頻預覽功能** (FFmpeg 9帧縮圖、3×3九宮格、Base64快取)。
