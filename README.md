# RideConnect - Ride-Sharing platform

A full-stack ride-sharing platform inspired by Uber/Careem, built with Django and a PostgreSQL spatial database. Supports map-based ride booking, geospatial driver matching, and dynamic surge pricing.

---

## Course Project

This project was developed as part of the **Database Management Systems** course.

### Main Topics Covered
- Spatial / Geospatial Databases (PostGIS)
- Location-Based Querying (ST_DWithin, ST_Distance)
- Relational Data Modeling (UUIDs, FK constraints)
- RESTful API Design (GeoJSON endpoints)

---
## Tech Stack

| Layer | Technologies |
|---|---|
| **Backend** | Python, Django 5.1, Django REST Framework |
| **Database** | PostgreSQL + PostGIS |
| **Frontend** | HTML5, CSS3, JavaScript |
| **Maps** | Leaflet.js, Leaflet Routing Machine, OpenStreetMap |
| **Security** | bcrypt, Django sessions, CSRF protection |
## Features

- Map-based ride booking with live route rendering and instant price estimates
- Geospatial driver search; finds available drivers within a 5 km radius using PostGIS
- Dynamic surge pricing; polygon surge zones apply a price multiplier at booking time
- Ride lifecycle state machine: `searching` → `waiting_for_driver` → `on_the_way` → `completed`
- Surge area explorer; interactive map visualizing active surge zones via a GeoJSON API
- Ride history with full details (route, price, duration, driver)
- Secure authentication with bcrypt password hashing and CSRF protection

---

## Requirements

- Python 3.11.9
- PostgreSQL with PostGIS extension
- GDAL / GEOS libraries

---

## Ride Booking Flow

| Step | Action |
|---|---|
| 1 | Log in and open the booking map |
| 2 | Click to set pickup and dropoff — route and price update instantly |
| 3 | Select vehicle type (Economy / Premium / Family) |
| 4 | Confirm booking — system queries PostGIS for drivers within 5 km |
| 5 | A driver is matched and the ride begins |
| 6 | Price is calculated from distance, duration, and active surge multiplier |
| 7 | Completed ride is saved to history with full spatial data |

---

## ERD

![ERD](docs/ERD.png)

---

## Indexing Decisions and Their Rationales

| Index | Type | Reason |
|---|---|---|
| `rides.id` | B_Tree  | Frequently used for equality searches (`WHERE r.id = ...`). B-Tree indexes are optimized for such queries. |
| `driver.location` | GIST  | Supports geospatial queries efficiently, such as `ST_DWithin` for finding drivers within a specific radius. |
| `driver.status` | B_Tree  | Used to filter available drivers (`WHERE d.status = 'Available'`). |

---

## Documentation

- [Project Report](docs/report.pdf)

---

## Project Structure

```
IS335Django/IS335django/
│
├── IS335APP/
│   ├── models.py        # Domain models
│   ├── views.py         # Request handlers — booking, matching, history, surge API
│   ├── urls.py          # URL routing
│   ├── serializers.py   # DRF serializers for API responses
│   ├── forms.py         # Registration and login forms
│   └── templates/app/   # HTML templates (Leaflet.js maps, booking UI)
│
└── IS335django/
    └── settings.py      # Django config — PostGIS backend, GDAL/GEOS paths
```
