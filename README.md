# Bài 1: Khởi tạo máy chủ Ubuntu và kết nối bằng SSH Key

## 1. Mục tiêu

Bài thực hành nhằm làm quen với việc khởi tạo một máy chủ Ubuntu trên nền tảng Cloud/VPS và cấu hình xác thực bằng SSH Key thay cho mật khẩu.

Các mục tiêu chính:

- Tạo máy chủ chạy Ubuntu Server.
- Chọn khu vực máy chủ gần Việt Nam.
- Sử dụng cấu hình máy chủ tiết kiệm chi phí.
- Tạo cặp khóa SSH trên máy cá nhân.
- Thêm Public Key vào máy chủ.
- Kết nối tới máy chủ bằng SSH.
- Kiểm tra tài khoản đăng nhập và phiên bản hệ điều hành.

---

## 2. Môi trường thực hiện

- Máy cá nhân: Windows
- Terminal: Windows PowerShell
- Hệ điều hành máy chủ: Ubuntu 22.04 LTS hoặc mới hơn
- Phương thức xác thực: SSH Key
- Tài khoản đăng nhập: `root`
- Khu vực máy chủ: Singapore
- Loại máy chủ: Shared CPU / Basic hoặc gói tương đương

> Lưu ý: Nếu sử dụng nhà cung cấp VPS khác DigitalOcean do hạn chế chi phí, cần đảm bảo máy chủ vẫn đáp ứng các yêu cầu kỹ thuật tương đương: Ubuntu, Public IP, SSH Key và khả năng kết nối SSH từ máy cá nhân.

---

## 3. Tạo SSH Key trên Windows

Mở PowerShell và kiểm tra OpenSSH:

```powershell
ssh -V
```

Tạo SSH Key bằng thuật toán Ed25519:

```powershell
ssh-keygen -t ed25519 -C "student@example.com"
```

Khi PowerShell hỏi vị trí lưu file, nhấn `Enter` để sử dụng đường dẫn mặc định.

Hai file được tạo:

```text
C:\Users\<username>\.ssh\id_ed25519
C:\Users\<username>\.ssh\id_ed25519.pub
```

Trong đó:

- `id_ed25519`: Private Key, không được chia sẻ hoặc đưa lên GitHub.
- `id_ed25519.pub`: Public Key, dùng để cấu hình trên máy chủ.

Hiển thị Public Key bằng lệnh:

```powershell
Get-Content "$env:USERPROFILE\.ssh\id_ed25519.pub"
```

Có thể copy Public Key trực tiếp vào clipboard:

```powershell
Get-Content "$env:USERPROFILE\.ssh\id_ed25519.pub" | Set-Clipboard
```

---

## 4. Khởi tạo máy chủ Ubuntu

Cấu hình máy chủ theo các thông số sau:

| Thành phần | Cấu hình |
|---|---|
| Region | Singapore |
| Operating System | Ubuntu 22.04 LTS hoặc mới hơn |
| CPU | Shared CPU |
| Plan | Basic hoặc gói tiết kiệm tương đương |
| Authentication | SSH Key |
| User | root |

Tại phần cấu hình SSH Key, dán toàn bộ nội dung của file:

```text
id_ed25519.pub
```

Sau đó hoàn tất việc tạo máy chủ.

Khi máy chủ được tạo thành công, nhà cung cấp sẽ cấp một địa chỉ Public IP.

Ví dụ:

```text
xxx.xxx.xxx.xxx
```

---

## 5. Kết nối máy chủ bằng SSH

Trên PowerShell, sử dụng lệnh:

```powershell
ssh -i "$env:USERPROFILE\.ssh\id_ed25519" root@<IP_ADDRESS>
```

Ví dụ cấu trúc:

```powershell
ssh -i "$env:USERPROFILE\.ssh\id_ed25519" root@xxx.xxx.xxx.xxx
```

Trong lần kết nối đầu tiên, hệ thống có thể hiển thị:

```text
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Nhập:

```text
yes
```

Nếu cấu hình SSH Key chính xác, máy chủ sẽ cho phép đăng nhập mà không yêu cầu mật khẩu của tài khoản `root`.

---

## 6. Kiểm tra sau khi đăng nhập

### 6.1. Kiểm tra user

```bash
whoami
```

Kết quả mong đợi:

```text
root
```

### 6.2. Kiểm tra hostname

```bash
hostname
```

### 6.3. Kiểm tra hệ điều hành

```bash
lsb_release -a
```

Hoặc:

```bash
cat /etc/os-release
```

Kết quả mong đợi hiển thị hệ điều hành Ubuntu Server.

---

## 7. Kết quả mong đợi

Sau khi hoàn thành cấu hình, terminal sẽ hiển thị nội dung tương tự:

```text
Welcome to Ubuntu

root@server:~#
```

Điều này chứng minh:

- Máy chủ Ubuntu đã hoạt động.
- Máy cá nhân kết nối được tới máy chủ qua Internet.
- SSH Key đã được cấu hình chính xác.
- Có thể đăng nhập bằng tài khoản `root`.
- Không cần sử dụng mật khẩu thô.

---

## 8. Minh chứng

### 8.1. Ảnh giao diện quản lý máy chủ

Thêm ảnh chụp màn hình tại đây:

```markdown
![Server](./images/server.png)
```

### 8.2. Ảnh hoặc log SSH thành công

Thêm ảnh chụp PowerShell tại đây:

```markdown
![SSH Success](./images/ssh-success.png)
```

Hoặc chèn đoạn log SSH thật đã nhận được khi kết nối thành công.

> Không sử dụng log giả. Phần này cần được thay bằng ảnh hoặc log thực tế trước khi nộp nếu giảng viên yêu cầu minh chứng.

---

## 9. Cấu trúc thư mục nộp bài

```text
homework/
└── session_02/
    └── ex1/
        ├── README.md
        └── images/
            ├── server.png
            └── ssh-success.png
```

---

## 10. Kết luận

Qua bài thực hành, em đã tìm hiểu quy trình tạo một máy chủ Ubuntu trên hạ tầng Cloud/VPS và phương pháp xác thực an toàn bằng SSH Key.

Các bước chính gồm:

1. Tạo cặp SSH Key trên máy cá nhân.
2. Cấu hình Public Key trên máy chủ.
3. Khởi tạo Ubuntu Server.
4. Lấy Public IP của máy chủ.
5. Kết nối bằng SSH.
6. Kiểm tra user và hệ điều hành.

SSH Key giúp tăng tính bảo mật so với việc sử dụng mật khẩu thông thường và là phương pháp phổ biến khi quản trị máy chủ Linux từ xa.
