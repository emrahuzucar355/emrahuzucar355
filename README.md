import numpy as np
import matplotlib.pyplot as plt

# Elektroliz Verimliliği Modeli
def electrolysis_efficiency(voltage, current_density):
    """
    Elektroliz işlemi için verimliliği hesaplar.
    :param voltage: Elektroliz sırasında kullanılan voltaj (V)
    :param current_density: Elektrot yüzeyine uygulanan akım yoğunluğu (A/cm²)
    :return: Verimlilik değeri (0-1 arasında)
    """
    theoretical_voltage = 1.23  # Teorik minimum voltaj (V)
    efficiency = (2 * theoretical_voltage) / voltage
    efficiency -= current_density / 1000  # Akım yoğunluğundan kaynaklanan kayıplar
    return max(0, efficiency)  # Verimlilik negatif olamaz

# Test ve Görselleştirme
voltages = np.linspace(1.5, 3.0, 50)  # Voltaj değerleri
current_density = 500  # Sabit akım yoğunluğu (A/cm²)

efficiencies = [electrolysis_efficiency(v, current_density) for v in voltages]

# Grafik
plt.figure(figsize=(8, 6))
plt.plot(voltages, efficiencies, label=f"Akım Yoğunluğu: {current_density} A/cm²", color="blue")
plt.title("Elektroliz Verimliliği")
plt.xlabel("Voltaj (V)")
plt.ylabel("Verimlilik")
plt.ylim(0, 1.2)  # Verimlilik %120'yi aşamaz
plt.grid()
plt.legend()
plt.show()

# Örnek Testler
print("Test Sonuçları:")
for v in [1.5, 2.0, 2.5]:
    efficiency = electrolysis_efficiency(v, current_density)
    print(f"Voltaj: {v} V, Akım Yoğunluğu: {current_density} A/cm² -> Verimlilik: {efficiency:.2f}")
