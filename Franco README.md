# Wialon – Trailer Group Automation

This workflow automates the reassignment of trailers in Wialon based on their **assignment or deassignment** to units. It ensures that each trailer is placed in the correct trailer group depending on whether it's in transit or left at a known location.

---

## 🔧 Technologies Used

- **n8n** – Workflow automation platform  
- **Wialon API** – Used to authenticate, search, and update trailer groups dynamically  
- **Neon (PostgreSQL)** – Stores trailer movement notifications and tracks their processing status

---

## 🚀 Workflow Overview

1. A webhook receives a trailer notification (assigned or deassigned event).
2. The notification is stored in a PostgreSQL table on Neon with a `pending` status.
3. The system retrieves the trailer list and group structure from Wialon via API.
4. For each trailer:
   - If it’s **assigned** to a unit → it is moved to the **IN TRIP** group.
   - If it’s **deassigned**:
     - The system checks if the trailer is within a known **geofence**.
     - If a geofence is found → the trailer is moved to a group with the same name.
     - If not → it is moved to the **OTHER LOCATION** group.
5. The workflow processes trailers **one by one**, supporting multiple deassignments simultaneously.
6. The notification status is updated to `processed` in Neon.

---

## 🧠 Key Features

- ✅ Automatic reassignment of trailers between Wialon groups
- 📍 Location-aware logic using geofences and fallback groups
- 🔁 Batch processing with one-by-one handling to avoid race conditions
- 📊 Logs all events and statuses in a PostgreSQL database
- ⚠️ Graceful fallback to "OTHER LOCATION" when geofence is not found

---

## 📁 Repository Contents

- `Wialon___Trailers.json` – Full n8n workflow
- `README.md` – Project documentation

---

## 📌 Example Use Case

> Trailer A is **assigned** to a unit → it is moved to the **IN TRIP** group  
> Trailer B is **deassigned** in a zone with geofence "Warehouse X" → it is moved to group **Warehouse X**  
> Trailer C is **deassigned** outside any known zone → it is moved to **OTHER LOCATION**  

---

## 🧱 Built by

Franco Cortez – Project Implementation Manager  
March 2025
