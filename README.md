## Hi there 👋

<!--
**emrahuzucar355/emrahuzucar355** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->import os
import requests

# Kişisel Erişim Belirteci
TOKEN = "KİŞİSEL_ERIŞIM_BELIRTECİNİZ"

# GitHub API URL'si
API_URL = "https://api.github.com/user"

# Yetkilendirme Başlığı
headers = {"Authorization": f"token {TOKEN}"}

def github_bilgi_al():
    """
    GitHub kullanıcı bilgilerini API üzerinden alır ve yazdırır.
    """
    response = requests.get(API_URL, headers=headers)
    if response.status_code == 200:
        print("Kullanıcı Bilgileri:")
        print(response.json())
    else:
        print(f"Hata: {response.status_code}\nHata Mesajı: {response.text}")

def git_kullanimi_bilgisi():
    """
    Git işlemleri için bilgi verir.
    """
    print("""
GitHub'da HTTPS ile kimlik doğrulaması yapmak için şu adımları izleyebilirsiniz:

1. Git deposunu klonlama:
   git clone https://github.com/kullanici_adiniz/depo_adi.git

2. Push veya Pull yaparken kullanıcı adı ve şifre sorulduğunda:
   - Kullanıcı Adı: GitHub kullanıcı adınızı yazın.
   - Şifre: Kişisel erişim belirtecinizi (TOKEN) yazın.

3. Örnek bir push işlemi:
   git add .
   git commit -m "Değişiklik açıklaması"
   git push origin main
    """)

def main():
    """
    Ana program: GitHub kullanıcı bilgilerini alır ve Git kullanımı hakkında bilgi verir.
    """
    print("GitHub API üzerinden bilgilerinizi alıyoruz...")
    github_bilgi_al()
    
    print("\nGit kullanımı ile ilgili bilgiler aşağıda:")
    git_kullanimi_bilgisi()

if __name__ == "__main__":
    main()
