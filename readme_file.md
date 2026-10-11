# Employee Payroll & Attendance GitHub App

An automated payroll, attendance logging, and leave management system built as a GitHub App. This application allows employees to interact via GitHub Issue comments using slash commands, processes attendance and leave records, and computes net monthly payouts based on standard working days, advances, and unpaid leave deductions.

---

## 🚀 Features

- **Slash Command Integration**:
  - `/check-in`: Log daily attendance directly from issue comments.
  - `/leave-request [YYYY-MM-DD] [Reason]`: Submit leave requests for admin approval.
- **Automated Payout Engine**: Calculates monthly payout using:
  $$\text{Net Payout} = \left(\frac{\text{Base Salary}}{\text{Working Days}} \times \text{Days Worked}\right) - \text{Advances} - \text{Unpaid Leave Deductions}$$
- **Secure Architecture**:
  - Webhook verification using GitHub App secrets (`X-Hub-Signature-256`).
  - Private key authentication with Octokit SDK.
  - Granular access controls ensuring salary privacy.

---

## 🛠️ Tech Stack

- **Runtime**: Node.js (ES Modules)
- **Framework**: Express.js
- **GitHub Integration**: `@octokit/app`, `@octokit/webhooks`
- **Database**: PostgreSQL / Supabase
- **Environment Management**: `dotenv`

---

## 📋 Prerequisites

1. **Node.js**: v18.0.0 or higher
2. **PostgreSQL Database** (or compatible SQL store)
3. **Registered GitHub App** with the following permissions:
   - **Issues**: Read & Write
   - **Pull Requests**: Read & Write
   - **Subscribe to Events**: `issue_comment`, `issues`, `pull_request`

---

## ⚙️ Installation & Setup

1. **Clone the repository**:
   ```bash
   git clone https://github.com/your-org/github-payroll-app.git
   cd github-payroll-app
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

3. **Configure Environment Variables**:
   Create a `.env` file in the root directory:
   ```env
   PORT=3000
   APP_ID=123456
   WEBHOOK_SECRET=your_webhook_secret_here
   PRIVATE_KEY_PATH=./private-key.pem
   DATABASE_URL=postgresql://user:password@localhost:5432/payroll_db
   ```

4. **Add Private Key**:
   Place your GitHub App's generated private key (`.pem` file) in the project root directory as specified in `PRIVATE_KEY_PATH`.

5. **Start the Application**:
   ```bash
   # Development mode (with file watching)
   npm run dev

   # Production mode
   npm start
   ```

6. **Expose Local Server for Webhooks (Development)**:
   ```bash
   npx ngrok http 3000
   ```
   Set your GitHub App's **Webhook URL** to `https://<your-ngrok-subdomain>.ngrok-free.app/api/webhook`.

---

## 💬 Usage & Commands

Employees can type slash commands inside any issue comment in monitored repositories:

| Command | Syntax | Description |
| :--- | :--- | :--- |
| **Check In** | `/check-in` | Logs check-in timestamp for the current date. |
| **Leave Request** | `/leave-request YYYY-MM-DD <Reason>` | Submits a leave request for administrative approval. |

---

## 📄 Database Schema Overview

The database comprises five core tables:
- `employees`: User profile, base salary, and GitHub mapping.
- `attendance_logs`: Daily check-in/check-out timestamps and status (`PRESENT`, `HALF_DAY`, `ABSENT`).
- `leave_records`: Paid/unpaid leave logs and approval states.
- `salary_advances`: Financial advances disbursed during the current pay cycle.
- `payroll_runs`: Processed monthly payouts and breakdown ledger.

---

## 🔒 Security & Data Privacy

- **Never commit `.env` or `*.pem` files** to public or private version control.
- Ensure all payroll calculation endpoints are served over HTTPS.
- Sensitive compensation details are restricted to admins and individual employees via Row-Level Security (RLS).

---

## 📜 License

Distributed under the MIT License. See [`LICENSE`](./LICENSE) for details.