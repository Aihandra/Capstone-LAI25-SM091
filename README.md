# Capstone-LAI25-SM091
# Analisis Sentimen Ulasan Pengguna DJP Online

Proyek ini melakukan analisis sentimen berbasis leksikon serta klasifikasi menggunakan machine learning terhadap ulasan aplikasi DJP Online berbahasa Indonesia. Analisis meliputi preprocessing teks, penghitungan skor sentimen, visualisasi word cloud, topic modeling untuk komentar negatif, dan pelatihan model klasifikasi.

## Daftar Isi
- Dataset
- Preprocessing Teks
- Pembangunan Leksikon
- Penghitungan Sentimen
- Visualisasi
- Topic Modeling
- Model Klasifikasi Sentimen
- Evaluasi Model
- Instalasi

---

## Dataset
Ulasan dibaca dari file CSV dan dimuat ke DataFrame (`df_all`) dengan kolom `content` berisi teks ulasan mentah.

## Preprocessing Teks
Langkah-langkah:
1. Konversi huruf menjadi huruf kecil  
2. Menghapus emoji menggunakan regex  
3. Menghapus karakter non-alfanumerik  
4. Tokenisasi  
5. Konversi kata tidak baku menggunakan kamus dari `kamuskatabaku.xlsx`

Contoh:
```python
text = remove_emoji(text)
text = re.sub(r'[^a-z0-9\s]', '', text)
tokens = text.split()
tokens_baku = konversi_kata_baku(tokens)
```

## Pembangunan Leksikon
Leksikon Positif berasal dari:
- `lexicon_positive.csv`
- `positive.tsv` (dataset InSet)
- Frasa/frasa manual seperti “mantab”, “josss”, dsb.

Leksikon Negatif dari:
- `lexicon_negative.csv`
- `negative.tsv` (dataset InSet)
- Frasa/frasa manual seperti “terlalu lama”, “error sistem”

Seluruh leksikon digabung menjadi `lexicon_combined`.

## Penghitungan Sentimen
Setiap ulasan yang telah ditokenisasi diberi skor berdasarkan jumlah bobot dari `lexicon_combined`.

Labeling sentimen:
```python
def label_sentiment(score):
    if score > 0:
        return 'positive'
    elif score < 0:
        return 'negative'
    else:
        return 'neutral'
```

Distribusi sentimen hasil skor leksikon:
- Negatif: 7724  
- Netral: 321  
- Positif: 320  

## Visualisasi
Visualisasi word cloud untuk masing-masing kategori sentimen menggunakan pustaka `WordCloud`.

Contoh:
```python
tampilkan_wordcloud(text_positive, 'WordCloud - Sentimen Positif')
```

## Topic Modeling
Topik dari komentar negatif dianalisis menggunakan LDA:

Langkah:
- Filter komentar dengan label 'negative'
- Tokenisasi + hapus stopword Bahasa Indonesia
- Vektorisasi dengan `CountVectorizer(max_df=0.9, min_df=5)`
- Latih LDA (`n_components=5`)
- Tampilkan 10 kata kunci teratas per topik

## Model Klasifikasi Sentimen
Preprocessing:
- TF-IDF vectorizer (`ngram_range=(1,2)`, `max_features=5000`)

Model yang digunakan:
1. Naive Bayes  
2. Logistic Regression  
3. Random Forest  
4. SVM (LinearSVC)  
5. SVM dengan `class_weight='balanced'` untuk menangani imbalance data

Split data:
```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

## Evaluasi Model
Evaluasi menggunakan:
- Akurasi  
- Precision, Recall, F1-Score  
- Confusion Matrix

Performa model SVM (balanced):
- Akurasi: 0.9588
- F1-Score:
  - Negatif: 0.98
  - Netral: 0.67
  - Positif: 0.63

Confusion matrix divisualisasikan menggunakan `seaborn`.

## Instalasi
Instal seluruh dependensi dengan:
```bash
pip install -r requirements.txt
```
