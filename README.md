# SodPhotobooth
# 📸 PhotoBooth System - Complete Documentation

ระบบโฟโต้บูทแบบครบวงจรที่ทำงานร่วมกับ Capture One พร้อมระบบ Socket.IO สำหรับการทำงานแบบ real-time

## � ภาพรวมระบบ

PhotoBooth System เป็นแอปพลิเคชันเว็บที่ออกแบบมาเพื่อ:
- **ตรวจจับรูปใหม่จาก Capture One อัตโนมัติ**
- **สร้างรูปโฟโต้บูทด้วยระบบ Frame และ Slot Mapping**
- **จัดการแกลเลอรี่รูปภาพแบบ real-time**
- **ให้บริการ Quick Session สำหรับงานถ่ายรูปเร่งด่วน**

---

## 🎯 ฟีเจอร์หลัก

### ✅ **Gallery System (ระบบแกลเลอรี่)**
- แสดงรูปภาพแบบ real-time จาก Capture One
- ระบบ thumbnail อัตโนมัติ
- ดาวน์โหลดรูปเดี่ยวหรือทั้งหมด
- ลบรูปและจัดการข้อมูล

### ✅ **Frame Management (ระบบจัดการเฟรม)**
- เลือกเฟรมรูปจากไลบรารี่
- แก้ไข Slot Mapping แบบ Visual
- สร้างและลบ Slots
- บันทึกการตั้งค่าเฟรม

### ✅ **PhotoBooth Creator (สร้างโฟโต้บูท)**
- ลากวางรูปใส่ Slots
- สร้างรูปโฟโต้บูทอัตโนมัติ
- Auto-fill รูปล่าสุด
- แสดงผลลัพธ์แบบ Preview
 - รองรับการใส่ข้อความคอมเมนต์ลงในเฟรมที่มี commentBox (เช่น เทมเพลต 3 ความคิดเห็น)

### ✅ **Quick Session (เซสชั่นด่วน)**
- ถ่ายรูปตามจำนวนที่กำหนด
- เลือกเฟรมและจัดเรียงรูปทันที
- สร้างโฟโต้บูทภายในเซสชั่น
- ระบบ Timer และ Progress

### ✅ **Admin Panel (ระบบจัดการ)**
- ตั้งค่าโฟลเดอร์ Capture One
- เปิด/ปิดการตรวจจับไฟล์
- Export รูปเป็น ZIP
- จัดการการตั้งค่าระบบ

---

## 🏗️ สถาปัตยกรรมระบบ

### **Backend Architecture**
```
Flask Application (main.py)
├── Socket.IO Server     # Real-time communication
├── Photo Handler        # File watching & processing
├── Frame Manager        # Frame templates & mapping
├── Session Manager      # Quick session handling
└── API Endpoints        # REST API services
```

### **Frontend Architecture**
```
Single Page Application (app.js)
├── Socket.IO Client     # Real-time updates
├── Photo Gallery        # Image display & management  
├── Frame Selector       # Frame selection interface
├── Mapping Editor       # Visual slot editing
├── Creator Interface    # Drag & drop photobooth creation
└── Session Modal        # Quick session workflow
```

---

## 🚀 การติดตั้งและใช้งาน

### **ความต้องการระบบ**
- Python 3.8+
- Flask 2.x
- Flask-SocketIO
- PIL (Pillow)
- Watchdog
- Capture One (สำหรับถ่ายรูป)

### **การติดตั้ง**
```bash
# 1. Clone repository
git clone <repository-url>
cd PTB-2

# 2. ติดตั้ง dependencies
pip install flask flask-socketio pillow watchdog

# 3. เริ่มระบบ
python main.py

# 4. เข้าใช้งาน
# เปิด browser: http://localhost:5000
```

### **การตั้งค่าเริ่มต้น**
1. **ตั้งค่าโฟลเดอร์**: ไปที่แท็บ "จัดการ" > กรอกโฟลเดอร์ Capture One
2. **เพิ่มเฟรม**: วางไฟล์เฟรมใน `/frames/` folder
3. **เริ่มใช้งาน**: คลิก "เริ่มดู" เพื่อเปิดการตรวจจับ

---

## 📁 โครงสร้างไฟล์

```
PTB-2/
├── main.py                 # 🔧 Flask server หลัก
├── config.json            # ⚙️ การตั้งค่าระบบ
├── templates/
│   └── index.html         # 🖥️ หน้าเว็บหลัก
├── static/
│   ├── js/
│   │   ├── app.js         # 💻 JavaScript หลัก
│   │   └── app_backup.js  # 💾 Backup file
│   └── css/
│       └── style.css      # 🎨 CSS styles
├── frames/
│   ├── templates.json     # 🖼️ Frame configurations
│   └── [frame-images]     # 🏞️ Frame image files
├── thumbnails/            # 🖼️ Generated thumbnails
└── exports/              # 📦 Exported files
```
---

## 🔧 API Documentation

### **Backend APIs (Flask Routes)**

#### **📸 Gallery APIs**

##### `GET /api/photos`
ดึงรายการรูปภาพทั้งหมด
```json
Response: {
  "photos": [
    {
      "filename": "IMG_001.jpg",
      "path": "/full/path/to/image.jpg",
      "size": 2048576,
      "created": "2025-08-11T10:30:00",
      "thumbnail": "/api/photos/IMG_001.jpg/thumbnail"
    }
  ]
}
```

##### `GET /api/photos/<filename>`
ดาวน์โหลดรูปต้นฉบับ

##### `GET /api/photos/<filename>/thumbnail`
ดึง thumbnail รูปภาพ (300x300px)

##### `DELETE /api/photos/<filename>`
ลบรูปภาพ
```json
Response: {"message": "ลบรูปสำเร็จ"}
```

##### `POST /api/photos/clear-all`
ลบรูปทั้งหมด
```json
Response: {"message": "ลบรูปทั้งหมดแล้ว"}
```

รายละเอียด: endpoint นี้จะลบไฟล์รูปทั้งหมดในโฟลเดอร์ processed_photos และลบ thumbnails ทั้งหมด พร้อมส่ง Socket.IO event ชื่อ `photos_cleared` ไปยังผู้ใช้

##### `POST /api/photos/export`
สร้างไฟล์ ZIP สำหรับดาวน์โหลด
```json
Response: {
  "download_url": "/exports/photos_2025-08-11.zip",
  "count": 25
}
```
หมายเหตุ: ระบบจะลบไฟล์ในโฟลเดอร์ exports อัตโนมัติตามค่า TTL ชั่วโมง (ค่าเริ่มต้น 72 ชั่วโมง) ตั้งค่าได้ที่ `config.exports_ttl_hours`

#### **🖼️ Frame Management APIs**

##### `GET /api/frames/templates`
ดึงข้อมูลเฟรมทั้งหมด
```json
Response: {
  "templates": {
    "blue_4pic": {
      "displayName": "Blue 4 Pictures",
      "background": "Blue_4Pic.jpg",
      "thumbnail": "Blue_4Pic.jpg",
      "slots": [
        {"x": 0.084, "y": 0.108, "width": 0.828, "height": 0.182},
        {"x": 0.084, "y": 0.298, "width": 0.828, "height": 0.182}
      ]
    }
  }
}
```

##### `GET /api/frames/current`
ดึงเฟรมที่เลือกปัจจุบัน
```json
Response: {"current": "blue_4pic"}
```

##### `POST /api/frames/current`
เลือกเฟรมใหม่
```json
Request: {"template_id": "pink_4pic"}
Response: {"message": "เลือกเฟรมสำเร็จ"}
```

##### `POST /api/frames/templates`
บันทึกการตั้งค่าเฟรม
```json
Request: {
  "templates": {...},
  "current_template": "blue_4pic",
  "version": 1
}
```

##### `GET /api/frames/preview/<filename>`
แสดงภาพตัวอย่างเฟรม

#### **🎯 PhotoBooth Creation APIs**

##### `POST /api/photobooth/create`
สร้างรูปโฟโต้บูท
```json
Request: {
  "template_id": "blue_4pic",
  "slot_assignments": {
    "0": "IMG_001.jpg",
    "1": "IMG_002.jpg",
    "2": "IMG_003.jpg",
    "3": "IMG_004.jpg"
  },
  "comment_text": "ยินดีด้วย! 11 ส.ค. 2025"
}
Response: {
  "status": "success",
  "message": "สร้างโฟโต้บูทสำเร็จ",
  "result_filename": "photobooth_20250811_103045.jpg",
  "composition_info": {
    "template": {...},
    "filled_slots": 4,
    "slots_count": 4,
    "comment_text": "ยินดีด้วย! 11 ส.ค. 2025"
  }
}
```

##### `POST /api/photobooth/auto-assign`
จัดเรียงรูปอัตโนมัติ
```json
Request: {"template_id": "blue_4pic"}
Response: {
  "assignments": {
    "0": "latest_photo1.jpg",
    "1": "latest_photo2.jpg"
  },
  "message": "จัดเรียงรูปอัตโนมัติแล้ว"
}
```

##### `GET /api/photobooth/info/<filename>`
ดึงข้อมูลโฟโต้บูท
```json
Response: {
  "filename": "photobooth_001.jpg",
  "template_id": "blue_4pic",
  "template_name": "Blue 4 Pictures",
  "created": "2025-08-11T10:30:45",
  "size": 3145728
}
```

#### **⚡ Quick Session APIs**

##### `POST /api/session/start`
เริ่มเซสชั่นถ่ายรูป
```json
Request: {"max_photos": 10}
Response: {
  "session_id": "session_20250811_103000",
  "max_photos": 10,
  "message": "เริ่มเซสชั่นแล้ว"
}
```

##### `POST /api/session/stop`
หยุดเซสชั่น
```json
Response: {"message": "หยุดเซสชั่นแล้ว"}
```

##### `GET /api/session/status`
ตรวจสอบสถานะเซสชั่น
```json
Response: {
  "active": true,
  "session_id": "session_20250811_103000",
  "max_photos": 10,
  "current_count": 3,
  "remaining_time": 300,
  "is_complete": false,
  "photos": [
    {"filename": "session_001.jpg", "captured_at": "2025-08-11T10:30:15"}
  ]
}
```

##### `POST /api/session/create-photobooth`
สร้างโฟโต้บูทจากเซสชั่น
```json
Request: {
  "template_id": "blue_4pic",
  "slot_assignments": {...},
  "session_id": "session_20250811_103000",
  "comment_text": "ข้อความในเซสชั่น"
}
```

#### **⚙️ System Management APIs**

##### `GET /api/status`
ดึงสถานะระบบ
```json
Response: {
  "watch_path": "/path/to/photos",
  "is_watching": true,
  "photos_count": 25,
  "last_scan": "2025-08-11T10:30:00",
  "system_status": "running"
}
```

##### `POST /api/scan`
สแกนโฟลเดอร์หารูปใหม่
```json
Response: {
  "new_photos": 3,
  "total_photos": 28,
  "message": "สแกนเสร็จแล้ว"
}
```

##### `POST /api/watch/toggle`
เปิด/ปิดการตรวจจับอัตโนมัติ
```json
Response: {
  "is_watching": true,
  "message": "เปิดการดูโฟลเดอร์แล้ว"
}
```

##### `POST /api/config/save-path`
บันทึกโฟลเดอร์ใหม่
```json
Request: {"path": "/new/photo/path"}
Response: {"message": "บันทึก path สำเร็จ"}
```

---

## 🌐 Socket.IO Events

### **Client Events (ส่งจาก Browser)**

##### `connect`
เชื่อมต่อกับ server
```javascript
socket.emit('connect');
```

##### `request_photos`
ขอรายการรูปภาพ
```javascript
socket.emit('request_photos');
```

##### `request_status`
ขอสถานะระบบ
```javascript
socket.emit('request_status');
```

### **Server Events (ส่งจาก Server)**

##### `photos_update`
อัพเดตรายการรูปภาพ
```javascript
socket.on('photos_update', function(data) {
    // data: {photos: [...], count: 25}
});
```

##### `new_photo`
แจ้งรูปใหม่
```javascript
socket.on('new_photo', function(data) {
    // data: {photo: {filename: "...", path: "..."}}
});
```

##### `photo_deleted`
แจ้งว่ารูปถูกลบ
```javascript
socket.on('photo_deleted', (data) => {
  // data: { filename: 'IMG_001.jpg' }
});
```

##### `photos_cleared`
แจ้งว่ารูปทั้งหมดถูกลบ
```javascript
socket.on('photos_cleared', (data) => {
  // data: { deleted: 25 }
});
```

---

## 🖼️ คุณภาพรูป: EXIF และการจัดวางแบบ Cover-Fit

- ระบบจะอ่าน EXIF Orientation (ถ้ามี) และหมุนรูปให้ถูกต้องทั้งในขั้นตอนการสร้าง thumbnail และการประกอบโฟโต้บูท
- การวางรูปลงในช่อง (slot) ใช้โหมด "cover" โดยจะขยายรูปให้เต็มช่องและครอปส่วนเกินตรงกลาง เพื่อป้องกันการยืดรูปผิดสัดส่วน

---

## ⚙️ ค่าตั้งค่าเพิ่มเติมใน config.json

- `exports_ttl_hours` (number): ชั่วโมงที่ไฟล์ ZIP ในโฟลเดอร์ exports จะถูกเก็บไว้ก่อนลบอัตโนมัติ (ค่าเริ่มต้น 72)

##### `status_update`
อัพเดตสถานะระบบ
```javascript
socket.on('status_update', function(data) {
    // data: {watch_path: "...", is_watching: true, ...}
});
```

##### `session_photo_captured`
แจ้งรูปใหม่ในเซสชั่น
```javascript
socket.on('session_photo_captured', function(data) {
    // data: {photo: {...}, session_id: "...", count: 5}
});
```

##### `session_complete`
แจ้งเซสชั่นเสร็จ
```javascript
socket.on('session_complete', function(data) {
    // data: {photos: [...], session_id: "...", total_count: 10}
});
```

---

## 💻 Frontend Functions (app.js)

### **🏗️ Core Functions**

##### `init()`
เริ่มต้นระบบ
- เชื่อมต่อ Socket.IO
- โหลดข้อมูลเริ่มต้น
- ผูก event listeners

##### `initSocketIO()`
ตั้งค่า Socket.IO
- เชื่อมต่อ server
- ผูก event handlers
- จัดการการเชื่อมต่อใหม่

##### `bindEvents()`
ผูก event listeners ทั้งหมด
- Button clicks
- Form submissions
- Tab switching

### **📸 Gallery Functions**

##### `loadPhotos()`
โหลดรายการรูปภาพ
```javascript
async loadPhotos() {
    // ดึงข้อมูลจาก /api/photos
    // อัพเดต UI
    // แสดงใน Gallery
}
```

##### `displayPhotos(photos)`
แสดงรูปในแกลเลอรี่
- สร้าง photo items
- จัดการ lazy loading
- เพิ่ม action buttons

##### `downloadPhoto(filename)`
ดาวน์โหลดรูป
```javascript
async downloadPhoto(filename) {
    // สร้าง download link
    // เรียก browser download
}
```

##### `deletePhoto(filename)`
ลบรูป
```javascript
async deletePhoto(filename) {
    // ยืนยันการลบ
    // เรียก DELETE /api/photos/<filename>
    // อัพเดต UI
}
```

### **🖼️ Frame Management Functions**

##### `loadFrameData()`
โหลดข้อมูลเฟรม
```javascript
async loadFrameData() {
    await this.loadTemplates();
    await this.loadCurrentTemplate();
}
```

##### `renderFrameSelector()`
แสดงตัวเลือกเฟรม
- สร้าง frame items
- แสดง thumbnails
- เพิ่ม selection handlers

##### `selectFrame(templateId)`
เลือกเฟรม
```javascript
async selectFrame(templateId) {
    // เรียก POST /api/frames/current
    // อัพเดต UI selection
    // โหลด preview ถ้าอยู่ในโหมดแก้ไข
}
```

##### `loadFramePreview()`
โหลด preview เฟรมสำหรับแก้ไข
- แสดงภาพเฟรม
- สร้าง slot overlays
- เปิดโหมดแก้ไข

##### `renderSlotsOverlay(slots)`
แสดง slot overlays บนเฟรม
```javascript
renderSlotsOverlay(slots) {
    // คำนวณตำแหน่งจากข้อมูล slot
    // สร้าง visual indicators
    // เพิ่ม click handlers
}
```

##### `addSlot()`
เพิ่ม slot ใหม่
- สร้าง slot ใหม่ด้วยค่า default
- อัพเดต UI
- บันทึกการเปลี่ยนแปลง

##### `removeSlot(index)`
ลบ slot
```javascript
removeSlot(index) {
    // ยืนยันการลบ
    // ลบจาก template data
    // อัพเดต UI
}
```

##### `saveMapping()`
บันทึกการตั้งค่าเฟรม
```javascript
async saveMapping() {
    // เรียก POST /api/frames/templates
    // บันทึกการเปลี่ยนแปลง
    // แสดงผลลัพธ์
}
```

### **🎨 PhotoBooth Creator Functions**

##### `loadCreatorData()`
โหลดข้อมูลสำหรับ Creator
```javascript
async loadCreatorData() {
    await this.loadTemplates();
    await this.populateCreatorFrameSelect();
    await this.loadCreatorPhotos();
}
```

##### `renderCreatorPhotos()`
แสดงรูปสำหรับเลือก
- สร้าง draggable photo items
- เพิ่ม drag event handlers
- จัดการ selection state

##### `loadCreatorFrame()`
โหลดเฟรมสำหรับสร้างโฟโต้บูท
```javascript
async loadCreatorFrame() {
    // ดึง template จาก selection
    // สร้าง slot assignment UI
    // เคลียร์ assignments เดิม
}
```

##### `assignPhotoToSlot(filename, slotIndex)`
กำหนดรูปเข้า slot
```javascript
assignPhotoToSlot(filename, slotIndex) {
    // บันทึก assignment
    // อัพเดต slot display
    // อัพเดต photo usage state
}
```

##### `autoFillSlots()`
จัดเรียงรูปอัตโนมัติ
```javascript
async autoFillSlots() {
    // เรียก POST /api/photobooth/auto-assign
    // อัพเดต UI ด้วยผลลัพธ์
}
```

##### `generatePhotoBooth()`
สร้างโฟโต้บูท
```javascript
async generatePhotoBooth() {
    // ตรวจสอบความครบถ้วน
    // เรียก POST /api/photobooth/create
    // แสดงผลลัพธ์
    // อัพเดต Gallery
}
```

### **⚡ Quick Session Functions**

##### `showQuickSessionModal()`
แสดง modal เซสชั่น
```javascript
async showQuickSessionModal() {
    // โหลดการตั้งค่า
    // เตรียม UI
    // แสดง modal
}
```

##### `startQuickSession()`
เริ่มเซสชั่น
```javascript
async startQuickSession() {
    // เรียก POST /api/session/start
    // เริ่ม timers
    // อัพเดต UI state
}
```

##### `stopQuickSession()`
หยุดเซสชั่น
```javascript
async stopQuickSession() {
    // เรียก POST /api/session/stop
    // หยุด timers
    // อัพเดต UI state
}
```

##### `updateSessionPhotos(photos)`
อัพเดตรูปในเซสชั่น
```javascript
updateSessionPhotos(photos) {
    // อัพเดต session photo slots
    // แสดงภาพ thumbnails
    // อัพเดต progress
}
```

##### `onSessionComplete(photos)`
จัดการเซสชั่นเสร็จ
```javascript
async onSessionComplete(photos) {
    // หยุด polling
    // โหลดเฟรมสำหรับเลือก
    // แสดง frame selector
}
```

##### `createSessionPhotoBooth()`
สร้างโฟโต้บูทจากเซสชั่น
```javascript
async createSessionPhotoBooth() {
    // ตรวจสอบการจัดเรียง
    // เรียก POST /api/session/create-photobooth
    // แสดงผลลัพธ์
}
```

### **⚙️ Admin Functions**

##### `loadAdminData()`
โหลดข้อมูลจัดการ
- โหลดการตั้งค่า
- อัพเดต form fields
- แสดงสถานะปัจจุบัน

##### `savePath()`
บันทึกโฟลเดอร์ใหม่
```javascript
async savePath() {
    // ตรวจสอบ path
    // เรียก POST /api/config/save-path
    // อัพเดตการตั้งค่า
}
```

##### `toggleWatch()`
เปิด/ปิดการตรวจจับ
```javascript
async toggleWatch() {
    // เรียก POST /api/watch/toggle
    // อัพเดต button state
    // แสดงสถานะใหม่
}
```

##### `manualScan()`
สแกนโฟลเดอร์ด้วยตนเอง
```javascript
async manualScan() {
    // แสดง loading
    // เรียก POST /api/scan
    // อัพเดตผลลัพธ์
}
```

##### `exportPhotos()`
ส่งออกรูปเป็น ZIP
```javascript
async exportPhotos() {
    // เรียก POST /api/photos/export
    // ดาวน์โหลดไฟล์
    // แสดงผลลัพธ์
}
```

### **🔧 Utility Functions**

##### `formatFileSize(bytes)`
แปลงขนาดไฟล์
```javascript
formatFileSize(bytes) {
    // แปลง bytes เป็น KB/MB/GB
    // return formatted string
}
```

##### `formatTime(seconds)`
แปลงเวลา
```javascript
formatTime(seconds) {
    // แปลงวินาทีเป็น MM:SS
    // return formatted time string
}
```

##### `showNotification(message, type)`
แสดง notification
```javascript
showNotification(message, type) {
    // แสดง toast notification
    // จัดการ styling ตาม type
    // auto-hide หลังจากเวลาที่กำหนด
}
```

##### `bindElement(id, handler)`
ผูก event handler
```javascript
bindElement(id, handler) {
    // หา element จาก id
    // ผูก click event
    // จัดการ error ถ้าไม่เจอ element
}
```

---

## 🎛️ การใช้งานแต่ละโหมด

### **📸 Gallery Mode**
1. **ดูรูป**: คลิกรูปเพื่อดูขนาดเต็ม
2. **ดาวน์โหลด**: คลิกปุ่ม "ดาวน์โหลด" ใต้รูป
3. **ลบรูป**: คลิกปุ่ม "ลบ" และยืนยัน
4. **รีเฟรช**: ระบบรีเฟรชอัตโนมัติ หรือคลิก "รีเฟรช"

### **🖼️ Frame Management Mode**
1. **เลือกเฟรม**: คลิกเฟรมที่ต้องการจาก Frame Selector
2. **แก้ไข Slots**: คลิก "โหลดเฟรม" เพื่อเข้าโหมดแก้ไข
3. **ปรับตำแหน่ง**: กรอกค่า x, y, width, height ใน form
4. **เพิ่ม/ลบ Slot**: ใช้ปุ่ม "เพิ่ม Slot" หรือ "ลบ" ในแต่ละ row
5. **บันทึก**: คลิก "บันทึก Mapping" เมื่อแก้ไขเสร็จ

### **🎨 PhotoBooth Creator Mode**
1. **เลือกเฟรม**: เลือกจาก dropdown "เลือกเฟรม"
2. **โหลดเฟรม**: คลิก "โหลดเฟรม" เพื่อแสดง Slots
3. **จัดเรียงรูป**: ลากรูปจาก Photo Selector ไปยัง Slots
4. **Auto-fill**: คลิก "ใส่รูปอัตโนมัติ" สำหรับรูปล่าสุด
5. **สร้าง**: คลิก "สร้างโฟโต้บูท" เมื่อจัดเรียงเสร็จ
6. **ดูผลลัพธ์**: ตรวจสอบใน Preview และดาวน์โหลดได้

### **⚡ Quick Session Mode**
1. **เริ่มเซสชั่น**: คลิก "เซสชั่นด่วน" เพื่อเปิด modal
2. **ตั้งค่า**: กำหนดจำนวนรูปที่ต้องการถ่าย
3. **เริ่มถ่าย**: คลิก "เริ่มเซสชั่น"
4. **ถ่ายรูป**: ใช้ Capture One ถ่ายรูปปกติ
5. **ติดตาม**: ดูสถานะและรูปที่ถ่ายแล้วใน modal
6. **เลือกเฟรม**: เมื่อถ่ายครบแล้ว เลือกเฟรมที่ต้องการ
7. **จัดเรียง**: ลากรูปใส่ Slots ตามต้องการ
8. **สร้างโฟโต้บูท**: คลิก "สร้างโฟโต้บูท" เพื่อดูผลลัพธ์

### **⚙️ Admin Mode**
1. **ตั้งค่าโฟลเดอร์**: กรอก path เต็มของโฟลเดอร์ Capture One
2. **ใช้โฟลเดอร์วันนี้**: คลิกเพื่อสร้าง path ตามวันที่
3. **บันทึก**: คลิก "บันทึก" เพื่อใช้ path ใหม่
4. **เริ่ม/หยุดดู**: คลิก "เริ่มดู" หรือ "หยุดดู" การตรวจจับ
5. **สแกนใหม่**: คลิก "สแกนใหม่" เพื่อหารูปใหม่
6. **ส่งออก**: คลิก "ส่งออกรูป" เพื่อดาวน์โหลด ZIP
7. **ลบทั้งหมด**: คลิก "ลบรูปทั้งหมด" และยืนยัน

---

## 🔍 Troubleshooting

### **ปัญหาที่พบบ่อย**

#### **❌ รูปไม่แสดงใน Gallery**
```
สาเหตุที่เป็นไปได้:
1. โฟลเดอร์ path ไม่ถูกต้อง
2. ไม่ได้เปิดการตรวจจับ (Watch)
3. ไฟล์ไม่ใช่รูปภาพที่รองรับ

การแก้ไข:
- ตรวจสอบ path ในแท็บ "จัดการ"
- คลิก "เริ่มดู" เพื่อเปิดการตรวจจับ
- คลิก "สแกนใหม่" เพื่อหารูปด้วยตนเอง
```

#### **❌ Socket.IO ไม่เชื่อมต่อ**
```
อาการ: ไม่มีการอัพเดต real-time

การแก้ไข:
- ตรวจสอบ console ใน browser (F12)
- รีสตาร์ท Flask server
- ตรวจสอบ firewall/proxy settings
```

#### **❌ ไม่สามารถสร้างโฟโต้บูทได้**
```
สาเหตุที่เป็นไปได้:
1. ไฟล์เฟรมเสียหาย
2. รูปที่เลือกไม่พบ
3. ขนาดรูปใหญ่เกินไป

การแก้ไข:
- ตรวจสอบไฟล์เฟรมใน /frames/
- ลองใช้รูปที่เล็กกว่า
- ตรวจสอบ console error
```

#### **❌ Quick Session ไม่ทำงาน**
```
การแก้ไข:
- ตรวจสอบการเชื่อมต่อ Socket.IO
- รีสตาร์ทเซสชั่นใหม่
- ตรวจสอบ session timeout settings
```

### **Debug Mode**
เปิด debug โดยแก้ไข `main.py`:
```python
if __name__ == '__main__':
    socketio.run(app, debug=True, host='0.0.0.0', port=5000)
```

---

## 🚧 Development Notes

### **การเพิ่มเฟรมใหม่**

1. **เพิ่มไฟล์รูปเฟรม** ใน `/frames/` folder
2. **อัพเดต `frames/templates.json`**:
```json
{
  "templates": {
    "new_frame_id": {
      "displayName": "ชื่อเฟรมใหม่",
      "background": "new_frame.jpg",
      "thumbnail": "new_frame.jpg",
      "slots": [
        {"x": 0.1, "y": 0.1, "width": 0.3, "height": 0.2}
      ]
    }
  },
  "current_template": "new_frame_id",
  "version": 1
}
```

### **การปรับแต่ง Slot Mapping**

Slot ใช้ระบบพิกัดแบบ relative (0.0 - 1.0):
- `x, y`: ตำแหน่งมุมซ้ายบน
- `width, height`: ขนาดของ slot
- พิกัด (0, 0) = มุมซ้ายบนของเฟรม
- พิกัด (1, 1) = มุมขวาล่างของเฟรม

### **การเพิ่ม API Endpoint ใหม่**

1. **เพิ่มใน `main.py`**:
```python
@app.route('/api/new-feature', methods=['POST'])
def new_feature():
    try:
        # ประมวลผล request
        result = process_feature(request.json)
        return jsonify({"status": "success", "data": result})
    except Exception as e:
        return jsonify({"error": str(e)}), 500
```

2. **เพิ่มใน `app.js`**:
```javascript
async callNewFeature(data) {
    try {
        const response = await fetch('/api/new-feature', {
            method: 'POST',
            headers: {'Content-Type': 'application/json'},
            body: JSON.stringify(data)
        });
        return await response.json();
    } catch (error) {
        console.error('Error:', error);
        this.showNotification('เกิดข้อผิดพลาด', 'error');
    }
}
```

---

## 📋 Configuration Reference

### **config.json Parameters**

```json
{
  "watch_path": "C:/Path/To/Photos",
  "processed_folder": "./processed_photos",
  "auto_scan": true,
  "auto_watch": false,
  "file_types": [".jpg", ".jpeg", ".png", ".tiff", ".cr2"],
  "thumbnail_size": [300, 300],
  "max_photos_display": 50,
  "scan_interval": 10,
  "session_timeout": 1800,
  "quick_session": {
    "max_photos": 20,
    "timeout": 1800,
    "auto_create": false
  }
}
```

#### **คำอธิบาย Parameters**
- `watch_path`: โฟลเดอร์หลักที่ต้องการติดตาม
- `auto_scan`: เปิดการสแกนอัตโนมัติเมื่อเริ่มระบบ
- `auto_watch`: เปิดการตรวจจับไฟล์อัตโนมัติ
- `file_types`: ประเภทไฟล์ที่รองรับ
- `thumbnail_size`: ขนาด thumbnail [width, height]
- `max_photos_display`: จำนวนรูปสูงสุดที่แสดงใน gallery
- `session_timeout`: timeout ของเซสชั่น (วินาที)

---

## 🔐 Security & Performance

### **Security Considerations**
- ระบบอ่านได้เฉพาะโฟลเดอร์ที่กำหนด
- ไม่อนุญาตให้เขียนไฟล์นอกโฟลเดอร์ที่กำหนด
- ตรวจสอบประเภทไฟล์ก่อนประมวลผล
- ใช้งานใน local network เท่านั้น

### **Performance Optimization**
- **Image Processing**: ใช้ thumbnail สำหรับ gallery
- **Socket.IO**: จำกัด frequency ของ events
- **Memory Management**: ล้าง intervals และ timeouts อัตโนมัติ
- **Caching**: thumbnail caching และ template caching

---

## 📞 Support & Updates

### **การรายงานปัญหา**
1. ตรวจสอบ console ใน browser (F12)
2. ดู log ใน terminal ที่รัน Flask
3. ลองทำตาม troubleshooting guide
4. รีสตาร์ทระบบและลองใหม่

### **การอัพเดต**
- เก็บ backup ของ `config.json` และ `frames/templates.json`
- ปรับปรุงโค้ดใน `main.py` และ `app.js`
- ทดสอบการทำงานหลังอัพเดต

---

## 📄 License & Credits

### **License**
MIT License - ดูไฟล์ [LICENSE](LICENSE) สำหรับรายละเอียด

### **Dependencies**
- **Flask Framework**: Web application framework
- **Socket.IO**: Real-time communication
- **Pillow (PIL)**: Image processing
- **Watchdog**: File system monitoring

### **Acknowledgments**
- **Capture One**: Professional photography software integration
- **Modern Web Technologies**: HTML5, CSS3, ES6 JavaScript

---

*📖 เอกสารนี้ครอบคลุมการใช้งานและพัฒนาระบบ PhotoBooth อย่างครบถ้วน สำหรับคำถามหรือการสนับสนุนเพิ่มเติม สามารถอ้างอิงจากเอกสาร API และ function documentation ข้างต้น*

---
