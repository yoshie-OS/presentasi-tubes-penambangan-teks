---
theme: default
background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
highlighter: shiki
lineNumbers: false
info: |
  ## Spam/Ham Classification using SVM
  Text Mining Project using Python
drawings:
  persist: false
transition: slide-left
title: Klasifikasi Spam/Ham dengan SVM
---

# Klasifikasi Spam/Ham dengan SVM

## Text Mining dengan Machine Learning

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Tekan <kbd>space</kbd> untuk lanjut <carbon:arrow-right class="inline"/>
  </span>
</div>

---
transition: fade-out
---

# Agenda

<Toc maxDepth="1"></Toc>

---
layout: two-cols
---

# Dataset Overview

<v-clicks>

- **Dataset**: spam-ham_v2.csv
- **Jumlah Data**: 5,572 baris
- **Kolom**:
  - `v1`: Label (ham/spam)
  - `v2`: Isi pesan
- **Encoding**: ISO-8859-1

</v-clicks>

::right::

<div class="mt-10">

```python
data.head()
```

```
     v1                                   v2
0   ham  Go until jurong point, crazy...
1   ham  Ok lar then Joking wif u oni
2  spam  Free entry in 2 a wkly comp...
3   ham  U dun say so early hor...
4   ham  Nah I don't think he goes...
```

</div>

---

# Tahapan Preprocessing

<v-clicks>

## 1. Data Cleaning
- Hapus kolom tidak perlu
- Rename kolom (`v1` → `label`, `v2` → `message`)
- Hapus duplikat

## 2. Text Preprocessing
- Lowercase semua teks
- Hapus simbol khusus dan tanda baca
- Hapus URL
- Hapus angka
- Hapus kata pendek (< 3 karakter)

</v-clicks>

---

# Kode Preprocessing

```python {all|1-3|6-15}
def preprocess_text(text):
    # Hapus simbol khusus dan tanda baca
    text = re.sub(r'[^\w\s]', '', text)
    
    # Hapus URL
    text = re.sub(r'http\S+|www.\S+', '', text)
    
    # Hapus angka
    text = re.sub(r'\d+', '', text)
    
    # Hapus kata-kata singkat (<3 karakter)
    text = ' '.join(word for word in text.split() if len(word) > 2)
    
    return text
```

---
layout: image-right
image: ./1.png
---

# Distribusi Data

<v-clicks>

- **Ham**: ~4,500 pesan (87%)
- **Spam**: ~700 pesan (13%)
- Dataset **tidak seimbang** (imbalanced)
- Perlu perhatian khusus pada metrik evaluasi

</v-clicks>

---
layout: image-right
image: ./2.png
---

# Analisis Panjang Pesan

<v-clicks>

## Temuan Menarik:
- **Ham**: Mayoritas pendek (< 100 karakter)
- **Spam**: Cenderung lebih panjang
- Panjang pesan bisa jadi fitur pembeda

</v-clicks>

---

# Feature Extraction

<v-clicks>

## CountVectorizer (Term Frequency)
```python
vectorizer = CountVectorizer(
    stop_words=stopwords.words('english')
)
X = vectorizer.fit_transform(data['message'])
```

## Pembagian Data
```python
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)
```

</v-clicks>

---

# Model: Support Vector Machine

<v-clicks>

## Konfigurasi
```python
svm = SVC(kernel='linear')
svm.fit(X_train, y_train)
```

## Kenapa SVM?
- Efektif untuk data teks high-dimensional
- Kernel linear cocok untuk text classification
- Robust terhadap overfitting

</v-clicks>

---
layout: image-right
image: ./3.png
---

# Hasil Evaluasi

<v-clicks>

## Metrik Performance
- **Akurasi**: 98%
- **Presisi**: 98%
- **Recall**: 87%
- **F1-Score**: 92%

## Confusion Matrix
- True Positive: 125
- True Negative: 889
- False Positive: 3
- False Negative: 18

</v-clicks>

---
layout: two-cols
---

# Word Cloud Analysis

<div class="mt-4">
  <img src="./4.png" class="h-80" />
  <p class="text-center text-sm">Ham Messages</p>
</div>

::right::

<div class="mt-4">
  <img src="./5.png" class="h-80" />
  <p class="text-center text-sm">Spam Messages</p>
</div>

---
layout: two-cols
---

# Feature Importance

<div class="mt-4">
  <img src="./6.png" class="h-80" />
  <p class="text-center text-sm">Top Ham Words</p>
</div>

::right::

<div class="mt-4">
  <img src="./7.png" class="h-80" />
  <p class="text-center text-sm">Top Spam Words</p>
</div>

---

# Key Insights

<v-clicks>

## Kata Kunci Ham
- Personal: "liked", "thought", "ask"
- Informal: "ltgt", "gonna", "bak"
- Conversational tone

## Kata Kunci Spam
- Promosi: "claim", "private", "service"
- Urgency: "statement", "wining"
- Commercial: "freephone", "voicemail"

</v-clicks>

---

# Kesimpulan

<v-clicks>

1. **Model SVM Linear berhasil** mengklasifikasi spam dengan akurasi 98%

2. **Panjang pesan** menjadi indikator penting (spam cenderung lebih panjang)

3. **Vocabulary berbeda signifikan** antara ham dan spam

4. **Trade-off**: Recall lebih rendah (87%) - beberapa spam lolos sebagai ham

5. **Rekomendasi**: 
   - Ensemble methods untuk meningkatkan recall
   - Feature engineering tambahan
   - Handling imbalanced data

</v-clicks>

---
layout: center
class: text-center
---

# Terima Kasih

## Ada Pertanyaan?

<div class="text-2xl">
  🤖 Spam Detection with SVM 📧
</div>
