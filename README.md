# MediaCloud Pro ☁️

> A cloud-native media management platform built on Microsoft Azure  
> **COM682 — Cloud Native Development | Ulster University (QAHE) | 2025-26**

---

## 👤 Student Information

| Field | Details |
|-------|---------|
| **Name** | Bhaskar Bhusal |
| **Student Number** | 10411409 |
| **Module** | COM682 — Cloud Native Development |
| **University** | Ulster University (QAHE) |
| **Coursework** | CW2 — Project Implementation (50%) |

---

## 🌐 Live Application

🔗 **[https://mediacloudpro-b9efcpfac0bqhfba.spaincentral-01.azurewebsites.net](https://mediacloudpro-b9efcpfac0bqhfba.spaincentral-01.azurewebsites.net)**

---

## 📋 Project Overview

**MediaCloud Pro** is a fully serverless, cloud-native media management web application deployed on Microsoft Azure. Users can register, login, and manage their personal media library — uploading, viewing, editing and deleting images, videos and audio files.

The application uses a **serverless architecture** — there is no traditional backend server. All business logic runs through **Azure Logic Apps**, media files are stored in **Azure Blob Storage**, metadata is stored in **Azure Cosmos DB**, and the frontend is hosted on **Azure App Service**.

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     USER BROWSER                            │
│              (HTML / CSS / JavaScript)                      │
└──────────────────────┬──────────────────────────────────────┘
                       │  HTTPS REST calls
                       ▼
┌─────────────────────────────────────────────────────────────┐
│                  AZURE APP SERVICE                          │
│              mediacloudpro (Node.js)                        │
│              Serves: index.html                             │
└──────────────────────┬──────────────────────────────────────┘
                       │  HTTP Trigger URLs
                       ▼
┌─────────────────────────────────────────────────────────────┐
│              AZURE LOGIC APPS (6 Workflows)                 │
│   ┌──────────────┐  ┌─────────────┐  ┌──────────────────┐  │
│   │   upload     │  │  get-all    │  │     update       │  │
│   │   (POST)     │  │   (GET)     │  │     (PUT)        │  │
│   └──────────────┘  └─────────────┘  └──────────────────┘  │
│   ┌──────────────┐  ┌─────────────┐  ┌──────────────────┐  │
│   │   delete     │  │  register   │  │      login       │  │
│   │   (POST)     │  │   (POST)    │  │     (POST)       │  │
│   └──────────────┘  └─────────────┘  └──────────────────┘  │
└────────────┬─────────────────────────────────┬──────────────┘
             │ azureblob connector              │ documentdb connector
             ▼                                 ▼
┌────────────────────────┐      ┌──────────────────────────────┐
│   AZURE BLOB STORAGE   │      │      AZURE COSMOS DB         │
│  mediacloudprostorage  │      │   mediacloudpro-cosmos       │
│  ┌──────────────────┐  │      │   ┌────────────────────────┐ │
│  │  media-uploads   │  │      │   │  MediaCloudDB          │ │
│  │  (blob container)│  │      │   │  ├── MediaAssets       │ │
│  │  images, videos, │  │      │   │  │   (partition:/owner)│ │
│  │  audio files     │  │      │   │  └── Users             │ │
│  └──────────────────┘  │      │   │      (partition:/email)│ │
└────────────────────────┘      │   └────────────────────────┘ │
                                └──────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│                   SUPPORTING SERVICES                        │
│  ┌─────────────────────┐    ┌────────────────────────────┐  │
│  │  Application        │    │    GitHub Actions CI/CD    │  │
│  │  Insights           │    │    (auto-deploy on push)   │  │
│  │  mediacloud-insights│    │    .github/workflows/      │  │
│  └─────────────────────┘    └────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

---

## ✨ Features

- 🔐 **User Authentication** — Register and login with email validation and strong password enforcement
- 📸 **Image Upload & View** — Upload JPEG, PNG, WEBP with full-screen lightbox viewer
- 🎬 **Video Upload & Play** — Upload MP4, MOV with in-app modal video player
- 🎵 **Audio Upload & Play** — Upload MP3, WAV, AAC with card-based audio player
- ✏️ **Edit Files** — Update file names and tags stored in Cosmos DB
- 🗑️ **Delete Files** — Permanently remove files from the gallery
- 🔍 **Filter Gallery** — Filter by All / Images / Videos / Audio
- 📊 **Stats Dashboard** — Live count of total files, images, videos, audio
- 🔒 **Multi-user Isolation** — Each user sees only their own uploaded files
- 📱 **Responsive Design** — Works on desktop and mobile

---

## ☁️ Azure Resources

| # | Resource | Service | Purpose |
|---|----------|---------|---------|
| 1 | mediacloud-pro-rg | Resource Group | Container for all resources |
| 2 | mediacloudpro-cosmos | Cosmos DB (NoSQL) | Stores media metadata and users |
| 3 | mediacloudprostorage | Storage Account | Hosts blob container |
| 4 | media-uploads | Blob Container | Stores actual media files |
| 5 | mediacloudpro | App Service | Hosts frontend application |
| 6 | ASP-mediacloudprorg-945f | App Service Plan | Free F1 compute plan |
| 7 | mediacloud-upload | Logic App | POST — upload media |
| 8 | mediacloud-get-all | Logic App | GET — retrieve all files |
| 9 | mediacloud-update | Logic App | PUT — update metadata |
| 10 | mediacloud-delete | Logic App | POST — delete file |
| 11 | mediacloud-register | Logic App | POST — register user |
| 12 | mediacloud-login | Logic App | POST — authenticate user |
| 13 | mediacloud-insights | Application Insights | Monitor performance |
| 14 | documentdb | API Connection | Logic Apps → Cosmos DB |
| 15 | azureblob | API Connection | Logic Apps → Blob Storage |

---

## 🔗 REST API Endpoints (Logic App URLs)

| Operation | Method | Logic App |
|-----------|--------|-----------|
| Upload File | POST | mediacloud-upload |
| Get All Files | GET | mediacloud-get-all |
| Update File | PUT | mediacloud-update |
| Delete File | POST | mediacloud-delete |
| Register User | POST | mediacloud-register |
| Login User | POST | mediacloud-login |

---

## 🚀 Advanced Features

### 1. Application Insights
- Real-time monitoring of server requests, response times and failures
- Connected to App Service via Azure portal
- Live metrics streaming

### 2. CI/CD with GitHub Actions
- Automatic deployment to Azure App Service on every push to `main`
- Uses `azure/webapps-deploy@v2` action
- Publish Profile stored as GitHub secret

### 3. User Authentication
- Email validation — must contain `@` and valid domain
- Password strength meter — Weak / Fair / Good / Strong
- Password rules enforced — 8+ chars, uppercase, number, symbol
- Auto-generated strong password suggestion
- Show/hide password toggle
- Session persistence with sessionStorage

### 4. Large File Upload via SAS Token
- Files over 10MB uploaded directly to Blob Storage using XMLHttpRequest
- SAS token with 1-year expiry bypasses Logic App 50MB limit
- Used for large MP4 videos

---

## 📁 Project Structure

```
mediacloud-pro/
├── index.html          # Complete frontend application (HTML + CSS + JS)
├── server.js           # Node.js server — serves index.html
├── .github/
│   └── workflows/
│       └── main_mediacloudpro.yml   # GitHub Actions CI/CD
└── README.md
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | HTML5, CSS3, Vanilla JavaScript |
| API Layer | Azure Logic Apps (Consumption) |
| Database | Azure Cosmos DB (NoSQL, Serverless) |
| File Storage | Azure Blob Storage |
| Hosting | Azure App Service (Node.js) |
| CI/CD | GitHub Actions |
| Monitoring | Azure Application Insights |

---

## 📖 References

- [Azure Logic Apps Documentation](https://docs.microsoft.com/en-us/azure/logic-apps/)
- [Azure Cosmos DB Documentation](https://docs.microsoft.com/en-us/azure/cosmos-db/)
- [Azure Blob Storage Documentation](https://docs.microsoft.com/en-us/azure/storage/blobs/)
- [Azure App Service Documentation](https://docs.microsoft.com/en-us/azure/app-service/)
- [GitHub Actions — Azure Web Apps Deploy](https://github.com/marketplace/actions/azure-webapp)
- [CRIS-SME — AI-assisted Cloud Risk Intelligence Framework](https://github.com/m-khan-97/CRIS-SME)

---

## 📄 License

This project was created for academic purposes as part of COM682 Cloud Native Development at Ulster University.

---

*Bhaskar Bhusal | 10411409 | Ulster University (QAHE) | May 2026*
