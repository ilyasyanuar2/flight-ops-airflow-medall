# ✈️ Flight Operations Data Pipeline (Medallion Architecture)

**Penulis:** Ilyas Yanuar
**Target Role:** Data Engineer
**Lokasi:** Indonesia

---

## 📌 Gambaran Project

Project ini merupakan implementasi **pipeline data end-to-end** menggunakan **Apache Airflow** dan **Snowflake**, yang dirancang dengan pendekatan **Medallion Architecture (Bronze → Silver → Gold)**.

Pipeline ini melakukan ingest data penerbangan (flight data), memproses data secara bertahap, lalu memuat hasil agregasi KPI ke dalam **cloud data warehouse** untuk kebutuhan analitik dan reporting.

Project ini dibuat sebagai **portfolio Data Engineer**, dengan fokus tidak hanya pada hasil akhir, tetapi juga pada **tantangan nyata, proses debugging, dan pengambilan keputusan teknis** yang umum terjadi di lingkungan kerja.

---

## 🏗️ Gambaran Arsitektur

**Alur Data:**

1. **Bronze Layer**

   * Ingest data penerbangan mentah (format JSON)
   * Data disimpan apa adanya untuk keperluan traceability dan reprocessing

2. **Silver Layer**

   * Pembersihan dan normalisasi data
   * Penyeragaman schema dan validasi kualitas data

3. **Gold Layer**

   * Agregasi data menjadi KPI bisnis
   * Data siap dikonsumsi untuk analitik

4. **Data Warehouse**

   * Data Gold dimuat ke **Snowflake** menggunakan logika `MERGE` (upsert)

**Orkestrasi:** Apache Airflow (berbasis Docker)

---

## ⚙️ Tech Stack

* **Apache Airflow** – Workflow orchestration
* **Python** – Pemrosesan dan transformasi data
* **Pandas** – Manipulasi data
* **Snowflake** – Cloud data warehouse
* **PostgreSQL** – Database metadata Airflow
* **Docker & Docker Compose** – Containerized environment

---

## 📂 Struktur Project

```text
.
├── dags/
│   └── flights_ops_medallion_pipe.py
├── scripts/
│   ├── bronze_ingest.py
│   ├── silver_transform.py
│   ├── gold_aggregate.py
│   └── load_gold_to_snowflake.py
├── data/
│   ├── bronze/
│   ├── silver/
│   └── gold/
├── logs/
├── plugins/
├── sql/
├── docker-compose.yml
└── requirements.txt
```

---

## 🔄 Desain DAG

DAG Airflow dirancang dengan **dependensi linear** untuk menjaga konsistensi data:

```text
<img width="421" height="842" alt="Untitled Diagram drawio" src="https://github.com/user-attachments/assets/c7ef7dd7-e3e2-4934-84ee-cd7750a176c8" />

```

* **Jadwal:** Setiap 30 menit
* **Retry Policy:** Dapat dikonfigurasi
* **XCom:** Digunakan untuk meneruskan path file antar task

---

## 🧊 Model Data di Snowflake

**Tabel:** `FLIGHT_KPIS`

| Kolom          | Deskripsi                       |
| -------------- | ------------------------------- |
| window_start   | Timestamp window agregasi       |
| origin_country | Negara asal penerbangan         |
| total_flights  | Jumlah penerbangan              |
| avg_velocity   | Rata-rata kecepatan penerbangan |
| on_ground      | Jumlah pesawat di darat         |
| load_time      | Waktu data dimuat               |

**Primary Key:** `(window_start, origin_country)`

---

## 🚧 Tantangan & Error yang Dihadapi

Project ini mendokumentasikan error nyata selama pengembangan, antara lain:

* Error context database di Snowflake (`USE DATABASE` belum diset)
* Perbedaan perilaku CLI antara **Airflow 3.x dan 2.x**
* `FileNotFoundError` akibat data upstream belum tersedia
* Kesalahan penggunaan Python datetime (`NameError: strftime`)
* Error Pandas akibat ketidaksesuaian schema

Seluruh error diselesaikan melalui proses debugging, membaca dokumentasi, dan penyesuaian arsitektur.

---

## 📚 Pembelajaran Utama

* Pentingnya **context database & schema** di Snowflake
* Perbedaan signifikan antar versi Airflow
* Manfaat **Medallion Architecture** untuk audit dan traceability
* Penggunaan `MERGE` untuk load data yang idempotent
* Mendesain pipeline dengan visibilitas kegagalan

---

## 🎯 Kenapa Project Ini Relevan

Project ini menunjukkan:

* Orkestrasi data pipeline menggunakan Airflow
* Implementasi cloud data warehouse dengan Snowflake
* Struktur pipeline yang menyerupai environment produksi
* Kemampuan debugging dan problem solving

Project ini merepresentasikan workflow Data Engineer di dunia kerja nyata, bukan sekadar contoh ideal.

---

## 👤 Tentang Saya

**Ilyas Yanuar**
Calon Data Engineer dengan pengalaman membangun pipeline data end-to-end, melakukan debugging sistem terdistribusi, dan menggunakan modern data stack.

---

## 📬 Kontak

* LinkedIn: *(tambahkan URL LinkedIn)*
* GitHub: *(tambahkan URL repository GitHub)*

---

> *"Membangun data pipeline yang andal bukan soal menghindari error, tetapi merancang sistem yang mampu menanganinya dengan baik."*
