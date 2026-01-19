---
layout: post
title: Dashboard Analisis Bisnis E-Commerce (SSDC 2025)
description: Dashboard interaktif berbasis Streamlit untuk menganalisis performa penjualan, efisiensi logistik, dan sentimen pelanggan guna mendukung pengambilan keputusan strategis.
skills:
  - Python
  - Streamlit
  - Pandas
  - Plotly Express
  - Data Cleaning
main-image: /assets/projects/ssdc-dashboard/main-preview.jpg
---

## Deskripsi Proyek

Proyek ini dikembangkan untuk kompetisi **Sebelas Maret Statistics Dashboard Competition (SSDC) 2025**. Dashboard ini bertujuan untuk mengolah data e-commerce yang kompleks menjadi wawasan bisnis yang dapat ditindaklanjuti (actionable insights).

Aplikasi ini membantu pemangku kepentingan dalam memantau KPI utama seperti total penjualan, kepuasan pelanggan, dan performa pengiriman secara *real-time*.

### Tautan Penting

* [**Lihat Live Demo Aplikasi**](https://ssdc-dashboard-ecommerce.streamlit.app/)
* [**Source Code di GitHub**](https://github.com/keishahz/SSDC-Dashboard-ECommerce)

---

## Tampilan Antarmuka

Berikut adalah tampilan visualisasi data dan fitur interaktif pada dashboard.

{% include image-gallery.html images="/assets/projects/ssdc-dashboard/screenshot1.jpg, /assets/projects/ssdc-dashboard/screenshot2.jpg" height="300" %}

<br>

## Fitur Utama

1. **Filter Data Dinamis**: Memungkinkan pengguna menyaring data berdasarkan rentang tanggal, kategori produk, dan status pesanan.
2. **Analisis Kualitas Produk**: Mengidentifikasi kategori produk dengan penjualan tertinggi dan ulasan terendah untuk perbaikan kualitas.
3. **Peta Geografis Pelanggan**: Visualisasi persebaran pelanggan untuk strategi ekspansi pasar.
4. **Analisis Logistik**: Membandingkan estimasi waktu pengiriman dengan durasi aktual.

---

## Sorotan Teknis (Code Snippets)

Berikut adalah beberapa bagian kode kunci yang digunakan dalam pembangunan dashboard ini.

### 1. Integrasi Data (Data Merging)

Tantangan utama dalam proyek ini adalah menyatukan 9 dataset berbeda (Order, Customer, Review, Product, dll). Berikut adalah fungsi yang digunakan untuk menggabungkan dataset tersebut menjadi satu DataFrame terpusat:

```python
# Fungsi untuk memuat dan menggabungkan seluruh dataset
@st.cache_data
def load_all_data():
    # ... (code loading csv) ...
    
    # Merge datasets secara bertahap
    df_merged = loaded_data['orders'].copy()
    merge_steps = [
        (loaded_data['order_items'], 'order_id'),
        (loaded_data['products'], 'product_id'),
        (loaded_data['product_translation'], 'product_category_name'),
        (loaded_data['order_reviews'], 'order_id'),
        (loaded_data['customers'], 'customer_id'),
        (loaded_data['sellers'], 'seller_id'),
        (loaded_data['order_payments'], 'order_id')
    ]
    
    for df_to_merge, on_key in merge_steps:
        df_merged = pd.merge(df_merged, df_to_merge, on=on_key, how='left')
    
    # Feature Engineering: Menghitung durasi pengiriman
    df_merged['delivery_duration_days'] = (
        df_merged['order_delivered_customer_date'] - df_merged['order_purchase_timestamp']
    ).dt.days

    return df_merged, loaded_data['geolocation']
```

### 2. Visualisasi Interaktif dengan Plotly

Menggunakan plotly.express untuk membuat grafik batang yang responsif guna menampilkan kategori produk dengan penjualan tertinggi.

```python
st.markdown("##### Top 10 Kategori Produk Berdasarkan Total Penjualan")

# Mengelompokkan data berdasarkan kategori dan menjumlahkan harga
top_categories = df_filtered.groupby('product_category_name_english')['price'].sum().nlargest(10).reset_index()

# Membuat Bar Chart Horizontal
fig = px.bar(
    top_categories, 
    x='price', 
    y='product_category_name_english',
    labels={'price': 'Total Penjualan (R$)', 'product_category_name_english': 'Kategori Produk'},
    orientation='h', 
    color='price', 
    color_continuous_scale=px.colors.sequential.Viridis
)

fig.update_layout(yaxis={'categoryorder':'total ascending'})
st.plotly_chart(fig, use_container_width=True)
```

### 3. Logika Filter Sidebar

Implementasi sidebar Streamlit untuk memfilter data secara dinamis berdasarkan input pengguna.

```python
# Widget Input Tanggal
date_range = st.sidebar.date_input(
    "Rentang Tanggal Pembelian:",
    value=(min_date, max_date),
    min_value=min_date,
    max_value=max_date
)

# Widget Multiselect Kategori
selected_categories = st.sidebar.multiselect(
    "Pilih Kategori Produk:", options=all_categories, default=all_categories
)

# Filtering Dataframe
if len(date_range) == 2:
    start_date, end_date = pd.to_datetime(date_range[0]), pd.to_datetime(date_range[1])
    df_filtered = df_main[
        (df_main['order_purchase_timestamp'] >= start_date) & 
        (df_main['order_purchase_timestamp'] <= end_date) &
        (df_main['product_category_name_english'].isin(selected_categories))
    ].copy()
```

---

## Pembelajaran & Pengalaman

Melalui proyek ini, saya memperoleh pengalaman dalam:
- Menangani dataset besar dan kompleks dengan multiple relationships
- Membuat dashboard interaktif yang user-friendly
- Feature engineering dan data preprocessing
- Presentasi data untuk business decision making
