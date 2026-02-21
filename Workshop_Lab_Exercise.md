# ใบงานการทดลอง: สร้าง Dashboard ด้วย Looker Studio
## สำหรับผู้บริหารโรงเรียน

---

## 🎯 วัตถุประสงค์ เพื่อให้นักศึกษาสามารถ
1. นำเข้าข้อมูลจาก CSV/Google Sheets เข้า Looker Studio ได้
2. เชื่อมโยงข้อมูล (Data Blending) ระหว่างตารางต่างๆ ได้
3. สร้าง Dashboard 6 หน้า ตามมาตรฐานการบริหารโรงเรียนได้
4. สร้าง Charts และ Metrics ที่ตอบโจทย์การบริหารจัดการได้

---

# 📝 ส่วนที่ 1: เตรียมข้อมูล 

## Lab 1.1: อัปโหลดไฟล์ CSV ไปยัง Google Drive

### ขั้นตอน:
1. ✅ เปิด Google Drive (drive.google.com)
2. ✅ สร้างโฟลเดอร์ชื่อ "Dashboard_School_Data"
3. ✅ อัปโหลดไฟล์ CSV ทั้ง 7 ไฟล์:
   - students.csv
   - teachers.csv
   - grades.csv
   - attendance.csv
   - budget.csv
   - documents.csv
   - subjects.csv

### ผลลัพธ์ที่ควรได้:
- [x] เห็นไฟล์ทั้ง 7 ไฟล์ใน Google Drive
- [x] สามารถเปิดไฟล์ด้วย Google Sheets ได้

---

## Lab 1.2: แปลงไฟล์เป็น Google Sheets

### ขั้นตอน:
1. ✅ คลิกขวาที่ไฟล์ **students.csv**
2. ✅ เลือก "Open with" → "Google Sheets"
3. ✅ ไฟล์จะเปิดเป็น Google Sheets
4. ✅ ทำซ้ำสำหรับไฟล์อื่นๆ ทั้ง 6 ไฟล์

### เช็คความถูกต้อง:
- [x] students.csv มี 60 แถว (ไม่นับ header)
- [x] teachers.csv มี 25 แถว
- [x] grades.csv มี 100 แถว
- [x] attendance.csv มี 100 แถว
- [x] budget.csv มี 35 แถว
- [x] documents.csv มี 35 แถว
- [x] subjects.csv มี 55 แถว

### 💡 Tips:
- ตรวจสอบว่าข้อมูลวันที่แสดงผลถูกต้อง (อาจต้องปรับ Format → Number → Date)
- ตรวจสอบว่าตัวเลขไม่ถูกอ่านเป็น Text

---

# 📊 ส่วนที่ 2: เชื่อมต่อข้อมูลกับ Looker Studio 

## Lab 2.1: สร้าง Report ใหม่

### ขั้นตอน:
1. ✅ เปิด Looker Studio (lookerstudio.google.com)
2. ✅ คลิก "Create" → "Report"
3. ✅ ระบบจะถามว่าต้องการเพิ่ม Data Source
4. ✅ คลิก "Google Sheets"
5. ✅ เลือก **students** sheet
6. ✅ คลิก "Add"
7. ✅ คลิก "Add to Report"

### ผลลัพธ์ที่ควรได้:
- [x] เห็นหน้า Report เปล่าๆ
- [x] ด้านขวามือมี Data Panel แสดงฟิลด์จาก students table

---

## Lab 2.2: เพิ่ม Data Sources อื่นๆ

### ขั้นตอน:
1. ✅ คลิก "Resource" (เมนูบน) → "Manage added data sources"
2. ✅ คลิก "+ Add a data source"
3. ✅ เลือก "Google Sheets"
4. ✅ เลือก **teachers** sheet → คลิก "Add"
5. ✅ ทำซ้ำสำหรับ:
   - grades
   - attendance
   - budget
   - documents
   - subjects

### เช็คความถูกต้อง:
- [x] ใน "Manage added data sources" เห็น Data Sources ทั้งหมด 7 ตัว
- [x] แต่ละ Data Source แสดงจำนวนฟิลด์ที่ถูกต้อง

---

## Lab 2.3: ตรวจสอบ Field Types

### ขั้นตอนสำคัญ - ต้องทำให้ถูกต้อง!

1. ✅ คลิก "Resource" → "Manage added data sources"
2. ✅ คลิกที่ **students** data source
3. ✅ ตรวจสอบ Field Type ของแต่ละฟิลด์:

| Field Name | Type ที่ถูกต้อง | วิธีแก้ถ้าผิด |
|-----------|----------------|---------------|
| student_id | Text | คลิกที่ ABC → เลือก Text |
| student_name | Text | Text |
| grade_level | Text | Text |
| gender | Text | Text |
| birth_date | Date | คลิกที่ Icon → เลือก Date |
| enrollment_date | Date | คลิกที่ Icon → เลือ Date |
| status | Text | Text |

4. ✅ ทำซ้ำสำหรับ Data Sources อื่นๆ:

**teachers:**
- teacher_id → Text
- hire_date → Date
- experience_years → Number

**grades:**
- student_id → Text
- subject_code → Text
- midterm_score → Number
- final_score → Number
- total_score → Number
- grade → Number
- academic_year → Number

**attendance:**
- student_id → Text
- date → Date
- status → Text

**budget:**
- budget_allocated → Number
- budget_spent → Number
- budget_remaining → Number

**documents:**
- created_date → Date
- approve_date_level1 → Date
- approve_date_level2 → Date

### ผลลัพธ์ที่ควรได้:
- [x] ทุก Date field เป็น Date type
- [x] ทุก Number field เป็น Number type
- [x] ID fields เป็น Text type

---

# 🔗 ส่วนที่ 3: Blend Data

## Lab 3.1: Blend ข้อมูลนักเรียนกับผลการเรียน

### วัตถุประสงค์: 
เชื่อมข้อมูลนักเรียนกับผลการเรียนเพื่อดู GPA ของแต่ละคน

### ขั้นตอน:
1. ✅ ที่หน้า Report คลิก "Add a chart"
2. ✅ เลือก "Table" (ตารางธรรมดา)
3. ✅ วาดตารางลงบน Canvas
4. ✅ ที่ Data Panel ด้านขวา คลิกที่ชื่อ Data Source (ตรง "students")
5. ✅ เลือก "Blend data"

### Blend Configuration:
6. ✅ Left Table: **students**
   - Join Key: **student_id**
   - เลือก Dimensions ทุกฟิลด์ที่ต้องการนำไปใช้งาน โดยการเลือก Add dimension (เลือกได้ 10 dimension)
   
7. ✅ คลิก "+ Join another table"

8. ✅ Right Table: **grades**
   - Join Key: **student_id**
   - เลือก Dimensions ทุกฟิลด์ที่ต้องการนำไปใช้งาน โดยการเลือก Add dimension (เลือกได้ 10 dimension)
   - เลือก condition ของการ join แบบ **Left Outer** (เพื่อให้เห็นนักเรียนทุกคน แม้ยังไม่มีเกรด)
   - ตรวจสอบ Join condition เป็นการ join ระหว่าง student_id ของ Table1 กับ student_id ของ Table2
   - กดปุ่ม Save เพื่อออกจากหน้า Join configuration
   - ตั้งชื่อ Blened Data เป็น Students_Grades

9. ✅ คลิก "Save" หลังจากนั้น กดปุุ่ม CLOSE เพื่อกลับไปหน้า Report

### ตั้งค่า Table:
10. ✅ ใน Dimension เลือก:
    - student_name (จาก students)
    - grade_level (จาก students)
    - class_room (จาก students)

11. ✅ ใน Metric เพิ่ม:
    - กดปุ่ม Add metric
    - เลือก Add calculated field เพื่อสร้างฟิลด์ใหม่ที่นำข้อมูลจากฟิลด์เดิมมาคำนวณ
    - กำหนดชื่อเป็น GPA
    - ในช่อง Formula ใส่สูตรคือ AVG(grade)
    - กดปุ่ม Apply หลังจากนั้นกดที่ส่วนอื่นของหน้าจอ เพื่อกลับมาหน้าจอหลัก
    - เพิ่ม Metric เพื่อแสดงจำนวนวิชา
    - กำหนด Formula คือ COUNT(record_id)

### ผลลัพธ์ที่ควรได้:
- [x] ตารางแสดงรายชื่อนักเรียนพร้อม GPA
- [x] เห็น GPA ของนักเรียนแต่ละคน
- [x] นักเรียนที่ยังไม่มีเกรดจะแสดง "null"

### 🎨 Style the Table:
12. ✅ คลิกที่ตาราง → ไปที่ "Style" tab
13. ✅ เปิด "Show row numbers"
14. ✅ เปิด "Table header" → ใส่สีพื้นหลัง
15. ✅ เปิด "Compact numbers" สำหรับ GPA

---

## Lab 3.2: Blend แบบ 3 ตาราง - นักเรียน + ผลเรียน + วิชา

### วัตถุประสงค์:
แสดงว่านักเรียนแต่ละคนเรียนวิชาอะไรบ้าง และได้เกรดเท่าไหร่

### ขั้นตอน:
1. ✅ สร้าง Table ใหม่
2. ✅ เลือก Data Source → Blend data

### Blend Configuration:
3. ✅ **Table 1: students**
   - Join Key: student_id

4. ✅ **Table 2: grades**
   - Join Key: student_id
   - Join Type: Left Outer

5. ✅ คลิก "+ Join another table"

6. ✅ **Table 3: subjects**
   - Join Key: subject_code (จาก grades) = subject_code (จาก subjects) และ teacher_id จาก grades กับ teacher_id จาก subjects
   - Join Type: Left outer

7. ✅ คลิก "Save"

### ตั้งค่า Dimensions & Metrics:
8. ✅ Dimensions:
   - student_name (จาก students)
   - grade_level (จาก students)
   - subject_name (จาก subjects)
   - teacher_id (จาก subjects)

9. ✅ Metrics:
   - AVG(total_score) → "คะแนนเฉลี่ย"
   - AVG(grade) → "เกรดเฉลี่ย"

### ผลลัพธ์ที่ควรได้:
- [x] ตารางแสดงนักเรียน → วิชาที่เรียน → คะแนนและเกรด
- [x] สามารถเห็นว่านักเรียนคนไหนเรียนวิชาอะไร ได้เกรดเท่าไหร่

---

## Lab 3.3: Blend ครูกับวิชาที่สอน

### วัตถุประสงค์:
แสดงว่าครูแต่ละคนสอนวิชาอะไรบ้าง มีนักเรียนกี่คน

### ขั้นตอน:
1. ✅ สร้าง Table ใหม่
2. ✅ Blend data: **teachers** (Join Key: teacher_id) + **subjects** (Join Key: teacher_id)  เลือก Join type เป็น inner join

3. ✅ Dimensions:
   - teacher_name (จาก teachers)
   - subject_name (จาก subjects)
   - position (จาก teachers)
   - department (จาก teachers)

4. ✅ Metrics:
   - SUM(students_enrolled) → "จำนวนนักเรียนทั้งหมด"

### ผลลัพธ์ที่ควรได้:
- [x] แสดงครูแต่ละคนสอนวิชาอะไร
- [x] มีนักเรียนรวมกี่คน

---

## Lab 3.4: Blend นักเรียนกับการเข้าเรียน

### วัตถุประสงค์:
คำนวณเปอร์เซ็นต์การเข้าเรียนของแต่ละคน

### ขั้นตอน:
1. ✅ สร้าง Table ใหม่
2. ✅ Blend: **students** + **attendance** (Join Key: student_id)

3. ✅ Dimensions:
   - student_name
   - grade_level
   - class_room

4. ✅ Metrics:
   - Record Count → "จำนวนครั้งทั้งหมด"
   - คลิก "Add metric" -> Add calculated field
     สร้าง Calculated Field:

```
ชื่อฟิลด์: มาเรียน
Formula: 
SUM(CASE WHEN status = "มาเรียน" THEN 1 ELSE 0 END)
```

```
ชื่อฟิลด์: เปอร์เซ็นต์เข้าเรียน
Formula:
SUM(CASE WHEN status = "มาเรียน" THEN 1 ELSE 0 END) / COUNT(attendance_id) * 100
```

5. ✅ เพิ่ม Metrics: ในตาราง
   - มาเรียน
   - เปอร์เซ็นต์เข้าเรียน

### ผลลัพธ์ที่ควรได้:
- [x] ตารางแสดงนักเรียนแต่ละคน
- [x] จำนวนครั้งที่มาเรียน
- [x] เปอร์เซ็นต์การเข้าเรียน

### 🎨 Style - Conditional Formatting:
7. ✅ ไปที่ "Style" tab
8. ✅ เปิด "Conditional formatting" กด Add formatting
9. เลือกฟิลด์ "ร้อยละการเข้าเรียน"
10. สร้างเงื่อนไข 3 เงื่อนไข
11. ✅ ตั้งค่า:
   - ร้อยละการเข้าเรียน ≥ 90: สีเขียว
   - ร้อยละการเข้าเรียน >=70 and ร้อยละการเข้าเรียน <=89 : สีเหลือง
   - ร้อยละการเข้าเรียน <=69 : สีแดง

---

## Lab 3.5: Blend งบประมาณตามแผนก

### วัตถุประสงค์:
สรุปงบประมาณแต่ละแผนก

### ขั้นตอน:
1. ✅ สร้าง Stacked Bar Chart
2. ✅ Data Source: **budget** (ไม่ต้อง Blend)
3. ✅ Dimension: department
4. ✅ Breakdown Dimension: category
5. ✅ Metric: SUM(budget_spent)

```

### ผลลัพธ์ที่ควรได้:
- [x] กราฟแท่งแสดงการใช้งบประมาณแต่ละแผนก
- [x] แยกสีตามหมวดงบประมาณ
```
---

# 📄 ส่วนที่ 4: สร้าง Dashboard

## Lab 4.1: Page 1 - ภาพรวมโรงเรียน

### เตรียม Layout:
1. ✅ เปลี่ยนชื่อหน้า: คลิก "Page 1" → เปลี่ยนเป็น "ภาพรวมโรงเรียน"
2. ✅ เพิ่ม Header: Insert → Text → พิมพ์ "Dashboard โรงเรียน - ภาพรวม"
3. ✅ ตั้งค่า Text: Font size 24, Bold, สีเข้ม

### Scorecard 1: จำนวนนักเรียนทั้งหมด
1. ✅ Insert → Scorecard
2. ✅ Data Source: students
3. ✅ Metric: COUNT(student_id)
4. ✅ เปลี่ยนชื่อ: "จำนวนนักเรียน"
5. ✅ Style: เปลี่ยนสีพื้นหลัง, ขนาดตัวเลขใหญ่ขึ้น

### Scorecard 2: จำนวนครู
1. ✅ คัดลอก Scorecard 1
2. ✅ Data Source: teachers
3. ✅ Metric: COUNT(teacher_id)
4. ✅ เปลี่ยนชื่อ: "จำนวนครู"

### Scorecard 3: GPA เฉลี่ย
1. ✅ Scorecard ใหม่
2. ✅ Data Source: grades
3. ✅ Metric: AVG(grade)
4. ✅ Format: ทศนิยม 2 ตำแหน่ง
5. ✅ Comparison: เปรียบเทียบกับเดือนที่แล้ว (ถ้าต้องการ)

### Scorecard 4: อัตราการเข้าเรียน
1. ✅ Scorecard ใหม่
2. ✅ Data Source: attendance
3. ✅ Metric: ใช้ Calculated Field "เปอร์เซ็นต์เข้าเรียน" ที่สร้างไว้
4. ✅ เพิ่มสัญลักษณ์ %

### Scorecard 5: งบประมาณคงเหลือ
1. ✅ Data Source: budget
2. ✅ Calculated Field:
```
Formula: SUM(budget_remaining) / SUM(budget_allocated) * 100
```

### Scorecard 6: เอกสารรออนุมัติ
1. ✅ Data Source: documents
2. ✅ Metric: COUNT(document_id)
3. ✅ Filter: status = "รอการอนุมัติ"

### Pie Chart: สัดส่วนนักเรียนตามระดับชั้น
1. ✅ Insert → Pie Chart
2. ✅ Data Source: students
3. ✅ Dimension: grade_level
4. ✅ Metric: COUNT(student_id)
5. ✅ Style: เลือก Color palette ที่สวยงาม

### Bar Chart: จำนวนนักเรียนแต่ละห้อง
1. ✅ Insert → Bar Chart (แนวนอน)
2. ✅ Data Source: students
3. ✅ Dimension: class_room
4. ✅ Sort: จำนวนนักเรียน (มาก → น้อย)
5. ✅ Metric: COUNT(student_id)

### Table: สรุปข้อมูลแต่ละชั้น
1. ✅ Insert → Table
2. ✅ Blend: students + grades
3. ✅ Dimensions:
   - grade_level
4. ✅ Metrics:
   - COUNT(student_id) → "จำนวนนักเรียน"
   - AVG(grade) → "GPA เฉลี่ย"

### เพิ่ม Date Range Control:
1. ✅ Add a control → Date Range Control
2. ✅ วางตำแหน่งด้านบนขวา
3. ✅ ตั้งค่า: แสดงเป็นช่วงเดือน
4. สังเกตผลเมื่อเลือกช่วงวันที่เปลี่ยนไป Dashboard ส่วนอื่น ๆ จะเปลี่ยนแปลงข้อมูลตาม

### ผลลัพธ์สำหรับ Page 1:
- [x] มี Scorecards 6 ตัวแสดงด้านบน
- [x] มี Pie Chart และ Bar Chart แสดงข้อมูลนักเรียน
- [x] มี Table สรุปข้อมูล
- [x] มี Date Range Control

---

## Lab 4.2: Page 2 - วิชาการและผลการเรียน

### เพิ่มหน้าใหม่:
1. ✅ คลิก "+ Add a page" (ด้านล่างซ้าย)
2. ✅ เปลี่ยนชื่อ: "วิชาการและผลการเรียน"

### Scorecards:
1. ✅ **GPA เฉลี่ยโรงเรียน**
   - Data: grades
   - Metric: AVG(grade)

2. ✅ **นักเรียน GPA >= 3.0**
   - Blend: students + grades
   - Filter: AVG(grade) >= 3.0
   - Metric: COUNT(student_id)

3. ✅ **นักเรียนต้องเสริม (GPA < 2.0)**
   - Filter: AVG(grade) < 2.0
   - Metric: COUNT(student_id)
   - Style: สีแดง เพื่อเตือน

### Bar Chart: คะแนนเฉลี่ยแต่ละวิชา
1. ✅ Insert → Bar Chart
2. ✅ Blend: grades + subjects
3. ✅ Dimension: subject_name
4. ✅ Metric: AVG(total_score)
5. ✅ Sort: คะแนนเฉลี่ย (สูง → ต่ำ)

### Stacked Bar Chart: การกระจายเกรด
1. ✅ Add a chart → Stacked Bar Chart
2. ✅ Data Source: grades
3. ✅ Dimension: subject_name
4. ✅ Breakdown Dimension: grade
5. ✅ Metric: COUNT(record_id)

### Table: ผลการเรียนแต่ละห้อง
1. ✅ Table ใหม่
2. ✅ Blend: students + grades + subjects
3. ✅ Dimensions:
   - grade_level
   - class_room
4. ✅ Metrics:
   - AVG(grade)
   - AVG(midterm_score)
   - AVG(final_score)

### Conditional Formatting:
5. ✅ คลิกคอลัมน์ AVG(grade)
6. ✅ Style → Conditional formatting:
   - >= 3.5: สีเขียวเข้ม
   - 3.0-3.49: สีเขียวอ่อน
   - 2.5-2.99: สีเหลือง
   - 2.0-2.49: สีส้ม
   - < 2.0: สีแดง

### Combo Chart: เปรียบเทียบคะแนนกลางภาค-ปลายภาค
1. ✅ Insert → Combo Chart
2. ✅ Blend: grades + subjects
3. ✅ Dimension: subject_name
4. ✅ Metric 1 (Bar): AVG(midterm_score)
5. ✅ Metric 2 (Line): AVG(final_score)

### Table: นักเรียนที่ต้องเสริม
1. ✅ Table ใหม่
2. ✅ Blend: students + grades
3. ✅ Filter: AVG(grade) < 2.0
4. ✅ Dimensions:
   - student_name
   - grade_level
   - class_room
5. ✅ Metrics:
   - AVG(grade) → format 2 ทศนิยม
   - COUNT(record_id) → "จำนวนวิชา"

### Filters สำหรับ Page นี้:
1. ✅ Insert → Drop-down list 3 ตัว เพื่อเลือกใช้กรองข้อมูล โดยเลือก data source ของ drop-down list แต่ละตัวตามข้อมูล Control field
2. ✅ เพิ่ม Filters ข้อมูล โดยกำหนด Control filed ให้กับ Drop-down list แต่ละตัว:
   - grade_level (ระดับชั้น)
   - subject_name (วิชา)
   - semester (ภาคเรียน)

---
## ส่วนนี้ข้ามไปได้ ยังไม่ต้องทำ 
## Lab 4.3: Page 3 - การเข้าเรียนและพฤติกรรม

### เพิ่มหน้าใหม่:
1. ✅ "+ Add a page" → "การเข้าเรียน"

### Scorecards:
1. ✅ **เปอร์เซ็นต์เข้าเรียนเฉลี่ย**
   - ใช้ Calculated Field ที่สร้างไว้

2. ✅ **นักเรียนขาดบ่อย (>3 ครั้ง)**
   - Calculated Field:
```
Formula: COUNTIF(status = "ขาดเรียน")
```
   - Filter: COUNTIF > 3

3. ✅ **การมาสายทั้งหมด**
   - Metric: COUNTIF(status = "มาสาย")

### Time Series: อัตราการเข้าเรียนรายวัน
1. ✅ Insert → Time Series Chart
2. ✅ Data Source: attendance
3. ✅ Date Range Dimension: date
4. ✅ Metric: 
```
Calculated Field: อัตราเข้าเรียน
Formula: COUNTIF(status = "มาเรียน") / COUNT(attendance_id) * 100
```
5. ✅ Style: Line ให้หนาขึ้น, เพิ่มจุดข้อมูล

### Stacked Column Chart: สถานะการเข้าเรียน
1. ✅ Insert → Stacked Column Chart
2. ✅ Dimension: date
3. ✅ Breakdown Dimension: status
4. ✅ Metric: COUNT(attendance_id)
5. ✅ Color:
   - มาเรียน: สีเขียว
   - ขาดเรียน: สีแดง
   - มาสาย: สีส้ม
   - ลากิจ/ลาป่วย: สีเหลือง

### Bar Chart: จำนวนการขาดแต่ละห้อง
1. ✅ Bar Chart
2. ✅ Blend: students + attendance
3. ✅ Filter: status = "ขาดเรียน"
4. ✅ Dimension: class_room
5. ✅ Metric: COUNT(attendance_id)

### Pie Chart: เหตุผลการขาดเรียน
1. ✅ Pie Chart
2. ✅ Data Source: attendance
3. ✅ Filter: status != "มาเรียน"
4. ✅ Dimension: reason
5. ✅ Metric: COUNT(attendance_id)

### Table: นักเรียนที่มีปัญหาการเข้าเรียน
1. ✅ Table
2. ✅ Blend: students + attendance
3. ✅ Dimensions:
   - student_name
   - grade_level
   - class_room
4. ✅ Metrics:
   - COUNT(attendance_id) → "ครั้งทั้งหมด"
   - COUNTIF(status = "มาเรียน") → "มาเรียน"
   - COUNTIF(status = "ขาดเรียน") → "ขาด"
   - COUNTIF(status = "มาสาย") → "สาย"
   - เปอร์เซ็นต์เข้าเรียน
5. ✅ Sort: จำนวนขาด (มาก → น้อย)
6. ✅ Filter: เปอร์เซ็นต์เข้าเรียน < 90

### Conditional Formatting:
- เปอร์เซ็นต์เข้าเรียน:
  - >= 95: เขียวเข้ม
  - 90-94: เขียวอ่อน
  - 80-89: เหลือง
  - < 80: แดง

---


## เริ่มทำต่อส่วนนี้
## Lab 4.4: Page 4 - งานบุคลากร

### เพิ่มหน้าใหม่:
1. ✅ "+ Add a page" → "งานบุคลากร"

### Scorecards:
1. ✅ **จำนวนครูทั้งหมด**
   - COUNT(teacher_id)
   - Filter: Include -> position -> CONTAINS ->"ครู"

2. ✅ **บุคลากรสนับสนุน**
   - COUNT(teacher_id)
   - Filter: Exclude -> position -> Contains ->"ครู"


### Pie Chart: สัดส่วนครูแต่ละกลุ่มสาระ
1. ✅ Pie Chart
2. ✅ Data Source: teachers
3. ✅ Filter: department = "วิชาการ"
4. ✅ Dimension: subject
5. ✅ Metric: COUNT(teacher_id)

### Bar Chart: ครูแยกตามวุฒิการศึกษา
1. ✅ Bar Chart (แนวนอน)
2. ✅ Dimension: education_level
3. ✅ Metric: COUNT(teacher_id)
4. ✅ Sort: ปริญญาเอก, ปริญญาโท, ปริญญาตรี, ปวช.

### Column Chart: ครูแยกตามวิทยฐานะ
1. ✅ Column Chart
2. ✅ Dimension: salary_level
3. ✅ Metric: COUNT(teacher_id)


### Donut Chart: อายุงาน
1. ✅ Donut Chart
2. ✅ Add a field -> Add Calculated Field:
```
ชื่อ: กลุ่มอายุงาน
Formula:
CASE
  WHEN experience_years <= 5 THEN "0-5 ปี"
  WHEN experience_years <= 10 THEN "6-10 ปี"
  WHEN experience_years <= 15 THEN "11-15 ปี"
  WHEN experience_years <= 20 THEN "16-20 ปี"
  ELSE "20+ ปี"
END
```
3. ✅ Dimension: กลุ่มอายุงาน
4. ✅ Metric: COUNT(teacher_id)

### Table: รายชื่อครูพร้อมข้อมูล
1. ✅ Table
2. ✅ Blend: teachers + subjects (Left Outer Join)
3. ✅ Dimensions:
   - teacher_name
   - subject
   - position
   - experience_years
   - department
4. ✅ Metrics:
   - COUNT(subject_code) → "จำนวนวิชาที่สอน"
   - SUM(students_enrolled) → "จำนวนนักเรียนทั้งหมด"

---

## Lab 4.5: Page 5 - งบประมาณและการเงิน

### เพิ่มหน้าใหม่:
1. ✅ "+ Add a page" → "งบประมาณและการเงิน"

### Scorecards: แบบที่ 2 (Scorecard with compact numbers)
1. ✅ **งบประมาณทั้งหมด**
   - SUM(budget_allocated)
   - Format: เพิ่มเครื่องหมาย ฿

2. ✅ **งบประมาณที่ใช้ไป**
   - SUM(budget_spent)

3. ✅ **งบประมาณคงเหลือ**
   - SUM(budget_remaining)

4. ✅ **% การใช้จ่าย**
   - Calculated Field:
```
Formula: SUM(budget_spent) / SUM(budget_allocated) * 100
```

### Gauge Chart: เปอร์เซ็นต์การใช้จ่าย
1. ✅ Insert → Gauge Chart With Renges
2. ✅ Metric: เปอร์เซ็นต์การใช้จ่าย
3. ✅ Style:
   - Show axis -> เลือก
   - Min: 0
   - Max: 100
   - กำหนด Range limits 4 Range 25,50,75 และ 100

### Stacked Bar Chart: งบแต่ละหมวด
1. ✅ Stacked Bar Chart (แนวนอน)
2. ✅ Dimension: category

### Pie Chart: สัดส่วนงบแต่ละแผนก
1. ✅ Pie Chart
2. ✅ Dimension: department
3. ✅ Metric: SUM(budget_spent)

### Waterfall Chart: การเปลี่ยนแปลงงบประมาณ
1. ✅ Insert → Waterfall Chart
2. ✅ Dimension: category
3. ✅ Metric: SUM(budget_spent)
4. ✅ Style: ปรับสีให้สวยงาม

### Time Series: แนวโน้มการใช้จ่ายรายเดือน
1. ✅ Time Series Chart
2. ✅ Date Range Dimension: month
3. ✅ Metric: SUM(budget_spent)
4. ✅ Breakdown Dimension: category (เพื่อดูว่าใช้ไปที่ไหนบ้าง)


### Filters:
1. ✅ Add a control : Drop-down เพื่อฟิลเตอร์ข้อมูล 4 ตัวเลือกด้านล่าง
   - budget_year (ปีงบประมาณ)
   - category (หมวดงบ)
   - department (แผนก)
   - status (สถานะ)

---

## Lab 4.6: Page 6 - E-Document และการอนุมัติ

### เพิ่มหน้าใหม่:
1. ✅ "+ Add a page" → "E-Document"

### Scorecards:
1. ✅ **เอกสารทั้งหมด**
   - COUNT(document_id)

2. ✅ **รออนุมัติ**
   - Filter: status = "รอการอนุมัติ"
   - Style: สีส้ม

3. ✅ **อนุมัติแล้ว**
   - Filter: status = "อนุมัติแล้ว"
   - Style: สีเขียว

4. ✅ **เอกสารร่าง**
   - Filter: status = "ร่าง"

5. ✅ **เอกสารด่วนรออนุมัติ**
   - Filter: status = "รอการอนุมัติ" AND priority IN ("ด่วน", "ด่วนที่สุด")
   - Style: สีแดง, เตือนด้วย Border

### Funnel Chart: สถานะเอกสาร
1. ✅ Insert → Funnel chart smoothed bar
2. ✅ Dimension: status
3. ✅ Metric: COUNT(document_id)
4. ✅ Sort Order:
   - ร่าง
   - รอการอนุมัติ
   - อนุมัติแล้ว

### Stacked Column Chart: เอกสารแต่ละประเภท
1. ✅ Stacked Column Chart
2. ✅ Dimension: document_type
3. ✅ Breakdown Dimension: status
4. ✅ Metric: COUNT(document_id)


### Time Series: เอกสารที่สร้างรายวัน
1. ✅ Time Series Chart
2. ✅ Date Range Dimension: created_date
3. ✅ Metric: COUNT(document_id)

### Pie Chart: ประเภทเอกสาร
1. ✅ Pie Chart
2. ✅ Dimension: document_type
3. ✅ Metric: COUNT(document_id)

### Table: เอกสารรออนุมัติ
1. ✅ Table
2. ✅ Blend: documents + teachers  join แบบ left join ด้วย created_by และ teacher_id
3. ✅ Filter: status = "รอการอนุมัติ"
4. ✅ Dimensions:
   - document_title
   - document_type
   - teacher_name (ผู้สร้าง)
   - created_date
   - priority
   - department
5. ✅ Calculated Field:
   
```
ชื่อ: วันที่รอ
Formula: DATE_DIFF(CURRENT_DATE(), created_date)
```
6. ✅ Sort: วันที่รอ (มาก → น้อย)

### Conditional Formatting สำหรับ Table:
- priority คอลัมน์:
  - "ด่วนที่สุด": พื้นหลังสีแดง, ตัวอักษรสีขาว
  - "ด่วน": พื้นหลังสีส้ม
  - "ปกติ": ไม่ระบายสี
- วันที่รอ คอลัมน์:
  - > 7 วัน: สีแดง
  - 4-7 วัน: สีเหลือง
  - < 4 วัน: สีเขียว

### Filters สำหรับหน้านี้:
1. ✅ Date Range Control (ช่วงวันที่สร้างเอกสาร)
2. ✅ Drop-down Filters:
   - status
   - document_type
   - priority
   - department

---


# 📤 ส่วนที่ 7: Sharing และ Export

## Lab 7.1: แชร์ Dashboard

### วิธีที่ 1: แชร์ผ่าน Link
1. ✅ คลิก Drop down ที่อยู่ข้างปุ่ม "Share" (ด้านบนขวา)
2. ✅ เลือก Get report link 
3. ✅ คัดลอก Link แชร์ส่งใน MS Teams

### วิธีที่ 2: Embed ลงในเว็บไซต์ 
1. ✅ คลิก "Share" → "Embed"
2. ✅ คัดลอก iframe code
3. ✅ นำไปใส่ในเว็บไซต์โรงเรียน

### วิธีที่ 3: Schedule Email
1. ✅ คลิก "Share" → "Schedule email delivery"
2. ✅ ตั้งค่า:
   - ผู้รับ: อีเมล์ผู้บริหาร
   - ความถี่: ทุกวัน/สัปดาห์/เดือน
   - Format: PDF

**Happy Data Visualization! 📊🎓**
