---
layout: post
title: Dashboard Visualisasi Data Iklim
description: Dashboard interaktif berbasis Streamlit untuk memantau korelasi antara transisi energi terbarukan, emisi CO2, dan dampak bencana di Pasifik.
skills:
  - Python
  - Streamlit
  - Data Visualization
  - Plotly Express
  - GitHub
main-image: /climate.jpg
---

## 🏆 Tentang Proyek

Proyek ini dikembangkan sebagai tugas besar untuk mata kuliah Visualisasi Data dan diikutsertakan dalam **Pacific Data Viz Challenge 2025**. Dasbor ini memvisualisasikan tantangan ganda yang dihadapi oleh negara-negara Kepulauan Pasifik: urgensi mitigasi perubahan iklim melalui energi terbarukan dan perlindungan terhadap dampak bencana alam.

Menggunakan data dari **Pacific Data Hub (Blue Pacific 2050)**, dashboard ini berfungsi sebagai alat bantu pengambilan keputusan bagi pembuat kebijakan untuk melihat apakah transisi energi yang dilakukan memiliki korelasi langsung dengan pengurangan dampak bencana.

**Tautan Aplikasi:** [**Live Demo Streamlit**](https://visdat-climate.streamlit.app/) | [**Repositori GitHub**](https://github.com/keishahz/DATAVIZ)

---

## 📊 Metodologi & Data

Analisis ini menggunakan dataset **Blue Pacific 2050 (Thematic Area 5)** yang mencakup indikator iklim dan bencana di negara-negara Pasifik.

**Tech Stack yang Digunakan:**

* **Streamlit:** Framework utama untuk membangun antarmuka web interaktif yang responsif.
* **Pandas:** Digunakan untuk *cleaning* dan standardisasi nama negara agar konsisten antar tabel.
* **Plotly Express:** Digunakan untuk membuat visualisasi interaktif (seperti Scatter Plot dan Line Chart) yang bisa di-*zoom* dan di-*hover*.

---

## 💡 Key Insights (Temuan Utama)

Berdasarkan analisis visual pada dashboard, berikut adalah temuan kuncinya:

### 1. Efektivitas Transisi Energi
Negara-negara yang konsisten meningkatkan kapasitas energi terbarukan (Watt/kapita) cenderung menunjukkan tren penurunan emisi CO₂ yang positif. Ini membuktikan bahwa investasi hijau mulai membuahkan hasil dalam mitigasi karbon.

### 2. Gap Mitigasi vs Adaptasi
Meskipun emisi karbon berhasil ditekan, data menunjukkan bahwa **jumlah korban terdampak bencana tidak serta-merta berkurang secara instan**.

* **Masalah:** Bencana alam masih sering terjadi dengan dampak besar.
* **Akar Masalah:** Mitigasi iklim (pengurangan emisi) bersifat jangka panjang, sedangkan perlindungan bencana butuh infrastruktur fisik (tanggul, peringatan dini) yang bersifat langsung.

---
## 📸 Fitur Dashboard & Visualisasi

Berikut adalah beberapa visualisasi kunci yang disajikan dalam dashboard ini:

### a. Analisis Korelasi Multivariat
Memvisualisasikan hubungan timbal balik antara tiga variabel krusial: kapasitas energi terbarukan, emisi CO₂, dan populasi terdampak.

<div style="width: 80%; margin: 0 auto; display: block;">
    {% include image-gallery.html images="newplot.png" height="300" %}
</div>

### b. Automated Insights (Wawasan Otomatis)
Fitur pintar yang secara otomatis menerjemahkan data grafik menjadi narasi teks, sehingga pengguna awam bisa langsung paham tanpa harus pusing baca grafik.

<div style="width: 80%; margin: 0 auto; display: block;">
    {% include image-gallery.html images="conclusion.png" height="300" %}
</div>
---

## 💻 Sorotan Kode (Code Snippets)
Berikut adalah implementasi fungsi `load_data` agar data tidak perlu dimuat ulang setiap kali user mengklik tombol:

```python
@st.cache_data
def load_data():
    file_path = 'Blue Pacific 2050_ Climate Change And Disasters.csv'
    df = pd.read_csv(file_path)
    
    # Data Cleaning & Renaming
    df = df.rename(columns={
        'Pacific Island Countries and territories': 'Country',
        'TIME_PERIOD': 'Year',
        'OBS_VALUE': 'Renewable Capacity (W/capita)'
    })
    
    # Type Casting untuk konsistensi data
    df = df.dropna(subset=['Year', 'Renewable Capacity (W/capita)'])
    df['Year'] = df['Year'].astype(int)
    
    return df
