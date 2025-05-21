---
icon: circle-chevron-right
---

# Điều khiển với kết nối SSH

## A. Hướng Dẫn Tạo SSH Key

SSH Key giúp bạn thiết lập kết nối an toàn giữa máy tính cá nhân và máy chủ từ xa. Dưới đây là các bước để tạo SSH Key:

***

### 1. Tạo SSH Key

<figure><img src="../.gitbook/assets/Screenshot 2025-01-24 231022.jpg" alt=""><figcaption></figcaption></figure>

#### Bước 1: Mở terminal trên máy tính của bạn

* Trên **Linux** và **macOS**: Sử dụng ứng dụng Terminal.
* Trên **Windows**: Dùng Git Bash hoặc Command Prompt có hỗ trợ OpenSSH.

#### Bước 2: Chạy lệnh tạo SSH Key

Nhập lệnh sau vào terminal:

```bash
ssh-keygen -t rsa -b 4096 -C "email@example.com"
```

Giải thích:

* `-t rsa`: Sử dụng thuật toán RSA.
* `-b 4096`: Đặt độ dài key là 4096 bit để tăng cường bảo mật.
* `-C "email@example.com"`: Gắn nhãn cho key bằng email của bạn.

#### Bước 3: Chọn đường dẫn lưu key

Khi được hỏi nơi lưu trữ key, bạn có thể:

* Nhấn **Enter** để lưu vào đường dẫn mặc định: `~/.ssh/id_rsa`.
* Hoặc nhập một đường dẫn khác nếu muốn.

#### Bước 4: Đặt mật khẩu bảo vệ key (tùy chọn)

* Bạn có thể đặt mật khẩu để bảo vệ private key.
* Nhấn **Enter** để bỏ qua nếu không cần mật khẩu.

#### Bước 5: Kiểm tra SSH Key

Để xem nội dung public key, sử dụng lệnh:

```bash
cat ~/.ssh/id_rsa.pub
```

Sao chép toàn bộ nội dung của public key (bắt đầu bằng `ssh-rsa`).

***

### 2. Cấu Hình Kết Nối SSH

#### Bước 1: Đăng nhập vào JupyterHub Server

Đăng nhập vào JupyterHub với username/password đã được cấp

#### Bước 2: Thêm public key vào máy chủ

Mở terminal trên Jupyter

Dán nội dung public key (sao chép ở bước trước) vào file `authorized_keys`:

```bash
echo "nội_dung_của_public_key" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

#### Bước 3: Thử kết nối SSH

Trên máy tính cá nhân thử kết nối lại bằng SSH Key trên terminal:

```bash
ssh -i ~/.ssh/id_rsa -p <ServerPort> <UserName>@<ServerDomain>
```

***

| ServerDomain            | ServerPort |
| ----------------------- | ---------- |
| notebook.iec-uit.com    | 8280       |
| bmtt.iec-uit.com        | 8286       |
| pre-master.iec-uit.com  | 8281       |
| master.iec-uit.com      | 8282       |
| post-master.iec-uit.com | 8283       |

## B. Hướng Dẫn Sử Dụng VSCode và PyCharm

### 1. Sử Dụng VSCode

VSCode (Visual Studio Code) hỗ trợ tích hợp SSH để kết nối và làm việc trực tiếp trên máy chủ từ xa.

#### Bước 1: Cài đặt phần mở rộng

* Mở VSCode và vào **Extensions**.
* Cài đặt tiện ích **Remote - SSH**.

#### Bước 2: Cấu hình kết nối

* Nhấn `Ctrl + Shift + P` (hoặc `Cmd + Shift + P` trên macOS) để mở Command Palette.
* Gõ `Remote-SSH: Connect to Host`.
* Nhập thông tin kết nối theo định dạng:

```bash
ssh -i ~/.ssh/id_rsa -p <ServerPort> <UserName>@<ServerDomain>
```

* Nếu được yêu cầu, chọn file private key của bạn (`~/.ssh/id_rsa`).

#### Bước 3: Làm việc trên máy chủ

* Sau khi kết nối thành công, bạn có thể mở thư mục hoặc tệp trên máy chủ và làm việc trực tiếp.

***

### 2. Sử Dụng PyCharm

PyCharm hỗ trợ kết nối SSH để phát triển phần mềm từ xa.

#### Bước 1: Thiết lập môi trường từ xa

1. Mở **Settings** (`Ctrl + Alt + S`) và vào **Project Settings > Python Interpreter**.
2. Nhấn vào biểu tượng `+`, chọn **Add Interpreter** > **SSH Interpreter**.
3. Nhập thông tin kết nối SSH:
   * Host: `ServerDomain`.
   * User: `UserName`.
   * Port: `ServerPort`.
   * Authentication: Chọn file private key (`~/.ssh/id_rsa`).

#### Bước 2: Đồng bộ dự án với máy chủ

1. Vào **Tools > Deployment > Configuration**.
2. Thêm kết nối SSH và cấu hình thư mục trên máy chủ.

#### Bước 3: Làm việc và chạy dự án

* Sau khi cấu hình, PyCharm sẽ đồng bộ mã nguồn giữa máy tính và máy chủ.
* Bạn có thể chạy hoặc gỡ lỗi mã trực tiếp trên máy chủ.

***

Với các bước trên, bạn đã sẵn sàng sử dụng SSH Key và các công cụ như VSCode hoặc PyCharm để làm việc hiệu quả với hệ thống từ xa. 😊
