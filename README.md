# Chang-Gung
# 🛡️ 防哽咽即時監測系統 (Anti-Choking Real-Time Monitoring System)

本專案是一個結合 **ESP32-S3 Sense** 影像與聲學採集、**Raspberry Pi 4B** 邊緣 AI 辨識、以及 **跨網路遠端 Web 監控網站** 的防哽咽與嗆咳即時預警系統。

---

## 📐 系統整體架構

```
[ ESP32-S3 Sense ]  --->  (QVGA 影像 / 16kHz PCM 音訊)
                                 │
                                 ▼
[ Raspberry Pi 4B (邊緣 AI)] ─ (MediaPipe 關鍵點 + YAMNet 聲學模型)
                                 │
                                 ▼
[ Cloudflare Tunnel (HTTPS) ] ─ (免費安全網路穿透)
                                 │
                                 ▼
[ 手機 / 平板 / 電腦 瀏覽器 ] ─ (跨網域 4G/5G/Wi-Fi 即時監控網頁)
```

---

## 🚨 警報分級與決策矩陣 (Decision Matrix)

| 警報等級 | 觸發條件 | 視覺與聲光響應 |
| :--- | :--- | :--- |
| **🚨 L3 哽噎警報 (Choking)** | • 手抓喉嚨 $\ge 5.0$ 秒 且 5 秒內伴隨咳嗽<br>• 手抓喉嚨 $\ge 1.0$ 秒 且伴隨身體劇烈晃動 | 網頁全螢幕紅色閃爍、聲光蜂鳴器、寫入資料庫、LINE 廣播通知 |
| **🚨 L2 劇烈嗆咳 (Severe Cough)** | • 偵測到咳嗽 且 同時伴隨身體劇烈晃動 (前傾掙扎) | 橘黃色警告閃爍、聲響提示 |
| **⚠️ L1 一般嗆咳 (Standard Cough)** | • 單獨偵測到咳嗽 (手部未停留於脖子，無劇烈晃動) | 藍色訊息提示、記錄時間戳 |
| **⚠️ 咀嚼超時預警** | • 閉嘴咀嚼時間超過 $4.0$ 秒未吞嚥 | 卡喉預警顯示 |

---

## 📁 專案目錄結構

```
anti-choking-system/
├── README.md                  # 專案說明文件
├── web_dashboard/             # 手機/平板/電腦 響應式 Web 監控網頁
│   ├── index.html             # 監控主介面
│   └── app.js                 # 即時 WebSocket 連線與警報元件
└── rpi_server/                # 樹莓派 邊緣計算與 Web 伺服器主程式
    ├── main.py                # FastAPI + MediaPipe + YAMNet 核心伺服器
    ├── requirements.txt       # Python 套件依賴清單
    └── setup_cloudflared.sh   # Cloudflare Tunnel 免費穿透一鍵安裝腳本
```

---

## 🚀 樹莓派快速啟動指南

1. **複製專案庫**：
   ```bash
   git clone https://github.com/951222-1/-.git anti-choking-system
   cd anti-choking-system/rpi_server
   ```

2. **安裝 Python 套件**：
   ```bash
   pip install -r requirements.txt
   ```

3. **啟動 Web 服務與邊緣計算**：
   ```bash
   python main.py
   ```

4. **啟用遠端 Cloudflare HTTPS 網址**（讓手機 4G/5G 可連）：
   ```bash
   chmod +x setup_cloudflared.sh
   ./setup_cloudflared.sh
   ```
