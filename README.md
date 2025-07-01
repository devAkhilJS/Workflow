
# 📬 Weather & Task Reminder Workflow (n8n)

This n8n workflow automatically sends users their daily weather updates and pending to-do tasks using data from Google Sheets and the OpenWeatherMap API. It is scheduled to run every morning at 8:00 AM.

---

## 🧩 Workflow Breakdown (Node-by-Node)

### 1. **Schedule Trigger**
- **Type**: Trigger
- **Purpose**: Automatically starts the workflow daily at 8:00 AM.
- **Cron**: `0 0 8 * * *`

---

### 2. **Get row(s) in sheet**
- **Type**: Google Sheets
- **Purpose**: Reads user data from **Sheet1**, including `User_name`, `Email`, and `City`.

---

### 3. **Loop Over Items**
- **Type**: SplitInBatches
- **Purpose**: Iterates over each user record one-by-one from the sheet.

---

### 4. **If**
- **Type**: IF Node
- **Purpose**: Checks whether the user's **City** field is non-empty.
  - ✅ If `City` is valid → continue.
  - ❌ If `City` is empty → go to `Stop and Error`.

---

### 5. **Stop and Error**
- **Type**: StopAndError
- **Purpose**: Stops the workflow and throws a clear error if the user has not filled in their city.
- **Error Message**: `"City value is missing for the user. Please check the input data."`

---

### 6. **fetch geo coding coordinates**
- **Type**: HTTP Request
- **Purpose**: Uses OpenWeatherMap's **Geo API** to convert the city name into latitude and longitude.
- **API Used**: `http://api.openweathermap.org/geo/1.0/direct`

---

### 7. **fetch weather temp?**
- **Type**: HTTP Request
- **Purpose**: Fetches the current weather using latitude and longitude coordinates.
- **API Used**: `https://api.openweathermap.org/data/2.5/weather`

---

### 8. **Get row(s) in sheet1**
- **Type**: Google Sheets
- **Purpose**: Reads to-do tasks from **Sheet2**, used to show daily task updates.

---

### 9. **Edit Fields**
- **Type**: Set Node
- **Purpose**: Normalizes/cleans the `Email` field (e.g., trims and lowercases it).

---

### 10. **Code**
- **Type**: Code Node (JavaScript)
- **Purpose**: Formats the weather + task data into a structured email message for each user.
- **Data used**:
  - `User_name`
  - Weather (temp, city, description)
  - Task list from Sheet2
- **Output**:
  - `email`, `subject`, `message`, etc.

---

### 11. **Send a message**
- **Type**: Gmail
- **Purpose**: Sends an email using Gmail OAuth to each user with:
  - Weather summary
  - To-do task list

---

##  Outcome
Each user receives an email with their city’s weather and their daily task list—unless their `City` field is empty (which causes the workflow to halt unless handled).


---

## 🌐 APIs Used
- **OpenWeatherMap Geocoding API**
- **OpenWeatherMap Weather API**
- **Gmail API (via OAuth2)**

---

## ⏱ Schedule
- Automatically runs daily at **8:00 AM IST** using the `Schedule Trigger`.

