# ASHS Academic Report System

**Atweaman Senior High School** | Version 1.0 | 2024/2025

A web-based school management and academic reporting system. Runs entirely in a single HTML file powered by Firebase — no server installation required.

---

## Table of Contents

1. [Overview](#1-overview)
2. [Key Features](#2-key-features)
3. [User Roles and Access](#3-user-roles-and-access)
4. [Firebase Setup](#4-firebase-setup)
5. [Grading System](#5-grading-system)
6. [How to Use](#6-how-to-use)
7. [Technical Notes](#7-technical-notes)
8. [Troubleshooting](#8-troubleshooting)
9. [Support and Contact](#9-support-and-contact)

---

## 1. Overview

The ASHS Academic Report System handles the full academic reporting cycle for Atweaman Senior High School:

- Student and teacher registration
- Score entry per subject (class score + exam score)
- Automated grade calculation and ranking
- A4-formatted printable report card generation
- School-wide announcements and messaging

All data is stored securely in Firebase (Firestore + Storage) and is accessible from any device with a modern browser.

---

## 2. Key Features

| Feature | Description |
|---|---|
| Student Management | Register, edit, and delete students with photos. Import/export via CSV. Assign to forms, programmes, houses, and boarding status. |
| Teacher Management | Register teachers with photos and signatures. Assign subjects and forms. Each teacher has their own login credentials. |
| Score Entry | Teachers enter class scores (30%) and exam scores (70%) per subject. Totals and grades are calculated automatically. |
| Report Card Generation | Full A4 report cards with student photo, grading table, performance bar chart, staff remarks, and signature lines. Print or save as PDF. |
| Class Rankings | Automatic ranking by total score within each form and programme. Includes overall and subject-specific views. |
| Class Tests | Record and track class test scores separately from semester scores. |
| Performance Analysis | Visual charts showing student and class performance trends across subjects. |
| Backup and Restore | Export all school data to a JSON file. Restore from backup at any time. |
| Announcements | Admin can post messages, timetables, and documents visible to all logged-in teachers. |
| School Settings | Configure school name, address, logo, headmaster signature, grading remarks, and admin password. |
| Dark Mode | Toggle between light and dark themes. Preference is saved between sessions. |
| CSV Import/Export | Bulk-import students or teachers from CSV. Export scores and records for external use. |

---

## 3. User Roles and Access

| Role | Login | Access Level |
|---|---|---|
| Administrator | Admin password (set in Settings) | Full access to all tabs: Students, Teachers, Scores, Reports, Rankings, Class Tests, Backup, Messages, Settings |
| Teacher | Registered email + password | Enter Scores, My Subject Rankings, Performance Analysis, Profile, Announcements (view only) |

> **Note:** Teachers can only see and edit scores for subjects they are assigned to. They cannot access student records, other teachers' data, or school settings.

---

## 4. Firebase Setup

The app requires a Firebase project. Follow these steps:

1. Go to [https://console.firebase.google.com](https://console.firebase.google.com) and create a new project.
2. Click "Add app", select Web (`</>`), and copy the `firebaseConfig` object shown.
3. Open `index.html` in a text editor. Find the `firebaseConfig` section near the bottom and replace it with your own values.
4. Enable **Firestore Database**: Build > Firestore Database > Create database. Start in test mode.
5. Enable **Storage**: Build > Storage > Get started.
6. Enable **Anonymous Authentication**: Build > Authentication > Sign-in method > Anonymous > Enable.
7. Apply the security rules below, then open `index.html` in a browser.

### Recommended Firestore Security Rules

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

### Recommended Storage Security Rules

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

---

## 5. Grading System

Scores are calculated as: **Class Score (30%) + Exam Score (70%) = Total (100%)**

| Score Range | Grade | Remark |
|---|---|---|
| 80 - 100 | A1 | Excellent |
| 70 - 79 | B2 | Very Good |
| 65 - 69 | B3 | Good |
| 60 - 64 | C4 | Credit |
| 55 - 59 | C5 | Credit |
| 50 - 54 | C6 | Credit |
| 45 - 49 | D7 | Pass |
| 40 - 44 | E8 | Pass |
| 0 - 39 | F9 | Fail |

---

## 6. How to Use

### 6.1 First-Time Setup (Admin)

1. Open `index.html` in a browser. Select **Administrator** and enter the default password: `admin123`
2. Go to **Settings**. Update school name, address, phone, email, and upload the school logo.
3. Upload the headmaster's/headmistress' signature in Settings.
4. Change the admin password in Settings for security.
5. Register teachers before registering students.

### 6.2 Registering Students (Admin)

1. Go to the **Students** tab and fill in the registration form.
2. Upload a student photo (auto-compressed to under 50 KB).
3. Select the student's form, programme, house, and boarding status.
4. Choose 5 core subjects and 4-5 elective subjects.
5. Click **Register Student**. The record is saved to Firebase immediately.
6. To bulk-import, click **Upload CSV** and follow the template format.

### 6.3 Registering Teachers (Admin)

1. Go to **Manage Teachers**. Fill in the teacher's full name, employee ID, email, and phone.
2. Assign subjects the teacher handles and the form(s) they are class master for.
3. Upload the teacher's photo and signature.
4. Set a login password. The teacher will use their email and this password to log in.

### 6.4 Entering Scores (Teacher)

1. Log in as a **Teacher** using your registered email and password.
2. Go to **Enter Scores**. Select your subject and the student's form.
3. Enter each student's class score (out of 30) and exam score (out of 70).
4. Click **Save Scores**. Totals and grades are calculated automatically.

### 6.5 Generating Report Cards (Admin)

1. Go to the **Reports** tab. Select a student from the dropdown.
2. Choose the semester and academic year.
3. Enter remarks from the class teacher, housemaster/housemistress, senior housemaster/housemistress, assistant headmaster, and headmaster.
4. Click **Generate Report**. A full A4 report card will appear.
5. Click **Print Report** to print or save as PDF.

### 6.6 Backup and Restore

1. Go to **Backup/Restore**. Click **Download Backup** to export all data as a JSON file.
2. Store the backup file safely.
3. To restore, click **Upload Backup** and select your JSON backup file.

---

## 7. Technical Notes

| Item | Detail |
|---|---|
| Technology Stack | HTML5, CSS3, Vanilla JavaScript. Firebase Firestore, Firebase Storage, Firebase Authentication. |
| Firebase SDK | Version 10.12.2 (compat mode) loaded via CDN — no npm or build step required. |
| Photo Compression | Photos are automatically compressed to under 50 KB using canvas-based JPEG compression before upload. |
| Offline Behaviour | The app requires an internet connection. All reads and writes go directly to Firebase. |
| Browser Support | Chrome 90+, Firefox 88+, Edge 90+, Safari 14+. Internet Explorer is not supported. |
| Print / PDF | Report cards are formatted for A4 paper. Use "Print to PDF" in the browser print dialog. |
| Data Storage | All persistent data is in Firebase. localStorage is used only for session state (current user, theme). |
| Security | Firestore and Storage are protected behind Firebase Anonymous Authentication. |

---

## 8. Troubleshooting

| Problem | Solution |
|---|---|
| "Could not connect to Firebase" | Check your internet connection. Verify the `firebaseConfig` values in `index.html` match your Firebase project. |
| Photos not showing after reload | Ensure you are using the latest `index.html` (v1.0+). The fix preserves Firebase Storage URLs through student edits. |
| "Cannot upload this picture" | Ensure you are using the latest `index.html` (v1.0+). This was caused by a mismatched internal function name. |
| Anonymous sign-in error on login | Go to Firebase Console > Authentication > Sign-in method and enable Anonymous authentication. |
| Scores not saving | Check Firestore security rules allow authenticated writes. Confirm the teacher is assigned to that subject. |
| Report card prints blank or cut off | Use Chrome or Edge. Set paper size to A4 and margins to Default or None in the print dialog. |
| Teacher cannot log in | Confirm the teacher's email and password were set correctly in Manage Teachers. Passwords are case-sensitive. |

---

## 9. Support and Contact

- **School:** Atweaman Senior High School, P.O. Box 9, Atweaman, Ghana
- **Email:** info@ashs.edu.gh
- **Phone:** +233-XXX-XXX-XXX

---

*ASHS Academic Report System — Built for Atweaman Senior High School — 2024/2025*
