import numpy as np
import matplotlib.pyplot as plt

# Fonksiyonlar
def enerji_verimliligi(bor_katkisi):
    """
    Bor katkısına bağlı enerji verimliliği hesaplama.
    Başlangıç verimliliği %85, bor katkısı başına %0.375 artış.
    """
    if bor_katkisi < 0:
        raise ValueError("Bor katkısı negatif olamaz.")
    return 85 + bor_katkisi * 0.375

def enerji_tasarrufu(verimlilik):
    """
    Verimliliğe bağlı enerji tasarrufu hesaplama.
    Tasarruf başlangıç verimliliği (%85) ile karşılaştırılır.
    """
    return (verimlilik - 85) / 7.5

def grafik_ciz(bor_degerleri, verimlilik_degerleri, tasarruf_degerleri, bor_katkisi_orani, toplam_verimlilik):
    """
    Bor katkısına bağlı enerji verimliliği ve tasarruf grafiği çizer.
    """
    plt.figure(figsize=(12, 6))
    plt.plot(bor_degerleri, verimlilik_degerleri, label="Verimlilik (%)", color="blue", linewidth=2)
    plt.plot(bor_degerleri, tasarruf_degerleri, label="Enerji Tasarrufu (%)", color="green", linewidth=2)
    plt.scatter(bor_katkisi_orani, toplam_verimlilik, color="red", label=f"Bor Katkısı: %{bor_katkisi_orani}", zorder=5)
    plt.axhline(85, color="gray", linestyle="--", label="Başlangıç Verimliliği (%85)")
    for x in range(0, 21, 5):
        plt.axvline(x, color="purple", linestyle="--", alpha=0.5, label=f"Bor Katkısı %{x}" if x == 0 else None)
    plt.title("Bor Katkısının Enerji Verimliliği ve Tasarruf Üzerine Etkisi", fontsize=14, fontweight="bold")
    plt.xlabel("Bor Katkısı (%)", fontsize=12)
    plt.ylabel("Yüzde (%)", fontsize=12)
    plt.grid(alpha=0.3)
    plt.legend(fontsize=10)
    plt.tight_layout()
    plt.show()

# Ana Program
def main():
    try:
        # Kullanıcıdan bor katkısı oranı al
        bor_katkisi_orani = float(input("Lütfen Bor Katkısı Oranını (% olarak) Girin: "))
        if bor_katkisi_orani < 0:
            raise ValueError("Bor katkısı negatif olamaz. Lütfen geçerli bir değer girin.")
        
        # Hesaplamalar
        toplam_verimlilik = enerji_verimliligi(bor_katkisi_orani)
        toplam_tasarruf = enerji_tasarrufu(toplam_verimlilik)

        # Sonuçları Yazdır
        print(f"\nBor Katkısı: {bor_katkisi_orani:.2f}%")
        print(f"Toplam Verimlilik: {toplam_verimlilik:.2f}%")
        print(f"Toplam Enerji Tasarrufu: {toplam_tasarruf:.2f}%\n")
        
        # Bor katkısı aralığı ve hesaplamalar
        bor_degerleri = np.linspace(0, 20, 100)
        verimlilik_degerleri = enerji_verimliligi(bor_degerleri)
        tasarruf_degerleri = enerji_tasarrufu(verimlilik_degerleri)

        # Grafik Çizimi
        grafik_ciz(bor_degerleri, verimlilik_degerleri, tasarruf_degerleri, bor_katkisi_orani, toplam_verimlilik)

    except ValueError as e:
        print(f"\nHatalı giriş: {e}\n")

# Program Çalıştırma
if __name__ == "__main__":
    main()