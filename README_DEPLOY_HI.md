# JNV Attendance — Google Cloud Run + Google Drive

Ye version:
- Windows/Linux/local machine par `data/*.json` me kaam karta hai.
- Google Drive configured hone par wahi JSON files Google Drive folder me save karta hai.
- Cloud Run ke PORT environment variable ko use karta hai.
- Flask app ko Gunicorn se production me chalata hai.

## 1. Local run

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
python main.py
```

Browser: http://127.0.0.1:8080

Agar Google Drive configure nahi hai to local `data` folder use hoga.

## 2. Google Drive folder

Google Drive me ek folder banao, example: `JNV Attendance Data`.
Us folder ka ID URL me `/folders/` ke baad wala part hota hai.

Cloud Run ke service account ko us folder par **Editor** access dena hai.

## 3. Google Cloud

Google Cloud project select karo, Cloud Run aur Drive API enable karo.

Google Cloud Shell me project folder upload/open karke:

```bash
gcloud run deploy jnv-attendance --source . --region asia-south2 --allow-unauthenticated --max 1
```

Deploy ke time environment variables set kar sakte ho:

```text
JNV_GOOGLE_DRIVE_FOLDER_ID=YOUR_DRIVE_FOLDER_ID
JNV_SECRET_KEY=CHANGE_THIS_TO_A_LONG_RANDOM_SECRET
```

## 4. Important Google Drive permission

Cloud Run jis service account se chalega, us service account ka email Google Drive folder ke Share dialog me **Editor** ke roop me add karo.

## 5. Gmail MOD email

Agar MOD credentials email karne hain to ye environment variables bhi set karo:

```text
JNV_GMAIL_ADDRESS=yourgmail@gmail.com
JNV_GMAIL_APP_PASSWORD=your_16_character_app_password
```

Normal Gmail password mat do.

## 6. Production note

Google Drive ko JSON storage ke roop me use kiya gaya hai. Cloud Run ko `--max 1` ke saath deploy kiya gaya hai taaki ek time par ek app instance JSON files update kare. School me bahut high concurrent usage ho to Firestore database par migrate karna better hoga.

## 7. Google search

Cloud Run URL public website URL hoga. Google Search me automatic listing guaranteed nahi hai. Agar website ko Google search results me bhi lana hai, baad me custom domain + Google Search Console setup karna hoga.
