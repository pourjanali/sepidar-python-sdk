🐍 Sepidar Python SDK | اس‌دی‌کی پایتون برای API سپیدار
English: An unofficial, community-driven Python SDK designed to streamline interactions with the Sepidar System API (targeting v111+). This library aims to provide a clean, Pythonic interface for developers integrating Sepidar's accounting and ERP functionalities into their applications.

فارسی: یک SDK غیررسمی پایتون که توسط جامعه توسعه‌دهندگان هدایت می‌شود و برای ساده‌سازی تعامل با API سپیدار سیستم (با هدف پشتیبانی از نسخه ۱۱۱ به بالا) طراحی شده است. هدف این کتابخانه ارائه یک رابط کاربری تمیز و پایتونیک برای توسعه‌دهندگانی است که می‌خواهند قابلیت‌های حسابداری و ERP سپیدار را در برنامه‌های خود ادغام کنند.

✨ Features | ویژگی‌ها
-------------------
* **Core Configuration (`Configuration.py`):** Easily manage base API URL and generation version.
    * **پیکربندی مرکزی:** مدیریت آسان آدرس پایه API و نسخه Generation.
* **Cryptography Utilities (`CryptoHelper.py`):** Built-in helpers for required AES encryption/decryption, RSA encryption, and MD5 hashing.
    * **ابزارهای رمزنگاری:** توابع کمکی داخلی برای رمزنگاری/رمزگشایی AES، رمزنگاری RSA و هش MD5 مورد نیاز API.
* **Device Registration (`DevicesService.py`):** Handles the initial device registration flow and public key retrieval.
    * **ثبت دستگاه:** مدیریت فرآیند ثبت اولیه دستگاه و دریافت کلید عمومی.
* **User Authentication (`UsersService.py`):** Manages user login via username/password and subsequent token handling for authenticated requests.
    * **احراز هویت کاربر:** مدیریت ورود کاربر با نام کاربری/رمز عبور و مدیریت توکن برای درخواست‌های احراز هویت شده بعدی.
* **Modular Service Structure:** Easily extendable with new services (e.g., `ItemsService.py` provided as a basic example).
    * **ساختار سرویس ماژولار:** قابلیت گسترش آسان با افزودن سرویس‌های جدید (مانند `ItemsService.py` که به عنوان نمونه اولیه ارائه شده است).
* **Error Handling:** Basic exception handling for API communication errors.
    * **مدیریت خطا:** مدیریت خطاهای اولیه برای ارتباط با API.

📋 Requirements | نیازمندی‌ها
-------------------------
* Python 3.7+
* Required Libraries:
    * `requests >= 2.20.0`
    * `pycryptodome >= 3.10.0`

🛠️ Installation | نصب
---------------------
Install the necessary libraries using pip:
برای نصب کتابخانه‌های مورد نیاز از pip استفاده کنید:
```bash
pip install requests pycryptodome
````

(Alternatively, if you package this SDK for PyPI):
(یا اگر این SDK را به عنوان پکیج در PyPI منتشر کردید):

```bash
# pip install sepidar-python-sdk # Uncomment and adjust if published
```

## ⚙️ Configuration | پیکربندی

Before using the SDK, set the following configuration values according to your Sepidar environment. These are demonstrated in the `main.py` example:
قبل از استفاده، مقادیر پیکربندی زیر را مطابق با محیط سپیدار خود تنظیم کنید. این مقادیر در مثال `main.py` نشان داده شده‌اند:

  * **`BASE_URL`**: The base URL of your Sepidar API instance (e.g., `'http://localhost:7373/'`).
      * آدرس پایه نمونه API سپیدار شما.
  * **`GENERATION_VERSION`**: The target API version (e.g., `'111'`).
      * نسخه API هدف.
  * **`REGISTRATION_CODE`**: Your unique 8-character device registration code.
      * کد ثبت ۸ کاراکتری منحصر به فرد دستگاه شما.
  * **`USERNAME`**: Sepidar username.
      * نام کاربری سپیدار.
  * **`PASSWORD`**: User's password.
      * رمز عبور کاربر.

## 🚀 Quick Start | شروع سریع

This example demonstrates the basic workflow: device registration, user login, and fetching items.
مثال زیر روند اصلی کار را نشان می‌دهد: ثبت دستگاه، ورود کاربر و دریافت لیست کالاها.

```python
# main.py
from Configuration import Configuration
from DevicesService import DevicesService
from ItemsService import ItemsService
from UsersService import UsersService
import json # For pretty printing

# --- Configuration Values ---
# --- مقادیر پیکربندی ---
BASE_URL = 'http://localhost:7373/'      # Replace with your Sepidar server URL | آدرس سرور سپیدار خود را وارد کنید
GENERATION_VERSION = '111'           # Target API Version | نسخه API هدف
REGISTRATION_CODE = 'YOUR_REGISTRATION_CODE' # Replace with your device code | کد ثبت دستگاه خود را وارد کنید
USERNAME = 'YOUR_USERNAME'                 # Replace with your username | نام کاربری خود را وارد کنید
PASSWORD = 'YOUR_PASSWORD'                 # Replace with your password | رمز عبور خود را وارد کنید
# ---------------------------

def main():
    """Demonstrates the basic SDK workflow."""
    try:
        # 1. Initialize Configuration
        # ۱. مقداردهی اولیه پیکربندی
        print("Initializing configuration...")
        config = Configuration(BASE_URL, GENERATION_VERSION)

        # 2. Register Device
        # ۲. ثبت دستگاه
        print(f"Registering device with code: {REGISTRATION_CODE[:4]}...") # Mask part of the code
        device = DevicesService(config, REGISTRATION_CODE)
        device.register()
        print(f"✅ Device registered successfully. Device Name: {device.DeviceName}")

        # 3. Login User
        # ۳. ورود کاربر
        print(f"Logging in user: {USERNAME}...")
        user = UsersService(device)
        user.login(USERNAME, PASSWORD)
        print(f"✅ User '{user.UserTitle}' logged in successfully.")

        # 4. Use API Services (Example: Get Items)
        # ۴. استفاده از سرویس‌های API (مثال: دریافت کالاها)
        print("Fetching items...")
        item_service = ItemsService(user)
        items = item_service.get_items()
        print("✅ Items fetched successfully.")

        # Pretty print first 2 items (if available) for demonstration
        # نمایش ۲ کالای اول (در صورت وجود) برای نمونه
        if items and isinstance(items, list):
              print("\n📦 Sample Items (First 2):")
              print(json.dumps(items[:2], indent=2, ensure_ascii=False))
        elif items:
              print("\n📦 Items Response:")
              print(json.dumps(items, indent=2, ensure_ascii=False))
        else:
              print("No items found or empty response.")

        # 5. Logout (Optional)
        # ۵. خروج (اختیاری)
        # user.logout()
        # print("🔵 User logged out.")

    except Exception as e:
        print(f"\n❌ An error occurred: {e}")
        # Consider more specific error handling based on response codes or types
        # می‌توانید مدیریت خطای دقیق‌تری بر اساس کدهای پاسخ یا نوع خطاها پیاده‌سازی کنید

if __name__ == '__main__':
    main()
```

## 📄 API Documentation | مستندات API

For complete details on Sepidar API endpoints, parameters, and data models (v111), refer to the unofficial Swagger documentation:
برای مشاهده جزئیات کامل Endpointها، پارامترها و مدل‌های داده API سپیدار (نسخه ۱۱۱)، به مستندات Swagger غیررسمی زیر مراجعه کنید:

🔗 **[Sepidar API Documentation (v111 - Unofficial Swagger UI)](https://www.google.com/search?q=https://sepidar-api-doc.pourjanali.com/)**

The official Sepidar documentation for version 110 is available here:
مستندات رسمی سپیدار برای نسخه ۱۱۰ در آدرس زیر موجود است:

🔗 **[Official Sepidar Documentation (v110)](https://www.google.com/search?q=https://github.com/SepidarSystem/Sepidar-Api-Doc)**

## 💡 How it Works | نحوه کارکرد

This SDK simplifies the Sepidar API complexities:
این SDK پیچیدگی‌های API سپیدار را ساده می‌کند:

1.  **Configuration:** Sets the base context (URL, API version).
2.  **Device Registration:** Performs the initial handshake using AES and RSA encryption as required by the API to obtain a server public key.
3.  **Authentication:** Uses the obtained keys and device details along with MD5-hashed passwords to log in the user and receive an authentication token.
4.  **Service Calls:** Subsequent calls use the authentication token and necessary headers (managed internally) to interact with specific API endpoints like `/items`.

## 🤝 Contributing | مشارکت

Contributions, issues, and feature requests are welcome\! Feel free to check the [issues page](https://www.google.com/search?q=https://github.com/pourjanali/sepidar-python-sdk/issues).
از مشارکت، گزارش مشکلات و درخواست ویژگی‌های جدید استقبال می‌شود\! لطفاً به [صفحه مشکلات (Issues)](https://www.google.com/search?q=https://github.com/pourjanali/sepidar-python-sdk/issues) مراجعه کنید.

Please adhere to this project's code of conduct (if you add one).
لطفاً به قواعد رفتاری این پروژه پایبند باشید (اگر اضافه کنید).

## 📜 License | مجوز

This project is licensed under the MIT License. See the `LICENSE` file for details.
این پروژه تحت مجوز MIT منتشر شده است. برای جزئیات بیشتر به فایل `LICENSE` مراجعه کنید.

-----

🛠 Maintained by [Behnam Pourjanali](https://www.google.com/search?q=https://github.com/pourjanali)

```
```
