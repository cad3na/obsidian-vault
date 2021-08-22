Empezamos actualizando el sistema operativo:

```bash
sudo apt update
sudo apt upgrade
```

Instalamos pip para python3:
```bash
sudo apt install python3-pip
```

Actualizamos pip a su ultima version
```bash
pip3 install -U pip
```

Instalamos una libreria necesaria para Airflow:
```bash
sudo apt install libffi-dev
```

Definimos algunas variables de bash para construir el nombre del archivo de requerimientos:
```bash
AIRFLOW_VERSION=2.1.2
PYTHON_VERSION="$(python3 --version | cut -d " " -f 2 | cut -d "." -f 1\-2)"
```

Construimos el nombre del archivo de requerimientos:
```bash
CONSTRAINT_URL="https://raw.githubusercontent.com/apache/airflow/constraints-${AIRFLOW_VERSION}/constraints-${PYTHON_VERSION}.txt"
```

Instalamos la version de Airflow correspondiente a nuestro sistema:
```bash
pip3 install "apache-airflow[postgres,google]==${AIRFLOW_VERSION}" --constraint "${CONSTRAINT_URL}"
```

Instalamos las dependencias de nuestros dags:
```bash
pip3 install numpy scipy matplotlib pandas pyarrow
```

Instalamos postgres:
```bash
sudo apt install postgresql libpq-dev postgresql-client postgresql-client-common
```

Cambiamos de usuario a postgres:
```bash
sudo su postgres
```

Creamos un usuario
```bash
createuser pi -P --interactive
```

Entramos a la consola de postgres:
```bash
psql
```

Creamos base de datos y usuario para Airflow
```SQL
CREATE DATABASE airflow_db;
CREATE USER airflow_user WITH PASSWORD 'airflow_pass';
GRANT ALL PRIVILEGES ON DATABASE airflow_db TO airflow_user;
```

Dentro de airflow.cfg se cambia:
```python
executor = LocalExecutor
```
 y
```python
sql_alchemy_conn = postgresql+psycopg2://airflow_user:airflow_pass@127.0.0.1:5432/airflow_db
```

Se crea usuario en Airflow:
```bash
airflow users create --username roberto --firstname Roberto --lastname "Cadena Vega" --role Admin --email roberto@cad3na.com
```

Se añaden aliases a ```.bashrc```:
```bash
alias afws="nohup airflow webserver --port 8080 > airflow-webserver.out 2> airflow-webserver.err < /dev/null &"
alias afsc="nohup airflow scheduler -D > airflow-scheduler.out 2>airflow-scheduler.err < /dev/null &"
```

Se instala git:
```bash
sudo apt install git
```

Se agrega repositorio de DAGs:
```bash
git clone https://github.com/cad3na/airflow-dags.git
```

Se modifica directorio de DAGs en ```airflow.cfg```:
```bash

```