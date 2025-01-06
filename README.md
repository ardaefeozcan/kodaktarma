from selenium import webdriver
from selenium.webdriver.firefox.service import Service
from selenium.webdriver.common.by import By
import time

# GeckoDriver'ın yolu
geckodriver_path = "/usr/local/bin/geckodriver"

# Firefox WebDriver ayarları
service = Service(geckodriver_path)
driver = webdriver.Firefox(service=service)

# Hedef web sayfası URL'si
url = "https://finalprojects.fabacademy.org/#/schedule/2024"  # Hedef URL
driver.get(url)
time.sleep(5)  # Sayfanın tamamen yüklenmesini bekleyin

# Video bağlantılarını bulma
video_links = []
elements = driver.find_elements(By.TAG_NAME, "a")  # Tüm <a> etiketlerini bul

for element in elements:
    href = element.get_attribute("href")
    if href and href.endswith(".mp4"):
        video_links.append(href)

# Tarayıcıyı kapatma
driver.quit()

# Bağlantıları bir dosyaya kaydetme
if video_links:
    with open("video_links.txt", "w") as file:
        for link in video_links:
            file.write(link + "\n")
    print(f"{len(video_links)} video bağlantısı bulundu ve 'video_links.txt' dosyasına kaydedildi.")
else:
    print("Hiçbir .mp4 bağlantısı bulunamadı.")
