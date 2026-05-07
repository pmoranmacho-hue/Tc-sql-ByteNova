
# Tc-sql-ByteNova

Base de datos relacional sobre nuestra empresa ByteNova, un e-commerce de electrónica y accesorios tecnológicos.

---

## Descripción del proyecto

ByteNova necesita una base de datos centralizada que le permita gestionar su catálogo de productos, registrar pedidos, controlar estados de envío, gestionar pagos y detectar incidencias. Este proyecto diseña e implementa dicha base de datos en Google BigQuery, poblada con datos sintéticos generados con Python.

---

## Equipo y distribución de tareas

| Integrante | Rol | Rama |
|---|---|---|
| Pablo | Scrum Master | `feature/setup-bigquery` |
| Lucía | Data Modeler | `feature/er-diagram` |
| Ana | Data Engineer A (tablas maestras) | `feature/data-generation-master` |
| Nil | Data Engineer B | Generación de transacciones y carga | `feature/data-generation-orders` |
| Enrique | QA / Docs | `feature/queries-validation` |

---

## Estructura del repositorio

```
TC-SQL-BYTENOVA/
├── data/                        # Vacío — los datos están en BigQuery
├── docs/
│   ├── er_diagram.png           # Diagrama entidad-relación
│   └── normalizacion.md         # Justificación de las 3 formas normales
├── notebooks/
│   ├── 01_setup_bigquery.ipynb  # Configuración del dataset y tablas en BigQuery
│   ├── 02_generate_data.ipynb   # Generación de datos sintéticos con Faker
│   └── 03_queries_verification.ipynb  # Queries analíticas de verificación
|   └── 04_data_engineer_b_transactions_load.ipynb  # Pipeline alternativo de carga masiva de transacciones:
                                                 # genera datos transaccionales, aplica validaciones,
                                                 # transforma formatos y realiza la inserción en BigQuery

├── .env.example                 # Plantilla de variables de entorno (sin credenciales reales)
├── .gitignore
├── README.md
└── requirements.txt
```

---

## Modelo de datos

El dataset `bytenova_db` en BigQuery contiene las siguientes tablas:

| Tabla | Descripción |
|---|---|
| `categories` | Categorías de productos (smartphones, laptops, audio, etc.) |
| `customers` | Clientes registrados con información de contacto y origen |
| `products` | Catálogo de productos con precio, coste y stock |
| `orders` | Pedidos realizados por los clientes |
| `order_items` | Líneas de detalle de cada pedido |
| `payments` | Información de pago por pedido |
| `reviews` | Valoraciones de los clientes sobre productos comprados |

---

## Setup del entorno

### 1. Clonar el repositorio

```bash
git clone <url-del-repositorio>
cd TC-SQL-BYTENOVA
```

### 2. Crear y activar el entorno virtual

```bash
python -m venv venv

# Mac / Linux
source venv/bin/activate

# Windows
venv\Scripts\activate
```

### 3. Instalar dependencias

```bash
pip install -r requirements.txt
```

### 4. Configurar credenciales de Google Cloud

Copia el archivo de ejemplo y rellena con tus credenciales reales:

```bash
cp .env.example .env
```

El archivo `.env` debe contener:

```
GCP_PROJECT_ID=tu-proyecto-gcp
BQ_DATASET_ID=bytenova_db
GOOGLE_APPLICATION_CREDENTIALS=./credentials/service-account.json
```

> **Importante:** El archivo `.env` y la carpeta `credentials/` nunca deben subirse al repositorio. Están incluidos en `.gitignore`.

---

## Ejecución

Ejecuta los notebooks en orden:


### Flujo principal

Ejecuta los notebooks en orden:

1. `01_setup_bigquery.ipynb` — crea el dataset y las tablas en BigQuery  
2. `02_generate_data.ipynb` — genera y carga los datos sintéticos  
3. `03_queries_verification.ipynb` — ejecuta las queries analíticas y validaciones de datos  

### Flujo alternativo (Data Engineering)

Opcionalmente, puedes ejecutar el pipeline orientado a ingeniería de datos:

1. `01_setup_bigquery.ipynb` — crea el dataset y las tablas en BigQuery  
2. `04_data_engineer_b_transactions_load.ipynb` — genera, transforma y carga transacciones masivas en BigQuery  
3. `03_queries_verification.ipynb` — ejecuta las queries analíticas y validaciones de datos  
---

## Stack tecnológico

- Python 3.x
- Google BigQuery
- Jupyter Notebooks
- Git + GitHub (feature branches y Pull Requests)

**Librerías principales:**

```
google-cloud-bigquery
google-auth
db-dtypes
pandas
faker
python-dotenv
pyarrow
```

---

## Flujo de trabajo Git

1. Cada integrante trabaja en su propia rama de feature
2. Al terminar, abre un Pull Request hacia `main`
3. El PR requiere al menos 1 aprobación antes de hacer merge
4. Usa Squash and Merge para mantener el historial limpio
5. Sincroniza `main` localmente tras cada merge:

```bash
git checkout main
git pull origin main
```

**Formato de commits:**

```
feat: crear esquema de tabla customers
fix: corregir FK de order_items
docs: añadir justificación de 3NF
```

---

## Deadline

Entrega antes de la sesión de presentación del Sprint 8.

**Contenido mínimo del repositorio para la entrega:**
- Diagrama ER en `docs/`
- Notebooks completos y ejecutables
- `README.md` con instrucciones de setup
- `requirements.txt` actualizado
- `.env.example` como plantilla
- `.gitignore` correctamente configurado
- Al menos 1 PR mergeado por integrante
