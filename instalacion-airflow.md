Empezamos actualizando el sistema operativo:

```bash
sudo apt update
sudo apt upgrade
```

Instalamos pip para python3:
```bash
sudo apt install python3-pip
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
