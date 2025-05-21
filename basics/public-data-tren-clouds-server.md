---
icon: bullhorn
---

# Public data Trên clouds Server

MinIO là một nền tảng lưu trữ đối tượng (object storage) hiệu suất cao, tương thích với S3. Bạn có thể sử dụng MinIO để chia sẻ và quản lý các **public datasets**, giúp dễ dàng truy cập từ các ứng dụng hoặc máy học (Machine Learning). Dưới đây là hướng dẫn chi tiết về cách làm điều này.

***

### 1. Điều Kiện Cần Thiết

* **MinIO Server** đã được thiết lập và hoạt động.
* Có quyền truy cập quản trị (admin) để tạo bucket và thiết lập quyền.
* **mc (MinIO Client)** đã được cài đặt hoặc sử dụng giao diện web MinIO.

***

### 2. Tạo Public Dataset Trên MinIO

#### 2.1. Tạo Bucket

**Sử dụng MinIO Client (`mc`):**

```bash
mc alias set myminio http://<minio-server>:<port> <access-key> <secret-key>
mc mb myminio/public-dataset
```

* Thay `<minio-server>:<port>` bằng địa chỉ và cổng của MinIO Server.
* `<access-key>` và `<secret-key>` là thông tin xác thực.

**Sử dụng Giao Diện Web:**

1. Đăng nhập vào MinIO Web UI.
2. Nhấp vào **"Create Bucket"**, nhập tên bucket (ví dụ: `public-dataset`), và nhấn **"Create"**.

***

#### 2.2. Thiết Lập Bucket Là Public

**Sử dụng MinIO Client (`mc`):**

```bash
mc anonymous set public myminio/public-dataset
```

**Sử dụng Giao Diện Web:**

1. Truy cập bucket `public-dataset`.
2. Nhấp vào **"Settings"** hoặc **"Access Policy"**.
3. Chọn **"Public"** và lưu thay đổi.

***

### 3. Tải Dataset Lên Bucket

**Sử dụng MinIO Client (`mc`):**

```bash
mc cp /path/to/local/dataset.csv myminio/public-dataset/dataset.csv
```

**Sử dụng Giao Diện Web:**

1. Truy cập vào bucket `public-dataset`.
2. Nhấp **"Upload"** và chọn tệp hoặc thư mục để tải lên.

***

### 4. Truy Cập Dataset Công Khai

Sau khi bucket được đặt là **public**, mọi người có thể truy cập các dataset thông qua URL:

```php-template
http://<minio-server>:<port>/public-dataset/<file-name>
```

Ví dụ:\
Nếu MinIO Server là `http://192.168.1.100:9000` và file là `dataset.csv`, đường dẫn sẽ là:

```vbnet
http://192.168.1.100:9000/public-dataset/dataset.csv
```

***

### 5. Tích Hợp Dataset Trong Machine Learning

#### Python (Sử Dụng `boto3`):

Nếu bạn muốn sử dụng Python để truy cập dataset:

```python
import boto3

# Cấu hình kết nối MinIO
s3 = boto3.client(
    's3',
    endpoint_url='http://<minio-server>:<port>',
    aws_access_key_id='<access-key>',
    aws_secret_access_key='<secret-key>'
)

# Tải dataset
bucket_name = 'public-dataset'
object_name = 'dataset.csv'
local_file = '/path/to/save/dataset.csv'

s3.download_file(bucket_name, object_name, local_file)
print(f"Dataset saved at {local_file}")
```

#### Truy cập trực tiếp từ Pandas:

```python
import pandas as pd

url = 'http://<minio-server>:<port>/public-dataset/dataset.csv'
df = pd.read_csv(url)
print(df.head())
```

***

### 6. Lưu Ý Quan Trọng

1. **Quyền Riêng Tư**:
   * Đảm bảo chỉ chia sẻ các dataset công khai, không chứa thông tin nhạy cảm.
2. **Giới Hạn Truy Cập**:
   * Nếu không muốn dataset hoàn toàn công khai, bạn có thể sử dụng pre-signed URLs hoặc chính sách bucket cụ thể.
3. **Tối Ưu Dữ Liệu**:
   * Nén dữ liệu trước khi tải lên để giảm băng thông và chi phí lưu trữ.

***

MinIO giúp bạn dễ dàng quản lý và chia sẻ datasets, là một công cụ tuyệt vời cho các dự án Machine Learning hoặc nghiên cứu dữ liệu. 🚀
