---
layout: post
title: Analisis Tren Film Berdasarkan Data IMDb
description: Aplikasi web interaktif berbasis Streamlit untuk menganalisis tren film dan memberikan rekomendasi berdasarkan data IMDb.
skills:
  - Python
  - Streamlit
  - Data Visualization
  - GitHub
main-image: /movie.jpg
---

## 🎬 Tentang Proyek

Proyek ini dikembangkan sebagai tugas eksplorasi data untuk menganalisis tren industri film dan membangun sistem rekomendasi sederhana menggunakan dataset **IMDb**. 

Aplikasi ini bertujuan membantu pengguna memahami pola film dari waktu ke waktu serta menemukan rekomendasi film yang sesuai dengan preferensi atau suasana hati mereka melalui antarmuka web yang interaktif.

<div style="display:flex; gap:12px; margin:20px 0; flex-wrap:wrap;">
  <a href="https://imdb-recommendation.streamlit.app" target="_blank" style="text-decoration:none; color:#333; background-color:#f4f4f4; padding:10px 18px; border-radius:8px; font-weight:600; border:1px solid #ddd; display:inline-flex; align-items:center;">
    🔗 Live Demo Streamlit
  </a>
  <a href="https://github.com/keishahz/IMDb-recommendation" target="_blank" style="text-decoration:none; color:#333; background-color:#f4f4f4; padding:10px 18px; border-radius:8px; font-weight:600; border:1px solid #ddd; display:inline-flex; align-items:center;">
    📂 Repositori GitHub
  </a>
</div>

<div class="summary" style="background: transparent; box-shadow: none; margin: 30px 0 30px 0; padding: 0;">
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

Analisis ini menggunakan **IMDb Dataset** yang berisi informasi film seperti tahun rilis, genre, rating, dan popularitas.

**Tech Stack yang Digunakan:**

* **Streamlit:** Digunakan untuk membangun antarmuka web interaktif.
* **Pandas & NumPy:** Digunakan untuk pembersihan data, eksplorasi, dan manipulasi dataset.
* **Matplotlib / Seaborn:** Digunakan untuk visualisasi tren film secara deskriptif.

---

## 💡 Key Insights (Temuan Utama)

Berdasarkan eksplorasi dan visualisasi data, diperoleh beberapa temuan utama:

### **1. Tren Produksi Film**
Jumlah film yang dirilis menunjukkan peningkatan signifikan dari tahun ke tahun, terutama setelah tahun 2000, seiring dengan berkembangnya industri hiburan dan platform digital.

### **2. Preferensi Genre**
Genre tertentu seperti **Drama, Action, dan Comedy** secara konsisten mendominasi produksi dan memiliki tingkat popularitas yang tinggi di berbagai periode waktu.

---

## 📸 Fitur Dashboard & Visualisasi

Berikut adalah fitur utama yang tersedia dalam aplikasi:

### **a. Visualisasi Tren Film**
Menyajikan grafik jumlah film dan distribusi genre berdasarkan tahun rilis untuk membantu pengguna memahami pola industri film.

<div style="width: 80%; margin: 0 auto; display: block;">
    {% include image-gallery.html images="trend.png" height="300" %}
</div>

### **b. Rekomendasi Berbasis Mood**
Fitur interaktif yang memungkinkan pengguna mendapatkan rekomendasi film berdasarkan genre atau suasana hati tertentu.

<div style="width: 80%; margin: 0 auto; display: block;">
    {% include image-gallery.html images="rec.png" height="300" %}
</div>

---

## 💻 Sorotan Kode

Berikut adalah implementasi fungsi `load_data` untuk memuat dataset IMDb secara efisien tanpa perlu membaca ulang file setiap kali aplikasi dijalankan:

```
# Optimasi performa dengan caching
@st.cache_data
def load_data():
df = pd.read_csv("imdb_movies.csv")

# Data Cleaning
df = df.dropna(subset=["title", "year", "genre", "rating"])
df["year"] = df["year"].astype(int)

return df
```
