# 🚀 Git & GitHub Workshop: จากศูนย์สู่ทีม (Zero to Pro)
คู่มือนี้ออกแบบมาเพื่อให้คุณทำตามแบบ Step-by-Step พร้อมคำอธิบายภาษาไทยและ Data Flow ให้เข้าใจง่ายที่สุดครับ

---

## 🎨 แผนผังการไหลของข้อมูล (Git Data Flow)
ก่อนเริ่มคำสั่ง มาเข้าใจก่อนว่าไฟล์เราเดินทางอย่างไร:
```text
[ Working Directory ] --(git add)--> [ Staging Area ] --(git commit)--> [ Local Repo ] --(git push)--> [ Remote (GitHub) ]
      (แก้ไขไฟล์)                      (เตรียมส่ง)                       (เก็บประวัติ)                    (สำรองออนไลน์)
```

---

## Phase 0: เตรียมตัวก่อนออกเดินทาง (Setup SSH & Config)

### 1. สร้างกุญใจ SSH (สำหรับ GitHub)
เราจะใช้ SSH เพื่อไม่ต้องพิมพ์รหัสผ่านบ่อยๆ และมีความปลอดภัยสูง
```bash
# สร้าง key ใหม่ (ตั้งชื่อไฟล์ว่า id_github_workshop เพื่อไม่ให้ทับของเก่า)
ssh-keygen -t ed25519 -C "your_email@example.com"
```
*   เมื่อถามชื่อไฟล์ให้ใส่: `/Users/yourname/.ssh/id_github_tutorial`
*   ก๊อปปี้ Public Key ไปแปะใน GitHub Settings -> SSH Keys:
```bash
cat ~/.ssh/id_github_tutorial.pub
```
* ตั้งค่าให้ working dir นี้ใช้ key นี้ในการเชื่อม Github
```bash
git config core.sshCommand "ssh -i ~/.ssh/id_github_tutorial"
```

### 2. ตั้งค่าตัวตน (Local Config)
เราจะตั้งค่าเฉพาะโฟลเดอร์นี้เท่านั้น ไม่ไปยุ่งกับ global
```bash
git config user.name "Your Name"
git config user.email "your_email@example.com"
# เช็คความเรียบร้อย
git config --list
```

---

## Phase 1: เริ่มต้นโปรเจกต์ (Initiation)

### 3. สร้างถังเก็บงาน (Init & Remote)
```bash
git init                            # เปลี่ยนโฟลเดอร์ปกติให้เป็น Git Repo
git remote add origin git@github.com:username/repo-name.git  # เชื่อมต่อกับ GitHub
git branch -M main                  # ตั้งชื่อ branch หลักเป็น main
```

---

## Phase 2: บันทึกงานครั้งแรก (Add -> Commit -> Push)

### 4. วงจรการทำงานปกติ
```bash
echo "# Git Tutorial" > README.md    # สร้างไฟล์
git status                          # เช็คสถานะ (จะเห็นไฟล์เป็นสีแดง)

git add README.md                   # ย้ายไป Staging (จองคิว)
git commit -m "feat: initial commit with readme"  # บันทึกประวัติ (ลงถังในเครื่อง)

git push -u origin main             # ดึงขึ้น GitHub ครั้งแรกพร้อมผูกความสัมพันธ์ (-u)
```

---

## Phase 3: การทำงานเป็นทีมและ Branching

### 5. กฎการตั้งชื่อ Branch (Semantic Naming)
แนะนำรูปแบบ: `ชื่อเล่น/ประเภทงาน/สิ่งที่ทำ`
*   `boom/feat/login-page` (ฟีเจอร์ใหม่)
*   `elon/fix/bug-css` (แก้บัค)
*   `mark/chore/update-deps` (งานบ้านทั่วไป)

### 6. สร้างและสลับ Branch
```bash
# สร้าง branch ใหม่จากจุดที่เรายืนอยู่
git checkout -b boom/feat/add-content

# ... แก้ไขไฟล์ ...
git add .
git commit -m "feat: add workshop content"
git push origin boom/feat/add-content  # ส่งงานขึ้นไปให้เพื่อนดู
```

### 7. การรวมงาน (Merge)
เมื่อคนอื่น (หรือเราเอง) ทำงานเสร็จแล้ว และต้องการเอามาประจบกันใน `main`
```bash
git checkout main                   # ย้ายกลับมาที่ main
git pull origin main                # ดึงงานใหม่ล่าสุดลงมาก่อน (กันพลาด)
git merge boom/feat/add-content     # รวมร่าง!
git push origin main                # ส่งงานที่รวมแล้วขึ้น GitHub
```

---

## Phase 4: การดูประวัติและเทคนิคเมื่อทำพลาด

### 8. ดูประวัติแบบสวยงาม (Graph Tree)
```bash
git log --oneline --graph --all     # ดูสายพานงานว่าใครแตกไปไหน รวมที่ไหน
```

### 9. เทคนิค "ทางลัดคนใจร้อน" (แทนการใช้ Reset)
ถ้าทำ branch ปัจจุบันพังจนกู่ไม่กลับ:
1.  ใช้ `git log` หาเลข Commit ID ที่ยังดีอยู่ (เช่น `abc1234`)
2.  สร้าง Branch ใหม่จากจุดนั้นเลย:
    ```bash
    git checkout -b new-start-branch abc1234
    ```
3.  ลบ Branch ที่พังทิ้งไป: `git branch -D branch-ที่-พัง`
*วิธีนี้ปลอดภัยกว่าการพยายามย้อนอดีต (Reset) เพราะประวัติงานจะไม่หายไปไหน*

---

## สรุปคำสั่งที่ใช้บ่อยในชีวิตจริง
*   `git status` : ฉันอยู่ที่ไหน มีอะไรแก้บ้าง?
*   `git pull` : ดึงงานเพื่อนมา
*   `git add .` : เก็บของลงตะกร้า
*   `git commit -m "..."` : แปะป้ายบันทึก
*   `git push` : ส่งของขึ้นหิ้ง
*   `git checkout -b ...` : เปิดร้านใหม่ (Work on new task)


# This is new content 
## test 123