# 🎫 BillFree TechSupport Ops

![Status](https://img.shields.io/badge/status-live-brightgreen)
![Version](https://img.shields.io/badge/version-10.0-blue)
![Platform](https://img.shields.io/badge/platform-Google%20Apps%20Script-orange)
![License](https://img.shields.io/badge/license-private-lightgrey)

> **Production IT support ticketing system** serving a live fintech team.
> Handles ticket lifecycle, agent workload balancing, WhatsApp bot integration,
> cross-functional team portal, and monthly performance reporting.

| Module | URL | Users |
|---|---|---|
| 📊 Agent Dashboard | `...exec` | IT Agents & Admin |
| 🔧 CF Ticket Creation & Check Status | `...exec?page=portal` | CrossFunctional team, Merchants |

| 🤖 WhatsApp Bot API | `POST ...exec` | Automated Ticket Creation -latest implementation  |

---

# 🎫 BillFree TechSupport Ops


**BillFree TechSupport** is a premium, standalone TechSupport ticket portal designed for cross-functional teams and customers. It features a modern two-tab interface for creating and tracking IT support tickets with real-time status updates and follow-up conversation history.

## 🚀 Features

- **Quick Fix Self-Help**: Expandable troubleshooting guides for common issues (Popup, Delivery, Print).
- **Create Support Ticket**: Simple form to raise IT issues with auto-assignment logic.
- **Track Support Ticket**: Real-time status tracking with a timeline of follow-up conversations.
- **Branding**: Premium Glassmorphism UI consistent with the BillFree design system.
- **Backend Integration**: Powered by Google Apps Script and Google Sheets.
- **Flexible Deployment**: Can be served via Google Apps Script or hosted on Cloudflare Pages.

## 📂 Repository Structure

- `Code.gs`: The backend logic for Google Apps Script (REST API, routing, and sheet management).
- `TicketPortal.html`: The frontend version designed to be served directly from Google Apps Script.
- `Index.html`: The standalone frontend version optimized for Cloudflare Pages (uses REST API).

## 🔗 Direct Portal Links

Depending on your deployment method, use the following links:

| Method | Link Format | Purpose |
| :--- | :--- | :--- |
| **Google Apps Script** | `https://script.google.com/.../exec?page=portal` | Direct access via GAS (uses `TicketPortal.html`) |
| **Cloudflare Pages** | `https://billfreequickfix.pages.dev` | Standalone premium URL (uses `Index.html`) | https://billfreequickfix.pages.dev | Ticket Portal url ('TicketPortal.html')
| **Main Dashboard** | `https://script.google.com/.../exec` | Admin view for agents to manage tickets |

## 🛠️ Deployment Guide

### 📋 Prerequisites
- A Google Sheet named **"IT Tracker 26"**.
- Columns set up as: `Ticket ID (A)`, `Created At (B)`, `Agent Email (C)`, `IT Email (D)`, `Requested By (E)`, `MID (F)`, `Business (G)`, `POS (H)`, `Support Type (I)`, `Concern (J)`, `Config Notes (K)`, `Remark (L)`, `Status (M)`, `Reason (N)`, `Phone (O)`.

### 1. Google Apps Script (Backend)
1. Create a new Google Apps Script project.
2. Copy the content of `Code.gs` into the editor.
3. If serving via GAS, add `TicketPortal.html` as an HTML file.
4. Deploy as a **Web App**:
   - Execute as: **Me**
   - Who has access: **Anyone**
5. Copy the **Web App URL**.

### 2. Cloudflare Pages (Frontend)
1. Open `Index.html`.
2. Update the `GAS_ENDPOINT` constant (around line 640) with your **Web App URL** from Step 1.
3. Push this repository to GitHub.
4. Connect the repository to **Cloudflare Pages**.
5. Set the build output directory to `/`.
6. **(Optional) Custom Subdomain**: 
   - Go to Cloudflare Pages → Your Project → **Custom domains**.
   - Add your own subdomain (e.g., `quickfix.billfree.in`).

## 🤖 Auto-Assignment Logic
Tickets created via this portal are automatically assigned to available IT agents using a round-robin logic to ensure balanced workload distribution.

## 🔒 Security & Performance
- **Rate Limiting**: Prevents spam by limiting searches and ticket creations per session.
- **Data Protection**: Public portal only exposes status, agent name, and follow-up history. Private notes remain hidden.
- **Fast Search**: Uses projected data caching to handle 3,000+ tickets with sub-second search times.

---
Built with ❤️ for the BillFree Support Team.
