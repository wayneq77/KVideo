# 貢獻指南 (Contributing Guide)

歡迎來到 **KVideo** 項目！我們非常感謝你願意爲這個項目做出貢獻。無論是修復 Bug、添加新功能、改進文檔，還是提出建議，你的每一份貢獻都將讓這個項目變得更好。

爲了確保協作順暢、代碼質量一致，請在提交貢獻前仔細閱讀本指南。

## 📋 目錄

- [行爲準則](#行爲準則)
- [快速開始](#快速開始)
- [開發環境設置](#開發環境設置)
- [代碼規範](#代碼規範)
- [Git 工作流程](#git-工作流程)
- [提交規範](#提交規範)
- [Pull Request 指南](#pull-request-指南)
- [設計系統規範](#設計系統規範)
- [測試要求](#測試要求)
- [常見問題](#常見問題)

## 🤝 行爲準則

我們致力於構建一個開放、友好、包容的社區環境。請在參與項目時：

- ✅ 保持尊重和禮貌
- ✅ 歡迎不同的觀點和經驗
- ✅ 接受建設性的批評
- ✅ 專注於對社區最有利的事情
- ❌ 不要使用性別化的語言或圖像
- ❌ 不要進行人身攻擊或政治攻擊
- ❌ 不要騷擾或歧視他人

詳細的行爲準則請參閱 [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)。

## 🚀 快速開始

### 我能貢獻什麼？

以下是一些你可以做出貢獻的方式：

1. **🐛 報告 Bug**：發現了問題？請提交 Issue
2. **💡 提出新功能**：有好想法？在 Discussions 或 Issues 中分享
3. **📝 改進文檔**：發現文檔不清晰或有錯誤？幫助我們改進
4. **🎨 優化 UI/UX**：讓界面更美觀、更易用
5. **⚡ 性能優化**：讓應用運行得更快
6. **🔧 修復 Bug**：解決現有的問題
7. **✨ 添加功能**：實現新的特性

### 第一次貢獻？

如果這是你第一次爲開源項目做貢獻，我們推薦：

1. 瀏覽 [GitHub Issues](https://github.com/KuekHaoYang/KVideo/issues)
2. 尋找標記爲 `good first issue` 的問題
3. 在 Issue 中評論，表明你想要解決這個問題
4. 按照本指南進行開發和提交

## 🛠 開發環境設置

### 系統要求

確保你的開發環境滿足以下要求：

| 工具 | 最低版本 | 推薦版本 | 檢查命令 |
|------|----------|----------|----------|
| **Node.js** | 20.0.0 | 20.x LTS | `node --version` |
| **npm** | 9.0.0 | 10.x | `npm --version` |
| **Git** | 2.30.0 | 最新版本 | `git --version` |

### 詳細設置步驟

#### 1. Fork 倉庫

點擊 GitHub 頁面右上角的 "Fork" 按鈕，將項目 Fork 到你的賬號下。

#### 2. 克隆倉庫

```bash
# 克隆你 Fork 的倉庫
git clone https://github.com/YOUR_USERNAME/KVideo.git
cd KVideo

# 添加上游倉庫
git remote add upstream https://github.com/KuekHaoYang/KVideo.git
```

#### 3. 安裝依賴

```bash
npm install
```

#### 4. 啓動開發服務器

```bash
npm run dev
```

訪問 `http://localhost:3000` 查看應用。

#### 5. 驗證環境

確保以下命令都能正常運行：

```bash
# 代碼檢查
npm run lint

# 構建測試
npm run build
```

## 📏 代碼規範

### 核心規範

#### 1. 文件長度限制 ⚠️

> [!CAUTION]
> **這是項目的硬性規則！所有項目文件必須保持在 150 行以內（除系統文件外）。**

**檢查命令：**

```bash
find . -type f -not -path "*/node_modules/*" -not -path "*/.next/*" -not -path "*/.git/*" -not -name "package-lock.json" -not -name "*.png" -not -name "*.md" | xargs wc -l | awk '$1 > 150 && $2 != "total" {print $2 " - " $1 "行"}'
```

**如果命令有輸出，說明有文件超過 150 行，必須重構！**

**重構策略：**

如果文件超過 150 行，請使用以下方法重構：

##### A. 提取組件

**問題：** 一個組件太長，包含太多 JSX

**解決方案：** 將大組件拆分爲多個小組件

```typescript
// ❌ 不好：一個 200 行的大組件
export function VideoPlayer() {
  // 150+ 行代碼
  return (
    <div>
      {/* 大量 JSX */}
    </div>
  );
}

// ✅ 好：拆分爲多個小組件
export function VideoPlayer() {
  return (
    <div>
      <PlayerControls />
      <ProgressBar />
      <VolumeControl />
    </div>
  );
}

// PlayerControls.tsx (單獨文件)
export function PlayerControls() { /* ... */ }

// ProgressBar.tsx (單獨文件)
export function ProgressBar() { /* ... */ }

// VolumeControl.tsx (單獨文件)
export function VolumeControl() { /* ... */ }
```

##### B. 提取自定義 Hook

**問題：** 組件包含大量狀態邏輯

**解決方案：** 將邏輯提取到自定義 Hook

```typescript
// ❌ 不好：組件內有大量狀態邏輯
export function SearchPage() {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState([]);
  const [loading, setLoading] = useState(false);
  // ... 大量邏輯
  
  const handleSearch = async () => {
    // ... 50+ 行邏輯
  };
  
  return <div>{/* JSX */}</div>;
}

// ✅ 好：提取到自定義 Hook
export function SearchPage() {
  const { query, results, loading, handleSearch } = useSearch();
  return <div>{/* JSX */}</div>;
}

// useSearch.ts (單獨文件)
export function useSearch() {
  // ... 所有狀態邏輯
  return { query, results, loading, handleSearch };
}
```

##### C. 提取工具函數

**問題：** 文件包含大量輔助函數

**解決方案：** 將工具函數移到 `lib/utils/`

```typescript
// ❌ 不好：組件文件包含工具函數
export function VideoCard() {
  const formatDuration = (seconds: number) => {
    // ... 格式化邏輯
  };
  
  const formatDate = (date: Date) => {
    // ... 格式化邏輯
  };
  
  // ... 更多工具函數
  
  return <div>{/* JSX */}</div>;
}

// ✅ 好：提取到工具文件
import { formatDuration, formatDate } from '@/lib/utils/format-utils';

export function VideoCard() {
  return <div>{/* JSX */}</div>;
}

// lib/utils/format-utils.ts
export function formatDuration(seconds: number) { /* ... */ }
export function formatDate(date: Date) { /* ... */ }
```

##### D. 模塊化

**問題：** 單個文件處理多個相關功能

**解決方案：** 按功能拆分文件並使用桶文件（barrel exports）

```typescript
// ❌ 不好：player-utils.ts 包含 200 行
export function parseHLS() { /* ... */ }
export function handlePlayback() { /* ... */ }
export function manageQuality() { /* ... */ }
// ... 更多函數

// ✅ 好：拆分爲多個文件
// lib/utils/player/index.ts
export * from './hls-parser';
export * from './playback-manager';
export * from './quality-manager';

// lib/utils/player/hls-parser.ts
export function parseHLS() { /* ... */ }

// lib/utils/player/playback-manager.ts
export function handlePlayback() { /* ... */ }

// lib/utils/player/quality-manager.ts
export function manageQuality() { /* ... */ }
```

#### 2. TypeScript 規範

**類型安全**

```typescript
// ❌ 避免使用 any
function processData(data: any) {
  return data.value;
}

// ✅ 使用具體類型
interface VideoData {
  id: string;
  title: string;
  url: string;
}

function processData(data: VideoData) {
  return data.title;
}

// ✅ 或使用 unknown（需要類型檢查）
function processData(data: unknown) {
  if (typeof data === 'object' && data !== null && 'value' in data) {
    return (data as { value: string }).value;
  }
  throw new Error('Invalid data');
}
```

**函數返回類型**

```typescript
// ❌ 缺少返回類型
function calculateTotal(items) {
  return items.reduce((sum, item) => sum + item.price, 0);
}

// ✅ 明確返回類型
function calculateTotal(items: Item[]): number {
  return items.reduce((sum, item) => sum + item.price, 0);
}
```

**接口定義**

```typescript
// ✅ 使用 interface 定義對象類型
interface VideoCardProps {
  video: Video;
  onPlay: (id: string) => void;
  className?: string;
}

// ✅ 使用 type 定義聯合類型
type ThemeMode = 'light' | 'dark' | 'system';
```

#### 3. React 組件規範

**函數組件**

```typescript
// ✅ 標準函數組件結構
interface ButtonProps {
  variant?: 'primary' | 'secondary';
  children: React.ReactNode;
  onClick?: () => void;
}

export function Button({ variant = 'primary', children, onClick }: ButtonProps) {
  return (
    <button
      className={`btn btn-${variant}`}
      onClick={onClick}
    >
      {children}
    </button>
  );
}
```

**組件文件組織**

```typescript
// 1. 導入
import React from 'react';
import { useState } from 'react';
import { useRouter } from 'next/navigation';

// 2. 類型定義
interface ComponentProps {
  // ...
}

// 3. 組件定義
export function Component({ prop1, prop2 }: ComponentProps) {
  // 4. Hooks
  const [state, setState] = useState();
  const router = useRouter();
  
  // 5. 事件處理函數
  const handleClick = () => {
    // ...
  };
  
  // 6. 渲染
  return (
    <div>{/* JSX */}</div>
  );
}
```

**單一職責原則**

```typescript
// ❌ 組件做太多事情
export function VideoSection() {
  // 獲取數據
  // 處理搜索
  // 渲染列表
  // 處理分頁
  // 處理過濾
}

// ✅ 拆分爲專注的組件
export function VideoSection() {
  const videos = useVideos();
  return (
    <div>
      <SearchBar />
      <FilterPanel />
      <VideoList videos={videos} />
      <Pagination />
    </div>
  );
}
```

#### 4. 樣式規範

**Tailwind CSS 優先**

```typescript
// ✅ 使用 Tailwind 類名
export function Card({ children }: { children: React.ReactNode }) {
  return (
    <div className="rounded-2xl glass p-6 hover:shadow-lg transition-shadow">
      {children}
    </div>
  );
}
```

**遵循 Liquid Glass 設計系統**

```typescript
// ✅ 正確使用圓角
<div className="rounded-2xl">  {/* 容器：大圓角 */}
<div className="rounded-full">  {/* 小元素：完全圓形 */}

// ❌ 不要使用其他圓角值
<div className="rounded-lg">   {/* 錯誤！ */}
<div className="rounded-xl">   {/* 錯誤！ */}
```

**響應式設計**

```typescript
// ✅ 移動優先的響應式設計
<div className="
  flex flex-col           {/* 移動端：垂直佈局 */}
  md:flex-row            {/* 平板及以上：水平佈局 */}
  gap-4 md:gap-6         {/* 響應式間距 */}
">
```

#### 5. 命名規範

**文件命名**

- 組件文件：`PascalCase.tsx`（例如：`VideoCard.tsx`）
- Hook 文件：`camelCase.ts`（例如：`useVideoPlayer.ts`）
- 工具文件：`kebab-case.ts`（例如：`format-utils.ts`）
- 類型文件：`kebab-case.ts`（例如：`video-types.ts`）

**變量命名**

```typescript
// ✅ 清晰的命名
const videoList = [...];
const isLoading = false;
const handleSubmit = () => {};

// ❌ 模糊的命名
const data = [...];
const flag = false;
const fn = () => {};
```

**常量命名**

```typescript
// ✅ 全大寫 + 下劃線
const MAX_VIDEO_DURATION = 7200;
const API_BASE_URL = 'https://api.example.com';
```

#### 6. 導入順序

```typescript
// 1. React 和 Next.js
import React from 'react';
import { useState } from 'react';
import Link from 'next/link';

// 2. 第三方庫
import { create } from 'zustand';

// 3. 項目別名導入
import { Button } from '@/components/ui/Button';
import { formatDate } from '@/lib/utils/date-utils';

// 4. 相對路徑導入
import { LocalComponent } from './LocalComponent';

// 5. 類型導入
import type { Video } from '@/lib/types/video';
```

## 🔄 Git 工作流程

### 分支策略

**主分支**

- `main`：穩定的生產分支，只接受 PR 合併

**功能分支命名**

遵循以下命名規範：

- `feat/功能名稱`：新功能（例如：`feat/add-playlist`）
- `fix/問題描述`：錯誤修復（例如：`fix/search-crash`）
- `docs/文檔修改`：文檔更新（例如：`docs/update-readme`）
- `refactor/重構名稱`：代碼重構（例如：`refactor/player-controls`）
- `perf/優化內容`：性能優化（例如：`perf/image-loading`）
- `style/樣式修改`：樣式調整（例如：`style/button-spacing`）
- `test/測試內容`：測試相關（例如：`test/add-unit-tests`）
- `chore/其他修改`：構建或工具變動（例如：`chore/update-deps`）

### 開發流程

#### 1. 同步上游倉庫

在開始新工作前，先同步最新的代碼：

```bash
# 獲取上游更新
git fetch upstream

# 切換到主分支
git checkout main

# 合併上游更新
git merge upstream/main

# 推送到你的 Fork
git push origin main
```

#### 2. 創建功能分支

```bash
# 從 main 創建新分支
git checkout -b feat/your-feature-name

# 確認當前分支
git branch
```

#### 3. 進行開發

在開發過程中：

- 頻繁提交小的、原子性的改動
- 編寫清晰的提交信息
- 定期運行 `npm run lint` 檢查代碼

#### 4. 提交前檢查

**必須通過的檢查：**

```bash
# 1. 代碼規範檢查
npm run lint

# 2. 文件長度檢查
find . -type f -not -path "*/node_modules/*" -not -path "*/.next/*" -not -path "*/.git/*" -not -name "package-lock.json" -not -name "*.png" -not -name "*.md" | xargs wc -l | awk '$1 > 150 && $2 != "total" {print $2 " - " $1 "行"}'

# 3. 構建測試
npm run build
```

**如果任何檢查失敗，必須先修復！**

#### 5. 推送分支

```bash
# 推送到你的 Fork
git push origin feat/your-feature-name
```

## 📝 提交規範

### Conventional Commits

我們使用 [Conventional Commits](https://www.conventionalcommits.org/) 規範：

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Type 類型：**

- `feat`：新功能
- `fix`：錯誤修復
- `docs`：文檔變更
- `style`：代碼格式（不影響代碼運行）
- `refactor`：重構
- `perf`：性能優化
- `test`：測試相關
- `chore`：構建過程或輔助工具的變動

**示例：**

```bash
# 簡單提交
git commit -m "feat: 添加視頻播放列表功能"

# 詳細提交
git commit -m "feat(player): 添加倍速播放功能

- 支持 0.5x 到 2x 的播放速度
- 添加速度選擇器 UI
- 保存用戶的速度偏好

Closes #123"
```

**提交信息最佳實踐：**

- ✅ 使用中文或英文（保持一致）
- ✅ 使用祈使句（"添加功能" 而不是 "添加了功能"）
- ✅ 第一行不超過 50 個字符
- ✅ 正文每行不超過 72 個字符
- ✅ 說明 "做了什麼" 和 "爲什麼"，而不僅是 "怎麼做"

## 🔍 Pull Request 指南

### 創建 PR

1. **推送分支到你的 Fork**

```bash
git push origin feat/your-feature-name
```

2. **在 GitHub 上創建 PR**

- 訪問你的 Fork 頁面
- 點擊 "Compare & pull request"
- 選擇目標分支：`KuekHaoYang/KVideo:main`

### PR 描述模板

```markdown
## 📝 變更說明

簡要描述這個 PR 做了什麼。

## 🎯 相關 Issue

Closes #123
Fixes #456

## 📸 截圖（如果是 UI 變更）

[如果有 UI 變更，添加截圖或 GIF]

## ✅ 檢查清單

- [ ] 代碼已通過 `npm run lint`
- [ ] 所有文件都在 150 行以內
- [ ] 構建成功（`npm run build`）
- [ ] 已在本地測試所有變更
- [ ] 遵循 Liquid Glass 設計系統
- [ ] 提交信息符合規範
- [ ] 已更新相關文檔

## 🧪 測試步驟

1. 第一步
2. 第二步
3. 預期結果

## 📌 額外說明

[任何其他需要 reviewer 知道的信息]
```

### PR 審查流程

1. **自動檢查**：GitHub Actions 會自動運行檢查
2. **代碼審查**：維護者會審查你的代碼
3. **修改請求**：如果需要修改，會留下評論
4. **批准和合並**：審查通過後會被合併

### 回應審查意見

```bash
# 進行修改後
git add .
git commit -m "refactor: 根據審查意見調整代碼"
git push origin feat/your-feature-name
```

PR 會自動更新。

## 🎨 設計系統規範

### Liquid Glass 原則

在編寫 UI 代碼時，必須遵循 Liquid Glass 設計系統：

#### 1. 圓角規範

> [!IMPORTANT]
> **只使用兩種圓角：`rounded-2xl` 和 `rounded-full`**

```typescript
// ✅ 正確
<div className="rounded-2xl">  {/* 容器、卡片、按鈕、輸入框 */}
<div className="rounded-full"> {/* 頭像、徽章、藥丸形狀 */}

// ❌ 錯誤
<div className="rounded-lg">
<div className="rounded-xl">
<div className="rounded-md">
```

#### 2. 玻璃效果

```typescript
// ✅ 使用 glass 類或 backdrop-filter
<div className="glass">
  {/* 內容 */}
</div>

// 或自定義玻璃效果
<div className="
  backdrop-blur-xl 
  backdrop-saturate-180 
  backdrop-brightness-110
  bg-white/10
  border border-white/20
">
```

#### 3. 動畫過渡

```typescript
// ✅ 使用標準過渡曲線
<button className="
  transition-all 
  duration-300 
  ease-out
  hover:scale-105
">
```

#### 4. 顏色系統

```typescript
// ✅ 使用 CSS 變量
<div className="bg-glass text-glass-text border-glass-border">

// 或 Tailwind 的語義化顏色
<div className="bg-primary text-primary-foreground">
```

### 組件複用

優先複用 `components/ui/` 下的基礎組件：

```typescript
// ✅ 好：複用基礎組件
import { Button } from '@/components/ui/Button';
import { Modal } from '@/components/ui/Modal';

export function Feature() {
  return (
    <Modal>
      <Button variant="primary">確定</Button>
    </Modal>
  );
}

// ❌ 不好：重新實現基礎組件
export function Feature() {
  return (
    <div className="modal">
      <button className="btn">確定</button>
    </div>
  );
}
```

## 🧪 測試要求

### 手動測試

在提交 PR 前，請手動測試以下內容：

#### 功能測試

- [ ] 新功能按預期工作
- [ ] 沒有破壞現有功能
- [ ] 邊界情況處理正確

#### 瀏覽器測試

在以下瀏覽器中測試：

- [ ] Chrome/Edge（最新版）
- [ ] Firefox（最新版）
- [ ] Safari（最新版）

#### 響應式測試

在以下設備尺寸測試：

- [ ] 移動端（375px - 428px）
- [ ] 平板端（768px - 1024px）
- [ ] 桌面端（1280px+）

#### 無障礙測試

- [ ] 鍵盤導航正常工作
- [ ] 焦點狀態清晰可見
- [ ] 屏幕閱讀器友好

### 代碼檢查

```bash
# 運行 ESLint
npm run lint

# 檢查文件長度
find . -type f -not -path "*/node_modules/*" -not -path "*/.next/*" -not -path "*/.git/*" -not -name "package-lock.json" -not -name "*.png" -not -name "*.md" | xargs wc -l | awk '$1 > 150 && $2 != "total" {print $2 " - " $1 "行"}'
```

## ❓ 常見問題

### Q1: 我應該從哪裏開始？

**A:** 查看標記爲 `good first issue` 的 Issues，這些通常比較簡單，適合新手。

### Q2: 如何讓文件保持在 150 行以內？

**A:** 參考 [文件長度限制](#1-文件長度限制-️) 部分的重構策略。關鍵是：
- 提取組件
- 提取 Hook
- 提取工具函數
- 模塊化

注：系統文件（如 README.md、CONTRIBUTING.md 等文檔）不受此限制。
- 提取組件
- 提取 Hook
- 提取工具函數
- 模塊化

### Q3: 我的 PR 多久會被審查？

**A:** 通常在 1-3 個工作日內。如果超過一週沒有回應，可以在 PR 中添加評論提醒。

### Q4: 可以同時提交多個 PR 嗎？

**A:** 可以，但建議每個 PR 專注於一個功能或修復。避免在一個 PR 中做太多不相關的改動。

### Q5: 如何解決合併衝突？

```bash
# 1. 同步上游
git fetch upstream
git checkout main
git merge upstream/main

# 2. 切換到功能分支並 rebase
git checkout feat/your-feature
git rebase main

# 3. 解決衝突後
git add .
git rebase --continue

# 4. 強制推送（因爲 rebase 改變了歷史）
git push origin feat/your-feature --force
```

### Q6: 我的提交信息寫錯了怎麼辦？

```bash
# 修改最後一次提交
git commit --amend -m "新的提交信息"

# 如果已經推送了
git push origin feat/your-feature --force
```

### Q7: 如何測試我的改動？

1. 啓動開發服務器：`npm run dev`
2. 在瀏覽器中手動測試功能
3. 測試不同的設備尺寸
4. 運行 `npm run build` 確保生產構建成功

### Q8: Liquid Glass 設計系統在哪裏定義？

在 `app/styles/glass.css` 文件中。所有組件都應該基於這個設計系統。

### Q9: 我需要更新文檔嗎？

如果你的 PR 包含以下內容，請更新相應文檔：

- 新功能：更新 README.md
- API 變化：更新相關注釋和文檔
- 配置變化：更新配置說明

### Q10: 如何報告安全漏洞？

請查看 [SECURITY.md](SECURITY.md) 瞭解安全漏洞報告流程。不要在公開 Issue 中討論安全問題。

## 📞 需要幫助？

如果你有任何問題：

1. **查看文檔**：README.md 和本指南
2. **搜索 Issues**：可能已經有人問過相同的問題
3. **提出問題**：在 Discussions 或 Issues 中提問
4. **聯繫維護者**：[@KuekHaoYang](https://github.com/KuekHaoYang)

## 🎉 感謝你的貢獻！

感謝你花時間閱讀本指南，併爲 KVideo 做出貢獻。每一個貢獻，無論大小，都讓這個項目變得更好。

我們期待看到你的 Pull Request！

---

<div align="center">
  <strong>讓我們一起打造更好的 KVideo！</strong>
</div>
