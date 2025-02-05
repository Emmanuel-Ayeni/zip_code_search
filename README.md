# **ZIP Code Search & CSV Upload Application for Google Sheets**The ZIP Code Search &amp; CSV Upload Application for Google Sheets automates the process of uploading, organising, and searching business lead data based on ZIP codes.

## **Introduction**
This tool is ideal for managers handling large datasets and looking for a quick way to retrieve relevant business contacts by ZIP code. 

## **Summary of Key Functions:**
✅ Upload & Update CSV Data – Imports business leads from a Google Drive CSV file into Google Sheets.
✅ Search by ZIP Code – Filters and displays businesses that match a given ZIP code prefix.
✅ Automated Data Management – Stores results in a separate "Search_Results" sheet and clears previous searches.
✅ Easy-to-Use Interface – Accessible via a custom menu ("Manager Tools") in Google Sheets.

## **Overview**
This project provides a **Google Apps Script** solution that allows users to:
1. **Upload and update CSV data** into Google Sheets.
2. **Search for phone numbers and company details** using ZIP code prefixes.
3. **Display search results in a separate worksheet** for better organisation.

This script is ideal for managers or businesses handling **large lead datasets** and needing quick access to filtered data based on ZIP codes.

## **Features**
- 📂 **Upload CSV from Google Drive** and auto-populate a Google Sheet.
- 🔎 **Search by ZIP code** and retrieve matching results.
- 📊 **Structured tabular display** in a separate worksheet.
- ⚡ **Integrated Google Sheets menu** for easy access.
- ✅ **Automatic updates & cleanup** to prevent data clutter.

---

## **Installation & Setup**

### **1️⃣ Create a Google Sheet**
- Open **Google Sheets**.
- Rename the first sheet to **"Lead Database"**.
- Ensure the first row has these **headers**:
  ```plaintext
  Lead | Company | Street | Zip Code | Phone Number
  ```

### **2️⃣ Open Apps Script**
- Click on **Extensions** → **Apps Script**.
- Delete any existing code and **paste the script below**.

### **3️⃣ Paste This Script**
```javascript
function onOpen() {
  var ui = SpreadsheetApp.getUi();
  ui.createMenu("Manager Tools")
    .addItem("Upload CSV", "uploadCSV")
    .addItem("Search by ZIP", "promptForZipSearch")
    .addToUi();
}

function uploadCSV() {
  var ss = SpreadsheetApp.getActiveSpreadsheet();
  var sheet = ss.getSheetByName("Lead Database");
  
  if (!sheet) {
    sheet = ss.insertSheet("Lead Database");
  } else {
    sheet.clear();
  }

  var csvUrl = Browser.inputBox("Enter CSV file URL (Google Drive Shareable Link):");
  if (!csvUrl) return;

  try {
    var fileId = csvUrl.match(/[-\w]{25,}/)[0];
    var file = DriveApp.getFileById(fileId);
    var csvContent = file.getBlob().getDataAsString();
    var csvData = Utilities.parseCsv(csvContent);

    sheet.getRange(1, 1, csvData.length, csvData[0].length).setValues(csvData);
    SpreadsheetApp.getUi().alert("CSV uploaded successfully!");
  } catch (e) {
    SpreadsheetApp.getUi().alert("Error uploading CSV: " + e.message);
  }
}

function promptForZipSearch() {
  var ui = SpreadsheetApp.getUi();
  var response = ui.prompt("Enter ZIP code prefix to search:");

  if (response.getSelectedButton() == ui.Button.OK) {
    var zipCode = response.getResponseText().trim();
    if (zipCode !== "") {
      filterByZip(zipCode);
    } else {
      ui.alert("Invalid ZIP code. Please try again.");
    }
  }
}

function filterByZip(zipCode) {
  var ss = SpreadsheetApp.getActiveSpreadsheet();
  var mainSheet = ss.getSheetByName("Lead Database");
  var resultSheet = ss.getSheetByName("Search_Results");

  if (!mainSheet) {
    SpreadsheetApp.getUi().alert("Lead Database sheet is missing.");
    return;
  }

  if (!resultSheet) {
    resultSheet = ss.insertSheet("Search_Results");
  } else {
    resultSheet.clear();
  }

  var data = mainSheet.getDataRange().getValues();
  var results = [["Lead", "Company", "Street", "Zip Code", "Phone Number"]];

  for (var i = 1; i < data.length; i++) {
    var zip = data[i][3].toUpperCase();
    if (zip.startsWith(zipCode.toUpperCase())) {
      results.push([data[i][0], data[i][1], data[i][2], data[i][3], data[i][4]]);
    }
  }

  if (results.length > 1) {
    resultSheet.getRange(1, 1, results.length, results[0].length).setValues(results);
    SpreadsheetApp.getUi().alert("Results updated in 'Search_Results' sheet.");
  } else {
    resultSheet.getRange(1, 1).setValue("No matches found.");
  }
}
```

---

## **Usage Instructions**

### **1️⃣ Uploading CSV Data**
1. Click **"Manager Tools"** in the menu bar.
2. Select **"Upload CSV"**.
3. Enter the **Google Drive shareable link** of the CSV file.
4. Data is uploaded into the **"Lead Database"** sheet.

### **2️⃣ Searching for ZIP Codes**
1. Click **"Manager Tools"** → **"Search by ZIP"**.
2. Enter a ZIP code prefix (e.g., `E1`).
3. Results appear in a **new "Search_Results" sheet**.

---

## **Contributing**
### **🔧 How to Contribute**
We welcome contributions! Feel free to:
- **Submit issues** for bugs or feature requests.
- **Fork the repository** and improve the script.
- **Submit a pull request** with enhancements.

### **📝 To-Do List**
- ✅ Add support for multiple ZIP codes.
- ✅ Improve UI for CSV uploads.
- ✅ Add logging for tracking searches.

---

## **License**
This project is licensed under the **MIT License** – feel free to modify and distribute it as needed.

---

## **Acknowledgments**
This script is inspired by the need to **automate ZIP code-based queries** for business leads and improve data handling in Google Sheets.

💡 If you find this useful, **give it a ⭐ on GitHub!** 🚀

