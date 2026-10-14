# MedVerify — Medicine Verification System

MedVerify is a web-based project that uses QR codes and an AI image-classification model to check medicine package information. It is built using Flask, SQLite, and OpenVINO.

The idea is simple: register a medicine package first, generate its QR code, and then scan the code later to check its saved details. The user can also upload a package image so the AI model can predict which medicine class it belongs to.

## Features

- Register medicine packages with a medicine name, batch number, manufacturer, and expiry date.
- Generate a unique ID and QR code for each registered package.
- Scan a QR code using the website camera or enter the package ID manually.
- Upload a medicine package image for AI-based classification.
- Compare the predicted medicine class with the class saved during registration.
- Check the expiry date and scan history.
- View registered medicine records.

## How it works

### 1. Register a medicine

The admin or supplier enters the medicine details, including its name, batch number, manufacturer, and expiry date. The system saves these details in the SQLite database and generates a unique package ID with a QR code.

### 2. Scan the QR code

During verification, the user scans the QR code or enters the package ID. The backend uses that ID to find the matching medicine record in the database.

### 3. Upload the package image

The user uploads a clear image of the medicine package. The MobileNetV4 model, running through OpenVINO, predicts the medicine class from the image.

### 4. Check the result

The backend compares the saved record with the verification results. It checks whether the package ID exists, whether the expiry date has passed, whether the predicted class matches the registered class, and whether the ID has been scanned before.

The website displays the result along with details such as the medicine name, batch number, manufacturer, expiry date, model prediction, confidence score, and scan count.

## Who uses it?

- **Admin / supplier:** Registers medicine packages and generates QR codes.
- **Pharmacist / retailer / customer:** Scans a package, uploads its image, and reviews the result.

In the current version, registration and verification are both available on the same website. Separate logins for these roles have not been implemented.

## Technology used

| Technology | Use |
|---|---|
| HTML, CSS, JavaScript | Frontend and user interface |
| Python Flask | Backend and API |
| SQLite | Stores medicine details and scan history |
| QR codes | Identifies registered package records |
| `html5-qrcode` | Scans QR codes through the browser |
| MobileNetV4 | Predicts the medicine class from an image |
| OpenVINO | Runs the image model for inference |

## Running the project

Make sure Python is installed. Open PowerShell in the project folder, where `app.py` and `requirements.txt` are located, and run:

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe app.py
```

When the server starts, open `http://127.0.0.1:5000` in your browser. Keep the terminal open while using the application.

For camera scanning, allow camera access in the browser. If the camera does not work, you can enter the package ID manually.

## Testing the project

You can test the model with medicine classes included in its training labels, such as:

- `Atoreza 28 tablets`
- `Augmentin 14 tablets`
- `Antopral 14 tablets`
- `Claritine 20 tablets`
- `Zyrtec 10 ml oral drops`

For example, register Atoreza with its expected class set to `Atoreza 28 tablets`. Scan its generated QR code and upload a clear image of an Atoreza package. Then repeat the process with Augmentin and an Augmentin image.

You can also test an unknown package ID, an expired demo record, a mismatched medicine image, and a repeated scan to see how the system responds.

## Understanding the result

The system may show different statuses depending on the checks performed:

- **Verified demo:** The registered record was found and the implemented checks passed.
- **Mismatch:** The model's predicted class differs from the registered class.
- **Expired:** The expiry date in the saved record has passed.
- **Suspicious:** The same package ID has been scanned before.
- **Unverified:** The model could not provide a usable or sufficiently confident match, or model inference was unavailable.
- **Not registered:** The package ID was not found in the database.

A repeated scan can happen during normal testing, so it does not automatically mean the package is counterfeit.

