## ZIP Code Search & CSV Upload Application for Google Sheets
* The ZIP Code Search &amp; CSV Upload Application for Google Sheets automates the process of uploading, organising, and searching business lead data based on ZIP codes.

## **Introduction**
This tool is ideal for managers handling large datasets and looking for a quick way to retrieve relevant business contacts by ZIP code. 

## **Summary of Key Functions:**
* ✅ Upload & Update CSV Data – Imports business leads from a Google Drive CSV file into Google Sheets.
* ✅ Search by ZIP Code – Filters and displays businesses that match a given ZIP code prefix.
* ✅ Automated Data Management – Stores results in a separate "Search_Results" sheet and clears previous searches.
* ✅ Easy-to-Use Interface – Accessible via a custom menu ("Manager Tools") in Google Sheets.

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
![image](https://github.com/user-attachments/assets/6ebb9ade-f489-418f-95f3-5602db78d912)

### **2️⃣ Open Apps Script**
- Click on **Extensions** → **Apps Script**.
- Delete any existing code and **paste the script below**.

  ![image](https://github.com/user-attachments/assets/a8e49cef-45f5-4765-bf7f-c74ad2fa8b14)


### **3️⃣ Paste This Script**
```javascript
function onEdit(e) {
  if (!e || !e.range) return; // Prevents errors if the function runs without an event object

  var editedCell = e.range;

  if (editedCell.getA1Notation() === "H1") {
    filterByZip();
  }
}

function filterByZip() {
  var ss = SpreadsheetApp.getActiveSpreadsheet();
  var mainSheet = ss.getActiveSheet();
  var searchZip = mainSheet.getRange("H1").getValue().toString().toUpperCase().trim();

  if (!searchZip) return; // Exit if input is empty

  var data = mainSheet.getDataRange().getValues(); // Read all data
  var results = [["Lead", "Company", "Street", "Zip Code", "Phone Number"]]; // Table headers

  for (var i = 1; i < data.length; i++) {
    var zip = data[i][3] ? data[i][3].toString().toUpperCase().trim() : "";
    if (zip.startsWith(searchZip)) {
      results.push([data[i][0], data[i][1], data[i][2], data[i][3], data[i][4]]);
    }
  }

  var resultSheet = ss.getSheetByName("Search_Results") || ss.insertSheet("Search_Results");
  resultSheet.clear(); // Clears previous results

  if (results.length > 1) {
    resultSheet.getRange(1, 1, results.length, results[0].length).setValues(results);
  } else {
    resultSheet.getRange(1, 1).setValue("No matches found.");
  }

    // Notify the user that the search is complete
  SpreadsheetApp.getActiveSpreadsheet().toast("Search complete!", "Notification", 3);
}

```
![image](https://github.com/user-attachments/assets/2a95e2d8-7c4d-4bde-b641-233505954948)

---

![image](https://github.com/user-attachments/assets/e3d86d42-be9a-4c58-8774-03a286b01cdf)

Click on the email address
![image](https://github.com/user-attachments/assets/bf65ac93-c816-47db-a64d-6f5e578e7864)

Click on the Advanced and the subsequent email address on the page.
![image](https://github.com/user-attachments/assets/df32ceb6-3d07-4766-b496-829cd1f01c54)

Google has to verify the app.
Click on the Advanced link to proceed to verify the application. Click on the Advanced link to allow verification for Application Security.

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

