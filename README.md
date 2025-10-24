# Sepidar Python SDK | اس‌دی‌کی پایتون برای API سپیدار

[![Python Version](https://img.shields.io/badge/python-3.7+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Contributions Welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat)](https://github.com/YOUR_USERNAME/YOUR_REPO/issues)

---

**English:** An unofficial, community-driven Python SDK designed to streamline interactions with the Sepidar System API (targeting v111+). Provides a clean, Pythonic interface for integrating Sepidar accounting and ERP functionalities.  

**فارسی:** یک SDK غیررسمی پایتون، هدایت شده توسط جامعه توسعه‌دهندگان، برای ساده‌سازی تعامل با API سپیدار سیستم (نسخه ۱۱۱ به بالا). ارائه رابط کاربری تمیز و پایتونیک برای ادغام قابلیت‌های حسابداری و ERP سپیدار.

---

## ✨ Features | ویژگی‌ها

- **Core Configuration (`Configuration.py`):** Manage base API URL and generation version easily.  
  *پیکربندی مرکزی: مدیریت آسان آدرس پایه API و نسخه Generation.*

- **Cryptography Utilities (`CryptoHelper.py`):** Built-in helpers for AES/RSA encryption and MD5 hashing.  
  *ابزارهای رمزنگاری: توابع کمکی داخلی برای رمزنگاری/رمزگشایی AES، رمزنگاری RSA و هش MD5.*

- **Device Registration (`DevicesService.py`):** Initial device registration and public key retrieval.  
  *ثبت دستگاه: مدیریت فرآیند ثبت اولیه و دریافت کلید عمومی.*

- **User Authentication (`UsersService.py`):** Login management and token handling.  
  *احراز هویت کاربر: مدیریت ورود و توکن برای درخواست‌های احراز هویت شده.*

- **Modular Service Structure:** Easily extendable with new services (e.g., `ItemsService.py`).  
  *ساختار سرویس ماژولار: افزودن سرویس‌های جدید به راحتی.*

- **Error Handling:** Basic exception handling for API errors.  
  *مدیریت خطا: مدیریت خطاهای اولیه برای ارتباط با API.*

---

## 📋 Requirements | نیازمندی‌ها

- Python 3.7+
- `requests >= 2.20.0`
- `pycryptodome >= 3.10.0`

---

## 🚀 Installation | نصب

```bash
pip install requests pycryptodome
````

*(Optional if packaged on PyPI):*

```bash
# pip install sepidar-python-sdk
```

---

## ⚙️ Configuration | پیکربندی

Before using the SDK, set these configuration values (shown in `main.py` example):

* `BASE_URL`: Your Sepidar API base URL (e.g., '[http://localhost:7373/](http://localhost:7373/)')
* `GENERATION_VERSION`: Target API version (e.g., '111')
* `REGISTRATION_CODE`: 8-character device registration code
* `USERNAME`: Sepidar username
* `PASSWORD`: User password

---

## ⚡ Quick Start | شروع سریع

```python
# main.py
from Configuration import Configuration
from DevicesService import DevicesService
from ItemsService import ItemsService
from UsersService import UsersService
import json

BASE_URL = 'http://localhost:7373/'
GENERATION_VERSION = '111'
REGISTRATION_CODE = 'YOUR_REG_CODE'
USERNAME = 'YOUR_USERNAME'
PASSWORD = 'YOUR_PASSWORD'

def main():
    try:
        # 1. Initialize Configuration
        config = Configuration(BASE_URL, GENERATION_VERSION)

        # 2. Register Device
        device = DevicesService(config, REGISTRATION_CODE)
        device.register()
        print(f"✅ Device registered: {device.DeviceName}")

        # 3. Login User
        user = UsersService(device)
        user.login(USERNAME, PASSWORD)
        print(f"✅ User '{user.UserTitle}' logged in.")

        # 4. Fetch Items
        item_service = ItemsService(user)
        items = item_service.get_items()
        print("✅ Items fetched successfully.")

        # Sample print
        if items and isinstance(items, list):
            print(json.dumps(items[:2], indent=2, ensure_ascii=False))
        elif items:
            print(json.dumps(items, indent=2, ensure_ascii=False))
        else:
            print("No items found.")

    except Exception as e:
        print(f"\n❌ Error: {e}")

if __name__ == '__main__':
    main()
```

---

## 📚 API Documentation | مستندات API

* **Unofficial v111 Docs (Recommended)**
  Complete endpoint, parameter, and data model documentation.

* **Official v110 Docs**
  [Sepidar Official Documentation (v110)](link)

---

## 🏗️ How it Works | نحوه کارکرد

1. **Configuration:** Set base URL and API version.
2. **Device Registration:** Initial handshake using AES/RSA.
3. **Authentication:** Login with MD5-hashed password, get token.
4. **Service Calls:** Use token to interact with endpoints.

---

## 🤝 Contributing | مشارکت

Contributions, issues, and feature requests welcome! Please follow the [Code of Conduct](link).
*از مشارکت و گزارش مشکلات استقبال می‌شود. لطفاً قواعد پروژه را رعایت کنید.*

---

## 📜 License | مجوز

MIT License. See LICENSE file for details.
*این پروژه تحت مجوز MIT منتشر شده است.*

---

Maintained by **Behnam Pourjanali**
