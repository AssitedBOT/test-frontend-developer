# Frontend Developer Test — Trading Terminal Dashboard

เป้าหมายคือสร้าง **Trading Dashboard แบบหน้าเทรดจริง (TradingView-like)**  
การตรวจงานจะทำจาก “หน้าเว็บอย่างเดียว” (รันบนเครื่องผู้ตรวจ)  
❌ ไม่ต้องเปิด DevTools  
❌ ไม่ต้อง deploy

> หมายเหตุสำคัญ: **Binance REST เรียกจาก Browser ตรงๆ ไม่ได้เพราะ CORS**  
> ดังนั้นให้ทำ **REST ผ่าน Proxy ของโปรเจกต์** (Vite dev proxy หรือ tiny proxy server) ตามที่กำหนดด้านล่าง

---

## 🔒 Tech Stack & Rules (บังคับ)
- React (TypeScript แนะนำ)
- TailwindCSS
- Chart: TradingView Lightweight Charts (บังคับ)
- Drag & Drop / Resize: ใช้ library ได้ แต่ต้อง **smooth + stable**
- Data (ตลาด): Binance (REST ผ่าน Proxy + WebSocket ตรงได้)
- Table: TanStack Table (บังคับ)
- ❌ ห้าม mock ข้อมูลกราฟ
- ❌ ห้ามใช้ DevTools เพื่ออธิบายการทำงาน (ทุกอย่างต้องดูได้จาก UI)

---

## 🔧 Environment Variables
ให้ copy ไปใส่ในไฟล์ `.env`

```env
# Binance WebSocket (ต่อจาก client ได้ตรง)
VITE_BINANCE_WS_BASE=wss://stream.binance.com:9443

# Fixed Symbol (ใช้เหมือนกันทุกคน)
VITE_SYMBOL=BTCUSDT

# localStorage key สำหรับ layout
VITE_LAYOUT_KEY=trading_dashboard_layout_v1

# REST ผ่าน Proxy ของโปรเจกต์ (ห้ามยิงไป api.binance.com ตรงๆ)
# ค่า default แนะนำให้เป็น Vite proxy path เดียวกันทุกคน
VITE_REST_PROXY_BASE=/proxy
```

---

## 🌐 API ที่ต้องใช้ (กำหนดตายตัว)

### REST (ผ่าน Proxy เท่านั้น) — Candlestick (Initial load / เปลี่ยน timeframe)
เรียกจาก frontend แบบนี้:
```
GET {VITE_REST_PROXY_BASE}/api/v3/klines?symbol=BTCUSDT&interval=<interval>&limit=500
```

> Proxy ต้อง forward ไป `https://api.binance.com` ให้ครบ path + query

### WebSocket (ต่อจาก client ได้ตรง)
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

## 🧠 ข้อกำหนด Proxy (เพื่อแก้ CORS) — เลือกทำ 1 วิธี

### Option A (แนะนำ): Vite Dev Server Proxy
- ตั้งค่าให้ path `/proxy` forward ไป `https://api.binance.com`
- ผู้ตรวจรัน `npm install` และ `npm run dev` แล้วใช้งานได้ทันที

ตัวอย่างแนวทาง (ให้ใส่ใน `vite.config.ts`):
```ts
server: {
  proxy: {
    "/proxy": {
      target: "https://api.binance.com",
      changeOrigin: true,
      rewrite: (path) => path.replace(/^\/proxy/, ""),
    },
  },
}
```

### Option B: Tiny Proxy Server ใน repo
- มีสคริปต์ `npm run proxy` เปิด proxy ที่ `http://localhost:<port>/proxy/...`
- แล้วตั้ง `VITE_REST_PROXY_BASE=http://localhost:<port>/proxy`

> เลือกแบบใดก็ได้ แต่ต้องทำให้ผู้ตรวจ “รันแล้วใช้ได้ทันที”

---

## ⏱ Timeframe (รูปแบบ TradingView)
### ต้องมีขั้นต่ำ
`1m, 5m, 15m, 1h, 4h, 1D`

### เพิ่มได้ (เฉพาะลิสต์นี้เท่านั้น)
- Minutes: `1m, 3m, 5m, 15m, 30m`
- Hours: `1h, 2h, 4h, 6h, 8h, 12h`
- Days / Weeks / Months: `1D, 3D, 1W, 1M`

### Behavior (บังคับ)
- เปลี่ยน timeframe → refetch REST (ผ่าน Proxy) + reconnect WebSocket
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
- Candlestick (OHLC) จาก REST (ผ่าน Proxy)
- Crosshair + Tooltip (เวลา + O/H/L/C)
- Zoom / Pan
- Realtime update แบบ incremental (จาก WS kline):
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

### 4) ✅ Custom Select Dropdown Component (ห้ามใช้ library)
สร้าง Select/Dropdown component เอง (ตกแต่งด้วย Tailwind) ต้องมี:

**Requirements**
- เปิด/ปิด dropdown ได้ (click + esc)
- รองรับ **Search**: มี input search อยู่ “ใน dropdown panel”
- เลือก option แล้วปิด dropdown และส่งค่าออก (controlled/uncontrolled ได้)
- รองรับ keyboard ขั้นพื้นฐาน: ↑ ↓ Enter Esc (ขั้นต่ำ: Esc ปิด + Enter เลือก)
- **ต้องไม่โดน `overflow-hidden` ของ parent บัง**
  - ต้องทำให้ dropdown แสดงทับ layer อื่นได้ (แนะนำใช้ `Portal` ไป `document.body` หรือใช้ `position: fixed` + คำนวณตำแหน่ง)
- ต้องมีตัวอย่างการใช้งานในหน้า (เช่นเลือก Symbol / Timeframe / Pagination mode)

---

### 5) ✅ Table Component (TanStack Table) + Column Resize + Pagination Mode
ใช้ TanStack Table (บังคับ) และต้องมี:

**A) Resizable Columns**
- ผู้ใช้ลากปรับความกว้าง column ได้
- มี visual handle ชัดเจน
- ระหว่าง resize ไม่ทำให้ UI กระตุกหนัก

**B) Pagination Mode Switch**
ต้องเลือกได้ 2 โหมด (มี toggle/select บน UI):
- `local` (client-side pagination): paginate จาก data ที่โหลดมาทั้งหมด
- `server` (server-side pagination): เปลี่ยนหน้าแล้ว “ต้องเรียก fetch ใหม่”

> สำหรับ `server` mode ในเทสต์นี้ อนุญาตให้ทำ “server pagination แบบจำลอง” ได้ 2 ทาง:
> 1) ใช้ endpoint proxy เดิม แต่ทำ slice/pagination ใน proxy server (Option B)  
> 2) หรือจำลอง server pagination ภายในแอป แต่ต้องทำให้ UI/Flow เหมือน server จริง (loading/disable next/prev ตาม page)

**C) Expand Row**
- ขยาย row เพื่อดูรายละเอียดเพิ่ม (อย่างน้อย 1 ระดับ)

---

### 6) QA Mode (ตรวจจากหน้าเว็บอย่างเดียว)
ต้องมีแผง **QA Mode / Status Panel** (เปิด–ปิดได้) แสดงข้อมูลต่อไปนี้:

- REST status + เวลาโหลดล่าสุด
- WebSocket status (`connecting / connected / reconnecting / disconnected`)
- Current WebSocket streams (เช่น `btcusdt@kline_15m`, `btcusdt@trade`)
- Current timeframe
- Counters:
  - kline updates received
  - trades received
- Layout autosave status + last saved time
- Drag smoothness indicator (Good / OK / Poor หรือ fps คร่าวๆ)
- Table pagination mode: `local | server` + current page info

**ปุ่มทดสอบบน UI**
- Simulate WS Disconnect
- Reconnect WS
- Reset Layout
- Clear Timeframe Favorites

---

## 🧪 Acceptance Checklist (ตรวจ 5–8 นาที)
- [ ] REST ผ่าน Proxy ใช้งานได้ (หน้าโหลด candle ได้) — ไม่ยิง Binance ตรง
- [ ] เปลี่ยน timeframe แล้ว REST + WS เปลี่ยนจริง (ดูใน QA Mode)
- [ ] Candlestick realtime update แบบ incremental (ไม่ refetch ทั้งชุด)
- [ ] Trades table ไหล + expand แล้วไม่เด้ง
- [ ] Drag / Resize ลื่น แม้มี realtime data
- [ ] Refresh หน้าแล้ว layout กลับมาเหมือนเดิม
- [ ] Select dropdown search ได้ + ไม่โดน overflow-hidden บัง (ทับ layer ได้จริง)
- [ ] TanStack table resize column ได้ + เปลี่ยน pagination mode (local/server) ได้จริง
- [ ] QA Mode แสดงสถานะครบถ้วน

---

## 📦 Deliverables
- Git repository หรือ zip
- README นี้ พร้อมคำอธิบายสั้นๆ:
  - โครงสร้างโปรเจกต์
  - Drag & Drop strategy (collision + snap) และเหตุผลเลือก lib
  - Proxy approach (Vite proxy หรือ tiny server) ที่ทำให้รันได้ทันที
  - Mapping Binance kline → candle
  - WebSocket reconnect strategy
  - Table pagination (local/server) ออกแบบยังไง

---

## 🧮 Scoring
- Layout (DnD/Resize/Snap/Persist + Smooth): 40%
- Chart (TradingView lightweight + realtime incremental): 25%
- Select Dropdown (no-lib + search + portal/overlay): 15%
- TanStack Table (resize + pagination local/server + expand): 15%
- Code quality / structure / UX: 5%
