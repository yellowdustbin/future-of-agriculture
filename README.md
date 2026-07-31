# 🌾 Agri-Resilient AI: Predictive Farm OS

> **A next-generation, data-driven "Digital Twin" operating system for agriculture.**  
> Shifting farm management from reactive sensor monitoring to proactive, predictive resource optimization, climate-resilient crop planning, and market-timed profit maximization.

---

## 🎯 Overview & Mission

Modern agriculture faces severe challenges from climate volatility, unpredictable rainfall, soil degradation, and market price fluctuations. Traditional farm management relies on reactive monitoring—checking soil sensors or weather forecasts *after* water shortages or crop stress occur.

**Agri-Resilient AI** introduces a **Predictive Farm Operating System**. By unifying live weather forecasts ($ET_0$ evapotranspiration, rainfall), real-time Mandi market prices (Agmarknet), and a continuous virtual IoT soil digital twin into a multi-layered Data Science pipeline, our platform empowers farmers with:

1. **Hydrological Water Balance Forecasting:** Predicting exact days until water exhaustion and running 100-simulation Monte Carlo risk assessments.
2. **Priority-Based Expert System Crop Rotation:** Preventing crop failures by evaluating soil pH drift, nutrient deficits, and drought risks before planting.
3. **Machine Learning Soil Matching:** KNN Euclidean centroid distance matching with Softmax probabilities to select maximum-yield crops.
4. **Market Intelligence & Price Trend Analytics:** OLS Linear Regression over 14-day price windows to generate Buy/Hold/Sell recommendations.

---

## 🏗️ System Architecture

The Agri-Resilient AI ecosystem is structured into four synchronized layers:

```
                  ┌─────────────────────────────────────────┐
                  │          React 19 + Vite Dashboard      │
                  │   (Real-Time Charts, Gauges, Widgets)   │
                  └────────────────────┬────────────────────┘
                                       │ HTTP / REST
                                       ▼
                  ┌─────────────────────────────────────────┐
                  │            FastAPI Backend              │
                  │  (/summary, /predict, /rotation, etc.)  │
                  └─────┬─────────────────────────────┬─────┘
                        │                             │
                        ▼                             ▼
┌────────────────────────────────────────┐ ┌────────────────────────────────────────┐
│           Data Science Brain           │ │           Integration Layer            │
│ • Hydrological Depletion Engine        │ │ • Open-Meteo Weather API Wrapper       │
│ • Monte Carlo Stochastic Risk (N=100)  │ │ • Agmarknet Mandi Price API            │
│ • KNN Euclidean Soil Centroid Matcher  │ │ • Virtual IoT "Digital Twin" Simulator │
│ • Climatology & Regional Matcher       │ │ • Live Data Synchronization Runner     │
│ • Profit Matrix & Leaching Physics     │ │                                        │
└────────────────────────────────────────┘ └────────────────────────────────────────┘
```

---

## ✨ Core Engines & Mathematical Models

### 1. 💧 Hydrological Depletion Engine & Monte Carlo Simulator
* **Physics-Based Water Balance:** Calculates usable water capacity $V_{\text{usable}} = V_{\text{total}} \times (1 - \text{reserve\_margin})$ and subtracts daily irrigation usage and hourly FAO-56 Evapotranspiration ($ET_0$) against 7-day rainfall forecasts.
* **100-Run Monte Carlo Simulation:** Accounts for micro-climate volatility by executing $N=100$ stochastic iterations with uniform random perturbations ($\pm 15\%$ ET variance, $\pm 20\%$ rainfall variance).
* **Risk-Adjusted Metrics:** Computes $P_{10}$ (Safe Harvest Deadline / worst-case buffer), $P_{50}$ (Median), $P_{90}$ (Optimistic scenario), standard deviation ($\sigma$), and auto-classifies risk levels (`CRITICAL <3d`, `HIGH <7d`, `MODERATE <14d`, `LOW`).

### 2. 🌱 Smart Crop Rotation Expert System Matrix
* **P0–P5 Priority Queue Rule Engine:**
  * **P0 (pH Acidic Gate):** Detects $pH < 5.5$, issuing agricultural liming alerts ($2\text{--}3\text{ t/ha}$) and enforcing acid-tolerant crops (*Tapioca*).
  * **P0.5 (Drought Emergency Mode):** Triggered when $Days < 3$ from the Depletion Engine. Dynamically blocks all water-intensive crops (water intensity $\ge 8$) and enforces drought-resistant cover crops (*Horsegram*, *Tapioca*, *Groundnut*).
  * **P1 (Post-High-Water Recovery):** Identifies previous crops with water intensity $\ge 8$, enforcing nitrogen-fixing recovery cover crops (*Horsegram*, $+80\text{ kg N/ha}$ soil restoration).
  * **P2–P4 (Nutrient Restoration):** Targeted restoration for deficient macronutrients (*Cowpea* fixes up to $+100\text{ kg N/ha}$; *Mustard* mobilizes locked $P$; *Horsegram* recycles $K$).
  * **P5 (Cash Crop Optimization):** Recommends high-demand staple cash crops (*Rice / Wheat*) when nutrients are healthy ($N \ge 60, P \ge 40, K \ge 40$).

### 3. 🎯 Soil Centroid KNN Classifier & Softmax Inference
* **3D Euclidean Centroid Distance:** Measures vector distance $d = \sqrt{\Delta N^2 + \Delta P^2 + \Delta K^2}$ from live soil parameters to target crop centroids.
* **Temperature-Scaled Softmax Probabilities ($T=10.0$):** Converts inverted Euclidean distances into calibrated probability distributions:
  $$P(\text{crop}_i) = \frac{\exp(-d_i / T)}{\sum_j \exp(-d_j / T)}$$
* **Dynamic Weather Penalty Matrix:** Penalizes water-sensitive crop distances during heavy rain forecasts ($+50.0$) and high-water crops during dry spells ($+80.0$).

### 4. 📊 Climatology Engine & Regional Profiles
* **Historical Regional Cross-Referencing:** Evaluates crops against 12-month climate profiles (e.g., *Kochi*, *Palakkad*) incorporating humidity, extreme heat probability, and flood probability.
* **Climate Vetoes:** Triggers an explicit `CLIMATE VETO` during severe flood ($\ge 70\%$) or extreme heat ($\ge 70\%$) periods.

### 5. 📈 Market Intelligence & Trend Analytics Engine
* **Agmarknet Mandi Pricing:** Connects to live government market data for Kerala districts.
* **OLS Linear Regression:** Fits 14-day price history windows to compute daily price slope $\beta$ (₹ change/day).
* **Volatility & Signal Generation:** Evaluates Coefficient of Variation $\left(\text{CV} = \frac{\sigma}{\mu}\right)$ combined with slope magnitude to classify market risk (`Low`, `Medium`, `High`) and generate trade signals (`SELL`, `HOLD`, `SELL QUICKLY`).

### 6. ⚡ Master Profit Matrix & Leaching Physics
* **Multi-Factor Score (0–100):** Combines Soil Health ($W=0.40$, quadratic Gaussian decay), Market Momentum ($W=0.35$), and Water Depletion Viability ($W=0.25$).
* **Heavy Rain Leaching Physics:** Detects rainfall $>10\text{ mm/hr}$ to apply a 10%–15% Nitrogen wash-away penalty and prompt top-dress fertilizer action.

### 7. 📡 Virtual IoT "Digital Twin"
* **Continuous Hardware Simulation:** A background process updating soil parameters ($N, P, K, pH$, moisture, temperature) every 5 seconds, reacting dynamically to real-world weather inputs (leaching from high moisture, photosynthetic depletion during sunny hours).

---

## 📡 API Reference & Endpoints

| Method | Endpoint | Description |
| :--- | :--- | :--- |
| `POST` | `/summary` | **Unified Dashboard API** — Single payload orchestrating Depletion, Rotation, and Market engines |
| `POST` | `/predict` | Runs Depletion Engine, Monte Carlo risk simulation, and drought alerts |
| `POST` | `/rotation` | Runs Smart Seed rotation rule matrix, KNN classifier, and yield potential prediction |
| `GET` | `/market` | Fetches live price trends, slope, volatility, and Buy/Sell signals |
| `GET` | `/encyclopedia` | Lists full Agri crop knowledge base and crop tolerance profiles |
| `GET` | `/sensors` | Returns current virtual IoT hardware simulation state |
| `GET` | `/weather` | Returns current weather, FAO-56 evapotranspiration, and 7-day forecast |
| `GET` | `/health` | System health check endpoint |

---

## 📂 Project Structure

```text
future-of-agriculture/
├── backend/                    # FastAPI Application & Business Logic
│   ├── main.py                 # Application entry point & router mounting
│   ├── models/                 # Pydantic schemas (PredictRequest, RotationResponse, etc.)
│   ├── routers/                # API Endpoints (/summary, /predict, /rotation, /market, /encyclopedia)
│   ├── services/               # Service wrappers (depletion_service, rotation_service, market_service)
│   └── data/                   # Crop knowledge base (agri_encyclopedia.json)
├── data_science/               # Data Science Algorithms & AI Models
│   ├── depletion_engine.py     # Water balance & 100-run Monte Carlo risk simulator
│   ├── model_inference.py      # KNN Euclidean soil matcher & Softmax probability model
│   ├── climatology_db.py       # Regional historical weather norms & climate veto engine
│   ├── price_predictor.py      # OLS Linear Regression trend & volatility scoring
│   ├── profit_matrix.py        # Master Profit Matrix & multi-factor scoring algorithm
│   ├── success_matrix.py       # Rain leaching detection & live score integration
│   ├── soil_health.py          # Gaussian soil health normalization
│   └── weather_integration.py  # Weather station integration & leaching penalty calculator
├── integration/                # Live Hardware & External API Integration Layer
│   ├── iot_simulator.py        # Virtual IoT Digital Twin engine (N-P-K, pH, jitter)
│   ├── meteo_client.py         # Open-Meteo live weather client wrapper
│   ├── market_client.py        # Agmarknet Mandi market price fetcher
│   └── data/                   # Live state JSON files (live_sensors, live_weather, etc.)
├── frontend/                   # React 19 + Vite Dashboard
│   ├── src/                    # Components, UI widgets, charts, and CSS design tokens
│   ├── package.json            # Node.js dependencies
│   └── vite.config.js          # Vite build configuration
├── Scripts/
│   └── data_runner.py          # Background heartbeat service syncing IoT, Weather, and Market data
├── test_member2.py             # Integration & Unit Test Suite for Data Science engines
├── final_integration_test.py   # End-to-end integration test runner
└── requirements.txt            # Python backend dependencies
```

---

## 🛠️ Tech Stack

* **Backend & API:** Python 3.10+, FastAPI, Pydantic v2, Uvicorn, Python-dotenv
* **Data Science & ML:** NumPy, SciPy, Pandas, Monte Carlo Simulations, KNN Euclidean Centroids, OLS Linear Regression
* **Frontend:** React 19, Vite, JavaScript (ES6+), Vanilla CSS
* **Data Sources & IoT:** Open-Meteo API, Data.gov.in (Agmarknet Mandi), Custom IoT Digital Twin Simulator

---

## 🚀 Quick Start & Local Setup

### Prerequisites
* Python 3.10 or higher
* Node.js 18+ and npm

### 1. Clone & Set Up Backend

```bash
# Clone repository
git clone https://github.com/AlexJoBobby770/future-of-agriculture.git
cd future-of-agriculture

# Create virtual environment
python -m venv venv

# Activate virtual environment
# Windows:
venv\Scripts\activate
# Linux/macOS:
# source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

### 2. Launch Services

#### Option A: Run FastAPI Backend Server
```bash
uvicorn backend.main:app --reload --port 8000
```
* Interactive API Documentation will be available at `http://localhost:8000/docs`.

#### Option B: Run Background Data Heartbeat Sync
```bash
python Scripts/data_runner.py
```

#### Option C: Launch Frontend Dashboard
```bash
cd frontend
npm install
npm run dev
```
* Dashboard will be live at `http://localhost:5173`.

---

## 🧪 Testing Suite

Run the full integration test suite to verify all Data Science engines, Monte Carlo simulations, price trends, and leaching penalties:

```bash
python test_member2.py
```

---

## 📜 License

Distributed under the MIT License. See `LICENSE` for details.
