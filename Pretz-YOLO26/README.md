# AI_YOLO

นายธนมินทร์ เปลี่ยนพร้อม 67543210032-8 

นางสาวรัฐจิกาลณ์ กวงคำ 67543210063-3

โปรเจกต์สำหรับการพัฒนาโมเดลปัญญาประดิษฐ์ด้วย **YOLO26** สำหรับตรวจจับและจำแนกวัตถุจากภาพ วิดีโอ และกล้องแบบ Real-time โดยใช้ **Ultralytics YOLO** ร่วมกับ **Label Studio** สำหรับสร้าง Dataset และกำหนด Bounding Box


drive สำหรับโหลด Video Dataset, Test image และ Video test
: https://drive.google.com/drive/folders/1ERlu3liw6lM2_fEC7ENjsPka92Qg8upJ?usp=sharing

---

## 📂 Project Structure

```text
AI_YOLO/
├── README.md
├── env/
├── dataset/
│   ├── images/
│   │   ├── train/
│   │   └── val/
│   ├── labels/
│   │   ├── train/
│   │   └── val/
│   ├── classes.txt
│   └── data.yaml
│
├── yolo26n.pt
├── data.yml
├── requirements.txt
│
├── 01-export_dataset.py
├── 02-train.py
├── 03-test_image.py
├── 04-test_video.py
└── 05-test-camera.py
```

โครงสร้างไฟล์หลักของโปรเจกต์ประกอบด้วยไฟล์สำหรับ Export Dataset, Training และการทดสอบโมเดลทั้งภาพ วิดีโอ และกล้อง

---

# 🏷️ Image Labeling

โปรเจกต์นี้ใช้ **Label Studio** สำหรับสร้าง Bounding Box และกำหนด Class ของวัตถุ

สามารถดาวน์โหลด Label Studio ได้จากเว็บไซต์ทางการ: [Label Studio](https://labelstud.io?utm_source=chatgpt.com)

จากนั้นทำตามขั้นตอน Quick Start ได้เลย แต่อย่าพึ่ง Launch Label studio ขึ้นมา ต้องเซ็ตระบบให้มันก่อน โดยเริ่มจากการเปิด Cmd ขึ้นมา สร้างโฟลเดอร์ และ env ให้เรียบร้อย โดยสร้างได้จากโค้ดนี้

---

# ⚙️ Installation

## 1. สร้าง Project Folder
เข้าไปในโฟลเดอร์เป้าหมายก่อน หรือถ้าไม่มีให้สร้างโฟลเดอร์ก่อนแล้วเข้าไปเพื่อสร้าง env แล้ว activate   
เปิด Command Prompt หรือ Terminal แล้วใช้คำสั่ง

```bash
mkdir AI_YOLO
cd AI_YOLO
python -m venv env
```

## 2. สร้าง Virtual Environment และ Activate Virtual Environment


### Windows

```bash
py -3 -m venv env
.\env\Scripts\activate.bat
```

### macOS

```bash
python3 -m venv env
source ./env/bin/activate
```

เมื่อ Activate สำเร็จ จะเห็นชื่อ

```text
(env)
```

---

## 2. Install Environment

```bash
pip install -U ultralytics
pip install opencv-python matplotlib
```

---

## 3. Install PyTorch สำหรับ NVIDIA GPU

หากใช้งาน **Windows + NVIDIA GPU + CUDA 12.1** สามารถติดตั้ง PyTorch ด้วยคำสั่ง

```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
```

> หากใช้งาน CPU หรือ macOS ไม่จำเป็นต้องใช้คำสั่ง CUDA นี้

---

## 4. ติดตั้ง Dependencies
ต่อไปจะต้องติดตั้ง Package python  โดยทำตามขั้นตอนนี้ได้เลย   

Upgrade pip:

```bash
python.exe -m pip install --upgrade pip
```

Install requirements (ดาวน์โหลด `requirements.txt` แล้ววางไว้ในโฟลเดอร์ของเราก่อน ที่เปิด env ไว้)

```bash
pip install -r requirements.txt
```

ตอนนี้เครื่องมือพร้อมแล้ว ต่อไปจะเป็นการทำ Image Labeling   


หมายเหตุ : clone github ของเราลงไปจะมีให้หมดแล้วพร้อมทดสอบได้เลย
---


## Run Dataset Conversion (ข้ามขั้นตอนนี้ไปได้เลย)

จากนั้นไปที่ cmd แล้วรันโค้ด python 01-export_dataset.py ถ้าขึ้นแบบนี้คือได้แล้ว และจะได้ โฟลเดอร์ dataset มาแล้ว   

```bash
python 01-export_dataset.py
```


---

# 🧠 Train YOLO26 (ข้ามขั้นตอนนี้ไปได้เลย)
โมเดลที่ใช้เริ่มต้นคือ

```text
yolo26n.pt
```

การ Train กำหนดค่าหลักดังนี้

```text
Epochs       : 50
Image Size   : 640
Optimizer    : MuSGD
Device       : 0
```

พร้อมใช้ Data Augmentation เช่น

```text
degrees      = 7.0
shear        = 5.0
perspective  = 0.001
fliplr       = 0.5
flipud       = 0.5
mosaic       = 0.1
mixup        = 0.1
close_mosaic = 10
```

สามารถดูและแก้ไขค่าต่าง ๆ ได้ใน

```text
02-train.py
```

---

## Run Training

```bash
python 02-train.py
```

เมื่อเทรนเสร็จจะได้หน้าตาแบบนี้

---

# 🧪 Test Model

## 1. Test Image

เปิดไฟล์

```text
03-test_image.py
```

แก้ชื่อไฟล์ภาพที่ต้องการทดสอบ

```python
results = model.predict("FILE_NAME", conf=0.01, save=True)
```

จากนั้นรัน

```bash
python 03-test_image.py
```

<img width="1107" height="830" alt="Screenshot 2026-10-06 022241" src="https://github.com/user-attachments/assets/70bc6864-1533-4fe0-8728-c122eb4315df" />


ผลลัพธ์จะถูกบันทึกโดยระบบ Ultralytics และสามารถดูภาพที่ตรวจจับแล้วได้


---

# 🎥 2. Test Video

ไฟล์ที่ใช้ทดสอบคือ

```text
04-test_video.py
```

กำหนดไฟล์วิดีโอ เช่น

```python
video_to_test = "video_candy1.MOV"
```

รันคำสั่ง

```bash
python 04-test_video.py
```

<img width="952" height="701" alt="image" src="https://github.com/user-attachments/assets/9f1ca5b8-7acc-454d-bf31-884d5f0662f1" />


ผลลัพธ์จะถูกบันทึกไว้ใน

```text
runs/detect/predict
```

---

# 📷 3. Test Camera

สำหรับการตรวจจับวัตถุแบบ Real-time ผ่านกล้อง Webcam ใช้ไฟล์

```text
05-test-camera.py
```

รันด้วย

```bash
python 05-test-camera.py
```

ระบบจะเปิดกล้องและแสดงผลการตรวจจับแบบ Real-time

<img width="795" height="642" alt="Screenshot 2026-10-06 023519" src="https://github.com/user-attachments/assets/b6880997-31a2-467b-b779-039ff0b7a9e2" />

กด

```text
q
```

เพื่อออกจากโปรแกรม

---

# ⚠️ Notes

* ต้องตรวจสอบ Path ในไฟล์ Python ให้ตรงกับตำแหน่งไฟล์จริงในเครื่อง
* `01-export_dataset.py` ต้องมีไฟล์ JSON ที่ Export จาก Label Studio อยู่ในโฟลเดอร์เดียวกัน
* ภาพที่ใช้ Label ต้องตรงกับภาพที่ระบุใน JSON
* ควรใช้ Class เฉพาะที่มีอยู่จริงใน Dataset
* ก่อน Train ควรตรวจสอบว่า `data.yml` ชี้ไปยัง Dataset ที่ถูกต้อง
* หากใช้ GPU ต้องตรวจสอบว่า PyTorch และ CUDA สามารถทำงานร่วมกับอุปกรณ์ของเครื่องได้
* หากไม่มี GPU สามารถปรับ `device` ในไฟล์ Training ให้เหมาะสมกับเครื่อง


