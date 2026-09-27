# CrediAdapt AI — Adaptive Lending Intelligence Workspace

CrediAdapt AI is an enterprise-grade AI loan decision engine and underwriting workspace built with **TanStack Start**, **React 19**, **TypeScript**, **Vite**, **Recharts**, and **Tailwind CSS**.

It evaluates personal and specialized loan applications in one pass through an intelligent multi-agent and ML-driven underwriting pipeline:
- **Multi-Product Financing Support**: Configured for Personal Loans, Vehicle/Auto Loans, Home Renovation, Higher Education & Upskilling, and Consumer Durables.
- **Granular Employment & Career Modeling**: Incorporates employment segment (Salaried, Self-Employed, Business Owner, Gig Worker) and continuous career tenure.
- **FOIR Affordability Analysis**: Strict 60% ceiling on Fixed Obligation to Income Ratio.
- **Decision Engine**: Supports `APPROVED_AS_REQUESTED`, `COUNTER_OFFER` (a lower affordable principal at the permitted requested tenure), and `REJECTED` states.
- **ML Default Risk & SHAP Factors**: Evaluates default probability and top additive feature impacts across bureau, income, employment, and product risk.
- **Policy Concessions & Dynamic Rate Engine**: Applies targeted discounts (e.g. Women Borrowers Scheme, Youth Career Starter, Super Prime Tier, Corporate Salaried Stability, Education Merit, Vehicle Incentive).
- **Amortisation & Foreclosure Simulator**: Calculates monthly EMIs, total interest, and early-closure interest savings.
- **AI Compliance Explanation Engine**: Generates executive summaries, affordability rationales, risk & pricing explanations, and prepayment advice.

> **Comprehensive Technical Specification**: For complete mathematical formulations, SHAP risk equations, counter-offer algorithms, and architectural blueprints, see [DOCUMENTATION.md](DOCUMENTATION.md).

---

## Tech Stack

- **Framework**: [TanStack Start](https://tanstack.com/router/latest/docs/framework/react/start/overview) + [React 19](https://react.dev/)
- **Build Tool**: [Vite](https://vitejs.dev/)
- **Language**: TypeScript (Strict Mode)
- **Styling**: Tailwind CSS v4 (Navy/Teal/Amber Editorial Fintech Theme)
- **Charts**: Recharts (Responsive, non-overlapping)
- **Icons**: Lucide React

---

## Getting Started


### Prerequisites

- Node.js `v20+` or `v22+`
- npm `v10+`

### Installation

```bash
# Clone the repository
git clone <repository-url>
cd Hackathon

# Install dependencies
npm install
```

### Development Server

Start the local development server on `http://localhost:8080`:

```bash
npm run dev
```

### Typecheck

Run full TypeScript compilation check without emitting files:

```bash
npm run typecheck
```

### Production Build & Server

Build the client assets and Nitro server bundle:

```bash
npm run build
```

To preview the production bundle:

```bash
npm run preview
```

---

## 📡 API Reference & Verification

### Endpoint: `POST /api/public/evaluate-loan`

Evaluates applicant financial profile and returns structured underwriting decision, risk analysis, policy discounts, simulation, and AI narrative.

#### Headers
```http
Content-Type: application/json
```

---

### Test Scenarios

#### 1. Approved Application
```json
POST /api/public/evaluate-loan
{
  "monthly_income": 95000,
  "age": 27,
  "gender": "Female",
  "house_rent": 18000,
  "existing_emi": 8000,
  "cibil_score": 795,
  "requested_loan": 500000,
  "requested_tenure": 36,
  "early_closure_month": 18
}
```
**Expected Outcome**: Status `200 OK`, `decision_type: "APPROVED_AS_REQUESTED"`, includes EMI, `LOW` risk band, SHAP factors, 3 policy concessions applied (Women Borrowers, Youth Career, Super Prime), and early closure savings data.

---

#### 2. Counter-Offer Application
```json
POST /api/public/evaluate-loan
{
  "monthly_income": 45000,
  "age": 34,
  "gender": "Male",
  "house_rent": 16000,
  "existing_emi": 9000,
  "cibil_score": 670,
  "requested_loan": 600000,
  "requested_tenure": 36,
  "early_closure_month": 18
}
```
**Expected Outcome**: Status `200 OK`, `decision_type: "COUNTER_OFFER"`, recommended principal reduced to fit the 60% FOIR ceiling at the permitted requested tenure, with recalculated EMI and risk metrics. A counter-offer never extends tenure to preserve the requested principal.

---

#### 3. Rejected Application
```json
POST /api/public/evaluate-loan
{
  "monthly_income": 30000,
  "age": 40,
  "gender": "Male",
  "house_rent": 15000,
  "existing_emi": 10000,
  "cibil_score": 600,
  "requested_loan": 500000,
  "requested_tenure": 36,
  "early_closure_month": 12
}
```
**Expected Outcome**: Status `200 OK`, `decision_type: "REJECTED"`, `is_eligible: false`, zero EMI (`0`), `risk.evaluated: false`, no fake foreclosure/risk values displayed.

---

#### 4. Invalid Input Validation (HTTP 422)
```json
POST /api/public/evaluate-loan
{
  "monthly_income": -1,
  "age": 15
}
```
**Expected Outcome**: Status `422 Unprocessable Entity`, detailed field-level validation errors.

---

#### 5. Malformed Request Body (HTTP 400)
```json
POST /api/public/evaluate-loan
INVALID_BODY
```
**Expected Outcome**: Status `400 Bad Request`, `{"detail": "Invalid JSON body."}`.

---

## 🎨 Design System

The workspace adheres to an editorial fintech theme:
- **Primary Ink Navy**: `#1e293b` / `oklch(0.28 0.075 258)`
- **Teal Accent**: `oklch(0.58 0.093 197)`
- **Warm Paper**: `oklch(0.965 0.01 95)`
- **Indian Rupee Formatting**: Formatted cleanly using Indian numbering standard (e.g. `₹6,00,000` or `₹1.15 L`).
