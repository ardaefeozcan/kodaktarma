from selenium import webdriver
from selenium.webdriver.firefox.service import Service
from selenium.webdriver.common.by import By
from urllib.parse import urljoin
import time
import os

# Tarayıcı ve WebDriver ayarları
service = Service("/usr/local/bin/geckodriver")  # GeckoDriver yolunu doğru yola göre değiştirin
driver = webdriver.Firefox(service=service)

# Web sayfasını aç
url = "https://example.com"  # Hedef web sayfasının URL'sini buraya yazın
driver.get(url)
time.sleep(5)  # JavaScript'in yüklenmesi için bekle

# Videoların bağlantılarını bul
video_links = []
elements = driver.find_elements(By.TAG_NAME, "a")  # <a> etiketlerini bul

# .mp4 bağlantılarını filtrele
for element in elements:
    href = element.get_attribute("href")
    if href and href.endswith(".mp4"):
        video_links.append(href)

# Tarayıcıyı kapat
driver.quit()

# Bağlantıları bir dosyaya kaydet
if video_links:
    with open("video_links.txt", "w") as file:
        for link in video_links:
            file.write(link + "\n")
    print(f"{len(video_links)} video bağlantısı bulundu ve 'video_links.txt' dosyasına kaydedildi.")
else:
    print("Hiçbir .mp4 bağlantısı bulunamadı.")
    exit()

# VLC ile video oynatma
playlist_path = "video_links.txt"

# VLC oynatma komutu
vlc_command = f"cvlc --fullscreen --loop --playlist {playlist_path}"
os.system(vlc_command)
