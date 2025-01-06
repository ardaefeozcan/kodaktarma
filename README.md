import subprocess

# Pip3'ün tam yolunu belirleme
pip3_path = "/usr/bin/pip3"

# Selenium'un yüklü olup olmadığını kontrol etme
try:
    import selenium
    print("Selenium modülü yüklü.")
except ModuleNotFoundError:
    print("Selenium modülü bulunamadı. Yükleniyor...")
    # Selenium'u yüklemek için pip3 kullanımı
    subprocess.run([pip3_path, "install", "selenium"], check=True)
    print("Selenium başarıyla yüklendi!")

# Selenium'u kullanarak işlem yapma
from selenium import webdriver
from selenium.webdriver.firefox.service import Service
from selenium.webdriver.common.by import By
from urllib.parse import urljoin
import time
import os

# GeckoDriver'ın yolu
geckodriver_path = "/usr/local/bin/geckodriver"

# Firefox WebDriver ayarları
service = Service(geckodriver_path)
driver = webdriver.Firefox(service=service)

# Web sayfasını aç
url = "https://example.com"  # Hedef URL'yi buraya yazın
driver.get(url)
time.sleep(5)  # Sayfanın yüklenmesi için bekle

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
