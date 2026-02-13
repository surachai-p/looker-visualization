# 🔗 Data Blending Examples - Looker Studio

## ภาพรวมการเชื่อมโยงข้อมูล

เอกสารนี้แสดงตัวอย่างการ Blend Data ทั้งหมดที่ใช้ใน Dashboard พร้อมอธิบายรายละเอียดและผลลัพธ์ที่คาดหวัง

---

## 🎯 Blend Example 1: นักเรียน + ผลการเรียน

### จุดประสงค์:
ต้องการดู GPA และจำนวนวิชาของนักเรียนแต่ละคน

### การตั้งค่า Blend:

**Table 1: students**
- Primary Key: `student_id`

**Table 2: grades**  
- Foreign Key: `student_id`
- Join Type: **Left Outer Join**

### ตรรกะการ Join (คล้าย SQL):
```sql
SELECT 
    s.student_name,
    s.grade_level,
    s.class_room,
    AVG(g.grade) as GPA,
    COUNT(g.record_id) as จำนวนวิชา
FROM students s
LEFT JOIN grades g ON s.student_id = g.student_id
GROUP BY s.student_name, s.grade_level, s.class_room
```

### Looker Studio Configuration:

**Dimensions:**
- student_name (จาก students)
- grade_level (จาก students)
- class_room (จาก students)

**Metrics:**
- AVG(grade) → เปลี่ยนชื่อเป็น "GPA"
- COUNT(record_id) → เปลี่ยนชื่อเป็น "จำนวนวิชา"

### ผลลัพธ์ที่คาดหวัง:

| student_name | grade_level | class_room | GPA | จำนวนวิชา |
|-------------|-------------|-----------|-----|----------|
| นางสาวกมลชนก สุขใจ | ม.1 | 1/1 | 3.10 | 10 |
| นายธนพล วงศ์ดี | ม.1 | 1/1 | 3.45 | 10 |
| นางสาวสุภาพร ใจงาม | ม.1 | 1/2 | 3.85 | 10 |
| ... | ... | ... | ... | ... |

### การใช้งาน:
- สร้างเป็น **Table** เพื่อแสดงรายละเอียด
- สร้างเป็น **Bar Chart** โดยใช้ student_name เป็น Dimension และ GPA เป็น Metric
- กรอง Filter เฉพาะนักเรียนที่ GPA < 2.0 เพื่อหานักเรียนที่ต้องเสริม

---

## 🎯 Blend Example 2: นักเรียน + ผลการเรียน + วิชา

### จุดประสงค์:
แสดงว่านักเรียนแต่ละคนเรียนวิชาอะไรบ้าง ได้คะแนนเท่าไหร่ และครูผู้สอนคือใคร

### การตั้งค่า Blend:

**Table 1: students**
- Primary Key: `student_id`

**Table 2: grades**
- Foreign Key: `student_id` → Join กับ students
- Join Type: **Left Outer Join**

**Table 3: subjects**
- Foreign Key: `subject_code` (จาก grades) → Join กับ grades
- Join Type: **Left Outer Join**

### ตรรกะการ Join (คล้าย SQL):
```sql
SELECT 
    s.student_name,
    s.grade_level,
    subj.subject_name,
    subj.teacher_id,
    g.midterm_score,
    g.final_score,
    g.total_score,
    g.grade
FROM students s
LEFT JOIN grades g ON s.student_id = g.student_id
LEFT JOIN subjects subj ON g.subject_code = subj.subject_code
```

### Looker Studio Configuration:

**Dimensions:**
- student_name (จาก students)
- grade_level (จาก students)
- subject_name (จาก subjects)
- teacher_id (จาก subjects)

**Metrics:**
- AVG(midterm_score) → "คะแนนกลางภาค"
- AVG(final_score) → "คะแนนปลายภาค"
- AVG(total_score) → "คะแนนเฉลี่ย"
- AVG(grade) → "เกรดเฉลี่ย"

### ผลลัพธ์ที่คาดหวัง:

| student_name | grade_level | subject_name | teacher_id | คะแนนเฉลี่ย | เกรดเฉลี่ย |
|-------------|-------------|-------------|-----------|-----------|----------|
| นางสาวกมลชนก สุขใจ | ม.1 | คณิตศาสตร์ ม.1 | T001 | 75 | 3.0 |
| นางสาวกมลชนก สุขใจ | ม.1 | ภาษาไทย ม.1 | T002 | 80 | 3.0 |
| นางสาวกมลชนก สุขใจ | ม.1 | วิทยาศาสตร์ ม.1 | T003 | 67 | 2.0 |
| ... | ... | ... | ... | ... | ... |

### การใช้งาน:
- สร้างเป็น **Table** แสดงรายละเอียดแต่ละวิชา
- สร้างเป็น **Pivot Table** โดยใช้ student_name เป็น Row, subject_name เป็น Column, grade เป็น Value
- สร้างเป็น **Heat Map** แสดงผลการเรียนของนักเรียนในแต่ละวิชา

---

## 🎯 Blend Example 3: ครู + วิชาที่สอน + นักเรียน

### จุดประสงค์:
แสดงว่าครูแต่ละคนสอนวิชาอะไรบ้าง มีนักเรียนกี่คน และผลการเรียนเป็นอย่างไร

### การตั้งค่า Blend:

**Table 1: teachers**
- Primary Key: `teacher_id`

**Table 2: subjects**
- Foreign Key: `teacher_id`
- Join Type: **Left Outer Join**

**Table 3: grades** (Optional)
- Foreign Key: `subject_code` (จาก subjects)
- Join Type: **Left Outer Join**

### ตรรกะการ Join (คล้าย SQL):
```sql
SELECT 
    t.teacher_name,
    t.subject AS teacher_subject,
    t.position,
    subj.subject_name,
    subj.class_room,
    subj.students_enrolled,
    AVG(g.grade) AS average_grade
FROM teachers t
LEFT JOIN subjects subj ON t.teacher_id = subj.teacher_id
LEFT JOIN grades g ON subj.subject_code = g.subject_code
GROUP BY t.teacher_name, t.subject, t.position, subj.subject_name, subj.class_room, subj.students_enrolled
```

### Looker Studio Configuration:

**Dimensions:**
- teacher_name (จาก teachers)
- subject (จาก teachers) → วิชาที่ถนัด
- position (จาก teachers)
- subject_name (จาก subjects) → วิชาที่สอนจริง
- class_room (จาก subjects)

**Metrics:**
- SUM(students_enrolled) → "จำนวนนักเรียนทั้งหมด"
- AVG(grade) → "GPA เฉลี่ยของนักเรียน"
- COUNT(subject_code) → "จำนวนชั้นเรียนที่สอน"

### ผลลัพธ์ที่คาดหวัง:

| teacher_name | วิชาถนัด | วิชาที่สอน | ห้อง | นักเรียน | GPA เฉลี่ย |
|-------------|----------|-----------|------|---------|----------|
| นายสมชาย ใจดี | คณิตศาสตร์ | คณิตศาสตร์ ม.1 | 1/1 | 30 | 3.25 |
| นายสมชาย ใจดี | คณิตศาสตร์ | คณิตศาสตร์ ม.1 | 1/2 | 28 | 3.10 |
| นางสาวประภา สว่างแสง | ภาษาไทย | ภาษาไทย ม.1 | 1/1 | 30 | 3.15 |
| ... | ... | ... | ... | ... | ... |

### การใช้งาน:
- แสดงภาระงานสอนของครูแต่ละคน
- วิเคราะห์ว่าครูคนไหนมีนักเรียนผลเรียนดี
- วางแผนการจัดตารางสอน

---

## 🎯 Blend Example 4: นักเรียน + การเข้าเรียน

### จุดประสงค์:
คำนวณเปอร์เซ็นต์การเข้าเรียนของนักเรียนแต่ละคน

### การตั้งค่า Blend:

**Table 1: students**
- Primary Key: `student_id`

**Table 2: attendance**
- Foreign Key: `student_id`
- Join Type: **Left Outer Join**

### ตรรกะการ Join (คล้าย SQL):
```sql
SELECT 
    s.student_name,
    s.grade_level,
    s.class_room,
    COUNT(a.attendance_id) AS total_records,
    SUM(CASE WHEN a.status = 'มาเรียน' THEN 1 ELSE 0 END) AS attended,
    SUM(CASE WHEN a.status = 'ขาดเรียน' THEN 1 ELSE 0 END) AS absent,
    SUM(CASE WHEN a.status = 'มาสาย' THEN 1 ELSE 0 END) AS late,
    (SUM(CASE WHEN a.status = 'มาเรียน' THEN 1 ELSE 0 END) * 100.0 / COUNT(a.attendance_id)) AS attendance_rate
FROM students s
LEFT JOIN attendance a ON s.student_id = a.student_id
GROUP BY s.student_name, s.grade_level, s.class_room
```

### Looker Studio Configuration:

**Dimensions:**
- student_name (จาก students)
- grade_level (จาก students)
- class_room (จาก students)

**Calculated Fields:**
```
ชื่อ: มาเรียน
Formula: COUNTIF(status = "มาเรียน")

ชื่อ: ขาดเรียน
Formula: COUNTIF(status = "ขาดเรียน")

ชื่อ: มาสาย
Formula: COUNTIF(status = "มาสาย")

ชื่อ: เปอร์เซ็นต์เข้าเรียน
Formula: COUNTIF(status = "มาเรียน") / COUNT(attendance_id) * 100
```

**Metrics:**
- Record Count → "ครั้งทั้งหมด"
- มาเรียน
- ขาดเรียน
- มาสาย
- เปอร์เซ็นต์เข้าเรียน

### ผลลัพธ์ที่คาดหวัง:

| student_name | grade_level | class_room | ครั้งทั้งหมด | มาเรียน | ขาด | สาย | % เข้าเรียน |
|-------------|-------------|-----------|------------|---------|-----|-----|-----------|
| นางสาวกมลชนก สุขใจ | ม.1 | 1/1 | 5 | 4 | 1 | 0 | 80.0 |
| นายธนพล วงศ์ดี | ม.1 | 1/1 | 6 | 5 | 0 | 1 | 83.3 |
| นางสาวสุภาพร ใจงาม | ม.1 | 1/2 | 5 | 5 | 0 | 0 | 100.0 |
| ... | ... | ... | ... | ... | ... | ... | ... |

### การใช้งาน:
- สร้าง **Table** พร้อม Conditional Formatting (สีแดงถ้า < 80%)
- สร้าง **Bar Chart** เรียงตามเปอร์เซ็นต์เข้าเรียน
- Filter หานักเรียนที่มีปัญหาการเข้าเรียน

---

## 🎯 Blend Example 5: การเข้าเรียน + วิชา + นักเรียน

### จุดประสงค์:
วิเคราะห์การเข้าเรียนแยกตามวิชา เพื่อหาว่านักเรียนขาดเรียนวิชาไหนบ่อย

### การตั้งค่า Blend:

**Table 1: attendance**
- Primary Key: (student_id + subject_code)

**Table 2: students**
- Foreign Key: `student_id`
- Join Type: **Inner Join**

**Table 3: subjects**
- Foreign Key: `subject_code` (จาก attendance)
- Join Type: **Inner Join**

### ตรรกะการ Join (คล้าย SQL):
```sql
SELECT 
    s.student_name,
    subj.subject_name,
    subj.teacher_id,
    COUNT(a.attendance_id) AS total,
    SUM(CASE WHEN a.status = 'มาเรียน' THEN 1 ELSE 0 END) AS attended,
    SUM(CASE WHEN a.status = 'ขาดเรียน' THEN 1 ELSE 0 END) AS absent,
    (SUM(CASE WHEN a.status = 'มาเรียน' THEN 1 ELSE 0 END) * 100.0 / COUNT(a.attendance_id)) AS rate
FROM attendance a
INNER JOIN students s ON a.student_id = s.student_id
INNER JOIN subjects subj ON a.subject_code = subj.subject_code
GROUP BY s.student_name, subj.subject_name, subj.teacher_id
```

### Looker Studio Configuration:

**Dimensions:**
- student_name (จาก students)
- subject_name (จาก subjects)
- teacher_id (จาก subjects)

**Metrics:**
- COUNT(attendance_id) → "ครั้งทั้งหมด"
- COUNTIF(status = "มาเรียน") → "มา"
- COUNTIF(status = "ขาดเรียน") → "ขาด"
- เปอร์เซ็นต์เข้าเรียน

### ผลลัพธ์ที่คาดหวัง:

| student_name | subject_name | teacher_id | มา | ขาด | % |
|-------------|-------------|-----------|-----|-----|-----|
| นางสาวกมลชนก สุขใจ | คณิตศาสตร์ ม.1 | T001 | 4 | 1 | 80 |
| นางสาวกมลชนก สุขใจ | ภาษาไทย ม.1 | T002 | 5 | 0 | 100 |
| ... | ... | ... | ... | ... | ... |

### การใช้งาน:
- หาว่าวิชาไหนที่นักเรียนขาดบ่อย
- ครูสามารถดูได้ว่านักเรียนในวิชาของตนเข้าเรียนหรือไม่

---

## 🎯 Blend Example 6: งบประมาณ (ไม่ต้อง Blend)

### จุดประสงค์:
แสดงการใช้งบประมาณแต่ละหมวด

### Data Source: budget (เดี่ยว)

### Looker Studio Configuration:

**Dimensions:**
- category
- sub_category
- department
- month

**Metrics:**
- SUM(budget_allocated) → "งบที่ได้รับ"
- SUM(budget_spent) → "งบที่ใช้ไป"
- SUM(budget_remaining) → "งบคงเหลือ"

**Calculated Fields:**
```
ชื่อ: เปอร์เซ็นต์ใช้จ่าย
Formula: SUM(budget_spent) / SUM(budget_allocated) * 100
```

### ผลลัพธ์ที่คาดหวัง:

| category | department | งบที่ได้รับ | ใช้ไป | คงเหลือ | % |
|----------|-----------|-----------|--------|---------|-----|
| งานวิชาการ | วิชาการ | 3,600,000 | 2,800,000 | 800,000 | 77.8 |
| งานบุคลากร | บุคลากร | 37,200,000 | 17,520,000 | 19,680,000 | 47.1 |
| ... | ... | ... | ... | ... | ... |

---

## 🎯 Blend Example 7: เอกสาร + ครู

### จุดประสงค์:
แสดงว่าครูแต่ละคนสร้างเอกสารอะไรบ้าง และใช้เวลาอนุมัตินานแค่ไหน

### การตั้งค่า Blend:

**Table 1: documents**
- Primary Key: `document_id`

**Table 2: teachers**
- Foreign Key: `created_by` (จาก documents) = `teacher_id` (จาก teachers)
- Join Type: **Left Outer Join**

### ตรรกะการ Join (คล้าย SQL):
```sql
SELECT 
    t.teacher_name,
    t.department,
    d.document_type,
    d.document_title,
    d.created_date,
    d.status,
    d.priority,
    DATEDIFF(COALESCE(d.approve_date_level2, d.approve_date_level1), d.created_date) AS days_to_approve
FROM documents d
LEFT JOIN teachers t ON d.created_by = t.teacher_id
```

### Looker Studio Configuration:

**Dimensions:**
- teacher_name (จาก teachers)
- department (จาก teachers)
- document_type (จาก documents)
- status (จาก documents)
- priority (จาก documents)

**Metrics:**
- COUNT(document_id) → "จำนวนเอกสาร"

**Calculated Fields:**
```
ชื่อ: วันที่รอ
Formula: DATE_DIFF(CURRENT_DATE(), created_date)

ชื่อ: ระยะเวลาอนุมัติ (สำหรับที่อนุมัติแล้ว)
Formula: DATE_DIFF(COALESCE(approve_date_level2, approve_date_level1), created_date)
```

### ผลลัพธ์ที่คาดหวัง:

| teacher_name | department | document_type | status | จำนวน | วันที่รอ |
|-------------|-----------|--------------|--------|-------|---------|
| นายสมชาย ใจดี | วิชาการ | หนังสือราชการ | อนุมัติแล้ว | 2 | - |
| นางสาวประภา สว่างแสง | วิชาการ | บันทึกข้อความ | รอการอนุมัติ | 1 | 3 |
| ... | ... | ... | ... | ... | ... |

---

## 🎯 Blend Example 8: Full Dashboard Blend - นักเรียน + เกรด + การเข้าเรียน

### จุดประสงค์:
แสดงภาพรวมนักเรียนแต่ละคน: ผลเรียน + การเข้าเรียน ในที่เดียว

### การตั้งค่า Blend:

**Table 1: students** (Base)
- Primary Key: `student_id`

**Table 2: grades**
- Foreign Key: `student_id`
- Join Type: **Left Outer Join**

**Table 3: attendance**
- Foreign Key: `student_id`
- Join Type: **Left Outer Join**

### ตรรกะการ Join (คล้าย SQL):
```sql
SELECT 
    s.student_name,
    s.grade_level,
    s.class_room,
    s.gender,
    AVG(g.grade) AS gpa,
    COUNT(DISTINCT g.subject_code) AS subject_count,
    COUNT(a.attendance_id) AS attendance_total,
    SUM(CASE WHEN a.status = 'มาเรียน' THEN 1 ELSE 0 END) AS attended,
    (SUM(CASE WHEN a.status = 'มาเรียน' THEN 1 ELSE 0 END) * 100.0 / COUNT(a.attendance_id)) AS attendance_rate
FROM students s
LEFT JOIN grades g ON s.student_id = g.student_id
LEFT JOIN attendance a ON s.student_id = a.student_id
GROUP BY s.student_name, s.grade_level, s.class_room, s.gender
```

### Looker Studio Configuration:

**Dimensions:**
- student_name (จาก students)
- grade_level (จาก students)
- class_room (จาก students)
- gender (จาก students)

**Metrics จาก grades:**
- AVG(grade) → "GPA"
- COUNT(DISTINCT subject_code) → "จำนวนวิชา"
- AVG(total_score) → "คะแนนเฉลี่ย"

**Metrics จาก attendance:**
- COUNT(attendance_id) → "บันทึกการเข้าเรียน"
- เปอร์เซ็นต์เข้าเรียน

### ผลลัพธ์ที่คาดหวัง:

| student_name | ชั้น | ห้อง | GPA | จำนวนวิชา | % เข้าเรียน | สถานะ |
|-------------|------|------|-----|----------|-----------|-------|
| นางสาวสุภาพร ใจงาม | ม.1 | 1/2 | 3.85 | 5 | 100 | ดีเยี่ยม |
| นายธนพล วงศ์ดี | ม.1 | 1/1 | 3.45 | 5 | 90 | ดี |
| นางสาวกมลชนก สุขใจ | ม.1 | 1/1 | 3.10 | 5 | 80 | ปานกลาง |
| นายอรรถพล มั่นคง | ม.1 | 1/3 | 1.50 | 5 | 60 | ต้องเสริม |

### การใช้งาน:
- Dashboard หน้าหลักสำหรับดูภาพรวมนักเรียน
- สามารถ Filter หานักเรียนที่มีปัญหาทั้งผลเรียนและการเข้าเรียน
- แบ่ง Segment นักเรียนได้หลายแบบ

---

## 💡 Tips สำหรับการ Blend Data

### 1. เลือก Join Type ให้เหมาะสม

| Join Type | เมื่อไหร่ควรใช้ | ตัวอย่าง |
|-----------|----------------|---------|
| **Left Outer** | ต้องการข้อมูลฝั่งซ้ายทั้งหมด แม้ไม่มี match ขวา | students LEFT JOIN grades (แสดงนักเรียนทุกคนแม้ยังไม่มีเกรด) |
| **Right Outer** | ต้องการข้อมูลฝั่งขวาทั้งหมด | ใช้น้อย เพราะสลับตำแหน่งแล้วใช้ Left ได้ |
| **Inner Join** | ต้องการเฉพาะข้อมูลที่ match กันทั้ง 2 ฝั่ง | grades INNER JOIN subjects (แสดงเฉพาะเกรดที่มีวิชา) |
| **Full Outer** | ต้องการข้อมูลทั้ง 2 ฝั่ง ไม่ว่าจะ match หรือไม่ | ใช้น้อยใน Looker Studio |

### 2. ตรวจสอบ Join Key

✅ **ถูกต้อง:**
- student_id (Text) = student_id (Text)
- teacher_id (Text) = teacher_id (Text)

❌ **ผิด:**
- student_id (Text) ≠ student_id (Number)
- มี leading/trailing spaces

### 3. Performance Tips

- Blend เฉพาะตารางที่จำเป็น
- ใช้ Filter ก่อน Blend เพื่อลดข้อมูล
- หลีกเลี่ยง Blend มากกว่า 3 ตาราง ถ้าไม่จำเป็น

### 4. Debugging Blend Issues

ถ้า Blend ไม่แสดงข้อมูล:
1. ตรวจสอบ Join Keys ว่า Type ตรงกัน
2. ตรวจสอบว่ามีข้อมูลที่ match กันจริง
3. ลองเปลี่ยน Join Type
4. ตรวจสอบ Filters ที่อาจซ่อนข้อมูล

---

## 📊 สรุปการใช้ Blend ในแต่ละหน้า

| หน้า Dashboard | Blend ที่ใช้ | จุดประสงค์ |
|---------------|------------|----------|
| **1. ภาพรวม** | students + grades | แสดง GPA รวม |
| **2. วิชาการ** | students + grades + subjects | วิเคราะห์ผลเรียนแต่ละวิชา |
| **3. การเข้าเรียน** | students + attendance | คำนวณ % การเข้าเรียน |
| **4. บุคลากร** | teachers + subjects + grades | ภาระงานและผลงานครู |
| **5. งบประมาณ** | ไม่ Blend (budget เดี่ยว) | การเงิน |
| **6. E-Document** | documents + teachers | เอกสารและผู้สร้าง |

---

## ✅ Checklist การ Blend

ก่อน Blend:
- [ ] ตรวจสอบว่า Join Keys มีอยู่ใน Data Sources
- [ ] ตรวจสอบ Field Types ว่าตรงกัน
- [ ] ตรวจสอบว่ามีข้อมูลที่จะ match กัน

หลัง Blend:
- [ ] ตรวจนับจำนวนแถวว่าถูกต้อง
- [ ] ตรวจสอบว่าไม่มีข้อมูล duplicate
- [ ] ลอง Filter ดูว่าทำงานถูกต้อง
- [ ] ทดสอบ Calculated Fields ที่ใช้ข้อมูลจาก 2 ตาราง

---

**เอกสารนี้ช่วยให้เข้าใจการ Blend Data ใน Looker Studio อย่างละเอียด พร้อมตัวอย่างที่ใช้งานจริงในโรงเรียน! 🎓**
