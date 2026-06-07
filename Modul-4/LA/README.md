# Laporan Akhir - Tugas Modul 4: Firewall & NAT

**Kelompok 01-Kabel LAN**
Anggota Kelompok:
1. Nicholas Benaya - 5024241050
2. Naufal Dzaki Hidayat - 5024241060
3. Farhat Azzam - 5024241033

---

## 1. Topologi Jaringan
Berikut adalah topologi jaringan yang digunakan pada praktikum ini:

![Topologi PNETLab]
<img width="872" height="720" alt="Modul 4_LA - Topologi" src="https://github.com/user-attachments/assets/898d5ea8-e33d-417e-991f-bac3a6f386c8" />


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
<img width="786" height="441" alt="image" src="https://github.com/user-attachments/assets/fecc32e9-ecd7-4859-be48-faeb7b5087a4" />


### 3.2. Cisco Router
Konfigurasi IP Address interface dan Default Route ke FortiGate.
<img width="483" height="459" alt="image" src="https://github.com/user-attachments/assets/f03f50dd-05fa-4bcd-b71c-aa81f13cfb01" />
<img width="450" height="481" alt="image" src="https://github.com/user-attachments/assets/be05ed52-e678-49a4-bd27-ada10cc3af87" />


### 3.3. FortiGate Firewall
Konfigurasi IP Address, Static Route, Firewall Addresses, Virtual IPs (Port Forwarding), dan Firewall Policies.
```text
[Tempel teks show full-configuration yang relevan di sini]
```
![Screenshot FortiGate]([link-gambar-screenshot-terminal-fortigate])

### 3.4. Ubuntu Server DMZ
Konfigurasi IP Statis menggunakan Netplan dan setup Web Server Nginx.
<img width="411" height="187" alt="image" src="https://github.com/user-attachments/assets/0ae18a72-90a8-4c8a-b5b3-cfad0e03d4e4" />


### 3.5. PC Client (LAN & WAN)
Konfigurasi IP statis pada TinyCore Linux (LAN dan WAN).
#### LAN
<img width="447" height="241" alt="image" src="https://github.com/user-attachments/assets/c4cdd33d-38c9-4a3e-8b57-038b1e35ae13" />
#### WAN
<img width="458" height="239" alt="image" src="https://github.com/user-attachments/assets/7667d3f4-8e5e-4c4e-8ee4-461493b89660" />


---

## 4. Hasil Pengujian
*(Pastikan screenshot menampilkan perintah yang diketik dan nama perangkat)*

1. **Pengujian Client LAN ke Gateway Cisco**
  <img width="346" height="145" alt="WhatsApp Image 2026-06-08 at 2 42 45 AM" src="https://github.com/user-attachments/assets/f59c3b44-9a54-4c41-ba2f-f6c2f166938b" />


2. **Pengujian Client LAN ke FortiGate**
  <img width="327" height="142" alt="WhatsApp Image 2026-06-08 at 2 42 45 AM (1)" src="https://github.com/user-attachments/assets/b701ede8-f140-45b1-b2c3-2d43879fd68f" />


3. **Pengujian Client LAN ke DMZ**
   <img width="337" height="151" alt="WhatsApp Image 2026-06-08 at 2 42 45 AM (2)" src="https://github.com/user-attachments/assets/9c5a12b3-f310-4c4a-b36a-38041e288658" />


4. **Pengujian Client LAN akses IP DMZ (Web Server)**
   <img width="581" height="418" alt="WhatsApp Image 2026-06-08 at 2 42 45 AM (3)" src="https://github.com/user-attachments/assets/47cb0eca-c297-4061-8c77-2369bce80388" />


5. **Pengujian Client WAN ping ke ISP MikroTik**
   <img width="345" height="145" alt="WhatsApp Image 2026-06-08 at 2 45 29 AM" src="https://github.com/user-attachments/assets/28e02b18-b01b-46d3-bd52-637f72765bd2" />


6. **Pengujian Client WAN ping ke FortiGate**
  <img width="334" height="126" alt="WhatsApp Image 2026-06-08 at 2 45 29 AM (1)" src="https://github.com/user-attachments/assets/d8837a0b-40ed-4bea-9f4b-b17cf49c0174" />


7. **Pengujian Client WAN akses http://10.10.10.2 (Port Forwarding)**
  <img width="566" height="414" alt="WhatsApp Image 2026-06-08 at 2 45 29 AM (2)" src="https://github.com/user-attachments/assets/74dbdbf7-d1c1-4d9e-9d54-37c9d1c599d4" />


8. **Pengujian Client WAN ping Client LAN (Hasil Drop/RTO)**
   <img width="351" height="68" alt="WhatsApp Image 2026-06-08 at 2 45 29 AM (3)" src="https://github.com/user-attachments/assets/a440e326-2dcc-427b-afab-d65979aa2328" />


9. **Pengujian Client WAN ping IP asli DMZ (Hasil Drop/RTO)**
  <img width="341" height="61" alt="WhatsApp Image 2026-06-08 at 2 46 34 AM" src="https://github.com/user-attachments/assets/6ff89536-6e84-4ece-8751-4fd79ac42a4c" />


10. **Pengujian Server DMZ ping LAN (Hasil Drop/RTO)**
   <img width="410" height="65" alt="WhatsApp Image 2026-06-08 at 2 52 11 AM" src="https://github.com/user-attachments/assets/cc2b5296-7c13-4066-b961-3c34aac550ec" />
