# 🏥 Residential Care Home Health Monitoring System

A real-time health monitoring system designed to **track residents’ vital signs** in a care home environment.  
This project integrates **Kafka** for streaming data, **PySpark** for real-time analytics, and a **Python-based IoT simulator** to generate live sensor data — all deployed using **Docker containers**.

---

## ⚙️ Architecture Overview

```
┌──────────────┐       ┌───────────┐       ┌──────────────┐
│ IoT Devices  │ --->  │   Kafka   │ --->  │   PySpark     │
│ (Simulator)  │       │ Streaming │       │   Processor   │
└──────────────┘       └───────────┘       └──────────────┘
                           │                     │
                           ▼                     ▼
                    ┌────────────┐         ┌──────────────┐
                    │  FastAPI   │ <----- │ Kafka (Output)│
                    │  Backend   │         └──────────────┘
                    └────────────┘
                           │
                           ▼
                    ┌────────────┐
                    │  Frontend  │
                    │ (Dashboard)│
                    └────────────┘
```

---

## 🚀 Features

- **IoT Simulation:** Generates synthetic vital-sign data (heart rate, temperature, oxygen levels, etc.)
- **Kafka Streaming:** Streams data in real-time between components.
- **PySpark Processing:** Cleans, aggregates, and analyzes streaming data.
- **FastAPI Backend:** Exposes processed data and WebSocket streams.
- **Containerized Deployment:** Fully managed with Docker & Docker Compose.
- **Kafka UI Dashboard:** Visualize topics and data flow in real time.

---

## 🧰 Tech Stack

| Component         | Technology Used |
|-------------------|-----------------|
| Backend API       | FastAPI (Python) |
| Data Streaming    | Apache Kafka |
| Data Processing   | PySpark |
| IoT Data Source   | Python IoT Simulator |
| Containerization  | Docker & Docker Compose |
| Monitoring        | Kafka UI Dashboard |

---

## 🐳 Quick Start (with Docker)

### 1. Clone the Repository
```bash
git clone https://github.com/<your-username>/carehome-health-monitoring.git
cd carehome-health-monitoring
```

### 2. Launch the Stack
```bash
docker-compose up -d
```

### 3. Verify Services
```bash
docker-compose ps
```

### 4. Access Services
| Service | URL |
|----------|-----|
| **API (FastAPI)** | http://localhost:8000 |
| **API Docs (Swagger UI)** | http://localhost:8000/docs |
| **Kafka UI** | http://localhost:8080 |
| **WebSocket** | ws://localhost:8000/ws |

---

## 🧪 Manual Setup (Optional)

If you prefer running components separately:

```bash
# Backend
cd backend
pip install -r requirements.txt
uvicorn main:app --reload

# IoT Simulator
cd simulator
pip install -r requirements.txt
python iot_simulator.py

# PySpark Processor
cd processor
pip install -r requirements.txt
spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.5.0 pyspark_processor.py
```

---

## ⚙️ Configuration

Set environment variables in `.env`:
```env
KAFKA_BOOTSTRAP_SERVERS=localhost:9092
KAFKA_TOPIC_INPUT=vital-signs-raw
KAFKA_TOPIC_OUTPUT=vital-signs-processed
NUM_RESIDENTS=12
UPDATE_INTERVAL=3
FASTAPI_PORT=8000
```

You can adjust `NUM_RESIDENTS` and `UPDATE_INTERVAL` in the simulator to control data flow.

---

## 📊 Monitoring & Testing

- **Kafka Topics**
  ```bash
  kafka-topics --list --bootstrap-server localhost:9092
  ```
- **View Messages**
  ```bash
  kafka-console-consumer --bootstrap-server localhost:9092 --topic vital-signs-raw --from-beginning
  ```
- **Spark UI:** [http://localhost:4040](http://localhost:4040)
- **WebSocket Load Test:**
  ```bash
  npm install -g artillery
  artillery quick --count 10 --num 50 ws://localhost:8000/ws
  ```

---

## 🧱 Project Structure

```
carehome-health-monitoring/
├── backend/
│   ├── main.py              # FastAPI backend + WebSocket
│   ├── requirements.txt
│   └── Dockerfile
├── simulator/
│   ├── iot_simulator.py     # IoT data generator
│   ├── requirements.txt
│   └── Dockerfile
├── processor/
│   ├── pyspark_processor.py # PySpark processing logic
│   ├── requirements.txt
│   └── Dockerfile
├── docker-compose.yml
├── .env
└── README.md
```

---

## 🔐 Production Notes

- Secure Kafka (SSL/SASL) and FastAPI (HTTPS/WSS)
- Add authentication (JWT tokens)
- Enable structured logging and metrics
- Use persistent Kafka storage and monitoring tools (Prometheus, Grafana)

---

## 🤝 Contributing

1. Fork the repo  
2. Create a new branch (`feature/your-feature-name`)  
3. Commit your changes  
4. Push and submit a Pull Request  

---

## 📄 License

This project is licensed under the **MIT License**.

---

## 👤 Author

**Your Name**  
📧 your.email@example.com  
🔗 [LinkedIn / GitHub / Portfolio link]
