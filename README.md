# Auto-Test-Framework

A lightweight and scalable **Python API Test Automation Framework** built with `Pytest`, `Requests`, and `Allure`.  
It supports **data-driven testing**, **custom assertions**, and an integrated **Flask mock server** for local API simulation.

---

## Features

- **Layered Architecture**  
  - `testcase/`: interface-level test cases  
  - `base/`: reusable API utilities  
  - `common/`: core framework (request, assertion, logging, connection)  
  - `data/`: YAML-driven test data  
  - `mock_server/`: local Flask-based mock service  

- **Data-Driven Testing**  
  - Test cases automatically load parameters from YAML files.  
  - One test script can drive multiple test scenarios.

- **Reusable Request Layer**  
  - Unified `RequestBase` encapsulation with `requests.Session()`  
  - Supports GET/POST, token handling, and global config from `config.yaml`.

- **Allure Reporting Integration**  
  - Beautiful visual test reports with detailed steps and assertions.  
  - Command:  
    ```bash
    pytest --alluredir=report/
    allure serve report/
    ```

- **Mock Server for Local Testing**  
  - Built-in Flask mock API (`mock_server/api_server/base/flask_service.py`)  
  - Start server:  
    ```bash
    python mock_server/api_server/base/flask_service.py
    ```

- **Database & Cache Support**  
  - `MySQL`, `ClickHouse`, `Redis`, and `MongoDB` connection utilities.  
  - SSH support via `paramiko`.

---

## Project Structure

````

Test-Automation-Framework/
├── base/                 # API utilities (RequestBase)
├── common/               # Core functions (request, assert, db, log)
├── conf/                 # Global configuration (YAML)
├── data/                 # Test data files
├── testcase/             # Test scripts (pytest)
├── mock_server/          # Local Flask mock API
├── report/               # Allure test reports
└── README.md

````

---

## Installation

```bash
git clone https://github.com/RemiYu/Auto-Test-Framework.git
cd Test-Automation-Framework
python3 -m venv venv
source venv/bin/activate

# Install all dependencies
pip install pytest requests PyYAML allure-pytest loguru jsonpath pytest-xdist \
flask flask_jwt_extended flask_cors \
clickhouse_sqlalchemy sqlalchemy pymysql redis pymongo paramiko pandas \
xlrd openpyxl numpy
````

---

## Run Tests

**Start the Mock Server**

```bash
python mock_server/api_server/base/flask_service.py
```

**Run Test Cases**

```bash
pytest -v
```

**Generate Allure Report**

```bash
pytest --alluredir=report/
allure serve report/
```

---

## Example Test Case

```python
import pytest
from base.apiutil import RequestBase

class TestLogin:
    @pytest.mark.parametrize("case", RequestBase.read_yaml("data/login.yaml"))
    def test_login(self, case):
        res = RequestBase().send(case["method"], case["url"], case["data"])
        assert res.status_code == case["expect"]["code"]
```

---

## Tech Stack

* **Language**: Python 3.10+
* **Framework**: Pytest + Allure
* **Mock Server**: Flask
* **Database Support**: MySQL, ClickHouse, MongoDB, Redis
* **Other Tools**: YAML, Requests, Loguru, Paramiko

---

> *This project demonstrates a full-stack API automation testing workflow with reusable architecture, data-driven test design, and mock API simulation.*