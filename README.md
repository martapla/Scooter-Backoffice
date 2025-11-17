# 🛵 Electric Scooter Management Backoffice (Vue.js)

A **backoffice web application** built with **Vue.js** that allows administrators to manage a fleet of electric scooters.  
The project includes a **paginated list view** and a **full CRUD detail view**, using a local **JSON file as mock API data**.

## 🔗 Live Demo

Check out the project live on Vercel:  
[Scooter Sharing Backoffice](https://scooter-backoffice.vercel.app/)

---

##  Features

###  1. List View (Home)
Displays a paginated list of electric scooters, including:

- `id`  
- `customId`  
- `plate` (vehicle license plate)  
- `batteryLevel` (displayed as a percentage, e.g., `85%`)  
- `autonomy` (displayed with `Km`)  
- `hasPromo` (shown as ✓ or ×)

#### **List functionalities**
- **Pagination**: default 3 items per page; configurable to **2, 3, or 5 items**.
- **Column filters** (search):
  - `id`
  - `plate`
- **Sortable columns**:
  - `batteryLevel`
  - `autonomy`
- Fixed header always visible.

---

###  2. Detail View (CRUD Screen)
Displays the full data for a selected scooter, including its location.

#### Editable fields:
- `customId`
- `plate`
- `hasPromo` (checkbox)
- `hasSpecialTrunk` (checkbox)
- `hasHelmetSensor` (checkbox)

All other fields are **read-only**.

#### Location
- The **Google Map** is embedded in the detail view, showing the scooter’s location.
- Users can see the exact position of the scooter visually on the map.

#### Additional UX features
- Fixed header with logo.
- Clicking on the logo returns to the **List View**.

## Tech Stack

- **Vue.js** 
- **JavaScript**
- **Local JSON file** simulating an API endpoint
- **CSS** 
- Google Maps JavaScript API

## ⚙️ About

This project was developed as a **technical test / assessment** to demonstrate skills in **Vue.js, JavaScript, and front-end development**.