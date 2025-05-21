---
description: >-
  SSHFS là một công cụ mạnh mẽ cho phép bạn gắn kết (mount) một thư mục từ xa
  qua giao thức SSH, giúp truy cập và chia sẻ dữ liệu như thể nó nằm trên máy
  cục bộ của bạn. Đây là giải pháp lý tưởng khi bạ
icon: markdown
---

# Chia sẻ dữ liệu giữa các server với SSHFS

### 1. Điều Kiện Yêu Cầu

* **SSH Key đã được tạo**: Nếu chưa có SSH Key, hãy tham khảo hướng dẫn tạo SSH Key trước.
* **Thêm SSH Key vào máy chủ từ xa**: Đảm bảo rằng bạn đã thêm public key (`id_rsa.pub`) vào file `~/.ssh/authorized_keys` trên máy chủ từ xa.

***

### 2. Kết Nối SSHFS Bằng SSH Key

#### Cú pháp:

```bash
sshfs -o IdentityFile=/path/to/private_key [user]@[remote_host]:[remote_directory] [local_mount_point]
```

#### Ví dụ:

Giả sử bạn muốn gắn kết thư mục `/data` trên máy chủ từ xa vào thư mục `/mnt/data` trên máy cục bộ, sử dụng SSH Key tại `~/.ssh/id_rsa`:

```bash
sshfs -o IdentityFile=~/.ssh/id_rsa username@192.168.1.100:/data /mnt/data
```

#### Giải thích:

* `-o IdentityFile=~/.ssh/id_rsa`: Chỉ định đường dẫn đến private key.
* `username`: Tên người dùng trên máy chủ từ xa.
* `192.168.1.100`: Địa chỉ IP hoặc tên miền của máy chủ từ xa.
* `/data`: Thư mục trên máy chủ từ xa cần gắn kết.
* `/mnt/data`: Thư mục cục bộ trên máy của bạn nơi thư mục từ xa sẽ được mount.

***

### 3. Tùy Chọn Hữu Ích Với SSH Key

*   **Tự động kết nối lại**:

    ```bash
    sshfs -o reconnect,IdentityFile=~/.ssh/id_rsa username@remote_host:/data /mnt/data
    ```
*   **Cho phép người dùng khác trên máy cục bộ truy cập vào thư mục đã mount**:

    ```bash
    sshfs -o allow_other,IdentityFile=~/.ssh/id_rsa username@remote_host:/data /mnt/data
    ```

***

### 4. Gỡ Gắn Kết Thư Mục

Khi không cần sử dụng nữa, bạn có thể gỡ thư mục bằng lệnh:

```bash
fusermount -u /mnt/data  # Trên Linux
umount /mnt/data         # Trên macOS
```

***

### 5. Một Số Lưu Ý

1.  **Phân quyền key**:\
    Đảm bảo private key (`id_rsa`) có quyền truy cập phù hợp (chỉ chủ sở hữu có quyền đọc):

    ```bash
    chmod 600 ~/.ssh/id_rsa
    ```
2.  **Quản lý key**:\
    Nếu sử dụng nhiều SSH Key, bạn có thể cấu hình `~/.ssh/config` để dễ dàng quản lý. Ví dụ:

    ```bash
    Host remote_server
        HostName 192.168.1.100
        User username
        IdentityFile ~/.ssh/id_rsa
    ```

    Sau đó, bạn có thể kết nối SSHFS đơn giản hơn:

    ```bash
    sshfs remote_server:/data /mnt/data
    ```
3. **Yêu cầu SSH đã được thiết lập**:
   * Máy chủ từ xa phải cho phép kết nối SSH.
   * Bạn cần quyền truy cập vào thư mục từ xa.
4. **Tốc độ truy cập phụ thuộc vào mạng**:
   * Kết nối mạng chậm sẽ làm giảm hiệu suất khi truy cập dữ liệu qua SSHFS.
5. **Phân quyền thư mục**:
   * Đảm bảo thư mục cục bộ (`local_mount_point`) có quyền truy cập phù hợp.

***

### 6. Ứng Dụng Trong Machine Learning

* **Chia sẻ dữ liệu huấn luyện**:\
  Gắn kết thư mục chứa tập dữ liệu từ một máy chủ chính vào nhiều máy khác để chạy các phiên bản huấn luyện song song.
* **Truy cập dữ liệu trực tiếp**:\
  Sử dụng SSHFS để truy cập dữ liệu từ xa trong các script Python hoặc Jupyter Notebook mà không cần sao chép dữ liệu về máy cục bộ.

Ví dụ, trong Python:

```python
import os
data_dir = "/mnt/data/dataset"
print(os.listdir(data_dir))  # Truy cập dữ liệu đã mount
```

Với **SSHFS**, bạn có thể tối ưu hoá việc sử dụng tài nguyên và tăng tốc độ phát triển dự án ML thông qua việc chia sẻ dữ liệu dễ dàng và an toàn.
