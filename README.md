📌 Frontend Developer Test (PM Brief)
🎯 Objective

สร้าง Customizable Dashboard (เหมือน Trading UI)
ที่ผู้ใช้สามารถ จัด Layout เองได้ (Drag / Resize / Arrange)
โดยใช้ React + TailwindCSS + Component-based Architecture

🧱 Scope งาน (สิ่งที่ต้องส่ง)
1. Dashboard Layout (Core Feature)
Requirement

พัฒนา Dashboard ที่ประกอบด้วยหลาย Widget (Panel) โดย:

ผู้ใช้สามารถ ลาก (Drag) เพื่อเปลี่ยนตำแหน่งได้
ผู้ใช้สามารถ Resize ได้ทุกทิศทาง (Top / Bottom / Left / Right)
Layout ต้องมีลักษณะ Flexible / Dynamic (ไม่ fix grid ตายตัว)
Expected Behavior
Layout ต้อง “ไม่พัง” เมื่อ resize หรือ drag ซ้อนกัน
Widget ต้องไม่ overlap กันแบบผิด logic
UX ต้อง usable จริง (ไม่ใช่แค่ demo)
Tech Guideline
ใช้ React (Functional Component + Hooks เท่านั้น)
ใช้ TailwindCSS สำหรับ styling เท่านั้น (no inline style-heavy)
แนะนำ (optional):
react-grid-layout / dnd-kit / react-rnd
2. Chart Components
Requirement

ต้องมี Chart อย่างน้อย 4 ประเภท โดยใช้ React ApexCharts:

Type	Requirement
Pie	แสดงสัดส่วน
Bar	เปรียบเทียบ
Line	ตามเวลา
Column	แนวตั้ง
Constraints
แต่ละ Chart ต้อง:
มี Title ชัดเจน
มี Mock Data ที่ realistic
Chart ต้องถูก embed อยู่ใน Widget (จากข้อ 1)
Expected Behavior
Resize แล้ว chart ต้อง responsive
ไม่มี UI break
3. Table Component
Requirement

สร้าง Table ที่มี feature:

✅ Resizable Columns
User drag เพื่อปรับ width column ได้
✅ Expandable Row
Click row → แสดง detail (ใต้ row หรือ modal ก็ได้)
Bonus (คะแนนพิเศษ)
Implement Table เอง (ไม่ใช้ library เช่น antd table)
Expected Behavior
Table ต้อง handle data ได้จริง (ไม่ hardcode แค่ UI)
UX ต้อง smooth (ไม่มี flick / lag หนัก)
4. Utility Function (Date Range)
Requirement

Implement function:

getDateRange(date: string, type: string): {
  start: string
  end: string
}
Supported Types
type	description
lastday	ย้อนหลัง 1 วัน
last7day	ย้อนหลัง 7 วัน
lastweek	สัปดาห์ก่อน
lastmonth	เดือนก่อน
last3month	3 เดือนย้อนหลัง
last6month	6 เดือนย้อนหลัง
last12month	12 เดือนย้อนหลัง
Rules
Output ต้องเป็น ISO string (UTC)
ต้อง handle edge case:
timezone
end of month
leap year
Example Output
{
  "start": "2025-10-01T00:00:00.000Z",
  "end": "2025-10-15T23:59:59.999Z"
}
🧩 Architecture Requirement
ต้องแยก component ชัดเจน เช่น:
DashboardLayout
WidgetContainer
ChartWidget
TableWidget
ห้ามเขียนทุกอย่างในไฟล์เดียว
Code ต้องอ่านง่าย และ maintain ได้
🎨 UX / UI Expectation
UX ต้อง “ใช้งานได้จริง” ไม่ใช่แค่โชว์
Layout ต้อง:
ไม่รก
spacing ดี
resize แล้วไม่แตก
Responsive (desktop เป็นหลัก, mobile basic รองรับได้จะดี)
📦 Deliverables

ผู้สมัครต้องส่ง:

Git Repository
README อธิบาย:
วิธี run project
library ที่ใช้ + เหตุผล
ปัญหาที่เจอ + วิธีแก้
Demo Screenshot / GIF
⚖️ Evaluation Criteria
🔥 Core (สำคัญมาก)
Drag & Drop + Resize ใช้งานได้จริง
Layout ไม่พัง
🧠 Code Quality
Component structure ดี
Clean code / readable
🎯 UX
ใช้งานง่าย ไม่งง
Interaction smooth
⚡ Performance
ไม่ lag เมื่อมีหลาย widget
🎁 Bonus
เขียน Table เอง
มี State management ดี (Zustand / Redux / Context)
🚫 สิ่งที่ “ห้ามพลาด”
ห้าม hardcode layout แบบ static
ห้าม chart ไม่ responsive
ห้ามใช้ UI library ใหญ่ ๆ มาทำทุกอย่างแทน (เช่นลากทั้ง dashboard ด้วย antd)
