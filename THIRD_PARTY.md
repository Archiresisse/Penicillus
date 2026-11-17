# 第三方元件 / Third-party components

| 元件 | 用途 | 授權 | 來源 |
|---|---|---|---|
| Anima Prompt Generator 0.1.0（GUntilB），已修改 | `modules/anima_prompt.py`、`assets/anima/system_prompt.txt` | GPL-3.0-or-later | https://github.com/GUntilB/Anima-Prompt-Generator |
| Danbooru 標籤字典（danbooru-tag-csv，newtextdoc1111） | `assets/anima/tags.csv`、`aliases.csv` | MIT（標籤資料來自 Danbooru） | https://huggingface.co/datasets/newtextdoc1111/danbooru-tag-csv |
| Hina Mincho、Antic Didone、Sail 字型 | `fonts/` | SIL Open Font License 1.1（`fonts/OFL.txt`） | https://github.com/google/fonts |
| PySide6 / Qt | 介面 | LGPL-3.0 | https://www.qt.io/qt-for-python |
| Pillow、NumPy | 影像處理 | MIT-CMU / BSD-3-Clause | https://python-pillow.org / https://numpy.org |
| FFmpeg（只在安裝檔內） | 影片 / GIF | GPL-3.0（gyan.dev / GyanD 的 full build） | https://ffmpeg.org、https://github.com/GyanD/codexffmpeg |
| Real-ESRGAN ncnn-vulkan（只在安裝檔內） | AI 放大 | BSD-3-Clause（模型另依 Real-ESRGAN 授權） | https://github.com/xinntao/Real-ESRGAN |

安裝檔內的 FFmpeg 為未修改的官方 Windows 建置；其原始碼可從上面的 FFmpeg 網址取得。

Penicillus 會呼叫、但**不包含**下列外部程式與模型，請依各自授權使用：
ComfyUI 與其擴充節點、kohya sd-scripts、各種 Stable Diffusion / Anima / Wan 模型、Civitai 與 HuggingFace 上下載的檔案、
RuiC Costume Atlas 與 Shisei 姿勢圖鑑（只在使用時讀取索引與縮圖，不重新散佈）。
