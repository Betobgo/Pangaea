import streamlit as st
import pandas as pd
from datetime import datetime

# ========== BACKEND MODULES (Your AI Logic) ==========
class AfricaRailFacts:
    CHINESE_PROJECTS = {
        "Mombasa-Nairobi SGR": {"cost_usd": 4.2e9, "year": 2017, "contractor": "CRBC"},
        "Addis-Djibouti": {"cost_usd": 3.6e9, "year": 2016, "contractor": "CREC"},
        "Lagos-Ibadan": {"cost_usd": 1.3e9, "year": 2021, "contractor": "CCECC"}
    }

    WESTERN_PROJECTS = {
        "Lobito Corridor": {"cost_usd": 1.2e9, "year": 2023, "sponsor": "US EXIM"},
        "Zambia-Tanzania": {"cost_usd": 3e8, "year": 2024, "sponsor": "EU"}
    }

    PRICING_BENCHMARKS = [
        {"service": "Feasibility Study", "min_usd": 1e5, "max_usd": 1e6, "buyers": ["African Govts"]},
        {"service": "Risk Assessment", "min_cny": 5e5, "max_cny": 2e6, "buyers": ["CRCC", "Sinohydro"]},
        {"service": "ESG Audit", "min_eur": 5e4, "max_eur": 2e5, "buyers": ["EU Agencies"]}
    ]

class RailwayIntelligenceModel:
    def __init__(self):
        self.clients = {
            "Chinese": ["CRCC", "CREC", "CCECC"],
            "Western": ["US EXIM", "EU Commission", "AfDB"],
            "African": ["Kenya Railways", "Angola MINFRA"]
        }

        self.delay_data = pd.DataFrame({
            "project": ["Mombasa-Nairobi", "Addis-Djibouti", "Lagos-Ibadan"],
            "delay_months": [18, 24, 12],
            "cost_overrun_pct": [22, 35, 18]
        })

    def calculate_revenue(self, target_client: str) -> dict:
        if target_client in self.clients["Chinese"]:
            return {"service": "BRI Risk Analytics", "min_price": "¥500K", "max_price": "¥2M"}
        elif target_client in self.clients["Western"]:
            return {"service": "ESG Compliance Package", "min_price": "€100K", "max_price": "€500K"}
        else:
            return {"service": "Feasibility AI", "min_price": "$50K", "max_price": "$200K"}

class GeopoliticalRiskEngine:
    def __init__(self):
        self.risk_factors = {
            "china": ["Debt Trap", "Environmental Fines", "Labor Strikes"],
            "west": ["ESG Compliance", "Local Content Rules"],
            "africa": ["Currency Risk", "Political Instability"]
        }

    def generate_risk_report(self, client_type: str) -> pd.DataFrame:
        risks = self.risk_factors.get(client_type.lower(), [])
        return pd.DataFrame({
            "Risk Factor": risks,
            "Suggested AI Tool": [
                "Debt Sustainability AI" if "Debt" in r else
                "Strike Prediction Model" if "Labor" in r else
                "Carbon Accounting Tool"
                for r in risks
            ]
        })

# ========== STREAMLIT FRONTEND ==========

st.set_page_config(page_title="Pangaea Foresight Rail AI", layout="wide")
st.title("🚄 Pangaea Foresight Labs – Rail Intelligence Dashboard")
st.caption("AI-powered geopolitical, ESG, and economic forecasting for African infrastructure.")

# Sidebar - client selection
st.sidebar.header("Client Intelligence Setup")
client = st.sidebar.selectbox("Select a client or sponsor:", ["CRCC", "CREC", "CCECC", "EU Commission", "US EXIM", "Kenya Railways"])

# Init models
facts = AfricaRailFacts()
intel_model = RailwayIntelligenceModel()
risk_engine = GeopoliticalRiskEngine()

# Revenue forecast
revenue = intel_model.calculate_revenue(client)
st.subheader("💰 Revenue Opportunity")
st.write(f"**Client:** {client}")
st.write(f"**Proposed Service:** {revenue['service']}")
st.write(f"**Pricing Estimate:** {revenue['min_price']} to {revenue['max_price']}")

# Risk report
client_type = "Chinese" if client in intel_model.clients["Chinese"] else "Western" if client in intel_model.clients["Western"] else "African"
st.subheader("⚠️ Risk Matrix and Mitigation")
risk_df = risk_engine.generate_risk_report(client_type.lower())
st.dataframe(risk_df, use_container_width=True)

# Delay analytics
st.subheader("⏳ Known Project Delays")
st.bar_chart(intel_model.delay_data.set_index("project"))

# PDF Download Stub (coming soon)
st.subheader("📄 Analyst Brief Generator")
st.info("📎 PDF briefing module coming soon. You'll be able to export client-ready reports from this dashboard.")

st.markdown("---")
st.caption(f"© {datetime.now().year} Pangaea Foresight Labs")
