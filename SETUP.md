# Penicillus 安裝與使用說明

本地多功能圖片工具：基礎編輯、截圖、塗鴉標註、濾鏡調色、AI 放大、影片 / GIF、LoRA 訓練。
介面採用液態玻璃（Liquid Glass）風格，字型為 Noto Serif TC / SC。

## 一、直接執行（開發模式）

需要 Python 3.10 以上（建議 3.12）。

```bat
cd /d Penicillus
py -3.12 -m venv .venv
.venv\Scripts\python -m pip install -r requirements.txt
.venv\Scripts\python main.py
```

## 二、打包成安裝程式（Setup.exe）

1. 第一次需要安裝 **Inno Setup**（免費）：到 https://jrsoftware.org/isdl.php 下載 `innosetup-…-x64.exe` 安裝（一路下一步即可）。
2. 雙擊 `build.bat`，會依序完成：安裝依賴 → 產生圖示 → PyInstaller 打包 → 放入 ffmpeg / Real-ESRGAN / 字型 → 編譯安裝程式。
3. 完成後得到：

| 檔案 | 說明 |
|---|---|
| `installer\Output\Penicillus-Setup-版本.exe` | **安裝程式**：雙擊安裝，會建立開始功能表捷徑（可選桌面捷徑），可從「設定 → 應用程式」解除安裝 |
| `dist\Penicillus\Penicillus.exe` | 免安裝版：整個 `dist\Penicillus` 資料夾可以直接壓縮帶走 |

安裝程式預設只替目前使用者安裝（不需要系統管理員權限，裝在 `%LOCALAPPDATA%\Programs\Penicillus`），
安裝時也可以選「替所有使用者安裝」（裝到 `C:\Program Files\Penicillus`，需要系統管理員）。
沒有安裝 Inno Setup 時，`build.bat` 仍會產生免安裝版，只是略過 Setup.exe。
版本號在 `main.py` 的 `VERSION`，安裝檔名會跟著變。

打包時會自動把下面這些放到 exe 旁邊（有的話）：

| 資料夾 | 用途 |
|---|---|
| `ffmpeg\` | 影片 / GIF |
| `realesrgan\` | AI 放大 |
| `fonts\` | Noto Serif TC / SC（沒有的話會從系統字型複製） |

## 三、外部工具

> 安裝檔（Setup.exe）已經內含 FFmpeg 與 Real-ESRGAN ncnn-vulkan，直接安裝就能用。
> GitHub 上的原始碼**不含**這兩個執行檔（檔案太大、授權各自不同），從原始碼執行或自己打包時，照下面的步驟放進對應資料夾。

### FFmpeg（影片 / GIF 板塊）

1. 下載 Windows 版 FFmpeg（例如 gyan.dev 的 `ffmpeg-release-essentials.zip`）。
2. 把 `bin` 裡的 `ffmpeg.exe`、`ffprobe.exe` 放到：

```
Penicillus\ffmpeg\ffmpeg.exe
Penicillus\ffmpeg\ffprobe.exe
```

已加入系統 PATH 的 FFmpeg 也可以直接使用。

### Real-ESRGAN（AI 放大板塊）

1. 到 GitHub `xinntao/Real-ESRGAN` 的 Releases 下載 `realesrgan-ncnn-vulkan-…-windows.zip`。
2. 整包解壓到 `Penicillus\realesrgan\`，結構如下（models 要和 exe 在一起）：

```
Penicillus\realesrgan\realesrgan-ncnn-vulkan.exe
Penicillus\realesrgan\models\realesrgan-x4plus.param / .bin …
```

N 卡、A 卡、內顯都能跑。顯存 8GB 以下建議「分塊大小」選 400 或 200。

放大上限是**最長邊 15360 px（16K）**：16:9 最大 15360×8640、4:3 最大 15360×11520、方圖最大 15360×15360。
超過上限會自動降低倍率；超過 8K 的輸出比較久，存成 PNG 也要等一下（WebP 格式最長邊只能到 16383，所以 16K 可以存）。
畫布上的超大圖會以縮小版顯示，存檔仍是原解析度。

### LoRA 訓練（需要 NVIDIA 顯示卡）

本板塊是 kohya-ss/sd-scripts 的圖形介面，需要你已經裝好以下其中一種：
秋葉 lora-scripts 整合包、kohya_ss GUI，或 sd-scripts 原版（含 CUDA 版 PyTorch）。

**兩種模式（左上角切換，會記住）**

- **新手模式**：四張步驟卡片，每張右上角會顯示是否就緒。
  1. 訓練工具：按「自動偵測」會搜尋各磁碟，找到訓練工具與 ComfyUI，並自動檢查顯示卡。
  2. 底模：直接從 ComfyUI 的 `models\checkpoints` 挑選，自動判斷 SD1.5 / SDXL。
  3. 圖片：選資料夾、填觸發詞；沒有標註檔可以勾「自動產生標註」。
  4. 訓練：選輕度 / 標準 / 加強，按「一鍵開始訓練」。重複次數、回合數、顯存設定都依你的顯卡與圖片數自動決定，
     完成後可自動放進 ComfyUI 的 `models\loras`。
- **老手模式**：所有參數都能調整。滑鼠移到任何參數（標籤後面有 ⓘ）上，右側「參數說明」會解釋作用與建議值。

不知道怎麼開始？按右上角「📖 新手教學」。

底模請自行下載，要和之後使用 LoRA 的模型同一系列（SD1.5 或 SDXL）。
訓練好的 `.safetensors` 放進 WebUI / ComfyUI 的 `models\Lora` 使用。

**8GB 顯存（例如筆電 RTX 4070）建議：**

- SD1.5：用「SD1.5 ｜ 8GB 顯卡」預設，穩定。
- SDXL（Illustrious、NoobAI 等）：用「SDXL ｜ 8GB 顯卡（吃緊）」預設 —— 768 解析度、只訓練 U-Net、
  快取文字編碼器輸出、fp8 載入底模。能跑但較慢，不保證每個底模都行；顯存不足就降 Dim 或解析度。
- 插電、切到高效能模式，訓練時關掉吃顯存的程式。

### 生圖（操控 ComfyUI）

「生圖」分頁在背景操控你電腦上的 ComfyUI，不需要碰節點和工作流：

1. 第一次打開會自動搜尋 ComfyUI（也可以在右上角「設定」手動指定）。
2. 按「啟動 ComfyUI」（約 30～90 秒）。如果你已經用自己的啟動器開了 ComfyUI，會自動連上
   （會試 8188、3198〔秋葉啟動器〕、8000 等連接埠）。
3. 選底模、需要的話加 LoRA、寫提示詞，按「生成」。生成中會即時顯示預覽。
4. 結果同時存在 ComfyUI 的 `output` 資料夾；也可以「另存」或「送到其他板塊」（例如送去 AI 放大）。

由 Penicillus 啟動的 ComfyUI 會在關閉 Penicillus 時一起關閉；你自己開的不會被關掉。
ComfyUI 的執行紀錄在 `%LOCALAPPDATA%\Penicillus\comfyui.log`。

### 下載模型（Civitai / HuggingFace）

「生圖 → 下載模型」：

- **搜尋 Civitai**：選類型（底模 / LoRA / VAE / Embedding / ControlNet / 動態模組…）、系列（SD1.5 / SDXL / Illustrious / Pony / Flux / Qwen / Wan 影片…）、排序。
  預設過濾成人內容；勾「顯示成人內容（NSFW）」後搜尋結果與預覽圖會包含成人內容（只在這台電腦生效）。
- **搜尋 HuggingFace**：可找 GGUF 量化模型、本地大語言模型（LLM）、看圖模型（VLM）等；切成多份的 GGUF 會一起下載。
  在「設定」可以填 HuggingFace 金鑰（下載需授權的模型）並切換成鏡像 hf-mirror.com（中國大陸較快）。
- **貼上網址**：Civitai 模型頁面（`civitai.com/models/…`）、HuggingFace 檔案網址（`…/blob/…` 或 `…/resolve/…`）。
- 按「下載到 ComfyUI」會讀檔頭自動放進對應資料夾（底模 → `checkpoints`、LoRA → `loras`、GGUF 擴散模型 → `unet`、
  文字編碼器 → `text_encoders`、本地 LLM → `LLM`…），有校驗碼的會自動驗證。
  下載中斷再按一次會續傳。
- 部分 Civitai 模型需要登入：到 Civitai「帳號設定 → API Keys」建立金鑰，填進 Penicillus 的「設定」。
- 下載前請留意每個模型的授權條款（是否可商用等）。

**匯入自己下載的模型**：在「下載模型」頁按「選擇模型檔…」，或直接把 `.safetensors` 拖進「生圖」分頁。
程式會讀檔頭判斷是底模（並分辨 SD1.5 / SDXL）、LoRA、VAE 還是 Embedding，複製到 ComfyUI 對應的資料夾；
勾「移動檔案」則不保留原檔（同一顆磁碟會瞬間完成）。判斷不出來的檔案會詢問要放哪裡。已存在的同名檔案會先問要不要覆蓋。

### 提示詞助手（本地語言模型）

「生圖」與「AI 影片」的「✨ 提示詞助手」使用 ComfyUI 裡的本地大語言模型（不會連網、不受線上審查）：

- 需要 ComfyUI 擴充 **ComfyUI-llama-cpp_vlm**，以及放在 `models\LLM` 的 GGUF 模型，例如 Qwen3-VL-8B-Instruct-abliterated（Q6_K 約 6.7 GB，8 GB 顯存可用）。
  沒有的話到「下載模型 → HuggingFace」搜尋 GGUF 下載，會自動放進 `models\LLM`。
- **文字 → 提示詞**：用中文 / 日文 / 英文描述想法，產生動漫標籤、寫實描述或影片描述。觸發詞（例如 `kaede_haruha`）會保留在最前面。
- **圖片反推提示詞**：需要模型對應的看圖檔（`mmproj`）。也支援 Florence-2 PromptGen；若你的 ComfyUI 裡 Florence-2 無法執行，會自動改用語言模型看圖。
- 成人內容只會描寫成年角色。

### 角色鎖定（選了角色 LoRA，角色就不會被改掉）

在「生圖」加入 LoRA 後，每個 LoRA 會標上類型：**角色**（藍）、**畫風**（紫）、**概念**（綠），按 ⚙ 可以修改。

- **角色 LoRA**：會出現「🔒 角色鎖定」面板，列出觸發詞與固定外貌（髮色、髮長、髮型、瞳色、標誌配件）。
  生成時程式會把它們放在提示詞最前面；場景提示詞裡和角色衝突的髮色、瞳色、髮型會被**自動移除**，
  提示詞助手與素材庫都只能改服裝、動作、表情、場景、光線。換底模只會改變畫風，不會改變角色。
- 設定會**永久記住**（%LOCALAPPDATA%\Penicillus\lora_profiles），下次選同一個 LoRA 自動套用。
  第一次使用的 LoRA 會從檔案內的訓練標籤自動推測；也可以按「從參考圖辨識特徵」用幾張角色圖自動找出固定外貌。
- **防漂移負面詞**：例如淺色頭髮的角色會自動加 lack hair，避免暗光、海邊等場景把頭髮畫黑。
  負面詞裡如果有角色自己的特徵（例如 long hair 之於超長髮角色）會被自動移除。
- **底模提示詞規則**：依底模自動判斷 Illustrious / NoobAI、Pony、SDXL 寫實、SD1.5、Flux 等自然語言模型，
  勾「自動加上底模建議的品質詞」會加上對應的品質詞（Pony 的 score_9…、寫實的 photorealistic…）；
  「套用建議負面詞」加上該類底模常用的負面詞。LoRA 和底模系列不相容時會先警告。
- 「預覽最終提示詞」可以看到實際送出的正向 / 負向提示詞，以及被移除的衝突描述。

### 素材庫（📚）

「生圖」與「AI 影片」的場景提示詞旁有「📚 素材庫」：

- **服裝**：RuiC Costume Atlas 的 1,407 套服裝，可依季節、品類、顏色篩選，選中後自動轉成服裝提示詞。
- **姿勢**：Shisei 的 280 組姿勢線稿（CC0），附中日英名稱與動作描述。
- **鏡頭構圖、光線、表情、場景、畫風氛圍**：社群通用的 Danbooru 標籤（Illustrious / NoobAI / Pony 訓練用詞），
  自然語言底模會顯示對應的句子寫法。
- 圖鑑只在打開時讀取索引，縮圖看到才下載並存在本機快取（%LOCALAPPDATA%\Penicillus\reference），不打包、不重新散佈；
  成人分類只在「顯示成人內容」開啟時出現。素材只會加進場景提示詞，不會改到角色鎖定的外貌。
### AI 影片（Wan 2.2，操控 ComfyUI）

「AI 影片」分頁與「生圖」共用同一個 ComfyUI 連線，功能：

| 功能 | 說明 | 模型組合 |
|---|---|---|
| 圖生影片 | 一張圖當第一格，寫動作與鏡頭 | Wan 2.2 TI2V 5B（快）或 I2V 14B（高畫質） |
| 文生影片 | 只用文字產生影片 | 5B 或 T2V 14B |
| 首尾幀 | 給第一格與最後一格，補出中間過程 | I2V 14B |
| 動作遷移 | 角色圖照著參考影片的動作、表情動起來 | Animate 14B |
| 人物替換 | 把影片裡的人換成你的角色，保留背景與動作 | Animate 14B |

- **模型管理** 頁會偵測 ComfyUI 已安裝的影片模型，缺少的可以一鍵下載到正確資料夾（都是 8 GB 顯存可跑的 GGUF 版本，支援續傳）。
  也可以「載入已下載的模型」：選檔案或整個資料夾，自動判斷種類；同一顆磁碟用硬連結，不佔額外空間。
- 「手動選擇模型檔」可以自己指定每個槽位要用哪個檔案。
- 動作遷移 / 人物替換需要 ComfyUI 擴充 **ComfyUI-WanAnimatePreprocess**、**ComfyUI-VideoHelperSuite**，人物替換另需 **segment-anything-2** 與 **KJNodes**；缺少時畫面會提示。
- 14B 模型勾「快速模式」會套用 4 步加速 LoRA，速度快很多。
- 筆電請插電並切到**遊戲 / 高效能模式**：省電或辦公模式會壓低顯示卡功耗，影片生成會慢上許多。
- 完成的影片存在 ComfyUI 的 `output\Penicillus`；可以「接續生成」（用最後一格當新的起始圖）或送到「影片 / GIF」轉檔。
## 四、介面小技巧

- 右上角 🌐 按鈕：切換介面語言（繁體中文 / 简体中文 / English / 日本語），立即生效。
- 右上角 ◐ 按鈕：深色 / 淺色 / 跟隨系統、背景（緞帶、極光、流體、散景、絲綢或自訂圖片）、背景色調、玻璃材質（玻璃 / 水晶 / 磨砂 / 亞克力 / 雲母）、字型。
- 「外觀 → 字型」：分別設定繁中 / 簡中 / 日文 / 英文字型，也可以上傳自己的字型檔（.ttf / .otf / .ttc），塗鴉的文字工具也能選用。
- 畫布：滾輪縮放、中鍵或右鍵拖曳平移、雙擊回到適合大小。
- 每個板塊都有「送到其他板塊」，例如截圖 → 塗鴉標註 → 濾鏡 → AI 放大，不用經過剪貼簿。
- 拖曳標題列空白處可移動視窗，雙擊最大化；視窗邊緣可拖曳縮放。

## 五、回報問題

未預期的錯誤會寫入 `%LOCALAPPDATA%\Penicillus\error.log`，把內容貼給我即可。
