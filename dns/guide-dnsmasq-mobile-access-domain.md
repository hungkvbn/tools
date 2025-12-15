# Setup dnsmasq trên macOS để iPhone truy cập domain nội bộ

## 🎯 Mục tiêu

- Domain: `mobile-dev.hungkvbn.vn`
- IP đích (server / backend): `100.250.30.5`
- DNS server chạy trên Mac: `100.123.24.2`
- iPhone / thiết bị trong cùng WiFi có thể truy cập domain này

---

## **1️⃣ Cài dnsmasq trên macOS**

Yêu cầu: đã cài Homebrew

```bash
brew install dnsmasq
```
---

## **2️⃣ Tạo file config cho domain**

Yêu cầu: đã cài Homebrew

```bash
sudo nano /opt/homebrew/etc/dnsmasq.d/mobile-dev.conf
```
Nội dung:

address=/mobile-dev.hungkvbn.vn/100.250.30.5


✅ dnsmasq không dùng format hosts, mà dùng address=/domain/ip

## **3️⃣ Cấu hình dnsmasq lắng nghe LAN**

Mở file chính:

```bash
sudo nano /opt/homebrew/etc/dnsmasq.conf
```

Đảm bảo có (hoặc thêm):

listen-address=127.0.0.1,100.123.24.2
bind-interfaces


📌 100.123.24.2 = IP Mac trong wifi

## **4️⃣ Start / Restart dnsmasq**

```bash
sudo brew services restart dnsmasq
Kiểm tra:
```
```bash
sudo lsof -i :53
```
## **5️⃣ Test trên chính Mac**

```bash
dig mobile-dev.hungkvbn.vn @100.123.24.2
Kết quả mong muốn:
```
```text
mobile-dev.hungkvbn.vn.   0   IN   A   100.250.30.5
```
## **6️⃣ Cấu hình DNS trên iPhone 📱**

Settings → Wi-Fi
→ (i) wifi đang dùng
→ Configure DNS → Manual
→ Add Server: 100.123.24.2
→ Remove DNS khác (1.1.1.1 / 8.8.8.8)

## **7️⃣ Clear DNS cache iPhone (rất quan trọng)**

Bật → Tắt Airplane Mode
Hoặc restart iPhone

## **8️⃣ Test trên iPhone**

Mở Safari:
https://mobile-dev.hungkvbn.vn

Hoặc test bằng app:
ping mobile-dev.hungkvbn.vn

