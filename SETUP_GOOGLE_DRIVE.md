# Google Drive setup for normal Gmail (My Drive)

For a normal personal Gmail Drive, create an OAuth token once on a computer, then put that token in Cloud Run as `JNV_GOOGLE_TOKEN_JSON`.

## A. Google Cloud project
1. Open Google Cloud Console.
2. Select the same project used for Cloud Run.
3. Enable **Google Drive API**.
4. Create OAuth Client ID credentials for a **Desktop app**.
5. Download the client JSON and save it as `client_secret.json`.

## B. Generate the user token
In this folder run:

```powershell
python setup_drive_auth.py
```

A browser window opens. Sign in with the Google account whose Drive should store the JNV JSON data and allow Drive access.

The script creates `token.json`. Keep this file private.

## C. Get the folder ID
Create a Drive folder such as `JNV Attendance Data`.
Copy the folder ID from its Drive URL. It is the text after `/folders/`.

## D. Cloud Run variables
Set:

```text
JNV_GOOGLE_DRIVE_FOLDER_ID=YOUR_FOLDER_ID
JNV_GOOGLE_TOKEN_JSON=PASTE_THE_COMPLETE_CONTENTS_OF_token.json
JNV_SECRET_KEY=LONG_RANDOM_SECRET
```

The app will then keep its JSON files in that Drive folder. If Drive variables are missing, it automatically uses local `data/` storage.

## E. Shared Drive alternative
If your school Google Workspace has a Shared Drive, a service account can be used instead. Service accounts do not have personal My Drive storage quota, so Shared Drive is the preferred service-account arrangement.
