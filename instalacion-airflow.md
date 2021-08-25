Empezamos actualizando el sistema operativo:

```bash
sudo apt update
sudo apt upgrade
```

Instalamos pip para python3, git y una libreria necesaria para Airflow:
```bash
sudo apt install python3-pip git libffi-dev
```

Actualizamos pip a su ultima version:
```bash
pip3 install -U pip
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
python3 -m pip install "apache-airflow[postgres]==${AIRFLOW_VERSION}" --constraint "${CONSTRAINT_URL}"
```

Es posible que el instalador se queje de algunos scripts inaccesibles porque no se encuentran en el PATH, especificamente ```airflow```, por lo que podemos agregar al archivo ```.bashrc```:
```bash
export PATH=/home/pi/.local/bin:$PATH
```
y posteriormente salir y entrar en una nueva sesión de ssh para forzar la ejecucion de ```.bashrc```.

Instalamos las dependencias de nuestros dags:
```bash
python3 -m pip install numpy scipy matplotlib pandas pyarrow ipython
```

Instalamos postgres:
```bash
sudo apt install postgresql
```

Cambiamos de usuario a postgres:
```bash
sudo su postgres
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

Solicitamos a airflow el valor actual de la conexion a la base de datos, forzando la creación del archivo de configuración:
```bash
airflow config get-value core sql_alchemy_conn
```

Dentro de ```~/airflow/airflow.cfg``` se cambia:
```python
dags_folder = /home/pi/airflow-dags
```
y
```python
executor = LocalExecutor
```
 y
```python
sql_alchemy_conn = postgresql+psycopg2://airflow_user:airflow_pass@127.0.0.1:5432/airflow_db
```
y
```python
load_examples = False
```

Se inicializa base de datos de Airflow:
```bash
airflow db init
```

Se crea usuario en Airflow:
```bash
airflow users create --username roberto --firstname Roberto --lastname "Cadena Vega" --role Admin --email roberto@cad3na.com
```

Se añaden usuarios y contraseñas para enviar emails a ```.bashrc```:
```bash
export AIRFLOW_EMAIL=airflow@example.com
export AIRFLOW_PASS=rubb3r-ducky
```

Se añaden aliases a ```.bashrc```:
```bash
alias afws="nohup airflow webserver --port 8080 > airflow-webserver.log &"
alias afsc="nohup airflow scheduler > airflow-scheduler.log &"
```

Se agrega repositorio de DAGs:
```bash
git clone https://github.com/cad3na/airflow-dags.git
```
