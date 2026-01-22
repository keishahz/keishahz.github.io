---
layout: post
title: Sistem Rekomendasi Kursus (End-to-End)
description: Proyek end-to-end untuk membangun sistem rekomendasi kursus, mulai dari web scraping hingga pemodelan content-based filtering.
skills:
  - Python
  - Scikit-Learn
  - NLP (TF-IDF)
  - Web Scraping (Selenium)
main-image: /ml.jpg
---

## 🏆 Tentang Proyek

Proyek ini merupakan **Proyek Akhir Program SISTECH 2025** yang berfokus pada pembangunan **sistem rekomendasi kursus secara end-to-end**, mulai dari proses akuisisi data mentah hingga pemodelan rekomendasi berbasis konten.

Studi kasus yang digunakan adalah platform **Class Central**, dengan tujuan membantu pengguna menemukan kursus yang relevan berdasarkan kemiripan judul kursus. Proyek ini menekankan integrasi antara **web scraping**, **Natural Language Processing (NLP)**, dan **machine learning** dalam satu alur sistem yang utuh.

<div style="display:flex; gap:12px; margin:20px 0; flex-wrap:wrap;">
  <a href="https://github.com/keishahz/Web-Scraping" target="_blank" style="text-decoration:none; color:#333; background-color:#f4f4f4; padding:10px 18px; border-radius:8px; font-weight:600; border:1px solid #ddd; display:inline-flex; align-items:center;">
    📂 Repository Web Scraping
  </a>
  <a href="https://github.com/keishahz/SystemRecommendation" target="_blank" style="text-decoration:none; color:#333; background-color:#f4f4f4; padding:10px 18px; border-radius:8px; font-weight:600; border:1px solid #ddd; display:inline-flex; align-items:center;">
    📂 Repository Sistem Rekomendasi
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

Proyek ini dibangun melalui beberapa tahapan utama yang mencerminkan alur kerja data science secara menyeluruh.

**Tech Stack yang Digunakan:**

* **Selenium:** Digunakan untuk melakukan web scraping data kursus secara dinamis dari platform Class Central.
* **NLTK:** Digunakan untuk preprocessing teks, termasuk *case folding*, *stopwords removal*, dan *stemming*.
* **Scikit-Learn:** Digunakan untuk membangun model TF-IDF dan menghitung Cosine Similarity.
* **Pandas:** Digunakan untuk pengolahan, penyimpanan, dan manipulasi data hasil scraping.

---

## 💡 Key Insights (Temuan Utama)

### **1. Efektivitas Content-Based Filtering**
Pendekatan *content-based filtering* menggunakan TF-IDF mampu menangkap kemiripan semantik antar judul kursus, meskipun hanya berbasis teks singkat.

### **2. Peran Penting Preprocessing Teks**
Kualitas rekomendasi sangat dipengaruhi oleh proses preprocessing. Pembersihan teks membantu model fokus pada kata kunci yang benar-benar merepresentasikan topik kursus.

---

## 💻 Sorotan Kode

### **a. Web Scraping Data Kursus (Selenium)**

Berikut adalah potongan kode untuk mengambil data judul kursus dari Class Central menggunakan Selenium dan menyimpannya dalam format terstruktur.

```

from selenium import webdriver
from selenium.webdriver.common.by import By
import pandas as pd
import time

driver = webdriver.Chrome()
driver.get("https://www.classcentral.com/")

courses = []

time.sleep(5)
course_elements = driver.find_elements(By.CLASS_NAME, "course-name")

for course in course_elements:
  courses.append(course.text)

driver.quit()

df = pd.DataFrame(courses, columns=["Title"])
df.to_csv("classcentral.csv", index=False)

```

---

### **b. Pemodelan Sistem Rekomendasi (TF-IDF & Cosine Similarity)**

Model rekomendasi dibangun dengan mengukur kemiripan antar judul kursus menggunakan TF-IDF dan Cosine Similarity.

```

from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.metrics.pairwise import cosine_similarity

course_titles = df['Title'].fillna('')

vectorizer = TfidfVectorizer(stop_words='english')
tfidf_matrix = vectorizer.fit_transform(course_titles)

cosine_sim = cosine_similarity(tfidf_matrix)

```

---

### **c. Fungsi Rekomendasi Kursus**

Fungsi berikut digunakan untuk menghasilkan rekomendasi kursus berdasarkan satu judul input.

```

indices = pd.Series(df.index, index=df['Title'])

def get_recommendations(title, top_n=5):
    idx = indices[title]
    sim_scores = list(enumerate(cosine_sim[idx]))
    sim_scores = sorted(sim_scores, key=lambda x: x[1], reverse=True)
    sim_scores = sim_scores[1:top_n+1]
    course_indices = [i[0] for i in sim_scores]
    return df['Title'].iloc[course_indices]

```
