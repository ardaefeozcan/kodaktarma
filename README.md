from selenium import webdriver
from selenium.webdriver.common.by import By
import time

# GeckoDriver'ın yolu
geckodriver_path = "/usr/local/bin/geckodriver"

# Firefox WebDriver
driver = webdriver.Firefox(executable_path=geckodriver_path)

# Hedef web sayfası
url = "https://finalprojects.fabacademy.org/#/schedule/2024"
driver.get(url)
time.sleep(5)

# Video bağlantılarını bulma
video_links = []
elements = driver.find_elements(By.TAG_NAME, "a")
for element in elements:
    href = element.get_attribute("href")
    if href and href.endswith(".mp4"):
        video_links.append(href)

# Tarayıcıyı kapat
driver.quit()

# Bağlantıları yazdır
print("Bulunan Video Bağlantıları:")
for link in video_links:
    print(link)
