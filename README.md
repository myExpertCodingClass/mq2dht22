# 🌬️ Sistem Monitoring Kualitas Udara & Suhu

Proyek ini adalah sistem pemantauan kualitas udara, suhu, dan kelembaban berbasis mikrokontroler (seperti Arduino Uno/Nano). Sistem ini menggunakan sensor **MQ-2** untuk mendeteksi keberadaan gas/asap dan sensor **DHT11** untuk mendeteksi suhu serta kelembaban udara. Sistem juga dilengkapi dengan indikator visual berupa **LED** dan indikator suara berupa **Buzzer** untuk memberikan peringatan dini ketika kondisi udara tidak aman.

---

## 📋 Fitur Utama
- **Real-Time Monitoring**: Membaca nilai konsentrasi gas secara analog serta nilai suhu dan kelembaban secara berkala.
- **Visual Alert**: LED Biru menyala jika kondisi aman. LED Merah berkedip/menyala jika kualitas udara buruk atau suhu melewati batas aman.
- **Audio Alarm**: Buzzer aktif akan berbunyi keras saat terdeteksi bahaya/kondisi tidak aman.
- **Serial Debugging**: Menampilkan seluruh data sensor dan status kondisi udara melalui Serial Monitor.

---

## 🛠️ Komponen yang Dibutuhkan
1. **Mikrokontroler**: Arduino Uno R3 / Nano / sejenisnya (1x)
2. **Sensor Gas MQ-2**: Untuk mendeteksi gas elpiji, asap, butana, propana, alkohol, hidrogen, dll. (1x)
3. **Sensor DHT11**: Untuk mengukur suhu dan kelembaban udara (1x)
4. **LED Merah**: Sebagai indikator peringatan / bahaya (1x)
5. **LED Biru**: Sebagai indikator kondisi aman (1x)
6. **Buzzer Aktif 5V**: Sebagai alarm suara (1x)
7. **Resistor 220 Ohm**: Untuk membatasi arus LED agar tidak rusak (2x)
8. **Breadboard**: Untuk merangkai komponen (1x)
9. **Kabel Jumper**: Jenis Male-to-Male & Male-to-Female secukupnya

---

## 🔌 Skema Wiring (Sambungan Kabel)

Berikut adalah tabel koneksi pin dari masing-masing komponen ke papan Arduino:

| Nama Komponen | Pin Komponen | Pin Arduino | Keterangan |
| :--- | :---: | :---: | :--- |
| **Sensor DHT11** | VCC | 5V / 3.3V | Daya Sensor |
| | GND | GND | Ground |
| | DATA / OUT | **D2** | Pin Digital untuk komunikasi data |
| **Sensor MQ-2** | VCC | 5V | Daya Sensor (MQ-2 membutuhkan daya 5V agar stabil) |
| | GND | GND | Ground |
| | AO (Analog Out) | **A0** | Pin Analog untuk membaca nilai konsentrasi gas |
| **LED Merah** | Anoda (+) | **D8** | Hubungkan melalui Resistor 220Ω |
| | Katoda (-) | GND | Ground |
| **LED Biru** | Anoda (+) | **D6** | Hubungkan melalui Resistor 220Ω |
| | Katoda (-) | GND | Ground |
| **Buzzer Aktif** | Positif (+) | **D9** | Pin Digital kontrol suara |
| | Negatif (-) | GND | Ground |

> [!IMPORTANT]
> - Sensor **MQ-2** memerlukan pemanasan awal (*pre-heating*) sekitar 20-60 detik saat pertama kali dinyalakan agar sensor mendapatkan suhu kerja stabil dan pembacaan nilai gas menjadi akurat.
> - Pastikan LED dihubungkan seri dengan **Resistor 220 Ohm** sebelum masuk ke pin Arduino untuk mencegah kerusakan pin atau LED akibat arus berlebih.

---

## ⚙️ Logika Kerja Program

Program di dalam [main.cpp](file:///c:/RUANGAN%20CODING/Monitoring%20kualitas%20udara/main.cpp) bekerja dengan aturan batas ambang (*threshold*) berikut:

- **Batas Ambang Gas (*gasThreshold*)**: `400`
- **Batas Ambang Suhu (*tempThreshold*)**: `35.0 °C`

### Alur Keputusan:
1. **KONDISI TIDAK AMAN**:
   Jika pembacaan sensor **Gas > 400** ATAU **Suhu > 35.0 °C**:
   - **LED Merah**: Menyala (`HIGH`)
   - **LED Biru**: Mati (`LOW`)
   - **Buzzer**: Bunyi nyaring (`HIGH`)
   - Serial Monitor mencetak: `"PERINGATAN! Kualitas Udara Tidak Aman"`

2. **KONDISI AMAN**:
   Jika pembacaan sensor **Gas ≤ 400** DAN **Suhu ≤ 35.0 °C**:
   - **LED Merah**: Mati (`LOW`)
   - **LED Biru**: Menyala (`HIGH`)
   - **Buzzer**: Mati (`LOW`)
   - Serial Monitor mencetak: `"Kondisi Aman"`

---

## 💻 Panduan Pemrograman & Upload

### 1. Kebutuhan Pustaka (Library)
Sebelum mengunggah kode, pastikan Anda telah menginstal pustaka berikut di Arduino IDE Anda:
* **DHT Sensor Library** oleh Adafruit (dapat dicari lewat *Library Manager* Arduino IDE).
* **Adafruit Unified Sensor Library** (dependensi bawaan untuk library DHT Adafruit).

### 2. Langkah-Langkah Mengunggah Kode
1. Sambungkan papan Arduino ke komputer Anda menggunakan kabel USB.
2. Buka software **Arduino IDE** atau **VSCode** dengan extension **PlatformIO**.
3. Salin kode dari file [main.cpp](file:///c:/RUANGAN%20CODING/Monitoring%20kualitas%20udara/main.cpp).
4. Di Arduino IDE, pilih tipe board Anda (contoh: *Arduino Uno*) pada menu **Tools > Board**.
5. Pilih port USB yang sesuai di menu **Tools > Port**.
6. Klik tombol **Upload** (ikon panah kanan).
7. Buka **Serial Monitor** pada baudrate **9600** untuk memantau data pembacaan sensor secara langsung.

---

## 📊 Contoh Output Serial Monitor
Ketika sistem berjalan normal, berikut adalah visualisasi data yang tampil pada Serial Monitor:
```text
=== Sistem Monitoring Kualitas Udara ===
Gas: 185 | Suhu: 28.40 °C | Kelembaban: 65.00 %
Kondisi Aman
Gas: 190 | Suhu: 28.50 °C | Kelembaban: 64.00 %
Kondisi Aman
Gas: 450 | Suhu: 28.50 °C | Kelembaban: 64.00 %
PERINGATAN! Kualitas Udara Tidak Aman
```

---
*Proyek ini dirancang untuk kemudahan instalasi sistem deteksi dini pencemaran udara dan suhu ruangan.*
