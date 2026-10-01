# FinHealth AI – Personal Financial Health Advisor

**FinHealth AI** is a modern, responsive, interactive fintech web application built specifically for **Indian banking and credit practices**. It helps users track their CIBIL/Experian credit score, evaluate their Debt-to-Income (DTI) ratio, visualize credit utilization, and receive personalized educational financial improvement roadmaps powered by **Google Gemini**.

---

## 🌟 Key Features

1. **User Authentication & Assessment**
   - Supabase Auth + Email/Password registration.
   - Dedicated initial onboarding financial assessment capturing income, expenses, loans, EMIs, credit card limits, and active loan lines.
2. **Automated Indian Debt-to-Income (DTI) Calculation**
   - Automatically computes: `DTI = (Total Monthly Debt Payments / Gross Monthly Income) * 100`.
   - Categorizes according to RBI prudence norms: *Healthy (<20%)*, *Manageable (20-35%)*, *Caution (36-49%)*, and *Critical Debt Burden (>=50%)*.
3. **Dynamic Dashboard**
   - 7 Real-time Summary Cards (Credit Score, Category, Monthly Income, Monthly Expenses, DTI Ratio, Total Outstanding Debt, Credit Utilization).
   - Semicircular Credit Score Gauge mapped across CIBIL brackets: 300–579 (Poor), 580–669 (Fair), 670–739 (Good), 740–799 (Very Good), 800–900 (Excellent).
   - Interactive Recharts Donut Chart displaying Used Credit vs Available Credit with RBI's recommended 30% utilization threshold.
4. **Google Gemini AI Financial Advisor**
   - Evaluates portfolio with prompt engineered for Indian financial context (RBI guidelines, emergency fund recommendations in Sweep FDs/Liquid funds, CIBIL bureau dispute practices).
   - Generates visual financial health rating bar (`████████░░ 80%`), credit health diagnostic notes, key observations, and a structured 5-step actionable recovery plan.
   - Graceful offline fallback simulation when an API key is not configured.
   - Transparent educational disclaimer compliance.
5. **Credit History Progress Tracking**
   - Interactive Recharts Line/Area Chart showing score trajectory over time.
   - Date-based timeframe filtering (3 Months, 6 Months, 1 Year, All Time).
   - Metric delta cards (Previous Score, Current Score, Net Point Delta, Percentage Improvement, and trend visual explanation).
   - Audit table with recorded monthly bureau score points.
6. **Real-time Credit & Financial Data Recalculation**
   - Instant "what-if" scenario testing for loan pre-payments (e.g. debt reduction from ₹2,50,000 to ₹1,50,000).
   - Live before/after comparison badges showing delta drops in DTI and utilization.
   - Updates all dashboard metrics and history records without page reload.
7. **1-Click Presentation / Demo Mode for Evaluators & Judges**
   - Features pre-seeded Indian salaried persona **Rahul Sharma** (Credit Score: 650, Income: ₹60,000, Debt: ₹2,50,000, EMI: ₹12,000).
   - Guided 8-step walkthrough modal demonstrating the entire end-to-end evaluation flow.
8. **Indian Rupee (₹) Formatting**
   - Formatted with Indian comma notation (`₹2,50,000`, `₹60,000`) and compact denominations (`₹2.50 Lakh`).

---

## 🛠️ Technology Stack

- **Frontend**: React 18, Vite, Tailwind CSS, Lucide React, Recharts
- **Backend**: Node.js, Express.js, CORS, Dotenv, JWT
- **Database**: Supabase PostgreSQL with Row Level Security (RLS)
- **AI Engine**: Google Gemini API (`@google/generative-ai` with structured fallback simulation)

---

## 🚀 Quick Start Guide

### Prerequisites
- Node.js v18+ (verified on v24.21.0)
- npm v10+

### 1. Clone & Install Dependencies

```bash
# Clone or navigate to the project directory
cd finhealth-ai

# Install backend packages
cd backend
npm install

# Install frontend packages
cd ../frontend
npm install
```

### 2. Configure Environment Variables

**Backend (`backend/.env`):**
```env
PORT=5000
NODE_ENV=development
FRONTEND_URL=http://localhost:5173
JWT_SECRET=finhealth-secure-secret-key-3f6078db

# Optional: Add your Google Gemini API Key from https://aistudio.google.com/
GEMINI_API_KEY=

# Optional: Add Supabase PostgreSQL cloud credentials
SUPABASE_URL=
SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=
```
*(Note: If keys are left blank, the application automatically uses its high-performance local store pre-seeded with Rahul Sharma's demo profile and an intelligent rule-based Indian financial advisory engine).*

### 3. Run Development Servers

**Start Backend (Port 5000):**
```bash
cd backend
npm start
```

**Start Frontend (Port 5173):**
```bash
cd frontend
npm run dev
```

Open your browser at **`http://localhost:5173`**.

---

## 📊 Database Schema (Supabase / PostgreSQL)

The complete SQL schema with Row Level Security (RLS) policies is located in [`backend/database/schema.sql`](backend/database/schema.sql).

### Tables:
1. `profiles`: `id`, `user_id`, `name`, `email`, `created_at`
2. `financial_profiles`: `id`, `user_id`, `credit_score`, `monthly_income`, `monthly_expenses`, `total_debt`, `monthly_emi`, `credit_limit`, `credit_card_outstanding`, `active_loans_count`, `credit_utilization`, `dti_ratio`, `created_at`, `updated_at`
3. `credit_history`: `id`, `user_id`, `credit_score`, `recorded_at`, `remarks`
4. `financial_updates`: `id`, `user_id`, `old_debt`, `new_debt`, `old_dti`, `new_dti`, `old_utilization`, `new_utilization`, `old_score`, `new_score`, `event_type`, `updated_at`
5. `ai_consultations`: `id`, `user_id`, `financial_snapshot`, `ai_analysis`, `action_plan`, `created_at`

---

## 📡 API Documentation

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/auth/register` | Register new user profile |
| `POST` | `/api/auth/login` | Log in user or 1-click Demo session |
| `GET` | `/api/auth/profile` | Retrieve active authenticated profile |
| `GET` | `/api/financial-profile` | Fetch user financial profile and DTI metrics |
| `POST` | `/api/financial-profile` | Save onboarding assessment and calculate DTI |
| `PUT` | `/api/financial-profile` | Real-time debt update, delta calculation & audit log |
| `GET` | `/api/credit-history?filter={3m\|6m\|1y\|all}` | Credit score timeline with delta and trend stats |
| `POST` | `/api/credit-history` | Log new verified bureau score point |
| `POST` | `/api/ai/analyze` | Run Gemini analysis and generate 5-step plan |
| `GET` | `/api/ai/history` | Retrieve past AI consultations |
| `GET` | `/api/dashboard` | Consolidated metrics, cards & utilization donut data |
| `POST` | `/api/demo/reset` | Reset demo state to Rahul Sharma baseline |
| `POST` | `/api/demo/simulate-repayment` | Simulate ₹1,00,000 repayment with instant recalculation |
| `GET` | `/api/health` | System diagnostics & API connection status |

---

## 🎯 Testing the Judges Demo Scenario (Requirement #24)

1. Open `http://localhost:5173`.
2. Click **"1-Click Demo Mode"** in the top navigation or click **"Judges Presentation Mode"** in the dashboard header.
3. The 8-step guided stepper will walk you through:
   - Rahul Sharma's baseline profile.
   - Dynamic summary cards and credit utilization donut chart.
   - AI Advisor consultation with visual health meter (`████████░░ 80%`) and 5-step roadmap.
   - Bureau credit trajectory chart.
   - Simulating debt reduction from ₹2,50,000 to ₹1,50,000.
   - Observing instantaneous DTI drop (20% $\rightarrow$ 13.33%), utilization drop (35% $\rightarrow$ 15%), and credit score ascent (650 $\rightarrow$ 685).

---

## 🔒 Security & Compliance

- **Zero-Credential Exposure**: No passwords or API keys are exposed to the frontend.
- **Zero-Sensitive Data Storage**: The application never collects or stores banking OTPs, card PINs, CVVs, or bank netbanking passwords.
- **Educational Disclaimers**: All AI outputs explicitly declare that recommendations are educational and do not constitute formal banking guarantees.
