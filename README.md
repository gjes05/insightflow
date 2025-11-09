# T-Mobile Real-Time Operations Monitor

A multi-agent AI system that simulates, analyzes, and visualizes T-Mobile customer experience metrics in real-time using LLMs for decision-making.

![System Architecture](https://img.shields.io/badge/Architecture-Multi--Agent-blue)
![Python](https://img.shields.io/badge/Python-3.8+-green)
![License](https://img.shields.io/badge/License-MIT-yellow)

## 🎯 Overview

This system demonstrates an intelligent operations monitoring pipeline that:
- **Simulates** realistic telecom events (network metrics, customer sentiment, support interactions)
- **Analyzes** data using LLM-powered agents (Nemotron via OpenRouter)
- **Decides** on proactive actions based on sentiment trends and network conditions
- **Visualizes** everything in a real-time Streamlit dashboard

## 🏗️ Architecture

```
┌─────────────┐
│  Simulator  │  Generates synthetic T-Mobile events
│  (Port 8000)│  - Network metrics (latency, packet loss)
└──────┬──────┘  - Social media posts (via Gemini)
       │         - Support interactions
       ▼
┌─────────────┐
│Agent/Listener│  Multi-agent AI system
│  (main.py)  │  - Perception Agent: Sentiment analysis
└──────┬──────┘  - Happiness Tracker: Stateful moving averages
       │         - Orchestrator: Makes proactive decisions
       ▼
┌─────────────┐
│   Reporter  │  Stores and serves reports
│  (Port 8001)│  - POST: Receives reports from agent
└──────┬──────┘  - GET: Serves data to dashboard
       │
       ▼
┌─────────────┐
│  Dashboard  │  Real-time visualization
│  Streamlit  │  - Combined & individual region charts
└─────────────┘  - Live events feed
                 - Agent decisions display
```

## ✨ Features

### 📊 Real-Time Dashboard
- **Combined Regional View**: Compare all regions (Dallas, New York, Chicago, Rural Iowa) on unified charts
- **Individual Metrics**: Deep dive into each region's performance
- **4 Key Metrics Tracked**:
  - Short-term sentiment scores
  - Short-term sentiment averages
  - Network latency (ms)
  - Packet loss (%)

### 🤖 Multi-Agent AI System
- **Agent 1 (Perception)**: Analyzes text for sentiment, topic, and urgency using Nemotron
- **Agent 2 (Happiness Tracker)**: Maintains stateful moving averages to detect trends
- **Agent 3 (Orchestrator)**: Makes proactive decisions (alerts, social replies, monitoring)

### 🎭 Intelligent Simulation
- **Regional Profiles**: Each region has unique characteristics
  - Dallas: Good signal, but negative sentiment (billing issues)
  - New York: Reliable coverage, positive sentiment
  - Chicago: Mixed performance and sentiment
  - Rural Iowa: Poor signal, negative sentiment
- **LLM-Generated Content**: Realistic tweets and support interactions via Gemini

## 🚀 Quick Start

### Prerequisites
- Python 3.8+
- Gemini API key (from [Google AI Studio](https://makersuite.google.com/app/apikey))
- OpenRouter API key (from [OpenRouter](https://openrouter.ai/))

### Installation

1. **Clone the repository**
```bash
git clone <your-repo-url>
cd tmobile-monitor
```

2. **Install dependencies**
```bash
pip install -r requirements.txt
```

3. **Set up environment variables**

Create a `.env` file in the project root:
```env
GEMINI_API_KEY=your_gemini_api_key_here
OPENROUTER_API_KEY=your_openrouter_api_key_here
```

### Running the System

**You need to run these 4 components in separate terminals:**

#### Terminal 1: Start the Simulator
```bash
python simulator.py
```
Generates synthetic events on port 8000.

#### Terminal 2: Start the Reporter Server
```bash
python reporter.py
```
Receives and stores reports on port 8001.

#### Terminal 3: Start the Agent System
```bash
python main.py
```
Polls simulator, analyzes data, makes decisions, sends reports.

#### Terminal 4: Start the Dashboard
```bash
streamlit run tmobile_dashboard.py
```
Opens browser at `http://localhost:8501` with real-time visualization.

## 📁 Project Structure

```
.
├── simulator.py              # Event generator (Gemini-powered)
├── reporter.py               # Report server (stores & serves data)
├── main.py                   # Multi-agent AI system (Nemotron-powered)
├── tmobile_dashboard.py      # Streamlit dashboard
├── requirements.txt          # Python dependencies
├── .env                      # API keys (not committed)
└── README.md                 # This file
```

## 🔧 Configuration

### Simulator Settings (simulator.py)
- `TICK_INTERVAL_SECONDS = 5`: How often to generate new events
- `PORT = 8000`: Web server port for serving data
- Regional profiles: Customize network ranges and sentiment biases

### Agent Settings (main.py)
- `SIMULATOR_URL`: Where to fetch events from
- `REPORTING_SERVER_URL`: Where to send reports
- `SHORT_TERM_WINDOW = 5`: Moving average window for quick trends
- `LONG_TERM_WINDOW = 10`: Moving average window for baseline
- `NEMOTRON_MODEL_ID`: LLM model for decision-making

### Dashboard Settings (tmobile_dashboard.py)
- `REPORTER_URL`: Where to fetch reports from
- `REFRESH_INTERVAL = 5`: Default auto-refresh rate (seconds)
- Chart history: 50 data points per metric per region

## 📊 Dashboard Features

### Combined Regional View
- **Tabbed Interface**: Switch between metrics to compare all regions
- **Unified Timeline**: See how regions perform relative to each other
- **Auto-refresh**: Configurable 1-10 second intervals

### Individual Region Details
- **Collapsible Sections**: Expand to see detailed multi-metric charts
- **Multi-axis Charts**: All 4 metrics on one chart with separate scales
- **Current Metrics**: Latest values displayed below each chart

### Live Events Feed
- **Filterable by Type**: Network metrics, social posts, support interactions, crashes
- **Color-coded**: Easy visual scanning
- **Last 15 Events**: Most recent activity across all regions

### Agent Decisions Panel
- **Decision History**: Last 10 proactive actions taken
- **Full Context**: View complete decision JSON
- **Happiness State**: See agent's assessment (TRENDING_UP, MAINTAIN_GOOD, etc.)

## 🧠 How the AI Works

### Sentiment Analysis
The Perception Agent uses Nemotron to analyze customer text and extract:
- **Sentiment**: positive, negative, or neutral
- **Topic**: network_signal, billing, customer_service, app_functionality, other
- **Urgency**: high, medium, or low

### Happiness Tracking
The stateful agent maintains dual moving averages:
- **Short-term (5 events)**: Detects recent changes
- **Long-term (10 events)**: Establishes baseline

**State Detection**:
- `TRENDING_UP`: Short-term crosses above long-term (Golden Cross)
- `TRENDING_DOWN`: Short-term crosses below long-term (Death Cross)
- `MAINTAIN_GOOD`: Short-term consistently above long-term
- `MAINTAIN_POOR`: Short-term consistently below long-term
- `PRIMING`: Collecting initial data (< 10 events)

### Decision Making
The Orchestrator Agent considers:
- Current happiness state
- Recent network metrics
- Analyzed customer posts

**Available Actions**:
1. `send_alert`: Notify teams (NetworkOps, BillingSupport, Marketing, AppDev)
2. `draft_social_reply`: Prepare public response for human review
3. `log_and_monitor`: Continue observing (for minor issues)

## 🎨 Customization

### Adding New Regions
Edit `simulator.py`:
```python
REGION_PROFILES.append({
    "name": "Los Angeles",
    "display_name": "Los Angeles",
    "type": "neutral",
    "network": {"latency_range": (40.0, 120.0), "loss_range": (0.3, 1.2), "spike_chance": 0.03},
    "prompt_bias": "Mixed sentiment about urban coverage..."
})
```

Update dashboard session state in `tmobile_dashboard.py` to include the new region.

### Changing LLM Models
**Simulator** (main.py, line 86):
```python
model = genai.GenerativeModel('gemini-2.0-flash-exp') 
```

**Agent** (main.py, line 31):
```python
NEMOTRON_MODEL_ID = "nvidia/nemotron-70b"  
```

### Adjusting Sensitivity
Make the agent more/less reactive by changing windows in `main.py`:
```python
SHORT_TERM_WINDOW = 3   # More reactive
LONG_TERM_WINDOW = 20   # More stable baseline
```

## 🐛 Troubleshooting

### Dashboard shows "No data"
- ✅ Ensure all 4 processes are running
- ✅ Check reporter.py is on port 8001
- ✅ Wait 10-15 seconds for first reports

### "Reporter Server: 🔴 Offline"
- ✅ Verify reporter.py is running
- ✅ Check if port 8001 is available
- ✅ Look for errors in reporter.py terminal

### No sentiment data appearing
- ✅ Agent needs time to collect posts
- ✅ Wait 15-30 seconds (3-6 ticks)
- ✅ Check main.py terminal for analysis logs

### API errors
- ✅ Verify API keys in `.env`
- ✅ Check API quota/rate limits
- ✅ Ensure internet connectivity

### High CPU usage
- ✅ Reduce dashboard refresh rate
- ✅ Increase TICK_INTERVAL in simulator
- ✅ Reduce GRAPH_HISTORY_LENGTH

## 📈 Performance Notes

- **Simulator**: Generates ~4-16 events per tick (every 5 seconds)
- **Agent**: Processes all events within 2-3 seconds
- **Dashboard**: Handles 200+ data points smoothly
- **Memory**: ~50-100 MB per component

## 🔮 Future Enhancements

- [ ] Persistent storage (SQLite/PostgreSQL)
- [ ] Email/SMS alerting integration
- [ ] Historical data export (CSV/JSON)
- [ ] Map visualization of regions
- [ ] Predictive analytics (trend forecasting)
- [ ] Multi-user authentication
- [ ] Custom alert thresholds
- [ ] Integration with real telecom APIs

## 📝 License

MIT License - feel free to use this for learning or commercial projects.

## 🤝 Contributing

Contributions welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## 💡 Use Cases

This architecture can be adapted for:
- **E-commerce**: Monitor customer satisfaction and order issues
- **SaaS**: Track app performance and user sentiment
- **Healthcare**: Alert on patient feedback and system metrics
- **Finance**: Detect anomalies in transaction patterns
- **Manufacturing**: Monitor equipment health and alerts

## 🙋 Support

For issues or questions:
- Open a GitHub issue
- Check existing issues for solutions
- Review troubleshooting section above

---

**Built with ❤️ using Gemini, Nemotron, and Streamlit**
