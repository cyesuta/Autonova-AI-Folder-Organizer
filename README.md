# Autonova-AI-Folder-Organizer
Autonova-AI-Folder-Organizer
# AI Smart Folder Organizer - Project Plan

## 1. 專案目標 (Project Goal)
開發一個運行於 **Windows 10** 的本機桌面應用程式，擁有**高品質、現代化 (Premium/Beautiful)** 的 GUI。
主要功能是協助使用者管理與分類雜亂的資料夾，結合 **AI 多模態 (Multimodal)** 能力來分析檔案內容，並提供強大的預覽功能。
**發布策略：主要推廣安裝版 (支援自動更新、右鍵整合)，同時提供 Portable 版本供進階用戶使用。**

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
| 格式 | 狀態 | 技術實現 |
|------|------|----------|
| 圖片 (JPG, PNG, WebP, GIF, BMP, SVG) | ✅ | 原生渲染 + `sharp` 縮圖 |
| PSD (Photoshop) | ✅ | `psd` 套件解析合成圖 + 快取 |
| PDF | ✅ | `pdfjs-dist` + `canvas` 渲染多頁網格 |
| AI (Adobe Illustrator) | ✅ | 複用 PDF 預覽（AI 檔案內嵌 PDF） |
| PPT/PPTX (PowerPoint) | ✅ | LibreOffice 轉 PDF 渲染 / 嵌入縮圖 |
| DWG/DXF (AutoCAD) | ✅ | 外掛 LibreDWG 轉 SVG + 快取 |
| Blender (.blend) | ✅ | 嵌入縮圖提取 + HQ 背景渲染 |
| Office (DOC/DOCX) | ✅ | `mammoth` + `word-extractor` + `officeparser` |
| Office (XLS/XLSX) | ✅ | `xlsx` (SheetJS) |
| 文本 (TXT, JSON, MD, Code) | ✅ | 原生讀取 + 截斷顯示 |
| 壓縮檔 (ZIP, 7Z, RAR) | ✅ | `node-7z` + `node-unrar-js` |
| 影片 (MP4, MKV, AVI, MOV...) | ✅ | FFmpeg 9 帧截圖 + 3×3 網格 |
| 3D 模型 (GLB, GLTF, FBX, OBJ, STL) | ✅ | Three.js + OrbitControls |
| 紋理 (DDS, KTX2, EXR, HDR) | ✅ | Three.js Texture Loaders |

### B. AI 多模態分析 (Multimodal AI Analysis)
| 功能 | 狀態 | 技術實現 |
|------|------|----------|
| 視覺分析 | ✅ | `sharp` 壓縮後送 OpenRouter API |
| PDF 分析 | ✅ | `pdfjs-dist` 渲染首/中/末頁合併圖 |
| 影片分析 | ✅ | 9 帧合併成 3×3 網格圖 |
| 文本分析 | ✅ | 讀取文字內容語意分類 |
| 壓縮檔分析 | ✅ | 根據內部檔名結構判斷用途 |
| AI 介面 | ✅ | OpenRouter API 統一接口，支援自訂 Prompt |
| 批量分析 | ✅ | 佇列處理 + 進度顯示 + 結果快取 |

### C. 互動體驗 (UX/Interaction)
| 功能 | 狀態 | 技術實現 |
|------|------|----------|
| 拖放操作 | ✅ | HTML5 Drag & Drop API |
| 動畫效果 | ✅ | `framer-motion` |
| 深色玻璃擬態 UI | ✅ | Tailwind CSS + Glassmorphism |
| 多語言 | ✅ | `i18next` + `react-i18next` (繁中/英/日) |
| 鍵盤快捷鍵 | ✅ | 1-9 快速分配，0 取消 |
| 智能選取 | ✅ | 同名/連號檔案偵測 |
| 歷史記錄 | ✅ | 移動/重命名/刪除操作可撤銷 |

## 4. 技術棧 (Tech Stack)

### 核心框架
| 類別 | 技術 | 說明 |
|------|------|------|
| 桌面框架 | Electron 30 | 跨平台桌面應用 |
| 前端框架 | React 18 | 組件化 UI |
| 建置工具 | Vite 5 | 快速 HMR 開發 |
| 語言 | TypeScript 5 | 類型安全 |
| 樣式 | Tailwind CSS 3 | 原子化 CSS |
| 動畫 | Framer Motion | 流暢動畫 |
| 打包 | Electron Builder | 安裝版 + Portable |

### 檔案處理庫
| 格式 | 套件 | 授權 |
|------|------|------|
| 圖片處理 | `sharp` | Apache 2.0 |
| PSD 解析 | `psd` | MIT |
| PDF 渲染 | `pdfjs-dist` + `canvas` | Apache 2.0 |
| PDF 生成 | `pdfkit` | MIT |
| Word 解析 | `mammoth` + `word-extractor` + `officeparser` | MIT |
| Excel 解析 | `xlsx` | Apache 2.0 |
| 壓縮檔 | `node-7z` + `7zip-bin` + `node-unrar-js` | LGPL/MIT |
| 影片處理 | FFmpeg (外掛) | LGPL |
| 編碼轉換 | `iconv-lite` | MIT |
| EXIF 讀取 | `exifr` | MIT |
| PNG 處理 | `pngjs` | MIT |
| 3D 渲染 | `three` | MIT |

### AI 與網路
| 功能 | 技術 |
|------|------|
| AI API | OpenRouter (支援 Gemini、Claude 等) |
| HTTP | Electron net 模組 |

### 國際化
| 功能 | 套件 |
|------|------|
| i18n 核心 | `i18next` |
| React 綁定 | `react-i18next` |
| 支援語言 | 繁體中文、English、日本語 |

## 5. 已完成進度 (Completed)
1.  ✅ 初始化 Electron + React + Vite + TypeScript 專案結構。
2.  ✅ 建立基礎 UI 左中右三欄佈局 (Glassmorphism 深色主題)。
3.  ✅ 實作檔案樹瀏覽與選取。
4.  ✅ 實作多種檔案格式預覽 (圖片、PDF、DWG、文字、壓縮檔、PSD、Office)。
5.  ✅ 實作拖放檔案到目標資料夾功能。
6.  ✅ 整合 AI 分析。
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
17. ✅ 智能選取建議 (偵測同名不同副檔名、連號檔案)，提供一鍵全選。
18. ✅ 左側檔案樹優化 (系統資料夾快捷鍵、內聯重命名、自動滾動)。
19. ✅ 背景預覽加載 (PDF, PSD, DWG, Blend) 與記憶體快取優化。
20. ✅ ADS 元數據讀取效能優化 (Native Node.js fs)。
21. ✅ 目標資料夾面板增強 (一鍵清空、未保存修改提示)。
22. ✅ 預覽快取自動管理 (旋轉/移動/重命名時自動清除過期快取)。
23. ✅ AI 體驗優化 (重命名後自動重置分析狀態)。
24. ✅ 目的地捷徑 (點擊九宮格圖示可開啟目標資料夾)。
25. ✅ 安全批量移動 (一鍵移動已標記檔案，包含 Hash 校驗與進度回饋)。
26. ✅ UI 優化 (簡化操作按鈕，提升空間利用率)。
27. ✅ 右側面板分區 (新增統計與工具儀表板)。
28. ✅ 圖片轉 PDF (多選圖片自動顯示轉檔面板，自動按檔名排序)。
29. ✅ 移除 GPL 依賴 (Poppler 改用 pdfjs-dist+canvas，LibreDWG 改為外挂模式)。
30. ✅ 正則表達式生成改用 OpenRouter API (支援用戶自訂 Prompt 設定)。
31. ✅ 資料夾雙擊重命名 (左側檔案樹中資料夾也可雙擊重命名)。
32. ✅ 自訂快捷資料夾 (1-4 快捷按鈕，左鍵儲存/切換，右鍵清除)。
33. ✅ 排序選單精簡 (移除標籤，簡化文字)。
34. ✅ **多語言支援 (i18n)** (繁體中文、英文、日文，340+ 翻譯鍵)。
35. ✅ **RAR 格式支援** (使用 node-unrar-js，MIT 授權可商用)。
36. ✅ **壓縮檔解壓縮** (ZIP/7Z/RAR，支援編碼設定，原檔移入解壓資料夾)。
37. ✅ **EXIF 資訊顯示** (拍攝日期、GPS、相機型號、曝光參數)。
38. ✅ **付費模型警告** (批量 AI 分析按鈕懸浮提示)。
39. ✅ **視頻預覽功能** (FFmpeg 9帧縮圖、3×3九宮格、Base64快取)。
40. ✅ **全域檔案搜索** (搜尋整個目錄樹，中文1字/英文2字即可搜索)。
41. ✅ **搜索偏好設定** (最大目錄/檔案/深度/結果數，跳過目錄)。
42. ✅ **加密壓縮包解壓** (ZIP/7Z/RAR 加密格式，密碼釘選功能)。
43. ✅ **錯誤日誌對話框** (本次執行/歷史紀錄分頁，可清除刷新)。
44. ✅ **AI 請求超時機制** (20-30秒超時，友好錯誤訊息)。
45. ✅ **緩存目錄隱藏** (`.autonova` + Windows 隱藏屬性)。
46. ✅ **影片 AI 分析優化** (9帧合併3x3網格圖送分析)。
47. ✅ **Prompt 設定優化** (「📋 使用預設」按鈕)。
48. ✅ **確認對話框捲軸** (最大高度 80vh，美化捲軸)。
49. ✅ **歷史記錄面板優化** (50筆記錄，美化捲軸)。
50. ✅ **檔案衝突處理** (自動重命名/跳過/覆蓋/Smart Merge)。
51. ✅ **PDF 快取優化** (批量 AI 分析時自動保存預覽快取)。
52. ✅ **PPT/PPTX 預覽** (LibreOffice 轉 PDF / 嵌入縮圖)。
53. ✅ **Word 預覽條文編號** (DOCX 轉 HTML 保留列表格式)。
54. ✅ **目標文件夾描述快捷編輯** (Enter 保存，Esc 取消，失焦保存)。
55. ✅ **Adobe Illustrator (.ai) 預覽** (複用 PDF 渲染邏輯)。
56. ✅ **3D 模型預覽** (GLB/GLTF/FBX/OBJ/STL，Three.js + OrbitControls)。
57. ✅ **紋理預覽** (DDS/KTX2/EXR/HDR，Three.js Texture Loaders)。
58. ✅ **主題色切換** (5 種強調色，CSS 變量架構)。

## 6. 待開發功能 (TODO)

### 🚀 追加功能
- [ ] 語音檔辨識
- [ ] 圖片修改 / 繪圖
- [ ] 關聯多文件同時命名
- [ ] 分享 / 列印功能

### 🐛 Bug 修復
- [ ] CAD 檔 / 3D 檔不要 AI 分析
- [ ] Code Review 全面檢查
- [ ] 加入 React ErrorBoundary 機制，發生渲染錯誤時顯示友好的錯誤介面而非整頁空白

### ⚡ 效能優化
- [ ] 虛擬滾動 (react-window 或 virtuoso) 避免大文件夾崩潰
- [ ] ASAR 與 Unpack 智慧分流：在 electron-builder 配置中開啟 asar，大檔案/二進制檔踢出去
- [ ] 避免 IPC 通道阻塞 (Serialization Cost)：大數據走 Invoke (非同步)
- [ ] 懶加載 IPC Handlers「空閒載入」或「按需載入」

### 📦 打包與發布
- [ ] electron-builder 一次打包出安裝版和 portable 版

#### 安裝版特性
- [ ] electron-updater 無痛更新
- [ ] 右鍵菜單整合
- [ ] 設定殘留檔案管理
- [ ] 快取集中管理 (Centralized Cache)：放在 appdata、各文件夾或系統暫存區，可選
- [ ] 更新版本檢查

#### Portable 版特性
- [ ] 設定檔保存在 exe 根目錄，啟動時檢測寫入權限
- [ ] 首次運行自動生成 Demo Folder + 教學 (driver.js 亮暗指示)
- [ ] 依賴管理：node_modules 裁剪，考慮 webpack/vite server-side build 打包 Main Process
- [ ] 壓縮權衡：compression: "store" 檔案最大但啟動最快
- [ ] 快取在各文件夾或系統暫存區，可選

### 💰 商業化準備

#### 授權系統
- [ ] 區別免費版功能開關 (分層授權)
- [ ] Electron 後端邏輯辨識 product_id / variant_id
- [ ] 建立「門神」組件 (TierGate)：等級不夠時按鈕變灰/消失/顯示鎖頭
- [ ] 序列號系統：LemonSqueezy 採購和序列號發行
- [ ] 序列號智慧快取驗證：本地讀取授權，定時查詢 key 是否過期
- [ ] 升級方案按鈕和序列號系統處理
- [ ] 模糊遮罩勾引升級（預覽區、圖表、進階設定面板）

#### 保護與安全
- [ ] 檔案名 / 作者名 / icon / logo 替換
- [ ] 免費版加廣告
- [ ] 打包測試依賴
- [ ] 防止反編譯：electron-vite Bytenode (V8 Bytecode) 保護前後端主檔
- [ ] 程式碼簽章：Certum 雲端標準版 (Standard Cloud)
- [ ] 遙測與崩潰報告：整合 Sentry (Electron 版)

### 🏆 專業版功能
- [ ] 本地 LLM 支援
- [ ] 分類訓練語義
- [ ] 人臉辨識

### 📄 頁面與支援
- [ ] 反饋頁面
- [ ] DC 技術支援 / LINE 流動人口



