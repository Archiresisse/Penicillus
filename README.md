# Penicillus

本地多功能圖片工具（Windows），液態玻璃風格介面，支援繁體中文 / 简体中文 / English / 日本語。

**下載**：到右邊的 [Releases](../../releases) 下載 `Penicillus-Setup-版本.exe`，雙擊安裝即可（已內含 FFmpeg 與 Real-ESRGAN）。
**原始碼**：同一個 Release 裡的 `Penicillus-source-版本.zip`（完整原始碼，解壓即可依下方步驟執行或打包）。

## 功能

| 板塊 | 內容 |
|---|---|
| 生圖 | 在背景操控你電腦上的 ComfyUI：SDXL / Illustrious / Pony / SD1.5 / Anima；角色 LoRA 鎖定、畫風配方、高清修復、臉部與逐隻手修復、嚴選模式（骨架檢查自動淘汰崩壞草稿）、提示詞矛盾檢查、局部重繪、圖生圖、姿勢控制、出圖紀錄一鍵重現 |
| 提示詞助手 | 用本地語言模型（Qwen3-VL GGUF）把中文想法變成提示詞、從圖片反推；內建 Anima 提示詞生成器（Danbooru 標籤字典校正） |
| AI 影片 | Wan 2.2：圖生影片、文生影片、首尾幀、動作遷移、人物替換 |
| LoRA 訓練 | kohya sd-scripts 圖形介面：新手一鍵 / 老手全參數、訓練圖品質檢查、各輪次自動比較 |
| 編輯 / 截圖 / 塗鴉 / 濾鏡 | 基本修圖、截圖標註、調色 |
| AI 放大 | Real-ESRGAN ncnn-vulkan，最長邊到 16K |
| 影片 / GIF | FFmpeg 轉檔、剪輯、轉 GIF |

生圖、提示詞助手、AI 影片需要 [ComfyUI](https://github.com/Comfy-Org/ComfyUI)；沒有的話，Penicillus 可以自動下載官方免安裝版到你指定的資料夾。
LoRA 訓練需要 NVIDIA 顯示卡與 kohya sd-scripts（例如秋葉 lora-scripts 整合包）。

## 從原始碼執行 / 打包

需要 Python 3.12：

```bat
py -3.12 -m venv .venv
.venv\Scripts\python -m pip install -r requirements.txt
.venv\Scripts\python main.py
```

打包成安裝程式：裝好 [Inno Setup](https://jrsoftware.org/isdl.php)，並把 FFmpeg 放進 `ffmpeg\`、Real-ESRGAN ncnn-vulkan 放進 `realesrgan\` 後，執行 `build.bat`。
詳細說明見 [SETUP.md](SETUP.md)。

自我測試：`.venv\Scripts\python tools\selftest.py`

## 授權

本專案以 **GNU GPL v3.0** 授權（見 [LICENSE](LICENSE)），因為內含 GPL-3.0 的
[Anima Prompt Generator](https://github.com/GUntilB/Anima-Prompt-Generator)（`modules/anima_prompt.py`、`assets/anima/`）。
第三方元件與授權見 [THIRD_PARTY.md](THIRD_PARTY.md)。

---

**English** — Penicillus is a local, all-in-one image tool for Windows with a Liquid Glass UI: a ComfyUI front-end for
image generation (character-locked LoRAs, style presets, hires fix, face and per-hand fix, best-of-N anatomy screening,
inpainting, generation history), a local-LLM prompt assistant, Wan 2.2 video, a kohya LoRA training GUI, image editing,
Real-ESRGAN upscaling and FFmpeg video tools. Download the installer from Releases. Licensed under GPL-3.0.
