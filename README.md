# Deteksi_Warna

**Soal :**
- Buat program pendeteksi warna serta Jelaskan program yang dibuat untuk apa.
  
  # Jawaban

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
from google.colab import files
from sklearn.cluster import KMeans

# Fungsi untuk mengupload foto
uploaded = files.upload()

# Membaca gambar yang diupload
image_path = list(uploaded.keys())[0]
image = cv2.imread(image_path)

# Mengkonversi gambar ke RGB (karena OpenCV membaca gambar dalam format BGR)
image_rgb = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)

# Fungsi untuk mendeteksi warna dominan di gambar menggunakan KMeans
def detect_color(image, n_colors=3):
    # Mengubah ukuran gambar untuk mempercepat proses
    image_resized = cv2.resize(image, (image.shape[1]//10, image.shape[0]//10))
    pixels = image_resized.reshape(-1, 3)
    
    # Menggunakan KMeans untuk mengelompokkan warna
    kmeans = KMeans(n_clusters=n_colors)
    kmeans.fit(pixels)
    
    # Mengambil warna dominan
    dominant_colors = kmeans.cluster_centers_
    
    return dominant_colors

# Menampilkan warna dominan
dominant_colors = detect_color(image_rgb, 5)

# Menampilkan hasil warna dominan
plt.figure(figsize=(8, 6))
plt.imshow([dominant_colors.astype(int)])
plt.axis('off')
plt.title("Warna Dominan pada Buah")
plt.show()

# Fungsi untuk menilai tingkat kematangan berdasarkan warna
def assess_ripeness(dominant_colors):
    # Misalnya kita mendefinisikan warna-warna yang menggambarkan tingkat kematangan
    # Anggaplah warna hijau untuk buah yang belum matang dan merah untuk yang matang
    ripe_color_range = [(150, 50, 50), (255, 0, 0)]  # Misalnya merah matang
    unripe_color_range = [(0, 255, 0), (50, 255, 50)]  # Hijau belum matang

    # Cek kemiripan warna dominan dengan warna matang atau belum matang
    ripe_score = 0
    unripe_score = 0
    
    for color in dominant_colors:
        if color[0] > 100 and color[1] < 80:  # Cek apakah lebih mendekati merah
            ripe_score += 1
        elif color[1] > 150:  # Cek apakah lebih mendekati hijau
            unripe_score += 1
    
    # Kalkulasi tingkat kematangan berdasarkan jumlah warna yang cocok
    total = ripe_score + unripe_score
    if total == 0:
        return "Tidak dapat menilai kematangan", 0
    ripe_percentage = (ripe_score / total) * 100
    unripe_percentage = (unripe_score / total) * 100

    return f"Buah sudah matang: {ripe_percentage:.2f}%\nBuah belum matang: {unripe_percentage:.2f}%", ripe_percentage

# Menilai kematangan berdasarkan warna dominan
result, ripeness_percentage = assess_ripeness(dominant_colors)
print(result)

# Menampilkan gambar asli buah
plt.figure(figsize=(8, 6))
plt.imshow(image_rgb)
plt.axis('off')
plt.title("Gambar Buah")
plt.show()
```

**Hasil Gambar :**
![buat github](https://github.com/user-attachments/assets/8f7fb732-9747-40b6-8611-9206803a03fe)

