# Electron 主程序重構進度 (main.ts Refactoring Progress)

本文檔追蹤 `electron/main.ts` 和 `src/App.tsx` 模組化重構的進度。

**最後更新時間**: 2026-01-03

---

## 重構成果總覽

### main.ts (後端)
| 指標 | 2025-12-27 | 目前 | 變化 |
|:---|:---|:---|:---|
| **行數** | 899 行 | **1146 行** | +247 行 (新功能) |

### App.tsx (前端)
| 指標 | 2025-12-27 | 目前 | 變化 |
|:---|:---|:---|:---|
| **行數** | 1007 行 | **1266 行** | +259 行 (新功能) |

> ⚠️ 行數增加主要來自新功能開發（關聯檔名分析、圖片編輯器、音頻逐字稿、開源授權等）

---

## 已拆分模組 - 後端 (共 21 個)

| # | 模組名稱 | 檔案路徑 | 功能描述 | 行數 |
|:---|:---|:---|:---|:---|
| 1 | AI Analysis | `electron/modules/aiAnalysis.ts` | 單檔 AI 分析 (圖片/PDF/PSD/DWG/Blend/Video/Text/Audio) | 870 行 |
| 2 | Image Operations | `electron/modules/imageOperations.ts` | EXIF 讀取、圖片旋轉、AI 編輯、遮罩修圖 | 642 行 |
| 3 | Settings | `electron/modules/settings.ts` | API Keys、Prompt 配置、語言、錯誤日誌 | 500 行 |
| 4 | File Operations | `electron/modules/fileOperations.ts` | 移動、安全移動、重命名、刪除檔案 | 452 行 |
| 5 | AI Classify | `electron/modules/aiClassify.ts` | 批次 AI 分類、embedding 分析 | 414 行 |
| 6 | Video Preview | `electron/modules/videoPreview.ts` | FFmpeg 視頻幀提取、快取、一鍵下載 | 407 行 |
| 7 | PPT Preview | `electron/modules/pptPreview.ts` | PowerPoint 預覽 (LibreOffice 轉 PDF) | 358 行 |
| 8 | Operation History | `electron/modules/operationHistory.ts` | 操作歷史記錄 (移動/重命名/刪除) | 351 行 |
| 9 | Blend Preview | `electron/modules/blendPreview.ts` | Blender 縮圖、高品質渲染 | 287 行 |
| 10 | RAW Preview | `electron/modules/rawPreview.ts` | Camera RAW 預覽 (dcraw) | 280 行 |
| 11 | Archive Preview | `electron/modules/archivePreview.ts` | 壓縮檔列表、解壓縮 (7zip/RAR) | 272 行 |
| 12 | Office Preview | `electron/modules/officePreview.ts` | PSD、DOCX、XLSX 預覽 | 272 行 |
| 13 | PDF Conversion | `electron/modules/pdfConversion.ts` | 圖片轉 PDF、PDF 轉圖片 | 242 行 |
| 14 | Font Preview | `electron/modules/fontPreview.ts` | 字體預覽 (TTF/OTF/WOFF) | 240 行 |
| 15 | PDF Preview | `electron/modules/pdfPreview.ts` | PDF 轉圖片、快取、九宮格預覽 | 231 行 |
| 16 | OCR | `electron/modules/ocr.ts` | 圖片/PDF 文字辨識 (Tesseract) | 231 行 |
| 17 | DWG Preview | `electron/modules/dwgPreview.ts` | DWG/DXF 轉 SVG | 185 行 |
| 18 | Audio Transcript | `electron/modules/audioTranscript.ts` | 音頻逐字稿 (OpenRouter) | 181 行 |
| 19 | AI Results | `electron/modules/aiResults.ts` | AI 結果存儲 (加載/保存/刪除) | 137 行 |
| 20 | ADS Metadata | `electron/modules/adsMetadata.ts` | Windows NTFS ADS 讀寫 | 111 行 |
| 21 | Cache Utils | `electron/modules/cacheUtils.ts` | 快取工具函數 | 46 行 |

**後端模組總計**: 約 6709 行

---

## 前端組件分析 (共 20 個)

### 🔴 需要優先拆分的大型組件

| # | 組件名稱 | 檔案路徑 | 行數 | 建議拆分 |
|:---|:---|:---|:---|:---|
| 1 | **PreviewPanel** | `src/components/PreviewPanel.tsx` | **2056 行** | 🔴 極需拆分 |
| 2 | **ImageEditor** | `src/components/ImageEditor.tsx` | **1196 行** | 🔴 需拆分 |
| 3 | **DestinationPanel** | `src/components/DestinationPanel.tsx` | **1150 行** | 🔴 需拆分 |
| 4 | **PromptSettingsDialog** | `src/components/PromptSettingsDialog.tsx` | **947 行** | 🟡 考慮拆分 |
| 5 | **FileTree** | `src/components/FileTree.tsx` | **867 行** | 🟡 考慮拆分 |

### 🟡 中型組件

| # | 組件名稱 | 檔案路徑 | 行數 |
|:---|:---|:---|:---|
| 6 | HelpDialog | `src/components/HelpDialog.tsx` | 604 行 |
| 7 | LinkedAnalysisDialog | `src/components/LinkedAnalysisDialog.tsx` | 443 行 |
| 8 | Toasts | `src/components/Toasts.tsx` | 434 行 |
| 9 | OperationHistoryPanel | `src/components/OperationHistoryPanel.tsx` | 414 行 |
| 10 | FontPreview | `src/components/FontPreview.tsx` | 401 行 |
| 11 | ThreeDPreview | `src/components/ThreeDPreview.tsx` | 397 行 |
| 12 | OCRDialog | `src/components/OCRDialog.tsx` | 342 行 |
| 13 | LegalDialog | `src/components/LegalDialog.tsx` | 319 行 |
| 14 | PreferencesDialog | `src/components/PreferencesDialog.tsx` | 296 行 |
| 15 | TexturePreview | `src/components/TexturePreview.tsx` | 293 行 |

### 🟢 小型組件 (無需拆分)

| # | 組件名稱 | 行數 |
|:---|:---|:---|
| 16 | ConfirmDialog | 206 行 |
| 17 | SettingsDialog | 199 行 |
| 18 | AboutDialog | 118 行 |
| 19 | LanguageDialog | 108 行 |
| 20 | ErrorBoundary | 40 行 |

**前端組件總計**: 約 10,830 行

---

## 已拆分 Hooks - 前端 (共 5 個)

| # | Hook 名稱 | 檔案路徑 | 功能描述 | 行數 |
|:---|:---|:---|:---|:---|
| 1 | useSmartSelection | `src/hooks/useSmartSelection.ts` | 智慧選擇 (序號/連號) | ~260 行 |
| 2 | useBatchApplyAi | `src/hooks/useBatchApplyAi.ts` | 批次套用 AI 分析結果 | ~185 行 |
| 3 | useBatchMove | `src/hooks/useBatchMove.ts` | 批次移動檔案 | ~165 行 |
| 4 | useKeyboardShortcuts | `src/hooks/useKeyboardShortcuts.ts` | 鍵盤快捷鍵 (數字鍵分配、Delete) | ~145 行 |
| 5 | useSettings | `src/hooks/useSettings.ts` | 設定管理 | ~100 行 |

**前端 Hooks 總計**: 約 855 行

---

## 🔴 建議優先拆分項目

### 1. PreviewPanel.tsx (2056 行) → 拆分為多個預覽器

```
src/components/preview/
├── PreviewPanel.tsx          # 主容器 (~300 行)
├── ImagePreview.tsx          # 圖片預覽 (~200 行)
├── VideoPreview.tsx          # 影片預覽 (~200 行)
├── AudioPreview.tsx          # 音頻預覽 (~150 行)
├── DocumentPreview.tsx       # 文件預覽 (PDF/Office) (~200 行)
├── ArchivePreview.tsx        # 壓縮檔預覽 (~200 行)
├── AiResultPanel.tsx         # AI 結果面板 (~300 行)
├── FileInfoPanel.tsx         # 檔案資訊面板 (~150 行)
├── PreviewToolbar.tsx        # 工具列 (~100 行)
└── hooks/
    ├── usePreviewCache.ts    # 預覽快取 (~100 行)
    └── useAiAnalysis.ts      # AI 分析狀態 (~100 行)
```

### 2. ImageEditor.tsx (1196 行) → 拆分為工具模組

```
src/components/imageEditor/
├── ImageEditor.tsx           # 主容器 (~200 行)
├── EditorCanvas.tsx          # Canvas 畫布 (~200 行)
├── EditorToolbar.tsx         # 工具列 (~150 行)
├── EditorSidebar.tsx         # 側邊控制面板 (~200 行)
├── tools/
│   ├── BrushTool.tsx         # 畫筆工具 (~80 行)
│   ├── ShapeTool.tsx         # 形狀工具 (~80 行)
│   ├── TextTool.tsx          # 文字工具 (~80 行)
│   └── CropTool.tsx          # 裁切工具 (~80 行)
└── hooks/
    └── useEditorState.ts     # 編輯器狀態 (~150 行)
```

### 3. DestinationPanel.tsx (1150 行) → 拆分為子組件

```
src/components/destination/
├── DestinationPanel.tsx      # 主容器 (~200 行)
├── FolderGrid.tsx            # 九宮格配置 (~200 行)
├── FolderCard.tsx            # 單一資料夾卡片 (~150 行)
├── PresetSelector.tsx        # 預設組選擇器 (~150 行)
├── BatchMovePanel.tsx        # 批次移動面板 (~200 行)
├── StatsPanel.tsx            # 統計面板 (~100 行)
└── hooks/
    └── useFolderConfig.ts    # 資料夾配置 (~100 行)
```

---

## main.ts 目前保留的功能

| 行號範圍 | 功能分類 | 功能描述 |
|:---|:---|:---|
| 1-30 | **導入** | 模組導入、ESM 兼容設定 |
| 31-80 | **預載入** | `preloadOtherModules` (PSD/XLSX/Docx) |
| 81-155 | **環境與錯誤處理** | APP_ROOT、logError、getUserFriendlyErrorMessage |
| 156-290 | **語言與翻譯** | menuTranslations (三語)、語言工具函數 |
| 291-390 | **應用菜單** | `createAppMenu` (Settings + Help) |
| 391-450 | **視窗創建** | `createWindow` |
| 451-520 | **對話框/平台** | dialog handlers, shell handlers |
| 521-650 | **檔案系統基礎** | `fs:readDir`, `fs:getFolderSizes`, `fs:searchFiles` |
| 651-730 | **Prompt 配置** | `loadPromptConfig`, `replacePromptVariables` |
| 731-800 | **檔案資訊** | `fs:getStats`, `fs:readImageAsBase64` |
| 801-900 | **縮圖快取** | `fs:checkPreviewCache`, `fs:savePreviewCache` |
| 901-1050 | **智慧重命名** | `fs:smartRename` |
| 1051-1146 | **App 生命週期** | `whenReady`, 模組註冊, IPC 監聽 |

---

## 可繼續拆分的功能 (可選)

### main.ts (後端)

| 功能 | 建議模組 | 行數 | 優先級 |
|:---|:---|:---|:---|
| 智慧重命名 | `smartRename.ts` | ~150 行 | 🟡 中 |
| 縮圖快取 | `thumbnailCache.ts` | ~100 行 | 🟢 低 |
| 檔案資訊 | `fileInfo.ts` | ~70 行 | 🟢 低 |
| 目錄操作 | `directoryOps.ts` | ~130 行 | 🟢 低 |
| Prompt 配置 | `promptConfig.ts` | ~80 行 | 🟢 低 |

### App.tsx (前端)

| 功能 | 建議 Hook | 行數 | 優先級 |
|:---|:---|:---|:---|
| AI 分析狀態 | `useAiAnalysis` | ~150 行 | 🔴 高 |
| 檔案選擇 | `useFileSelection` | ~100 行 | 🟡 中 |
| 移動分配 | `useMoveAssignment` | ~120 行 | 🟡 中 |
| 音頻逐字稿 | `useAudioTranscript` | ~80 行 | 🟢 低 |

---

## 模組化進度

### ✅ 後端已完成 (21 個模組)
- [x] AI Analysis (870 行)
- [x] Image Operations (642 行)
- [x] Settings (500 行)
- [x] File Operations (452 行)
- [x] AI Classify (414 行)
- [x] Video Preview (407 行)
- [x] PPT Preview (358 行)
- [x] Operation History (351 行)
- [x] Blend Preview (287 行)
- [x] RAW Preview (280 行)
- [x] Archive Preview (272 行)
- [x] Office Preview (272 行)
- [x] PDF Conversion (242 行)
- [x] Font Preview (240 行)
- [x] PDF Preview (231 行)
- [x] OCR (231 行)
- [x] DWG Preview (185 行)
- [x] Audio Transcript (181 行)
- [x] AI Results (137 行)
- [x] ADS Metadata (111 行)
- [x] Cache Utils (46 行)

### ⏳ 前端待拆分
- [ ] PreviewPanel.tsx (2056 行) → 分拆為 8-10 個子組件
- [ ] ImageEditor.tsx (1196 行) → 分拆為 6-8 個子組件
- [ ] DestinationPanel.tsx (1150 行) → 分拆為 5-7 個子組件
- [ ] PromptSettingsDialog.tsx (947 行) → 考慮拆分
- [ ] FileTree.tsx (867 行) → 考慮拆分

---

## 修改歷史

| 日期 | 變更 |
|:---|:---|
| 2026-01-03 | 更新模組統計 (21 個後端模組)，新增前端組件分析，建議拆分方案 |
| 2025-12-27 | 完成 useKeyboardShortcuts、useBatchMove、useBatchApplyAi Hooks |
| 2025-12-27 | 完成 PDF Conversion、Image Operations、AI Results 模組 |
| 2025-12-27 | 完成 ADS Metadata 模組、修復默認 thumbnail prompt |
| 2025-12-27 | 完成 Settings、File Operations 模組 |
| 2025-12-26 | 完成 AI Analysis、AI Classify、Office Preview、Archive Preview 模組 |
| 2025-12-26 | 完成 Video Preview 模組 |
| 2025-12-25 | 完成 PDF、Blend、DWG Preview 模組 |
