# Announcer — Standalone Medic Tool

เครื่องมือแยกสำหรับแพทย์ BAD TOWN — ส่งประกาศ, จัดการ Blacklist, และจับเวลากลุ่ม  
ใช้งานได้อิสระโดยไม่ต้องรัน `fivem_bot.py`

---

## รันโปรแกรม

```
python announcer.py
```

ต้องการ Python 3.9+ และ dependency เดียวกับ `fivem_bot`:

```
pip install pywin32 keyboard
```

---

## หน้าต่าง

หน้าต่างจะลอยอยู่บน FiveM (`-topmost`) ลากย้ายตำแหน่งได้ด้วยเมาส์  
กด **✕** หรือปิดหน้าต่างเพื่อออกจากโปรแกรม

---

## Panel 1 — ANNOUNCE

ปุ่ม preset จาก `config.json → ANNOUNCE.PRESETS`

- กดปุ่มเพื่อส่งข้อความเข้าแชทเกมทันที
- **BL Preset** (ปุ่มสีแดง) — เปิด dialog ให้กรอกชื่อผู้เล่นและค่าปรับ ก่อนส่งประกาศ `/amd`

---

## Panel 2 — TIMERS

จับเวลากลุ่มหลายรายการพร้อมกัน

| ปุ่ม | การทำงาน |
|------|-----------|
| **+** | เพิ่ม timer ใหม่ |
| **🗑** | ลบ timer ที่ `DONE` ทั้งหมด |
| **×** | ลบ timer รายการนั้น |

**Dialog เพิ่ม Timer:**
- ชื่อ/กลุ่ม — ชื่อที่แสดงใน list
- Zone — `GREEN / ORANGE / RED / HARD / —`
- เวลา — 5 / 10 / 15 / 20 นาที

Progress bar เปลี่ยนสี: 🟢 เหลืออยู่มาก → 🟡 ครึ่งทาง → 🔴 ใกล้หมด

---

## Panel 3 — BLACKLIST

จัดการรายชื่อ BL พร้อมส่งประกาศอัตโนมัติ

### เพิ่มรายชื่อ ( + )
1. กรอกชื่อผู้เล่น
2. เลือกประเภท: **ประชาชน** หรือ **แก๊ง/ครอบครัว** (ราคาต่างกัน)
3. ติ๊กเลือกข้อหา (คำนวณยอดรวมอัตโนมัติ)
4. กด **ยืนยัน** → เพิ่มใน BL + ส่งประกาศเข้าเกมทันที

### การดำเนินการในแต่ละรายการ
| ปุ่ม | การทำงาน |
|------|-----------|
| `[สถานะ]` | คลิกเพื่อเปลี่ยนสถานะถัดไป |
| **✎** | แก้ไขหมายเหตุ |
| **×** | เปิด dialog ปลด BL |

**สถานะ:** รอดำเนินการ → ติดต่อแล้ว → รอจ่าย → ปฏิเสธ → จ่ายแล้ว

### ปลด Blacklist ( × )
- **จ่ายเงินแล้ว** — ลบ + ส่งประกาศปลด BL
- **ความผิดพลาด** — ลบ + ส่งประกาศปลด BL
- **ลบเงียบๆ** — ลบโดยไม่ส่งประกาศ

### ค้นหา 🔍
พิมพ์ชื่อในช่องค้นหาเพื่อกรองรายการแบบ real-time

### Export 📋
คัดลอกรายชื่อ BL ทั้งหมดเป็นข้อความลงคลิปบอร์ด

---

## config.json

ส่วนที่ Announcer ใช้:

```json
"ANNOUNCE": {
  "CHAT_KEY": "t",
  "PRESETS": [
    { "label": "ชื่อปุ่ม", "text": "ข้อความที่ส่ง" }
  ],
  "BL_SUFFIX": " โดยแพทย์จะไม่ทำการรักษา...",
  "BL_PRESETS": [
    { "label": "ชื่อปุ่ม", "reason": "เหตุผล", "default_amount": 300000 }
  ]
}
```

`CHAT_KEY` คือ key เปิดแชทในเกม (default `t`)

---

## ไฟล์ที่เกี่ยวข้อง

| ไฟล์ | หน้าที่ |
|------|---------|
| `announcer.py` | โปรแกรมหลัก (รันแยกหรือ import ก็ได้) |
| `blacklist.py` | จัดการข้อมูล BL + บันทึก `logs/blacklist.json` |
| `multi_timer.py` | จัดการ timer หลายรายการ |
| `bot_logger.py` | Logging → `logs/announce_YYYYMMDD.log` |
| `config.json` | ตั้งค่า preset, chat key, window class |

---

## การใช้งานร่วมกับ fivem_bot

`announcer.py` สามารถ import ได้โดยไม่เปิด UI:

```python
from announcer import Announcer

announcer = Announcer(get_hwid=lambda: hwid, chat_key="t")
announcer.set_root(lambda: root)   # ส่ง tkinter root เพื่อ clipboard
announcer.send("ข้อความ")
```

`fivem_bot.py` ใช้วิธีนี้ — UI จะไม่เปิดเมื่อ import
