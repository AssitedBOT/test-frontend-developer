# 📌 Frontend Developer Test

## 🎯 Objective
สร้าง **Customizable Dashboard (ลักษณะ Trading UI)**  
ที่ผู้ใช้สามารถ **จัด Layout เองได้ (Drag / Resize / Arrange)**  
โดยใช้ **React + TailwindCSS + Component-based Architecture**

---

# 🧱 Scope งาน

## 1. Dashboard Layout (Core Feature)

### Requirement
พัฒนา Dashboard ที่ประกอบด้วยหลาย Widget (Panel) โดย:

- ผู้ใช้สามารถ **ลาก (Drag) เพื่อเปลี่ยนตำแหน่งได้**
- ผู้ใช้สามารถ **Resize ได้ทุกทิศทาง (Top / Bottom / Left / Right)**
- Layout ต้องเป็นแบบ **Flexible / Dynamic (ไม่ fix grid ตายตัว)**

### Expected Behavior
- Layout ต้องไม่พังเมื่อ drag หรือ resize
- Widget ต้องไม่ overlap กันผิด logic
- UX ต้องสามารถใช้งานได้จริง

### Tech Guideline
- ใช้ React (Functional Components + Hooks)
- ใช้ TailwindCSS สำหรับ styling
- สามารถใช้ library เสริมได้ เช่น:
  - react-grid-layout
  - dnd-kit
  - react-rnd

---

## 2. Chart Components

### Requirement
ต้องมี Chart อย่างน้อย **4 ประเภท** โดยใช้ React ApexCharts:

| Type | Description |
|------|------------|
| Pie | แสดงสัดส่วน |
| Bar | เปรียบเทียบข้อมูล |
| Line | แสดงข้อมูลตามเวลา |
| Column | แสดงข้อมูลแนวตั้ง |

### Constraints
- แต่ละ Chart ต้องมี:
  - Title ชัดเจน
  - Mock data ที่สมเหตุสมผล
- Chart ต้องอยู่ภายใน Widget

### Expected Behavior
- รองรับการ resize
- UI ไม่แตกเมื่อเปลี่ยนขนาด

---

## 3. Table Component

### Requirement

#### ✅ Resizable Columns
- ผู้ใช้สามารถลากเพื่อปรับขนาด column ได้

#### ✅ Expandable Row
- คลิก row เพื่อแสดงรายละเอียดเพิ่มเติม

### Bonus (Optional)
- Implement Table เอง (ไม่ใช้ library สำเร็จรูป)

### Expected Behavior
- รองรับข้อมูลจริง
- UX smooth ไม่มี lag หรือ flick

---

## 4. Utility Function (Date Range)

### Requirement

```ts
getDateRange(date: string, type: string): {
  start: string
  end: string
}
