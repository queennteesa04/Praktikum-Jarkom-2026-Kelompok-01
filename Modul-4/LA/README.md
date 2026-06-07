# Laporan Akhir - Tugas Modul 4: Firewall & NAT

**Kelompok 03**
Anggota Kelompok:
1. Nicholas Benaya - 5024241050
2. Naufal Dzaki Hidayat - 5024241060
3. Farhat Azzam - 5024241033

---

## 1. Topologi Jaringan
Berikut adalah topologi jaringan yang digunakan pada praktikum ini:

![Topologi PNETLab]([link-atau-path-gambar-topologi-kamu-di-github])
*(Hapus teks ini dan tempel screenshot topologi PNETLab kamu secara utuh)*

---

## 2. Tabel IP Address
Berdasarkan segmentasi jaringan, berikut adalah alokasi IP Address untuk setiap perangkat:

| Perangkat | Interface | IP Address | Gateway | Keterangan |
|:----------|:----------|:-----------|:--------|:-----------|
| MikroTik ISP | ether1 | DHCP Client | DHCP Lab | Terhubung ke jaringan lab |
| MikroTik ISP | ether2 | `10.10.10.1/30` | - | Terhubung ke FortiGate port1 |
| MikroTik ISP | ether3 | `172.16.100.1/24` | - | Gateway untuk Client-WAN |
| FortiGate | port1 | `10.10.10.2/30` | `10.10.10.1` | Interface WAN |
| FortiGate | port2 | `10.20.20.1/30` | - | Interface INSIDE ke Cisco |
| FortiGate | port3 | `192.168.20.1/24` | - | Interface DMZ |
| Cisco Router | G0/0 | `10.20.20.2/30` | - | Terhubung ke FortiGate port2 |
| Cisco Router | G0/1 | `192.168.10.1/24` | - | Gateway LAN |
| Client LAN (TinyCore) | eth0 | `192.168.10.10/24` | `192.168.10.1` | Client internal |
| Client WAN (TinyCore) | eth0 | `172.16.100.10/24` | `172.16.100.1` | Client luar |
| Ubuntu Server DMZ | eth0 / ens3 | `192.168.20.10/24` | `192.168.20.1` | Web server DMZ |

---

## 3. Konfigurasi Tiap Perangkat

### 3.1. MikroTik ISP
Konfigurasi DHCP Client, IP Address, NAT masquerade, dan rute statis ke LAN dan DMZ.
```routeros
[Tempel teks export konfigurasi MikroTik di sini]
```
![Screenshot MikroTik]([link-gambar-screenshot-terminal-mikrotik])

### 3.2. Cisco Router
Konfigurasi IP Address interface dan Default Route ke FortiGate.
```ios
[Tempel teks show running-config bagian interface dan route di sini]
```
![Screenshot Cisco]([link-gambar-screenshot-terminal-cisco])

### 3.3. FortiGate Firewall
Konfigurasi IP Address, Static Route, Firewall Addresses, Virtual IPs (Port Forwarding), dan Firewall Policies.
```text
[Tempel teks show full-configuration yang relevan di sini]
```
![Screenshot FortiGate]([link-gambar-screenshot-terminal-fortigate])

### 3.4. Ubuntu Server DMZ
Konfigurasi IP Statis menggunakan Netplan dan setup Web Server Nginx.
```yaml
[Tempel teks konfigurasi /etc/netplan/01-netcfg.yaml di sini]
```
![Screenshot Netplan & Nginx]([link-gambar-screenshot-terminal-ubuntu])

### 3.5. PC Client (LAN & WAN)
Konfigurasi IP statis pada TinyCore Linux (LAN dan WAN).
![Screenshot IP LAN]([link-gambar-network-control-panel-LAN])
![Screenshot IP WAN]([link-gambar-network-control-panel-WAN])

---

## 4. Hasil Pengujian
*(Pastikan screenshot menampilkan perintah yang diketik dan nama perangkat)*

1. **Pengujian Client LAN ke Gateway Cisco**
   ![Ping LAN ke Cisco]([link-gambar])

2. **Pengujian Client LAN ke FortiGate**
   ![Ping LAN ke FortiGate]([link-gambar])

3. **Pengujian Client LAN ke DMZ**
   ![Ping LAN ke DMZ]([link-gambar])

4. **Pengujian Client LAN akses IP DMZ (Web Server)**
   ![Akses Web DMZ dari LAN]([link-gambar])

5. **Pengujian Client WAN ping ke ISP MikroTik**
   ![Ping WAN ke MikroTik]([link-gambar])

6. **Pengujian Client WAN ping ke FortiGate**
   ![Ping WAN ke FortiGate]([link-gambar])

7. **Pengujian Client WAN akses http://10.10.10.2 (Port Forwarding)**
   ![Akses Web DMZ dari WAN via VIP]([link-gambar])

8. **Pengujian Client WAN ping Client LAN (Hasil Drop/RTO)**
   ![Ping WAN ke LAN RTO]([link-gambar])

9. **Pengujian Client WAN ping IP asli DMZ (Hasil Drop/RTO)**
   ![Ping WAN ke IP DMZ RTO]([link-gambar])

10. **Pengujian Server DMZ ping LAN (Hasil Drop/RTO)**
    ![Ping DMZ ke LAN RTO]([link-gambar])

---

## 5. Analisis dan Kesimpulan

### Analisis
* **NAT dan Port Forwarding:** [Tulis analisismu di sini. Contoh: Bagaimana fitur VIP (Destination NAT) di FortiGate membelokkan trafik dari Client WAN ke Ubuntu Server tanpa mengekspos IP asli server.]
* **Firewall Filter dan DMZ:** [Tulis analisismu di sini. Jelaskan kenapa pada pengujian nomor 10, DMZ gagal melakukan ping ke LAN, kaitkan dengan prinsip *Connection Tracking*, *Stateful Firewall*, dan konsep *One-Way Trust* pada arsitektur DMZ.]

### Kesimpulan
[Tulis kesimpulan akhir mengenai praktikum ini. Contoh: Praktikum ini membuktikan pentingnya segmentasi jaringan dan implementasi *firewall* berlapis. Penggunaan *zone* DMZ memisahkan layanan publik dari jaringan internal, sehingga jika server publik dikompromikan, 
jaringan internal (LAN) tetap aman dari serangan...]
