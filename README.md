# SmartShop AI – Intelligent Shopping Agent

> **Your Intelligent Shopping Companion** — An AI-powered shopping agent that aggregates products, analyzes prices, reads reviews, and gives smart recommendations using IBM Granite on IBM Cloud.

---

## Problem Statement

**#11 – Intelligent Shopping Agent**

Users struggle to compare products across multiple platforms. Information about prices, reviews, sustainability, and features is scattered. SmartShop AI unifies all this into one intelligent platform — telling you not just **what** to buy, but **why** and **when**.

---

## Features

| Feature | Description |
|---|---|
| 🔍 Smart Search | Natural language search with AI match scoring |
| 🎤 Voice Search | Browser Web Speech API microphone search |
| 🖼️ Image Search | Upload a product image to find similar products |
| 🤖 IBM Granite AI | Powered by IBM Granite on IBM Cloud/watsonx.ai |
| 📊 Product Comparison | Compare up to 4 products side by side |
| 💰 Price Intelligence | Price history, trend analysis, buy/wait advice |
| 🔔 Price Alerts | Set alerts for price drops |
| ⭐ Review Analysis | AI sentiment + authenticity scoring |
| 🌿 Sustainability Score | 0–100 eco-friendliness estimate |
| 📱 AI Assistant | Floating chat assistant for shopping questions |
| 📋 Shopping List | Wishlist with priority, quantity and notes |
| 📈 Trending | Real-time trending product tracking |
| 🎯 Recommendations | Personalized product recommendations |
| 🌐 Responsive Design | Works on desktop, tablet, and mobile |

---

## Architecture

```
User
 ↓
React + TypeScript Frontend (Vite)
 ↓ REST API
FastAPI Python Backend
 ↓
Granite Service (IBM Cloud / watsonx.ai)
 ↓
IBM Granite LLM
 ↓
Structured AI Response → Frontend
```

**Demo Mode:** If IBM Granite credentials are not configured, the system automatically switches to a deterministic local AI fallback.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 18, TypeScript, Vite, Tailwind CSS |
| Backend | Python 3.11, FastAPI, Uvicorn |
| AI | IBM Granite (via IBM Cloud watsonx.ai) |
| Charts | Recharts |
| Icons | Lucide React |
| Deployment | Docker, docker-compose |
| Cloud | IBM Cloud (watsonx.ai) |

---

## Project Structure

```
smartshop-ai/
├── frontend/
│   ├── src/
│   │   ├── components/    # Shared components (ProductCard, AIAssistant, etc.)
│   │   ├── pages/         # Dashboard, Search, Compare, ProductDetail, etc.
│   │   ├── layouts/       # MainLayout
│   │   ├── services/      # API service (axios)
│   │   ├── types/         # TypeScript types
│   │   ├── App.tsx
│   │   └── main.tsx
│   ├── package.json
│   └── vite.config.ts
├── backend/
│   ├── app/
│   │   ├── main.py
│   │   ├── routes/api.py
│   │   ├── services/
│   │   │   ├── granite_service.py      # IBM Granite integration
│   │   │   ├── recommendation_service.py
│   │   │   ├── price_service.py
│   │   │   └── sustainability_service.py
│   │   ├── data/products.py            # 30 demo products
│   │   └── schemas/requests.py
│   ├── requirements.txt
│   └── .env.example
├── deployment/
│   ├── Dockerfile.backend
│   ├── Dockerfile.frontend
│   └── nginx.conf
├── docker-compose.yml
└── README.md
```

---

## Installation

### Prerequisites
- Python 3.11+
- Node.js 20+
- npm or yarn

---

## Backend Setup

```bash
cd smartshop-ai/backend

# Create virtual environment
python -m venv venv
# Windows:
venv\Scripts\activate
# macOS/Linux:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Copy environment file
copy .env.example .env   # Windows
cp .env.example .env     # macOS/Linux

# Run backend
uvicorn app.main:app --reload --port 8000
```

Backend API is available at: http://localhost:8000
API Documentation: http://localhost:8000/docs

---

## Frontend Setup

```bash
cd smartshop-ai/frontend

# Install dependencies
npm install

# Run dev server
npm run dev
```

Frontend is available at: http://localhost:5173

---

## IBM Cloud Setup

1. Create an [IBM Cloud account](https://cloud.ibm.com)
2. Navigate to **watsonx.ai** service
3. Create a project
4. Generate an API key from **Manage → Access (IAM) → API Keys**
5. Copy your **Project ID** from the watsonx.ai project settings

---

## IBM Granite Setup

Edit `backend/.env` with your credentials:

```env
IBM_CLOUD_API_KEY=your_ibm_cloud_api_key
IBM_CLOUD_PROJECT_ID=your_project_id
IBM_GRANITE_ENDPOINT=https://us-south.ml.cloud.ibm.com
IBM_GRANITE_MODEL=ibm/granite-13b-chat-v2
```

Restart the backend — the system will automatically activate IBM Granite mode.

---

## Environment Variables

| Variable | Description | Default |
|---|---|---|
| `IBM_CLOUD_API_KEY` | IBM Cloud API Key | (empty = Demo mode) |
| `IBM_CLOUD_PROJECT_ID` | watsonx.ai Project ID | (empty = Demo mode) |
| `IBM_GRANITE_ENDPOINT` | IBM Cloud endpoint URL | us-south.ml.cloud.ibm.com |
| `IBM_GRANITE_MODEL` | Granite model ID | ibm/granite-13b-chat-v2 |
| `APP_PORT` | Backend port | 8000 |
| `CORS_ORIGINS` | Allowed CORS origins | localhost:5173 |

---

## Demo Mode

When IBM Granite credentials are not configured:

- A deterministic scoring engine handles recommendations
- Review analysis uses sentiment keyword matching
- Chat assistant responds with product data
- All features remain fully functional
- UI displays **"AI Mode: Demo"** badge

---

## API Documentation

| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/health` | System health check |
| GET | `/api/products` | Get all products |
| GET | `/api/products/{id}` | Get product details |
| POST | `/api/search` | Natural language search |
| POST | `/api/search/image` | Image-based search |
| POST | `/api/voice/query` | Voice query search |
| POST | `/api/recommend` | AI recommendation |
| POST | `/api/reviews/analyze` | Review analysis |
| GET | `/api/products/{id}/price-history` | Price history |
| POST | `/api/alerts` | Create price alert |
| GET | `/api/alerts` | Get all alerts |
| DELETE | `/api/alerts/{id}` | Delete alert |
| GET | `/api/trending` | Trending products |
| GET | `/api/recommendations` | Get recommendations |
| GET | `/api/shopping-list` | Get shopping list |
| POST | `/api/shopping-list` | Add to list |
| DELETE | `/api/shopping-list/{id}` | Remove from list |
| POST | `/api/chat` | Chat with AI assistant |
| GET | `/api/dashboard` | Dashboard data |

---

## Docker Deployment

```bash
# Build and run all services
docker-compose up --build

# Application available at:
# Frontend: http://localhost:80
# Backend:  http://localhost:8000
```

---

## IBM Cloud Deployment

1. Build Docker images:
```bash
docker build -f deployment/Dockerfile.backend -t smartshop-backend ./backend
docker build -f deployment/Dockerfile.frontend -t smartshop-frontend ./frontend
```

2. Push to IBM Cloud Container Registry:
```bash
ibmcloud login
ibmcloud cr login
docker tag smartshop-backend us.icr.io/your-namespace/smartshop-backend
docker push us.icr.io/your-namespace/smartshop-backend
```

3. Deploy to IBM Cloud Code Engine or Kubernetes Service

---

## Future Improvements

- Real-time price scraping from Amazon, Flipkart, Myntra
- User authentication and persistent preferences
- Email/SMS price alert delivery
- IBM Granite multimodal image understanding
- Price prediction using time-series ML
- Browser extension for in-page price comparison
- Social sharing and wishlist collaboration
- Mobile app (React Native)

---

## Disclaimer

- Product data is **demo/mock data** and does not represent real-time prices
- Review authenticity scores are **AI estimates** and not definitive proof
- Sustainability scores are **estimates** based on product attributes, not verified certifications
- Price history is **simulated** for demonstration purposes

---

## Built With

- **IBM Bob** — Development & AI pair-programming
- **IBM Cloud** — Cloud infrastructure & deployment
- **IBM Granite** — AI intelligence & recommendation engine

*SmartShop AI — An IBM Hackathon Project*
