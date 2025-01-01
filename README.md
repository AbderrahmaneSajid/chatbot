# Project Setup Instructions

Follow the steps below to set up and run the project.

## Prerequisites

- Python installed on your system
- Docker installed and running
- Docker Desktop (optional but recommended)

---

## Setup Steps

### 1. Prepare the Virtual Environment

1. Navigate to the project root directory:
    ```bash
    cd <folder_root>
    ```
2. Delete the existing virtual environment (if any):
    ```bash
    rm -rf venv
    ```
3. Create a new virtual environment:
    ```bash
    python -m venv venv
    ```
4. Activate the virtual environment:
    - On Windows:
      ```bash
      ./venv/Scripts/activate
      ```
    - On macOS/Linux:
      ```bash
      source ./venv/bin/activate
      ```
5. Install the required dependencies:
    ```bash
    pip install -r requirements.txt
    ```

---

### 2. Set Up Neo4j Database

1. Run the Neo4j container with the following command:
    ```bash
    docker run --name neo4j-apoc -e NEO4J_AUTH=neo4j/password \
        -p 7474:7474 -p 7687:7687 \
        -e NEO4J_apoc_export_file_enabled=true \
        -e NEO4J_apoc_import_file_enabled=true \
        -e NEO4J_apoc_import_file_use__neo4j__config=true \
        -e NEO4J_PLUGINS='["apoc"]' \
        -e NEO4J_dbms_security_procedures_unrestricted=apoc.* \
        -e NEO4J_dbms_security_procedures_allowlist=apoc.* \
        neo4j:latest
    ```

2. Open Docker Desktop and locate the Neo4j container.

3. Use the `Exec` option in Docker Desktop to access the container.

4. Copy the APOC plugin into the Neo4j plugins folder:
    ```bash
    cp /var/lib/neo4j/labs/apoc-5.26.0-core.jar /var/lib/neo4j/plugins/
    ```

---

### 3. Run the ETL Script

1. Navigate to the ETL script directory:
    ```bash
    cd hospital_neo4j_etl/src
    ```

2. Execute the script:
    ```bash
    python hospital_bulk_csv_write.py
    ```

---

### 4. Run the Chatbot API

1. Navigate to the API source directory:
    ```bash
    cd chatbot_api/src
    ```

2. Start the API server:
    ```bash
    uvicorn main:app --host 0.0.0.0 --port 8000
    ```

3. Access the API documentation:
    [http://localhost:8000/docs](http://localhost:8000/docs)

---

### 5. Run the Chatbot Frontend

1. Navigate to the frontend source directory:
    ```bash
    cd chatbot_frontend/src
    ```

2. Start the Streamlit application:
    ```bash
    streamlit run main.py
    ```

3. Access the frontend application:
    [http://localhost:8501/](http://localhost:8501/)

---

### 6. Test the Application

Try the following questions in the chatbot interface:

- **Which hospitals are in the hospital system?**
- **How much was billed for patient 789's stay?**
