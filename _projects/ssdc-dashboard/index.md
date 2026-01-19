---
layout: post
title: E-Commerce Business Insight (SSDC 2025)
description: Finalis Sebelas Maret Statistics Fair 2025. Dashboard interaktif untuk menganalisis preferensi pembeli, kualitas produk, dan efisiensi logistik menggunakan Brazilian E-Commerce Dataset.
skills:
  - Python
  - Streamlit
  - Plotly Express
  - Pandas
  - Data Storytelling
main-image: ./_projects/ssdc-dashboard/ekomer.jpg
---

## 🏆 Tentang Proyek

Proyek ini merupakan karya tim **"Mohon Bantuannya"** yang disusun untuk kompetisi **Sebelas Maret Statistics Dashboard Competition (SSDC) 2025**. 

Tujuan utama dari dashboard ini adalah membantu pemangku kepentingan (stakeholder) perusahaan e-commerce dalam mengambil keputusan strategis berbasis data. Analisis difokuskan pada tiga pilar utama: **Preferensi Pembeli, Kualitas Produk, dan Kinerja Pengiriman**.

### Tautan Penting

* [**Lihat Live Demo Aplikasi**](https://ssdc-dashboard-ecommerce.streamlit.app/)
* [**Source Code di GitHub**](https://github.com/keishahz/SSDC-Dashboard-ECommerce)

---

## 📊 Metodologi & Data

Analisis ini menggunakan **Brazilian E-Commerce Public Dataset by Olist** (2016-2018) yang terdiri dari 11 tabel terelasi (Pesanan, Pelanggan, Geolokasi, Ulasan, dll).

**Tech Stack yang Digunakan:**

* **Streamlit:** Framework utama untuk membangun antarmuka web interaktif
* **Pandas:** Digunakan untuk *data wrangling* dan penggabungan (merging) dataset yang kompleks
* **Plotly Express:** Digunakan untuk visualisasi interaktif seperti *Scatter Mapbox* (Peta) dan *Bar Chart* dinamis

---

## 💡 Key Insights (Temuan Utama)

Berdasarkan analisis visual pada dashboard, berikut adalah temuan kuncinya:

### 1. Konsentrasi Pendapatan & Masalah Kualitas

Pendapatan perusahaan sangat bergantung pada kategori *Bed Bath Table* (cama_mesa_banho) dan *Health Beauty* (beleza_saude). Namun, volume penjualan tinggi ini tidak berbanding lurus dengan kepuasan pelanggan.

* **Masalah:** Ditemukan banyak ulasan negatif pada kategori terlaris.
* **Akar Masalah:** Ketidaksesuaian barang dengan deskripsi/foto, kualitas material rendah, dan kerusakan saat pengiriman

### 2. Efisiensi Logistik vs Durasi Aktual

Meskipun mayoritas pesanan tiba **lebih cepat dari estimasi** sistem, durasi pengiriman aktual rata-rata masih memakan waktu sekitar **12 hari**. Ini menjadi titik lemah yang berpotensi mengurangi kepuasan pelanggan.

### 3. Konsentrasi Pasar Geografis

Visualisasi peta (*Scatter Mapbox*) menunjukkan pasar sangat terkonsentrasi di wilayah tenggara Brazil, khususnya provinsi **São Paulo (SP)**. Ini menandakan ketergantungan tinggi pada satu wilayah, namun juga peluang ekspansi ke area lain.

---

## 📸 Fitur Dashboard & Visualisasi

Berikut adalah beberapa visualisasi kunci yang disajikan dalam dashboard ini:

### a. Peta Persebaran Pelanggan (Scatter Mapbox)

Visualisasi interaktif yang memetakan lokasi pelanggan berdasarkan koordinat latitude/longitude untuk melihat densitas pasar secara *real-time*.

{% include image-gallery.html images="/assets/projects/ssdc-dashboard/map-preview.jpg" height="400" %}

### b. Analisis Ulasan & Sentimen

Grafik batang yang membandingkan total penjualan dengan rata-rata skor ulasan untuk mengidentifikasi produk yang "Laris tapi Mengecewakan".

{% include image-gallery.html images="/assets/projects/ssdc-dashboard/chart-review.jpg" height="300" %}

---

## 💻 Sorotan Kode (Code Snippets)

Salah satu tantangan terbesar adalah menggabungkan 9 dataset berbeda menjadi satu kesatuan data yang utuh. Berikut adalah implementasi fungsi `load_all_data` menggunakan Pandas:

```python
@st.cache_data
def load_all_data():
    # Load dataset csv...
    # Proses merging bertahap untuk mempertahankan integritas data
    merge_steps = [
        (loaded_data['order_items'], 'order_id'),
        (loaded_data['products'], 'product_id'),
        (loaded_data['order_reviews'], 'order_id'),
        (loaded_data['customers'], 'customer_id'),
        # ... (tabel lainnya)
    ]
    
    for df_to_merge, on_key in merge_steps:
        df_merged = pd.merge(df_merged, df_to_merge, on=on_key, how='left')
    
    # Feature Engineering: Menghitung Delivery Performance
    # (Selisih waktu estimasi vs aktual)
    df_merged['delivery_performance_days'] = (
        df_merged['order_estimated_delivery_date'] - df_merged['order_delivered_customer_date']
    ).dt.days

    return df_merged
```

---

## 🎯 Rekomendasi Strategis

Berdasarkan insight di atas, kami merekomendasikan:

1. **Improvement Kualitas Produk:** Lebih ketat dalam quality control untuk kategori terlaris, khususnya *Bed Bath Table* dan *Health Beauty*.
2. **Optimasi Logistik:** Bermitra dengan *logistics partner* yang lebih cepat untuk mengurangi durasi pengiriman rata-rata menjadi < 10 hari.
3. **Ekspansi Geografis:** Menargetkan wilayah di luar São Paulo untuk mengurangi konsentrasi pasar dan meningkatkan revenue.
4. **Monitoring Real-Time:** Menggunakan dashboard ini untuk memantau KPI harian dan membuat keputusan operasional dengan cepat.
