# 🧩 Frontend Developer Test

ทดสอบความสามารถในการพัฒนา UI/UX เชิงโครงสร้างและการจัดการ Layout ที่ซับซ้อน  
โดยใช้ **React + TailwindCSS** และแนวคิด Component-based Architecture  

---

## 📝 ภาพรวมของแบบทดสอบ

ผู้สมัครต้องพัฒนา **Dashboard Layout ที่ผู้ใช้สามารถปรับแต่งได้เอง**  
พร้อมด้วย Chart, Table, และ Utility Function ที่เกี่ยวกับ Date Range

---

## 🚀 สิ่งที่ต้องทำ

### 1. Dashboard Layout (Drag & Drop + Resize)

สร้างหน้า **Dashboard** ที่มีกล่อง (Panel / Widget) หลายกล่อง  
และผู้ใช้สามารถ **ย้ายตำแหน่ง** และ **ปรับขนาด** ได้อย่างอิสระ

**คุณสมบัติหลัก:**
- ✅ Drag & Drop ย้ายตำแหน่งกล่องได้  
- ✅ Resize ได้ทุกด้าน (บน / ล่าง / ซ้าย / ขวา)  
- ✅ Layout คล้ายหน้าเทรดของ Binance (ผู้ใช้ปรับได้ตามต้องการ)  
- ✅ ใช้ **TailwindCSS** จัดการ Layout และ Styling

---

### 2. Chart Components (React ApexCharts)

เพิ่ม Chart อย่างน้อย 4 ประเภท โดยใช้ **React ApexCharts**

| Type | Requirement |
|------|--------------|
| 🥧 Pie Chart | แสดงสัดส่วนข้อมูล |
| 📊 Bar Chart | แสดงการเปรียบเทียบข้อมูล |
| 📈 Line Chart | แสดงข้อมูลตามเวลา |
| 🧱 Column Chart | แสดงข้อมูลแบบแนวตั้ง |

**แต่ละกราฟต้องมี:**
- Title ชัดเจน
- แสดงข้อมูลจริง (mock data ได้)

---

### 3. Table Component

สร้าง **Table Component** ที่มีความสามารถดังนี้:

- 📏 **Resizable Columns** — ผู้ใช้สามารถปรับขนาดคอลัมน์ได้  
- 🔽 **Expandable Row** — เมื่อคลิกที่ row จะโชว์รายละเอียดเพิ่มเติมของข้อมูลแถวนั้น  

> **Hint:** หากพัฒนา Table เองจะได้รับคะแนนพิเศษ 🎯

---

### 4. Utility Function (Date Range Calculator)

สร้างฟังก์ชันที่ชื่อว่า `getDateRange(date: string, type: string): { start: string, end: string }`

**รองรับ type ต่อไปนี้:**

| Type | Description |
|------|--------------|
| `lastday` | วันที่ผ่านมา |
| `last7day` | ย้อนหลัง 7 วัน |
| `lastweek` | สัปดาห์ก่อนหน้า |
| `lastmonth` | เดือนก่อนหน้า |
| `last3month` | 3 เดือนล่าสุด |
| `last6month` | 6 เดือนล่าสุด |
| `last12month` | 12 เดือนล่าสุด |

**Output Example:**
```js
{
  start: "2025-10-01T00:00:00.000Z",
  end: "2025-10-15T23:59:59.999Z"
}
