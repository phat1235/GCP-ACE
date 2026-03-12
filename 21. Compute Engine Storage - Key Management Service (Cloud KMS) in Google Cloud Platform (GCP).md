**Phần 21: Compute Engine Storage – Key Management Service (Cloud KMS) trong Google Cloud Platform (GCP)**

---
![](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F0dy9ru92zyur4jxj39uh.png)
# Mã hóa dữ liệu theo trạng thái (Data States Encryption)

Dữ liệu có thể tồn tại ở nhiều trạng thái khác nhau, và việc mã hóa có thể được áp dụng tùy theo trạng thái của dữ liệu.

---

# Các loại mã hóa dữ liệu
![](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fn4lcc0otxmp527hnoww3.png)
Khuyến nghị **mã hóa cả hai loại dữ liệu sau**:

* **Data at Rest** – Dữ liệu được lưu trữ (trên ổ đĩa, cơ sở dữ liệu, storage)
* **Data in Transit** – Dữ liệu đang được truyền qua mạng

Việc mã hóa cả hai trạng thái giúp **tăng cường bảo mật cho hệ thống**.

---

# Mã hóa khóa đối xứng (Symmetric Key Encryption)
![](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F6ddo1a6xc1p4ty65mf6s.png)
**Symmetric Key Encryption** sử dụng **cùng một khóa** cho cả hai quá trình:

* **Mã hóa (Encryption)**
* **Giải mã (Decryption)**

### Ví dụ các thuật toán mã hóa

* **DES** – Data Encryption Standard
* **Triple DES**
* **AES** – Advanced Encryption Standard
* **IDEA** – International Data Encryption Algorithm

---

## Ưu điểm

**Bảo mật:**
Các thuật toán như **AES** có thể mất **hàng tỷ năm** để bị phá vỡ bằng phương pháp brute-force.

**Tốc độ:**
Do sử dụng khóa ngắn hơn nên quá trình **mã hóa và giải mã nhanh hơn**, tiêu tốn ít tài nguyên hệ thống hơn như:

* CPU
* Memory

**Được sử dụng rộng rãi trong ngành:**
Các thuật toán như **AES** đã trở thành **tiêu chuẩn vàng trong mã hóa dữ liệu** nhờ vào tính bảo mật cao và hiệu năng tốt.

👉 **Được khuyến nghị sử dụng cho việc truyền dữ liệu dung lượng lớn (Bulk Data Transfers)**

---

## Thách thức

* Làm thế nào để **bảo vệ khóa mã hóa**?
* Làm thế nào để **chia sẻ khóa mã hóa một cách an toàn**?

---

# Mã hóa khóa bất đối xứng (Asymmetric Key Encryption)
![](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fv8y682vz9qwz98rsw7fk.png)
**Asymmetric Key Encryption** sử dụng **hai khóa khác nhau**:

* **Public Key**
* **Private Key**

Nguyên tắc hoạt động:

* **Mã hóa dữ liệu bằng Public Key**
* **Giải mã dữ liệu bằng Private Key**

---

### Ví dụ các thuật toán mã hóa

* **RSA** – Digital Signature Standard
* **DSC** – Digital Signature Standard
* **DSA** – Digital Signature Algorithm
* **ECC** – Elliptic Curve Cryptography

---

## Ưu điểm

* **Private key không cần chia sẻ**, vì vậy toàn bộ quá trình **an toàn hơn so với mã hóa đối xứng**

---

## Nhược điểm

* Quá trình **mã hóa chậm hơn**
* **Tiêu tốn nhiều tài nguyên hệ thống hơn**
* **Không khuyến nghị sử dụng cho truyền dữ liệu dung lượng lớn**

---

# Google Cloud – Key Management Service (KMS)
![](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fw0yhiv2z1ov76j2yib7z.png)
**Cloud KMS** là dịch vụ dùng để **quản lý tập trung các khóa mã hóa trên GCP**.

Cloud KMS hỗ trợ:

* **Mã hóa đối xứng (Symmetric Encryption)**
* **Mã hóa bất đối xứng (Asymmetric Encryption)**

Bạn có thể sử dụng các **khóa mã hóa được tạo bởi KMS** trong:

* Ứng dụng của bạn
* Các dịch vụ GCP như:

  * **Compute Engine**
  * **Cloud SQL**

Cloud KMS cung cấp **API** để:

* **Mã hóa dữ liệu (Encrypt)**
* **Giải mã dữ liệu (Decrypt)**
* **Ký dữ liệu (Sign)**

Các API này có thể được sử dụng trong **quá trình phát triển ứng dụng**.

---

# Các tùy chọn quản lý khóa

Có ba phương thức quản lý khóa mã hóa:

### 1. Google-managed encryption key

* Khóa mã hóa được **Google tự động quản lý**
* **Không cần cấu hình**

### 2. CMEK – Customer Managed Encryption Key

* Khóa mã hóa được **khách hàng quản lý**
* Quản lý thông qua **Cloud KMS**

### 3. CSEK – Customer Supplied Encryption Key

* Khóa mã hóa do **khách hàng tự cung cấp**
* Được quản lý **bên ngoài Google Cloud**

---

📌 **Google-managed encryption key được áp dụng mặc định.**
