# 📖 Quick Reference Guide - Looker Studio Dashboard

## 🔗 ตารางการเชื่อมโยงข้อมูล (Data Relationships)

| ตารางหลัก | เชื่อมกับ | Join Key | Join Type |
|-----------|----------|----------|-----------|
| **students** | grades | student_id = student_id | Left Outer |
| **students** | attendance | student_id = student_id | Left Outer |
| **grades** | subjects | subject_code = subject_code | Left Outer |
| **grades** | students | student_id = student_id | Left Outer |
| **subjects** | teachers | teacher_id = teacher_id | Left Outer |
| **teachers** | subjects | teacher_id = teacher_id | Left Outer |
| **documents** | teachers | created_by = teacher_id | Left Outer |

---

## 📊 Calculated Fields ที่ใช้บ่อย

### 1. GPA เฉลี่ย
```
AVG(grade)
```

### 2. เปอร์เซ็นต์การเข้าเรียน
```
COUNTIF(status = "มาเรียน") / COUNT(attendance_id) * 100
```

### 3. นักเรียนที่มาเรียน
```
COUNTIF(status = "มาเรียน")
```

### 4. นักเรียนที่ขาดเรียน
```
COUNTIF(status = "ขาดเรียน")
```

### 5. เปอร์เซ็นต์การใช้งบประมาณ
```
SUM(budget_spent) / SUM(budget_allocated) * 100
```

### 6. งบประมาณคงเหลือ (%)
```
SUM(budget_remaining) / SUM(budget_allocated) * 100
```

### 7. จำนวนวันที่รออนุมัติ
```
DATE_DIFF(CURRENT_DATE(), created_date)
```

### 8. ระดับผลการเรียน
```
CASE
  WHEN AVG(grade) >= 3.5 THEN "ดีเยี่ยม"
  WHEN AVG(grade) >= 3.0 THEN "ดี"
  WHEN AVG(grade) >= 2.5 THEN "ปานกลาง"
  WHEN AVG(grade) >= 2.0 THEN "อ่อน"
  ELSE "ต้องเสริม"
END
```

### 9. สถานะการเข้าเรียน
```
CASE
  WHEN (COUNTIF(status = "มาเรียน") / COUNT(attendance_id) * 100) >= 95 THEN "ดีเยี่ยม"
  WHEN (COUNTIF(status = "มาเรียน") / COUNT(attendance_id) * 100) >= 90 THEN "ดี"
  WHEN (COUNTIF(status = "มาเรียน") / COUNT(attendance_id) * 100) >= 80 THEN "พอใช้"
  ELSE "ต้องปรับปรุง"
END
```

### 10. กลุ่มอายุงาน (สำหรับครู)
```
CASE
  WHEN experience_years <= 5 THEN "0-5 ปี"
  WHEN experience_years <= 10 THEN "6-10 ปี"
  WHEN experience_years <= 15 THEN "11-15 ปี"
  WHEN experience_years <= 20 THEN "16-20 ปี"
  ELSE "20+ ปี"
END
```

### 11. สถานะงบประมาณ
```
CASE
  WHEN (SUM(budget_spent) / SUM(budget_allocated) * 100) < 60 THEN "ปลอดภัย"
  WHEN (SUM(budget_spent) / SUM(budget_allocated) * 100) < 80 THEN "ควรระวัง"
  ELSE "ใกล้หมด"
END
```

### 12. ระยะเวลาการอนุมัติ (วัน)
```
DATE_DIFF(
  COALESCE(approve_date_level2, approve_date_level1),
  created_date
)
```

### 13. คะแนนเฉลี่ยรวม
```
(AVG(midterm_score) + AVG(final_score)) / 2
```

### 14. นักเรียนที่ต้องเสริม
```
CASE
  WHEN AVG(grade) < 2.0 THEN student_name
  ELSE NULL
END
```

### 15. อัตราส่วนครู:นักเรียน
```
(SELECT COUNT(student_id) FROM students) / 
(SELECT COUNT(teacher_id) FROM teachers WHERE position CONTAINS "ครู")
```

---

## 🎨 Conditional Formatting - เกณฑ์สี

### สำหรับ GPA (เกรด 0-4):
- 🟢 **>= 3.5**: สีเขียวเข้ม `#0F9D58`
- 🟢 **3.0 - 3.49**: สีเขียวอ่อน `#7CB342`
- 🟡 **2.5 - 2.99**: สีเหลือง `#FFC107`
- 🟠 **2.0 - 2.49**: สีส้ม `#FF9800`
- 🔴 **< 2.0**: สีแดง `#DB4437`

### สำหรับเปอร์เซ็นต์การเข้าเรียน:
- 🟢 **>= 95%**: สีเขียวเข้ม `#0F9D58`
- 🟢 **90 - 94%**: สีเขียวอ่อน `#7CB342`
- 🟡 **80 - 89%**: สีเหลือง `#FFC107`
- 🔴 **< 80%**: สีแดง `#DB4437`

### สำหรับงบประมาณ (% ที่ใช้ไป):
- 🟢 **0 - 60%**: ปลอดภัย - สีเขียว `#0F9D58`
- 🟡 **61 - 80%**: ควรระวัง - สีเหลือง `#FFC107`
- 🔴 **81 - 100%**: ใกล้หมด - สีแดง `#DB4437`

### สำหรับสถานะเอกสาร:
- ⚪ **ร่าง**: สีเทา `#9E9E9E`
- 🟠 **รอการอนุมัติ**: สีส้ม `#FF9800`
- 🟢 **อนุมัติแล้ว**: สีเขียว `#0F9D58`

### สำหรับความเร่งด่วนเอกสาร:
- 🔴 **ด่วนที่สุด**: พื้นหลังสีแดง `#DB4437`, ตัวอักษรสีขาว
- 🟠 **ด่วน**: พื้นหลังสีส้ม `#FF9800`
- ⚪ **ปกติ**: ไม่ระบายสี

### สำหรับวันที่รออนุมัติ:
- 🔴 **> 7 วัน**: สีแดง `#DB4437`
- 🟡 **4-7 วัน**: สีเหลือง `#FFC107`
- 🟢 **< 4 วัน**: สีเขียว `#0F9D58`

---

## 📈 Charts แนะนำสำหรับแต่ละประเภทข้อมูล

| ประเภทข้อมูล | Chart ที่เหมาะสม |
|--------------|-----------------|
| **ตัวเลขสำคัญ (KPI)** | Scorecard, Gauge Chart |
| **สัดส่วน/เปอร์เซ็นต์** | Pie Chart, Donut Chart |
| **เปรียบเทียบหมวดหมู่** | Bar Chart, Column Chart |
| **แนวโน้มตามเวลา** | Time Series (Line Chart) |
| **เปรียบเทียบหลายมิติ** | Stacked Bar/Column Chart |
| **การกระจายข้อมูล** | Histogram, Scatter Plot |
| **รายละเอียด** | Table |
| **ความสัมพันธ์** | Scatter Chart, Bubble Chart |
| **การไหลของข้อมูล** | Sankey Diagram, Funnel Chart |
| **พื้นที่/สถานที่** | Geo Map |
| **ค่าหลายๆ แกน** | Combo Chart (Bar + Line) |
| **ความก้าวหน้า** | Bullet Chart, Progress Bar |

---

## 🔧 Keyboard Shortcuts

| การทำงาน | Windows/Linux | Mac |
|---------|---------------|-----|
| บันทึก | Ctrl + S | Cmd + S |
| Undo | Ctrl + Z | Cmd + Z |
| Redo | Ctrl + Y | Cmd + Shift + Z |
| คัดลอก | Ctrl + C | Cmd + C |
| วาง | Ctrl + V | Cmd + V |
| ลบ | Delete | Delete |
| Duplicate | Ctrl + D | Cmd + D |
| Select All | Ctrl + A | Cmd + A |
| ค้นหา | Ctrl + F | Cmd + F |

---

## 📋 Checklist การสร้าง Dashboard

### ก่อนเริ่ม:
- [ ] มีไฟล์ CSV ครบทั้ง 7 ไฟล์
- [ ] Upload ไปยัง Google Drive แล้ว
- [ ] แปลงเป็น Google Sheets แล้ว
- [ ] ตรวจสอบข้อมูลในแต่ละไฟล์แล้ว

### ขั้นตอนการสร้าง:
- [ ] เชื่อมต่อ Data Sources ทั้งหมด
- [ ] ตรวจสอบ Field Types ถูกต้อง
- [ ] สร้าง Calculated Fields ที่จำเป็น
- [ ] Blend Data ตามที่ต้องการ
- [ ] สร้าง Charts แต่ละหน้า
- [ ] ตั้งค่า Conditional Formatting
- [ ] เพิ่ม Filters และ Controls
- [ ] ปรับแต่ง Theme และสี
- [ ] ทดสอบ Interactivity
- [ ] เพิ่ม Instructions/คำอธิบาย

### ก่อน Present:
- [ ] ทดสอบทุก Filter
- [ ] ทดสอบ Date Range Control
- [ ] ตรวจสอบตัวเลขว่าถูกต้อง
- [ ] ตรวจสอบ Performance (โหลดเร็วไหม)
- [ ] ตั้งค่าสิทธิ์การเข้าถึง
- [ ] เตรียม PDF Backup (ถ้าจำเป็น)

---

## 🎯 Best Practices

### การออกแบบ:
✅ ใช้สีสอดคล้องกันทั้ง Dashboard
✅ จัดวาง Charts ให้อ่านง่าย (ซ้าย → ขวา, บน → ล่าง)
✅ ใส่ชื่อ Charts ให้ชัดเจน
✅ เลือก Font ขนาดเหมาะสม (อ่านง่าย)
✅ ใช้ White space อย่างเหมาะสม

### ข้อมูล:
✅ เลือก Chart ที่เหมาะกับข้อมูล
✅ ไม่ใส่ข้อมูลมากเกินไปใน 1 Chart
✅ ใช้ Aggregation ที่เหมาะสม (AVG, SUM, COUNT)
✅ ตรวจสอบความถูกต้องของตัวเลข
✅ เพิ่ม Tooltips เพื่ออธิบายข้อมูล

### Performance:
✅ จำกัด Date Range ถ้าข้อมูลเยอะ
✅ ใช้ Extract Data ถ้าข้อมูลช้า
✅ ลด Blend ที่ไม่จำเป็น
✅ ใช้ Filter เพื่อลดข้อมูลที่โหลด

### User Experience:
✅ เพิ่ม Navigation ที่ชัดเจน
✅ เพิ่ม Instructions สำหรับผู้ใช้
✅ ใช้ Consistent Layout ทุกหน้า
✅ ทดสอบบนหน้าจอขนาดต่างๆ
✅ เพิ่ม Error Messages ถ้าไม่มีข้อมูล

---

## 🆘 Troubleshooting Quick Fix

### ปัญหา: Chart ไม่แสดงข้อมูล
1. ตรวจสอบ Date Range Control
2. ตรวจสอบ Filters
3. ตรวจสอบว่ามีข้อมูลในช่วงนั้นจริงหรือไม่
4. ลอง Refresh Data Source

### ปัญหา: Blend ไม่ได้
1. ตรวจสอบ Join Key ว่าเป็น Field Type เดียวกันหรือไม่
2. ตรวจสอบว่ามีข้อมูลที่ตรงกันหรือไม่
3. ลองเปลี่ยน Join Type (Left/Right/Inner/Full Outer)

### ปัญหา: Calculated Field Error
1. ตรวจสอบวงเล็บให้ครบ
2. ตรวจสอบ Syntax (ใช้ Aggregation function ถูกต้องไหม)
3. ตรวจสอบชื่อ Field ว่าถูกหรือไม่
4. ลองแบ่งเป็นหลาย Fields

### ปัญหา: Dashboard ช้า
1. ลด Date Range
2. ลดจำนวน Charts ในหน้า
3. ใช้ Filter เพื่อจำกัดข้อมูล
4. Extract Data แทน Live Connection

### ปัญหา: สีไม่แสดง
1. ตรวจสอบว่าตั้งค่า Color ใน Style tab แล้วหรือไม่
2. ตรวจสอบ Conditional Formatting
3. ตรวจสอบว่า Field เป็น Number หรือ Text

---

## 📞 ลิงก์ที่เป็นประโยชน์

- **Looker Studio Home:** https://lookerstudio.google.com
- **Help Center:** https://support.google.com/looker-studio
- **Community:** https://support.google.com/looker-studio/community
- **Function List:** https://support.google.com/looker-studio/table/6379764
- **Templates:** https://lookerstudio.google.com/navigation/reporting

---

## 💡 Tips & Tricks

### Tip 1: ใช้ REGEX เพื่อ Filter
```
REGEXP_MATCH(field_name, "pattern")
```

### Tip 2: Format ตัวเลขให้สวย
- คลิกที่ Metric
- เลือก Type: Number
- ตั้งค่า Decimal places
- เปิด Compact numbers (1,000,000 → 1M)

### Tip 3: สร้าง Custom Sort
```
CASE
  WHEN grade_level = "ม.1" THEN 1
  WHEN grade_level = "ม.2" THEN 2
  WHEN grade_level = "ม.3" THEN 3
  WHEN grade_level = "ม.4" THEN 4
  WHEN grade_level = "ม.5" THEN 5
  WHEN grade_level = "ม.6" THEN 6
END
```

### Tip 4: แสดงค่า Null เป็นข้อความ
```
COALESCE(field_name, "ไม่มีข้อมูล")
```

### Tip 5: สร้าง Hyperlink
```
HYPERLINK(url, "ข้อความที่แสดง")
```

### Tip 6: Concatenate Text
```
CONCAT(first_name, " ", last_name)
```

### Tip 7: Round ทศนิยม
```
ROUND(field_name, 2)  // 2 ตำแหน่ง
```

### Tip 8: ดึงเดือนจาก Date
```
MONTH(date_field)
YEAR(date_field)
QUARTER(date_field)
```

---

## 📊 Dashboard Page Summary

| Page | หน้าที่ | Charts หลัก | Filters |
|------|--------|------------|---------|
| **1. ภาพรวม** | Overview KPI | Scorecards, Pie, Bar | Date, Grade Level |
| **2. วิชาการ** | ผลการเรียน | Bar, Table, Combo | Semester, Subject |
| **3. การเข้าเรียน** | Attendance | Time Series, Stacked | Date Range, Status |
| **4. บุคลากร** | ข้อมูลครู | Pie, Bar, Table | Department, Position |
| **5. งบประมาณ** | การเงิน | Gauge, Stacked Bar | Category, Year |
| **6. E-Document** | เอกสาร | Funnel, Table | Status, Priority |

---

✨ **Happy Dashboard Building!** ✨
