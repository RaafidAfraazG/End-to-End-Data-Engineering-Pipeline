# End-to-End Data Engineering Pipeline

A real-time data streaming and processing system using Apache Airflow, Kafka, Spark, PostgreSQL, and Cassandra.

## System Architecture

![System Architecture](https://github.com/raafidafraazg/End-to-End-Data-Engineering-Pipeline/blob/main/Data%20engineering%20architecture.png)

## 🛠️ Tech Stack

- **Orchestration**: Apache Airflow
- **Streaming**: Apache Kafka + Zookeeper
- **Processing**: Apache Spark (Master + Workers)
- **Storage**: PostgreSQL (staging), Cassandra (analytics)
- **Containerization**: Docker & Docker Compose
- **Language**: Python 3.9+

## 📦 Prerequisites

- Docker (20.10+)
- Docker Compose (1.29+)
- 8GB RAM minimum, 4+ CPU cores
- 20GB disk space

## 🚀 Quick Start

```bash
# Clone repo
git clone https://github.com/your-username/End-to-End-Data-Engineering-Pipeline.git
cd End-to-End-Data-Engineering-Pipeline

# Start all services
docker-compose up -d

# Wait 2-3 minutes for services to be healthy
docker-compose ps
```

## 📊 Access Dashboards

- **Airflow**: http://localhost:8080 (airflow / airflow)
- **Kafka Control Center**: http://localhost:9021
- **Spark UI**: http://localhost:4040

## ▶️ Run the Pipeline

### Via Airflow UI
1. Open http://localhost:8080
2. Find `kafka_stream_dag`
3. Click play button to trigger

### Via Command Line
```bash
# Trigger DAG
docker-compose exec airflow airflow dags trigger kafka_stream_dag

# View logs
docker-compose logs -f airflow-scheduler
```

### Start Spark Streaming Job
```bash
docker-compose exec spark-master spark-submit \
  --master spark://master:7077 \
  --executor-cores 2 \
  --executor-memory 2g \
  /opt/spark-apps/spark_stream.py
```

## 📁 Project Structure

```
├── docker-compose.yml          # Container setup
├── requirements.txt            # Python dependencies
├── dags/
│   └── kafka_stream.py        # Airflow DAG
├── spark_jobs/
│   └── spark_stream.py        # Spark streaming job
└── scripts/
    ├── entrypoint.sh          # Startup script
    └── setup_cassandra.py     # Schema initialization
```

## 🔧 Pipeline Flow

1. **Ingestion**: Airflow fetches data from API → PostgreSQL
2. **Streaming**: Kafka reads from PostgreSQL → publishes to topic
3. **Processing**: Spark consumes Kafka → transforms data
4. **Storage**: Spark writes processed data → Cassandra

## 🏥 Troubleshooting

**Airflow DAG not triggering?**
```bash
docker-compose logs airflow-scheduler | tail -20
docker-compose exec airflow python -m py_compile dags/kafka_stream.py
```

**Kafka connection issues?**
```bash
docker-compose exec kafka kafka-broker-api-versions --bootstrap-server kafka:9092
```

**Cassandra not connecting?**
```bash
docker-compose exec cassandra cqlsh
docker-compose ps cassandra
```

**Services not starting?**
```bash
docker-compose down
docker system prune -a
docker-compose up -d
```

## 📊 Monitoring

```bash
# View service health
docker-compose ps

# Check logs
docker-compose logs -f [service-name]

# Monitor resource usage
docker stats

# List active topics
docker-compose exec kafka kafka-topics --list --bootstrap-server kafka:9092
```

## 📝 License

MIT License - See LICENSE file

## 🤝 Contributing

1. Fork the repo
2. Create feature branch (`git checkout -b feature/feature-name`)
3. Commit changes (`git commit -m 'feat: description'`)
4. Push to branch (`git push origin feature/feature-name`)
5. Open a Pull Request

## 📞 Support

- Issues: GitHub Issues
- Documentation: See docs/ directory
- Questions: GitHub Discussions


