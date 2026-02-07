# Frontend Developer Test — Trading Terminal Dashboard

เป้าหมายคือสร้าง **Trading Dashboard แบบหน้าเทรดจริง (TradingView-like)**  
การตรวจงานจะทำจาก “หน้าเว็บอย่างเดียว”  
❌ ไม่ต้องเปิด DevTools  
❌ ไม่ต้อง deploy

---

## 🔒 Tech Stack & Rules (บังคับ)
- React (TypeScript แนะนำ)
- TailwindCSS
- Chart: TradingView Lightweight Charts (บังคับ)
- Drag & Drop / Resize: ใช้ library ได้ แต่ต้อง **smooth + stable**
- Data: Binance Public Market Data (REST + WebSocket)
- ❌ ห้าม mock ข้อมูลกราฟ
- ❌ ห้ามใช้ DevTools เพื่ออธิบายการทำงาน (ทุกอย่างต้องดูได้จาก UI)

---

## 🔧 Environment Variables
ให้ copy ไปใส่ในไฟล์ `.env`

```env
VITE_BINANCE_REST_BASE=https://api.binance.com
VITE_BINANCE_WS_BASE=wss://stream.binance.com:9443
VITE_SYMBOL=BTCUSDT
VITE_LAYOUT_KEY=trading_dashboard_layout_v1
```

---

## 🌐 API ที่ต้องใช้ (กำหนดตายตัว)

### REST — Candlestick (Initial load / เปลี่ยน timeframe)
```
GET /api/v3/klines?symbol=BTCUSDT&interval=<interval>&limit=500
```

### WebSocket
- Realtime Candlestick
```
/ws/btcusdt@kline_<interval>
```

- Realtime Trades
```
/ws/btcusdt@trade
```

> สามารถใช้ combined stream ได้

---

## ⏱ Timeframe (รูปแบบ TradingView)

### ต้องมีขั้นต่ำ
`1m, 5m, 15m, 1h, 4h, 1D`

### เพิ่มได้ (เฉพาะลิสต์นี้เท่านั้น)
- Minutes: `1m, 3m, 5m, 15m, 30m`
- Hours: `1h, 2h, 4h, 6h, 8h, 12h`
- Days / Weeks / Months: `1D, 3D, 1W, 1M`

### Behavior (บังคับ)
- เปลี่ยน timeframe → refetch REST + reconnect WebSocket
- มี Quick presets + Dropdown
- สามารถ ⭐ Favorite timeframe ได้
- Favorites ต้อง persist (localStorage)

---

## 🧩 Features ที่ต้องทำ

### 1) Dashboard Layout (Drag & Resize)
Widgets ขั้นต่ำ 4 ตัว:
1) Main Chart  
2) Trades (realtime table)  
3) Watchlist  
4) Status / Controls (timeframe + WS status)

**Requirements**
- Drag & Drop + Resize
- Drop preview + snap (8px หรือ 16px)
- Collision handling: เลือกอย่างใดอย่างหนึ่ง  
  - Swap (สลับตำแหน่ง)  
  - Push (ดัน widget อื่น)
- Persist layout ด้วย localStorage
- Refresh หน้า → layout ต้องกลับมาเหมือนเดิม 100%

---

### 2) TradingView Lightweight Chart
- Candlestick (OHLC)
- Crosshair + Tooltip (เวลา + O/H/L/C)
- Zoom / Pan
- Realtime update แบบ incremental:
  - อยู่แท่งเดิม → update แท่งล่าสุด
  - ข้ามแท่ง → append แท่งใหม่

---

### 3) Trades Table (Realtime)
- ใช้ WebSocket `btcusdt@trade`
- แสดง 50 รายการล่าสุด (prepend)
- Columns: Time, Price, Qty, Side
- Expand row เพื่อดูรายละเอียดเพิ่ม
- ต้องไม่ทำให้ Drag & Drop หน่วง

---

### 4) QA Mode (ตรวจจากหน้าเว็บอย่างเดียว)
ต้องมีแผง **QA Mode / Status Panel** (เปิด–ปิดได้) แสดงข้อมูลต่อไปนี้:

- REST status + เวลาโหลดล่าสุด
- WebSocket status  
  (`connecting / connected / reconnecting / disconnected`)
- Current WebSocket streams  
  (เช่น `btcusdt@kline_15m`, `btcusdt@trade`)
- Current timeframe
- Counters:
  - kline updates received
  - trades received
- Layout autosave status + last saved time
- Drag smoothness indicator  
  (Good / OK / Poor หรือ fps คร่าวๆ)

**ปุ่มทดสอบบน UI**
- Simulate WS Disconnect
- Reconnect WS
- Reset Layout
- Clear Timeframe Favorites

---

## 🧪 Acceptance Checklist (ตรวจ 3–5 นาที)
- [ ] เปลี่ยน timeframe แล้ว REST + WS เปลี่ยนจริง
- [ ] Candlestick realtime update แบบไม่ refetch ทั้งชุด
- [ ] Trades table ไหล + expand แล้วไม่เด้ง
- [ ] Drag / Resize ลื่น แม้มี realtime data
- [ ] Refresh หน้าแล้ว layout กลับมาเหมือนเดิม
- [ ] QA Mode แสดงสถานะครบถ้วน

---

## 📦 Deliverables
- Git repository หรือ zip
- README นี้ พร้อมคำอธิบายสั้นๆ:
  - โครงสร้างโปรเจกต์
  - กลยุทธ์ Drag & Drop (collision + snap)
  - Mapping Binance kline → candle
  - WebSocket reconnect strategy

---

## 🧮 Scoring
- Layout (Drag / Resize / Snap / Persist + Smooth): 50%
- Chart (TradingView lightweight + realtime): 30%
- Trades Table (realtime + stability): 10%
- Code quality / structure / UX: 10%
