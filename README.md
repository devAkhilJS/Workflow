
# 📬 Weather & Task Reminder Workflow (n8n)

This n8n workflow automatically sends users their daily weather updates and pending to-do tasks using data from Google Sheets and the OpenWeatherMap API. It is scheduled to run every morning at 8:00 AM.

---

## 🧩 Workflow Breakdown (Node-by-Node)

### 1. **Schedule Trigger**
- **Type**: Trigger
- **Work**: Automatically starts the workflow daily at 8:00 AM.
- **Cron**: `0 0 8 * * *`

---

### 2. **Get row(s) in sheet**
- **Type**: Google Sheets
- **Work**: Reads user data from **Sheet1**, including `User_name`, `Email`, and `City`.
- **Process**: First we have to build a Google sheet account using Google cloud platform so first we go to the Google cloud platform in this there is a section of API and services , in this section on the upper side there is a option of new project where we can click and will are requirements according to our project after filling the requirements there is a client key and client secret generated which we have to put in the n8n work flow Google sheet node after filling the Google client and secret  we go back to browser and search Google library in which the Google sheet API option is there after searching Google sheet API we can simply enable the API to work the Google sheet account of the n8n node here is the video link:https://youtu.be/3Ai1EPznlAc?si=_VOenBEcNhjry7Uf
- 

---

### 3. **Loop Over Items**
- **Type**: SplitInBatches
- **Work**: Iterates over each user record one-by-one from the sheet.

---

### 4. **Edit Fields**
- **Type**: Set Node
- **Work**: Normalizes/cleans the `Email` field (e.g., trims and lowercases it).

### 5. **If**
- **Type**: IF Node
- **work**: Checks whether the user's **City** field is non-empty.
  - If `City` is valid → continue.
  - If `City` is empty → go to `Stop and Error`.

---

### 6. **Stop and Error**
- **Type**: StopAndError
- **Work**: Stops the workflow and throws a clear error if the user has not filled in their city.
- **Error Message**: `"City value is missing for the user. Please check the input data."`

---

### 7. **fetch geo coding coordinates**
- **Type**: HTTP Request
- **Work**: Uses OpenWeatherMap's **Geo API** to convert the city name into latitude and longitude.
- **API Used**: `http://api.openweathermap.org/geo/1.0/direct`
- **Process**:First we have to go open weather map Geo coding API to get the latitude and longitude coordinates so, first we have to build a account on open weather map API to generate a API key in the account section there is a option of my API key where we can simply create a API after creating API key it take time to activate. The API key is activate after approximate 5 to 8 hours after the activated API key we get the base open weather map Geo co-ordinate API with the queries with simply put all the detail in http node to generate a specific latitude and longitude coordinates to get the data of this different cities.

---

### 8. **fetch weather temp?**
- **Type**: HTTP Request
- **Work**: Fetches the current weather using latitude and longitude coordinates.
- **API Used**: `https://api.openweathermap.org/data/2.5/weather`
**Process**: After getting geo coodinates , First we have to go open weather map  API to get the temperature coordinates so we click on surrent weather section in which there is a option of open weather map api using the city name so we click on that to get the base api so,  we have to build a account on open weather map API to generate a API key in the account section there is a option of my API key where we can simply create a API after creating API key it take time to activate. The API key is activate after approximate 5 to 8 hours after the activated API key we get the base open weather map  co-ordinate API with the queries with simply put all the detail in http node to generate a specific  coordinates to get the data of this different cities. video link : https://youtu.be/MdIfZJ08g2I?si=dNKIbneXf13cr0lG
---



---

### 9. **Code**
- **Type**: Code Node (JavaScript)
- **Work**: Formats the weather + task data into a structured email message for each user.
- **Data used**:
  - `User_name`
  - Weather (temp, city, description)
  - Task list from Sheet2
- **Output**:
  - `email`, `subject`, `message`, etc.

---

### 10. **Send a message**
- **Type**: Gmail
- **Work**: Sends an email using Gmail OAuth to each user with:
  - Weather summary
  - To-do task list

---

##  Outcome
This workflow triggers daily at 8:00 AM and reads user and task data from a Google Sheet.
It loops through each user, checks if the City field is filled.
If city is missing, it stops with an error; otherwise, it fetches coordinates via OpenWeatherMap Geo API.
The Code node combines weather and task info into a personalized email message.
Finally, the Gmail node sends this email to the user with their daily weather and task update...


---
## Worflow :
## Workflow video link : https://drive.google.com/file/d/1M1WlZCPU2B0KkIwzAp3XJzbIaB4tWA3g/view?usp=sharing
---

## 🌐 APIs Used
- **OpenWeatherMap Geocoding API**
- **OpenWeatherMap Weather API**
- **Gmail API (via OAuth2)**

---

## ⏱ Schedule
- Automatically runs daily at **8:00 AM IST** using the `Schedule Trigger`.

