# RC4WD 4WD 遙控車模擬器

電腦開 `sim.html`（3D 車 + 重寫自 `pico/main.py` 的控制邏輯 + 物理），手機開 `controller.html`
（你原本 `esp32/index.html` 的介面，只把 WebSocket 換成 WebRTC），兩邊用 PeerJS 配對後點對點直連。

## 部署到 GitHub Pages
1. 新增 repo，把這個資料夾的全部檔案（含 `js/`）放到根目錄 push 上去
2. Settings → Pages → Branch 選 `main` / `(root)` → Save
3. 約 1 分鐘後 `https://<帳號>.github.io/<repo>/` 即可使用

## 使用
電腦開 `sim.html` → 手機掃 QR → 手機橫放操作。沒手機時鍵盤：W/S 油門、A/D 轉向、Space 放油門、M 切模式、O 避障、1/2/3 或 C 切視角、R 重置。

## 檔案
- `js/sim-core.js`：Pico 邏輯移植 + 底盤物理 + 場地（無 DOM，可在 Node 測）
- `js/net.js`：PeerJS 載入與連線碼
- 外部函式庫走 CDN：three.js r128、qrcodejs、PeerJS 1.5.4（配對用公用 broker `0.peerjs.com`）

## 注意
- 手機與電腦在同一個 Wi-Fi 最穩；跨網路（手機 4G）可能因 NAT 連不上（只配 STUN，沒有 TURN）
- 物理參數（質量、摩擦、馬達扭矩）是估的，用來「看起來合理」，不能取代實車調 PID

## 模擬器 ⇄ 韌體 同步
手機介面的唯一來源是韌體包裡的 `esp32/index.html`；`controller.html` 只是把它的 WebSocket 換成 WebRTC。
模擬器右上角「網路」選單可模擬 Wi-Fi 延遲/抖動/卡頓，用來看延遲數值與失控保護（300 ms）的表現。
