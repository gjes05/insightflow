🤖 InsightsFlow: Network & Sentiment Monitor
A real-time dashboard that simulates regional network performance and customer sentiment, then uses an agentic AI (NVIDIA Nemotron) to analyze the data and recommend proactive actions.
<img width="1920" height="1020" alt="Screenshot 2025-11-09 082346" src="https://github.com/user-attachments/assets/e2c2393c-0a58-4636-a03d-50abfb7cdbcc" />
🚀 Key Features
* Real-Time Simulation: Uses Gemini to generate a realistic, continuous stream of network metrics (latency, loss) and customer feedback (tweets, support calls).
* AI-Powered Perception: Leverages NVIDIA Nemotron to perform real-time analysis on customer feedback, extracting sentiment, topic, and urgency.
* Stateful Happiness Tracking: Calculates short-term and long-term "happiness scores" for each region using a dual moving average to identify trends.
* Agentic Decision-Making: An AI orchestrator analyzes the combined network and happiness data to make proactive decisions, like send_alert or log_and_monitor.
* Live Streamlit Dashboard: A multi-faceted dashboard visualizes all key metrics, including time-series graphs of network health, customer happiness, and a live-updating log of all AI-driven actions.
🛠️ Project Architecture
This project runs as four separate, communicating components:
* simulator.py (The World):
   * Generates network/sentiment data.
   * Serves this data at http://localhost:8000.
* agent_listener.py (The Brain):
   * Pulls data from the simulator (:8000).
   * Analyzes it using Nemotron.
   * Makes a decision.
   * POSTs its final report (data + decision) to the Report Server (:8001).
* reporter_with_storage.py (The Cache):
   * Receives and stores the final reports from the AI agent.
   * Serves these stored reports at http://localhost:8001/reports.
* streamlit_dashboard.py (The Dashboard):
   * Fetches data from the Report Server (:8001/reports) every 5 seconds.
   * Visualizes the data for the user.
⚙️ How to Run
You will need four separate terminals open to run the full system.
1. Setup
First, clone the repository and set up the environment.
# Clone this repository
git clone [https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git](https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git)
cd YOUR_REPO_NAME

# Create and activate a virtual environment (recommended)
python -m venv venv
source venv/bin/activate  # On Windows, use: venv\Scripts\activate

# Install all required packages
pip install -r requirements.txt

# Create your secret .env file
cp .env.example .env

Now, edit the .env file and add your GEMINI_API_KEY and OPENROUTER_API_KEY.
2. Run the System
Run each command in a separate terminal, in this specific order.
Terminal 1: Start the Data Simulator
python simulator.py

(Wait until you see it serving at http://localhost:8000)
Terminal 2: Start the Report Server
python reporter_with_storage.py

(Wait until you see it listening on http://localhost:8001)
Terminal 3: Start the AI Agent
python agent_listener.py

(You will see this terminal start polling for data and making decisions)
Terminal 4: Start the Streamlit Dashboard
streamlit run streamlit_dashboard.py

(This will automatically open the dashboard in your web browser!)
