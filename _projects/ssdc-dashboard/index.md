---
layout: post
title: E-Commerce Business Insight Dashboard
description: Dashboard interaktif untuk menganalisis performa bisnis e-commerce, kualitas produk, dan logistik menggunakan data Olist.
skills:
  - Python
  - Streamlit
  - Pandas
  - Plotly
  - Data Analysis
main-image: /ecomm.jpg
---

## 🏆 Tentang Proyek

Proyek ini merupakan karya tim **"Mohon Bantuannya"** yang disusun untuk kompetisi **Sebelas Maret Statistics Dashboard Competition (SSDC) 2025**. 

Tujuan utama dari dashboard ini adalah membantu pemangku kepentingan (stakeholder) perusahaan e-commerce dalam mengambil keputusan strategis berbasis data. Analisis difokuskan pada tiga pilar utama: **Preferensi Pembeli, Kualitas Produk, dan Kinerja Pengiriman**.

<div style="display:flex; gap:12px; margin:20px 0; flex-wrap:wrap;">
  <a href="https://ssdc-dashboard-ecommerce.streamlit.app/" target="_blank" style="text-decoration:none; color:#333; background-color:#f4f4f4; padding:10px 18px; border-radius:8px; font-weight:600; border:1px solid #ddd; display:inline-flex; align-items:center;">
    🔗 Live Demo Streamlit
  </a>
  <a href="https://github.com/keishahz/SSDC-Ecommerce-Dashboard" target="_blank" style="text-decoration:none; color:#333; background-color:#f4f4f4; padding:10px 18px; border-radius:8px; font-weight:600; border:1px solid #ddd; display:inline-flex; align-items:center;">
    📂 Repositori GitHub
  </a>
</div>

<div class="summary" style="background: transparent; box-shadow: none; margin: 30px 0; padding: 0;">
  <div class="skills-card">
    <h2>Keahlian yang Digunakan</h2>
    <div class="skills-list">
      {% for skill in page.skills %}
      <span class="skill">{{ skill }}</span>
      {% endfor %}
    </div>
  </div>
</div>

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

{% include image-gallery.html images="/assets/projects/ssdc-dashboard/map-preview.png" height="400" %}

### b. Analisis Ulasan & Sentimen

Grafik batang yang membandingkan total penjualan dengan rata-rata skor ulasan untuk mengidentifikasi produk yang "Laris tapi Mengecewakan".

{% include image-gallery.html images="/assets/projects/ssdc-dashboard/chart-review.png" height="300" %}

---

## 💻 Sorotan Kode

### **a. Konfigurasi & Load Data Dashboard**

Potongan kode berikut menunjukkan konfigurasi awal Streamlit dan proses pemuatan serta penggabungan dataset utama.

```
st.set_page_config(
    page_title="Dashboard E-Commerce SSDC 2025",
    layout="wide",
    initial_sidebar_state="expanded"
)

@st.cache_data
def load_all_data():
    df_orders = pd.read_csv("data/orders_dataset.csv")
    df_items = pd.read_csv("data/order_items_dataset.csv")
    df_reviews = pd.read_csv("data/order_reviews_dataset.csv")

    df = df_orders.merge(df_items, on="order_id", how="left") \
                   .merge(df_reviews, on="order_id", how="left")
    return df
```
### **b. Feature Engineering Pengiriman**

Digunakan untuk mengevaluasi durasi pengiriman dan ketepatan estimasi.

```
df['delivery_duration_days'] = (
    df['order_delivered_customer_date'] -
    df['order_purchase_timestamp']
).dt.days

df['delivery_performance_days'] = (
    df['order_estimated_delivery_date'] -
    df['order_delivered_customer_date']
).dt.days
```
### **c. Analisis Ulasan Negatif per Kategori**

Potongan ini digunakan untuk mengidentifikasi kategori dengan performa ulasan terburuk.

```
avg_reviews = df.groupby(
    'product_category_name_english'
)['review_score'].mean().nsmallest(5)
```

---

## 🎯 Rekomendasi Strategis

Berdasarkan insight di atas, kami merekomendasikan:

1. **Improvement Kualitas Produk:** Lebih ketat dalam quality control untuk kategori terlaris, khususnya *Bed Bath Table* dan *Health Beauty*.
2. **Optimasi Logistik:** Bermitra dengan *logistics partner* yang lebih cepat untuk mengurangi durasi pengiriman rata-rata menjadi < 10 hari.
3. **Ekspansi Geografis:** Menargetkan wilayah di luar São Paulo untuk mengurangi konsentrasi pasar dan meningkatkan revenue.
4. **Monitoring Real-Time:** Menggunakan dashboard ini untuk memantau KPI harian dan membuat keputusan operasional dengan cepat.
