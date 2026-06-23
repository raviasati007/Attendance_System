# 📋 Student Attendance System

A responsive web-based attendance system built with **HTML, CSS, and JavaScript**, integrated with **Google Sheets** via Google Apps Script.  
This project allows faculty to mark attendance for a fixed list of student USNs, with each date recorded as a new column in the sheet.

---

## 🌟 Features
- Responsive **card-based UI** (mobile & desktop friendly).
- **Date picker** to select the attendance date.
- **Present (P)** and **Absent (A)** buttons for each student.
- Neutral buttons initially → change to **green (P)** or **red (A)** when clicked.
- Confirmation prompt before submission.
- Attendance stored in Google Sheet:
  - Column A → USN list (fixed).
  - Row 1 → Date headers.
  - Each submission fills **P/A under the selected date column**.

---

## 🛠️ Tech Stack
- **Frontend:** HTML, CSS, JavaScript
- **Backend:** Google Apps Script Web App
- **Database:** Google Sheets

---

## ⚙️ Setup Instructions

### 1. Google Sheet
- Create a Google Sheet with:
  - Column A: `USN`
  - Row 1: `USN` header + future date headers
- Fill Column A with student USNs.

### 2. Apps Script Backend
- Open the sheet → **Extensions → Apps Script**.
- Paste the following code:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var data = JSON.parse(e.postData.contents);

  data.forEach(function(item) {
    var rollNo = item.rollNo;
    var attendance = item.attendance;
    var date = item.date;

    // Find or create column for this date
    var headerRow = sheet.getRange(1, 1, 1, sheet.getLastColumn()).getValues()[0];
    var colIndex = headerRow.indexOf(date) + 1;
    if (colIndex === 0) {
      colIndex = headerRow.length + 1;
      sheet.getRange(1, colIndex).setValue(date);
    }

    // Find row for USN
    var usnRange = sheet.getRange(2, 1, sheet.getLastRow()-1, 1).getValues();
    var rowIndex = -1;
    for (var i = 0; i < usnRange.length; i++) {
      if (usnRange[i][0] === rollNo) {
        rowIndex = i + 2;
        break;
      }
    }

    // If USN not found, append new row
    if (rowIndex === -1) {
      rowIndex = sheet.getLastRow() + 1;
      sheet.getRange(rowIndex, 1).setValue(rollNo);
    }

    // Write attendance under the date column
    sheet.getRange(rowIndex, colIndex).setValue(attendance);
  });

  return ContentService.createTextOutput("Success");
}


### Deploy as Web App → set access to Anyone.

Copy the Web App URL.

### 3. Frontend
Save the provided HTML file (attendance.html).

Replace sheetURL in the script with your Web App URL.

Open the file in a browser.

### 🚀 Usage
Select the date from the date picker.

Mark P or A for each student.

Click Submit Attendance.

Confirm submission → Attendance updates in Google Sheet.

Each date creates a new column in the sheet, with USNs fixed in rows.

📂 Project Structure
attendance.html   # Frontend UI
README.md         # Documentation
Google Sheet      # Data storage
Apps Script       # Backend logic


🌐 Live Demo
👉 Attendance Webpage (raviasati007.github.io in Bing)

(Replace with your actual GitHub Pages or hosting link once deployed)

License
This project is for academic use. Modify and extend as needed for your institution.