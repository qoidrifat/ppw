---
title: Word Embeding

---

## **Word Embeding**

### **Skip-gram**
Skip-Gram adalah metode yang digunakan untuk membuat komputer memahami arti kata dengan melihat kata-kata yang ada di sekitarnya.

Penjelasan Sederhana:
Bayangkan saat kita membaca kalimat, dan fokus pada satu kata. Skip-Gram akan mencoba mempelajari kata-kata apa saja yang biasanya muncul di dekat kata tersebut.

Contoh: Kalimat: "Anak itu sedang bermain bola."

Jika kata yang dipelajari adalah "bermain," Skip-Gram akan mencoba memahami kata-kata yang sering muncul di dekatnya, seperti "anak" dan "bola."
Dengan cara ini, Skip-Gram membantu komputer mengerti hubungan antar kata, sehingga bisa "memahami" makna kata berdasarkan konteksnya.

Jadi, intinya Skip-Gram adalah cara untuk mengajarkan komputer memahami arti kata dari kata-kata di sekitarnya. 

#### **Contoh Perhitungan**

contoh dalam perhitungannya adalah sebagai berikut  dengan kalimat yang akan kita gunakan adalah : **"kucing itu sedang tidur di atas sofa"**


#### **Representasi one-hot encoding untuk setiap kata dalam kalimat tersebut**

| No | Kata   | One-Hot Encoding        |
|----|--------|-------------------------|
| 1  | Kucing | [1, 0, 0, 0, 0, 0, 0]   |
| 2  | itu    | [0, 1, 0, 0, 0, 0, 0]   |
| 3  | sedang | [0, 0, 1, 0, 0, 0, 0]   |
| 4  | tidur  | [0, 0, 0, 1, 0, 0, 0]   |
| 5  | di     | [0, 0, 0, 0, 1, 0, 0]   |
| 6  | atas   | [0, 0, 0, 0, 0, 1, 0]   |
| 7  | sofa   | [0, 0, 0, 0, 0, 0, 1]   |


#### **Berikut kata target dan konteks**

| Kata target  | kata konteks  |
|--------------|---------------|
| kucing       | itu           |
| itu          | kucing, sedang|
| sedang       | itu, tidur    |
| tidur        | sedang, di    |
| di           | tidur, atas   |
| atas         | di, sofa      |
| sofa         | atas          |


#### **Membuat word Embedding**

Setelah mendapatkan pasangan kata, gunakan model embedding untuk mengonversi setiap kata menjadi vektor. Misalkan kita tentukan ukuran dimensi vektor embedding adalah 3

buat nilai embedding acak untuk setiap kata sebagai berikut :

| Kata   | Word Embedding       |
|--------|----------------------|
| Kucing | [0.1, 0.2, 0.3]      |
| itu    | [0.4, 0.5, 0.6]      |
| sedang | [0.7, 0.8, 0.9]      |
| tidur  | [0.3, 0.2, 0.1]      |
| di     | [0.6, 0.5, 0.4]      |
| atas   | [0.9, 0.8, 0.7]      |
| sofa   | [0.8, 0.2, 0.1]      |



#### **Iterasi ke 1**
sekarang akan menghitung representasi untuk kata "itu" menggunakan tetangga kiri dan kanan yaitu 'kucing' dan 'sedang'
1. untuk "itu" dengan tetangga kiri 'kucing':
        Vector "itu" = [0.4,0.5,0.6]
        Vector "kucing" = [0.1,0.2,0.3]
        
2. Untuk "itu" dengan tetangga "sedang":
        Vector "sedang" = [0.7,0.8,0.9]
     
     Vector itu = $$\frac{[0.1, 0.2, 0.3] + [0.7, 0.8, 0.9]}{2} = \frac{[0.8, 1.0, 1.2]}{2} = [0.4, 0.5, 0.6]
$$

Kesimpulan
Perhitungan di atas merupakan bagian dari iterasi pertama, di mana kita menggunakan pasangan kata-tetangga untuk membentuk representasi awal dari kata. Setelah beberapa iterasi, representasi vektor akan diperbarui dan menjadi lebih akurat dalam merepresentasikan hubungan semantik antar kata.

#### **Iterasi ke 2**
Menggunakan contoh kata sama dengan kata target "itu" dan tetangganya "kucing" serta "sedang". Misalkan setelah iterasi pertama, kita menghitung error dan ingin memperbarui vektor embedding untuk kata "itu".

Vektor Awal dari Iterasi Pertama: Pada iterasi pertama, kita telah menghitung representasi sementara sebagai berikut:
kucing : [0.1,0.2,0.3]
itu: [0.4,0.5,0.6]
sedang : [0.7,0.8,0.9]

Perbarui vektor embedding untuk kata target "itu" dengan memperhitungkan error

Misalnya, untuk pasangan "itu" dengan tetangga "kucing" dan "sedang", kita perbarui embedding "itu" menggunakan rumus:

Rumus:$$\text{vektor_itu} = \text{vektor_itu} -\text{learning rate} \times \text{gradien error}
$$


1. Gradien error mengacu pada perbedaan antara prediksi model dan hasil yang diharapkan. Misalkan gradien yang dihasilkan adalah 0.05 untuk setiap dimensi vektor.
2. Pembaruan dilakukan berdasarkan gradient descent, di mana bobot embedding diubah sedikit untuk memperbaiki error. Misalkan kita menggunakan learning rate sebesar 0.1

Berikut perhitungannya

**Pembaruan vektor itu:**

a. Dimensi pertama vector itu:
- vektor_itu_baru=0.4−(0.1×0.05)=0.395

b. Dimensi kedua vector itu:
- vektor_itu_baru=0.5−(0.1×0.05)=0.495

c. Dimensi Ketiga vector itu:
- vektor_itu_baru=0.6−(0.1×0.05)=0.595

Setelah pembaruan untuk pasangan pertama, vektor embedding untuk "itu" adalah:[0.395,0.495,0.595]


**Pembaruan Vektor "kucing"**
Kita akan memperbarui vektor embedding untuk kata "kucing" berdasarkan error yang dihitung setelah iterasi pertama.
a. Dimensi pertama vector kucing
- vektor_kucing_baru=0.1−(0.1×0.05)=0.095

b. Dimensi kedua vector kucing
- vektor_kucing_baru=0.2−(0.1×0.05)=0.195

c. Dimensi ketiga vector kucing
- vektor_kucing_baru=0.3−(0.1×0.05)=0.295

Hasil Vektor "kucing" setelah Iterasi Kedua =[0.095,0.195,0.295]


Perbarui Vektor Embedding Tetangga "sedang": Sama halnya, kita lakukan pembaruan untuk tetangga "sedang" menggunakan vektor embedding "itu". Misalkan gradien error untuk "sedang" juga sebesar 0.05 .

a. Dimensi pertama vector sedang
- vektor_sedang_baru=0.7−(0.1×0.05)=0.695

b. Dimensi kedua vector sedang
- vektor_sedang_baru=0.8−(0.1×0.05)=0.795

c. Dimensi ketiga vector sedang
- vektor_sedang_baru=0.9−(0.1×0.05)=0.895

Setelah pembaruan untuk tetangga "sedang" =[0.695,0.795,0.895]

Rekapitulasi Vektor Embedding 
Setelah iterasi kedua, berikut adalah vektor embedding yang diperbarui untuk kata "itu", "kucing", dan "sedang":
- Vektor "itu": [0.395,0.495,0.595]
- Vektor "sedang": [0.695,0.795,0.895]
- Vektor "kucing": [0.095,0.195,0.295]

Kesimpulan
Pada iterasi kedua, kita telah memperbarui vektor embedding untuk kata "itu", "kucing", dan "sedang" berdasarkan hasil dari iterasi pertama dengan menggunakan backpropagation dan pembaruan bobot (embedding).

#### **Iterasi ke 3**
Pada iterasi ketiga, kita akan melanjutkan pembaruan vektor embedding untuk kata "itu", "kucing", dan "sedang" berdasarkan hasil dari iterasi kedua.

Vektor embedding setelah iterasi kedua:

Vektor "itu": [0.395,0.495,0.595]
Vektor "kucing": [0.095,0.195,0.295]
Vektor "sedang": [0.695,0.795,0.895]

Pembaruan pada Iterasi Ketiga
Untuk iterasi ketiga, kita asumsikan nilai gradien error tetap 0.05 per dimensi, dan learning rate tetap 0.1 .

**Pembaruan Vektor "itu"**

a. Dimensi pertama vektor "itu":
- vektor_itu_baru=0.395−(0.1×0.05)=0.39

b. Dimensi kedua vektor "itu":
- vektor_itu_baru=0.495−(0.1×0.05)=0.49

c. Dimensi ketiga vektor "itu":
- vektor_itu_baru=0.595−(0.1×0.05)=0.59

Jadi, setelah memperbarui vektor embedding untuk "itu", vektor "itu" menjadi:[0.39,0.49,0.59]


**Pembaruan vector "kucing"**

a. Dimensi pertama vektor "kucing":
- vektor_kucing_baru=0.095−(0.1×0.05)=0.09

b. Dimensi kedua vektor "kucing":
- vektor_kucing_baru=0.195−(0.1×0.05)=0.19

c. Dimensi ketiga vektor "kucing":
- vektor_kucing_baru=0.295−(0.1×0.05)=0.29

Setelah pembaruan, vektor embedding "kucing" menjadi: [0.09,0.19,0.29]

**Pembaruan Vektor "sedang"**

a. Dimensi pertama vektor "sedang":
- vektor_sedang_baru=0.695−(0.1×0.05)=0.69

b. Dimensi kedua vektor "sedang":
- vektor_sedang_baru=0.795−(0.1×0.05)=0.79

c. Dimensi ketiga vektor "sedang":
- vektor_sedang_baru=0.895−(0.1×0.05)=0.89

Setelah pembaruan, vektor embedding "sedang" menjadi: [0.69,0.79,0.89]

Rekapitulasi Vektor
Embedding Setelah Iterasi Ketiga
Setelah iterasi ketiga, berikut adalah vektor embedding yang diperbarui untuk kata "itu", "kucing", dan "sedang":

- Vektor "itu": [0.39,0.49,0.59]
- Vektor "kucing": [0.09,0.19,0.29]
- Vektor "sedang": [0.69,0.79,0.89]

Kesimpulan
Pada iterasi ketiga, kita telah memperbarui vektor embedding untuk kata "itu", "kucing", dan "sedang" dengan menggunakan proses backpropagation yang didasarkan pada gradien error dari iterasi sebelumnya