# Gemini 啟動說明

本文件說明了啟動 Gemini 應用程式的各種常見方式。請根據您的環境和需求選擇最適合的方法。

## 1. 從命令列啟動

這是最常見且直接的啟動方式，特別適用於開發和測試。

### 執行可執行檔 (如果適用)
如果 Gemini 是一個編譯過的可執行檔 (例如 `.exe` 在 Windows, 或無副檔名的二進位檔在 Linux/macOS):
```bash
./gemini
```
或
```bash
/path/to/gemini/executable
```

### 執行腳本 (如果適用)
如果 Gemini 是透過腳本 (例如 Python, Node.js, Shell Script) 啟動:

**Python 腳本:**
```bash
python main.py
```
或
```bash
python /path/to/gemini/main.py
```

**Node.js 腳本:**
```bash
node app.js
```
或
```bash
node /path/to/gemini/app.js
```

**Shell 腳本:**
```bash
./start.sh
```
或
```bash
bash start.sh
```

### 帶有參數啟動
許多應用程式支援透過命令列參數進行配置。例如：
```bash
./gemini --port 8080 --env production
```

## 2. 使用整合開發環境 (IDE) 啟動

如果您正在使用 IDE (如 VS Code, PyCharm, IntelliJ IDEA 等) 進行開發，通常可以直接從 IDE 內部啟動 Gemini。

1.  開啟您的 Gemini 專案。
2.  找到主啟動文件 (例如 `main.py`, `app.js`)。
3.  使用 IDE 提供的「執行 (Run)」或「偵錯 (Debug)」按鈕。
4.  IDE 通常會自動處理環境變數和依賴項。

## 3. 作為服務啟動 (背景執行)

對於生產環境或需要持續運行的情況，通常會將 Gemini 作為系統服務啟動，使其在背景運行並在系統啟動時自動啟動。

### 使用 `nohup` 和 `&` (簡單背景執行)
這是一種簡單的背景執行方式，但管理起來不如專門的服務管理器方便。
```bash
nohup ./gemini > output.log 2>&1 &
```
這會將輸出重定向到 `output.log` 並在背景運行。

### 使用 `systemd` (Linux)
對於 Linux 系統，`systemd` 是管理服務的標準方式。
1.  建立一個 `.service` 檔案 (例如 `/etc/systemd/system/gemini.service`):
    ```ini
    [Unit]
    Description=Gemini Application
    After=network.target

    [Service]
    User=your_user
    WorkingDirectory=/path/to/gemini
    ExecStart=/path/to/gemini/executable --config /path/to/config.yaml
    Restart=always

    [Install]
    WantedBy=multi-user.target
    ```
2.  重新載入 systemd 配置並啟用服務：
    ```bash
    sudo systemctl daemon-reload
    sudo systemctl enable gemini
    sudo systemctl start gemini
    ```

### 使用 `Docker` 容器啟動
如果 Gemini 已經被容器化，您可以使用 Docker 啟動它。
1.  建置 Docker 映像檔 (如果尚未建置):
    ```bash
    docker build -t gemini-app .
    ```
2.  運行 Docker 容器:
    ```bash
    docker run -d -p 80:8080 --name gemini-instance gemini-app
    ```
    這會在背景運行容器，並將容器的 8080 埠映射到主機的 80 埠。

## 4. 使用任務排程器啟動 (例如 `cron`)

如果您需要 Gemini 在特定時間或間隔運行，可以使用任務排程器。

### 使用 `cron` (Linux/macOS)
編輯您的 crontab:
```bash
crontab -e
```
新增一行來排程任務 (例如，每天凌晨 3 點運行):
```cron
0 3 * * * /path/to/gemini/executable >> /var/log/gemini_daily.log 2>&1
```

---

請根據您的具體 Gemini 應用程式類型和部署環境，參考上述方法進行啟動。