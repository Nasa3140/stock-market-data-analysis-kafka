# stock-market-data-analysis-kafka
stock-market-data-analysis-kafka-project

---

```markdown
# 📈 Kafka Stock Market Streaming Project using EC2, S3, Glue, and Athena
<img width="1850" height="1017" alt="Architecture (2)" src="https://github.com/user-attachments/assets/558302c6-e235-4731-8a4c-ba053ee0f644" />

This project demonstrates **real-time stock market data streaming** using **Apache Kafka** hosted on an AWS EC2 instance. The streamed data is consumed, stored in an **S3 bucket**, and later analyzed using **AWS Glue and Athena**.

---

## 🔧 Tech Stack

- ⚙️ Apache Kafka (v3.3.1)
- ☁️ AWS EC2 (Amazon Linux 2023)
- 🐍 Python 3.10 with:
  - `kafka-python`
  - `pandas`
  - `s3fs`
  - `boto3`
- 🗃️ Amazon S3
- 🔎 AWS Glue (Crawler + Table)
- 📊 Amazon Athena (SQL queries on S3 data)

---

## 🔄 Project Workflow

```

<img width="817" height="635" alt="Folder structure" src="https://github.com/user-attachments/assets/00c5f37d-831b-4433-ad8d-69680ad3ab8f" />

→ Query stock data using SQL

```

---

## 📁 Folder Structure

```

├── data/
│   └── indexProcessed.csv             # Sample stock data
├── producer.py                        # Kafka producer script
├── consumer\_to\_s3.py                  # Kafka consumer script (saves to S3)
├── requirements.txt                   # Python packages used
├── screenshots/                       # EC2 setup, Kafka config, Glue UI
└── README.md

````

---

## 🚀 How to Run

### ✅ Prerequisites

- Python 3.10+
- AWS EC2 instance (Amazon Linux) with ports `22`, `9092` open
- AWS S3 Bucket created
- AWS CLI configured with access keys

### 🔌 Step 1: Setup Kafka on EC2

1. SSH into EC2:

   ```bash
   ssh -i "your-key.pem" ec2-user@<your-ec2-ip>
````

2. Install Java:

   ```bash
   sudo yum install java-1.8.0-openjdk
   ```

3. Download and extract Kafka:

   ```bash
   wget https://downloads.apache.org/kafka/3.3.1/kafka_2.12-3.3.1.tgz
   tar -xvf kafka_2.12-3.3.1.tgz
   cd kafka_2.12-3.3.1
   ```

4. Edit `config/server.properties` to use public IP:

   ```bash
   sudo nano config/server.properties
   # Replace advertised.listeners with:
   advertised.listeners=PLAINTEXT://<your-ec2-public-ip>:9092
   ```

5. Start ZooKeeper:

   ```bash
   bin/zookeeper-server-start.sh config/zookeeper.properties
   ```

6. Open new terminal and start Kafka server:

   ```bash
   export KAFKA_HEAP_OPTS="-Xmx256M -Xms128M"
   bin/kafka-server-start.sh config/server.properties
   ```

7. Create Kafka Topic:

   ```bash
   bin/kafka-topics.sh --create --topic demo_test --bootstrap-server <EC2-IP>:9092 --replication-factor 1 --partitions 1
   ```

---

### 💻 Step 2: Run Python Producer (on Windows)

1. Install dependencies:

   ```bash
   pip install kafka-python pandas
   ```

2. Run:

   ```python
   python producer.py
   ```

* Reads stock data from CSV
* Sends one record per second to Kafka topic

---

### 💾 Step 3: Run Python Consumer + Upload to S3

1. Install:

   ```bash
   pip install kafka-python s3fs boto3
   ```

2. Run:

   ```python
   python consumer_to_s3.py
   ```

* Consumes messages from Kafka
* Saves each message to S3 as `.json`

---

### 🧠 Step 4: Analyze in Glue + Athena

1. Create AWS Glue Crawler:

   * Source: Your S3 Bucket
   * Target: Glue database

2. Run crawler → creates table

3. Go to **Athena**:

   * Select database
   * Run SQL queries like:

     ```sql
     SELECT * FROM stock_market_data limit 10;
     ```

---

## 📊 Sample Dataset

| Date       | Open   | High   | Low    | Close  | Volume  | Adj Close |
| ---------- | ------ | ------ | ------ | ------ | ------- | --------- |
| 2023-01-01 | 5000.5 | 5100.1 | 4990.3 | 5080.2 | 1200000 | 5080.2    |

---

👤 Nasira Kadgaonkar
🔗 [narisense.com](https://narisense.com) • [LinkedIn](https://www.linkedin.com/in/nasira-kadgaonkar/)

---
This project is for educational/demo purposes. You're welcome to adapt it!

```



