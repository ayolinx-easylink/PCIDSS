# 💳 PCI DSS – Full Card Data Flow (CDE Architecture)

## 📌 Overview
Diagram ini menggambarkan alur pemrosesan kartu kredit pada arsitektur **Full PCI DSS** menggunakan **Card Data Environment (CDE)**.

- Data kartu diproses di environment terisolasi (CDE)
- PAN disimpan dalam bentuk terenkripsi
- Key dikelola oleh HSM (bukan di aplikasi)
- Semua komunikasi menggunakan TLS
