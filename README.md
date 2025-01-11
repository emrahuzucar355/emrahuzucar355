import numpy as np
import matplotlib.pyplot as plt

# Sabitler
g = 9.81  # Yerçekimi ivmesi (m/s²)
isp = 450  # Spesifik impuls (saniye)
fuel_mass = 100000  # Başlangıç yakıt kütlesi (kg)
dry_mass = 20000  # Gemi kuru kütlesi (kg)
engine_power = 5000  # Motor gücü (kW)
radiation_limit = 2.0  # Radyasyon limiti (Sv/saat)
energy_efficiency = 0.8  # Enerji sistem verimliliği

# Simülasyon parametreleri
time = np.linspace(0, 5000, 1000)  # Zaman (saniye)
dt = time[1] - time[0]  # Zaman adımı
fuel = np.zeros_like(time)  # Yakıt miktarı
speed = np.zeros_like(time)  # Hız
radiation = np.zeros_like(time)  # Radyasyon seviyesi
energy = np.zeros_like(time)  # Enerji seviyesi
oxygen = np.zeros_like(time)  # Oksijen seviyesi
fuel[0] = fuel_mass
energy[0] = engine_power * energy_efficiency  # Başlangıç enerjisi
oxygen[0] = 100  # Başlangıç oksijen seviyesi (örnek birim)

# Simülasyon
for i in range(1, len(time)):
    if fuel[i - 1] > 0:  # Yakıt varsa
        fuel_consumption = engine_power * dt / (isp * g)  # Yakıt tüketimi (kg)
        fuel[i] = fuel[i - 1] - fuel_consumption
        if fuel[i] < 0:  # Yakıt tükenirse sıfırla
            fuel[i] = 0
        thrust = isp * g * fuel_consumption  # İtki gücü (N)
        acceleration = thrust / (dry_mass + fuel[i])  # İvme (m/s²)
        speed[i] = speed[i - 1] + acceleration * dt  # Hız güncellemesi
    else:
        speed[i] = speed[i - 1]  # Yakıt yoksa hız sabit

    # Enerji seviyesi
    energy_loss = engine_power * (1 - energy_efficiency) * dt  # Enerji kaybı
    energy[i] = energy[i - 1] - energy_loss
    if energy[i] < 0:  # Enerji tükenirse sıfırla
        energy[i] = 0

    # Radyasyon seviyesi
    radiation[i] = radiation[i - 1] + 0.01 * speed[i] * dt  # Hızla artan radyasyon

    # Oksijen tüketimi
    oxygen_consumption = 0.001 * dt  # Oksijen tüketim oranı
    oxygen[i] = oxygen[i - 1] - oxygen_consumption
    if oxygen[i] < 0:  # Oksijen tükenirse sıfırla
        oxygen[i] = 0

# Grafikler
plt.figure(figsize=(12, 8))

# Yakıt, Hız ve Enerji
plt.subplot(3, 1, 1)
plt.plot(time, fuel, label="Yakıt (kg)", color='blue')
plt.plot(time, energy, label="Enerji (kW)", color='green')
plt.title("Yakıt ve Enerji Yönetimi")
plt.xlabel("Zaman (saniye)")
plt.ylabel("Miktar")
plt.legend()

# Hız
plt.subplot(3, 1, 2)
plt.plot(time, speed, label="Hız (m/s)", color='orange')
plt.title("Gemi Hızlanması")
plt.xlabel("Zaman (saniye)")
plt.ylabel("Hız (m/s)")
plt.legend()

# Radyasyon ve Oksijen
plt.subplot(3, 1, 3)
plt.plot(time, radiation, label="Radyasyon Seviyesi (Sv)", color='red')
plt.plot(time, oxygen, label="Oksijen Seviyesi", color='purple')
plt.title("Radyasyon ve Oksijen Yönetimi")
plt.xlabel("Zaman (saniye)")
plt.ylabel("Miktar")
plt.legend()

plt.tight_layout()
plt.show()