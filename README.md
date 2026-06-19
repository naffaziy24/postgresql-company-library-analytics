# postgresql-company-library-analytics
Relational database design and complex query analytics (Joins, Aggregations, Sub-queries) using PostgreSQL

## 📌 Project Overview
Proyek ini mendemonstrasikan implementasi database relasional dan pengujian query analitik kompleks menggunakan **PostgreSQL**. Analisis dilakukan pada dua skema database terpisah: **Sistem Manajemen Perusahaan (Company)** dan **Sistem Sirkulasi Perpustakaan (Library)**.

Tujuan utama dari proyek ini adalah mentransformasikan skema konseptual (ERD) menjadi database fisik, menjaga integritas referensial data, serta menjawab berbagai kebutuhan informasi bisnis melalui query SQL tingkat lanjut.

---

## 🛠️ Tech Stack & Skills Demonstrated
* **DBMS:** PostgreSQL
* **SQL Concepts:** Data Definition Language (DDL), Data Manipulation Language (DML), Transaction Control Language (TCL)
* **Advanced Querying:** Multi-table Inner/Left Joins, Aggregate Functions (`SUM`, `AVG`, `COUNT`), `GROUP BY` & `HAVING`, Nested Sub-queries (`NOT EXISTS`, `NOT IN`).

---

## 📊 Database Schemas
Proyek ini menggunakan dua arsitektur database:
1. **Company Database:** Mengelola data karyawan (`EMPLOYEE`), departemen (`DEPARTMENT`), lokasi (`DEP_LOCATIONS`), proyek (`PROJECT`), dan tanggungan (`DEPENDENT`).
2. **Library Database:** Mengelola sirkulasi buku (`BOOK`), data peminjam (`BORROWER`), cabang perpustakaan (`LIBRARY_BRANCH`), serta histori peminjaman (`BOOK_LOANS`).

---
