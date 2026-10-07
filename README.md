# Bài 3: Quản lý Quy trình và Phân tích Hiệu năng Hệ thống

## Mục tiêu
- Phân tích tiến trình hệ thống, phát hiện dịch vụ chiếm dụng tài nguyên CPU/RAM bất thường bằng `top` và `ps`.
- Xử lý dừng tiến trình bị treo (`kill` / `pkill`) và cấu hình dịch vụ khởi động cùng hệ thống.

---

## 1. Giả lập Tiến trình Ngốn Tài nguyên (High-CPU / High-Memory Process)

### Chạy tiến trình vòng lặp vô tận chạy ngầm:
```bash
python3 -c "while True: pass" &
```
*Kết quả:* Trả về Tiến trình PID, ví dụ: `[1] 14205`.

---

## 2. Truy vết Tiến trình bằng `ps` và `top`

### Lệnh `ps aux | grep python3`:
```bash
$ ps aux | grep python3
user1    14205 98.5  0.4  28540  9120 pts/0    R    10:45   0:15 python3 -c while True: pass
```

### Màn hình theo dõi tài nguyên `top`:
```text
top - 10:46:10 up 2 days,  4:12,  1 user,  load average: 1.05, 0.60, 0.35
Tasks: 112 total,   2 running, 110 sleeping,   0 stopped,   0 zombie
%Cpu(s): 99.1 us,  0.9 sy,  0.0 ni,  0.0 id,  0.0 wa,  0.0 hi,  0.0 si

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
14205 user1     20   0   28540   9120   3412 R  98.5   0.4   0:35.12 python3
```

---

## 3. Xử lý Dừng Tiến trình (`kill -9`)

Gửi tín hiệu buộc dừng tiến trình PID `14205`:
```bash
sudo kill -9 14205
```

Xác nhận tiến trình đã ngưng hoạt động:
```bash
$ ps aux | grep 14205
# Không còn tiến trình xuất hiện
```

---

## 4. Quản lý Dịch vụ Tự động Khởi động cùng Hệ thống (`systemctl`)

### Kích hoạt Nginx tự động khởi động cùng hệ thống:
```bash
sudo systemctl enable nginx
```

**Kết quả màn hình `systemctl status nginx`:**
```text
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2026-10-05 08:00:00 UTC; 2 days ago
   Main PID: 1234 (nginx)
```
*(Trạng thái `enabled` chứng minh Nginx sẽ tự động chạy mỗi khi reboot máy chủ)*
