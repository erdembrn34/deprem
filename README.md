# Türkiye Deprem Verisi Analizi

Bu proje, Türkiye’deki deprem verilerini analiz etmek için yapılmıştır.

## İçerik
- Büyüklük dağılımı
- Derinlik analizi
- Tarihsel deprem trendleri
- Harita üzerinde deprem lokasyonları

## Kullanım
1. Deprem verilerini CSV formatında indirin.
2. Aşağıdaki kodları jupyter notebook üzerinden çalıştırın.
3. Grafikleri ve analizleri görüntüleyin.

## Gereksinimler
```bash
pip install pandas matplotlib seaborn folium


---

## 3️⃣ Jupyter Notebook Örneği

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import folium

# Veri Yükleme
# turkey_df = pd.read_csv('turkey_earthquakes.csv')  # Dosya adınızı parantezin içine yazın

# 1️⃣ Genel Bilgi
print(turkey_df.info())
print(turkey_df.head())

# 2️⃣ Büyüklük ve Derinlik Dağılımı
plt.figure(figsize=(10,5))
sns.histplot(turkey_df['mag'], bins=30, kde=True)
plt.title("Deprem Büyüklük Dağılımı")
plt.xlabel("Büyüklük (Magnitude)")
plt.ylabel("Deprem Sayısı")
plt.show()

plt.figure(figsize=(10,5))
sns.histplot(turkey_df['depth'], bins=30, kde=True)
plt.title("Deprem Derinliği Dağılımı")
plt.xlabel("Derinlik (km)")
plt.ylabel("Deprem Sayısı")
plt.show()

# 3️⃣ Zaman Serisi: Yıllara Göre Deprem Sayısı
turkey_df['time'] = pd.to_datetime(turkey_df['time'])
turkey_df['year'] = turkey_df['time'].dt.year
yearly_counts = turkey_df.groupby('year').size()

plt.figure(figsize=(10,5))
sns.lineplot(x=yearly_counts.index, y=yearly_counts.values, marker='o')
plt.title("Türkiye'de Yıllara Göre Deprem Sayısı")
plt.xlabel("Yıl")
plt.ylabel("Deprem Sayısı")
plt.show()

# 4️⃣ Harita Üzerinde Depremler
map_center = [38.0, 35.0]  # Türkiye'nin merkez koordinatları
m = folium.Map(location=map_center, zoom_start=5)

for idx, row in turkey_df.iterrows():
    folium.CircleMarker(
        location=[row['latitude'], row['longitude']],
        radius=row['mag']*2,
        color='red',
        fill=True,
        fill_opacity=0.6
    ).add_to(m)

m.save("turkey_earthquakes_map.html")
Gereksinimler
pandas
matplotlib
seaborn
folium
