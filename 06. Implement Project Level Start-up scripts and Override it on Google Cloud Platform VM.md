![](https://media2.dev.to/dynamic/image/width=1000,height=420,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fnbbznlwk91a6wky0j12g.png)
## **Phần 6: Triển khai Startup Script ở cấp độ Project và ghi đè bằng Startup Script ở cấp độ VM trong Google Cloud Platform (GCP)**

---

## 1. Khái niệm Startup Script ở cấp độ Project

**Startup Script** là một kịch bản được thực thi tự động mỗi khi máy ảo (VM) khởi động.

Phân loại theo phạm vi áp dụng:

* **Startup Script cấp độ VM (VM-level):**
  Chỉ áp dụng cho một VM cụ thể.
* **Startup Script cấp độ Project (Project-level):**
  Mặc định áp dụng cho **tất cả các VM** thuộc cùng một dự án Google Cloud.

Startup Script ở cấp độ Project được sử dụng khi cần **áp đặt các cấu hình chung** cho toàn bộ VM trong dự án, ví dụ:

* Cài đặt phần mềm diệt virus.
* Cài đặt agent giám sát (monitoring agent).
* Cài đặt agent ghi log (logging agent).
* Chuẩn hóa môi trường hệ điều hành.

---

## 2. Bước 1: Định nghĩa Startup Script ở cấp độ Project


Thực hiện theo đường dẫn:

**Compute Engine → Settings → Metadata**

* Chọn **Edit**
* Thêm Metadata với các thông tin sau:

**Metadata Key:**
`startup-script`

**Metadata Value:**

```bash
#!/bin/bash
sudo apt install -y telnet
sudo apt install -y nginx
sudo systemctl enable nginx
sudo chmod -R 755 /var/www/html
HOSTNAME=$(hostname)
sudo echo "<!DOCTYPE html> <html> <body style='background-color:rgb(250, 210, 310);'> <h1>PROJECT-LEVEL STARTUP SCRIPT Welcome to Latchu@DevOps - WebVM App1 </h1> <p><strong>VM Hostname:</strong> $HOSTNAME</p> <p><strong>VM IP Address:</strong> $(hostname -I)</p> <p><strong>Application Version:</strong> V1</p> <p>Google Cloud Platform - Demos</p> </body></html>" | sudo tee /var/www/html/index.html
```

Startup Script này sẽ tự động:

* Cài đặt `telnet`.
* Cài đặt và kích hoạt dịch vụ **Nginx**.
* Thiết lập quyền truy cập thư mục web.
* Tạo trang HTML hiển thị thông tin VM.

Lưu lại thay đổi metadata ở cấp độ Project.

![](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fb8h77ysx2x6ubu126nk4.png)

---

## 3. Bước 2: Tạo VM mới để kiểm tra Startup Script cấp Project

![](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fb8h77ysx2x6ubu126nk4.png)

Thực hiện:

**Compute Engine → VM Instances → Create Instance**

Cấu hình VM:

* **Name:** `projectlevel-startuscript-demo`
* **Machine Type:** `e2-micro`
* **Firewall:** Cho phép HTTP traffic
* Các thiết lập khác: Giữ mặc định

Chọn **Create** để tạo VM.

Sau khi VM khởi động xong, truy cập **External IP** của VM trên trình duyệt. Trang web hiển thị sẽ được tạo bởi **Startup Script ở cấp độ Project**, xác nhận script đã được áp dụng thành công.

![](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F7hkn6g88l9i9fp1of2co.png)
---

## 4. Bước 3: Ghi đè Startup Script cấp Project bằng Startup Script cấp VM

Trong trường hợp không muốn VM áp dụng Startup Script ở cấp độ Project, có thể **ghi đè** bằng cách cấu hình **Startup Script ở cấp độ VM**.

Tạo một VM mới với cấu hình sau:

* **Name:** `override-projectlevel-startuscript`
* **Machine Type:** `e2-micro`
* **Firewall:** Cho phép HTTP traffic

Tại:

**Management → Automation → Startup Script**

Dán nội dung script sau:

```bash
#!/bin/bash
sudo apt install -y telnet
sudo apt install -y nginx
sudo systemctl enable nginx
sudo chmod -R 755 /var/www/html
HOSTNAME=$(hostname)
sudo echo "<!DOCTYPE html> <html> <body style='background-color:rgb(2, 210, 210);'> <h1>OVERRIDE STARTUP-SCRIPT - Welcome to Latchu@DevOps - WebVM App1 </h1> <p><strong>VM Hostname:</strong> $HOSTNAME</p> <p><strong>VM IP Address:</strong> $(hostname -I)</p> <p><strong>Application Version:</strong> V1</p> <p>Google Cloud Platform - Demos</p> </body></html>" | sudo tee /var/www/html/index.html
```

Chọn **Create** để khởi tạo VM.

VM này sẽ **chỉ thực thi Startup Script ở cấp độ VM**, và **bỏ qua hoàn toàn Startup Script ở cấp độ Project**.

![](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fwwxmpopwbwwto6lxyogi.png)
---

## 5. Bước 4: Dọn dẹp tài nguyên (Clean Up)

![](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fy60nn35gbrgso2e2t2lv.png)

* Xóa các VM đã tạo phục vụ cho mục đích kiểm thử.

* Gỡ bỏ Startup Script ở cấp độ Project:

  * Vào **Compute Engine → Settings → Metadata → Edit**
  * Xóa metadata key `startup-script`
  * Chọn **Save**

Hoàn tất việc dọn dẹp tài nguyên và cấu hình.
