**Phần 19: GCE Ops Agent – Logging & Monitoring trong Google Cloud Platform (GCP)**

#

Khi chạy các workload trên **Google Compute Engine (GCE)**, việc **giám sát (monitoring)** và **ghi log (logging)** là yếu tố rất quan trọng để giữ cho hệ thống luôn ổn định và ứng dụng hoạt động đáng tin cậy.

Hiện nay, Google khuyến nghị sử dụng **Ops Agent** — một giải pháp hiện đại, hợp nhất để thu thập **logs, metrics và traces** từ các máy ảo (VM).

Hãy cùng tìm hiểu chi tiết. 👇

---

# Vì sao nên sử dụng Ops Agent?

Trước đây Google sử dụng các **agent cũ (legacy agents)** cho logging và monitoring, tuy nhiên:

❌ Không còn phát triển thêm tính năng mới
❌ Không hỗ trợ các phiên bản hệ điều hành mới
⚠️ Chỉ còn ở chế độ **bảo trì (maintenance-only)**

Vì vậy, **Ops Agent** hiện là lựa chọn được khuyến nghị cho tất cả các workload mới.

Nếu bạn vẫn đang sử dụng các **agent cũ**, đã đến lúc **chuyển sang Ops Agent**.

---

# Ops Agent là gì?

**Ops Agent** là một **agent duy nhất** chạy trên các VM của Compute Engine để:

📜 Thu thập **logs** → gửi về **Cloud Logging**

📊 Thu thập **metrics và traces** → gửi về **Cloud Monitoring**

Ops Agent sử dụng các công nghệ:

🛠 **Fluent Bit** để xử lý log

🛠 **OpenTelemetry Collector** để thu thập metrics và traces

Ops Agent được thiết kế để chạy trên cả:

* **Linux VM**
* **Windows VM**

và hỗ trợ nhiều phương thức cài đặt linh hoạt.
![](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fzp7ra4fmpoj2m1zo0kax.png)
---

# Các tính năng chính

## 🔧 Cài đặt và quản lý

Bạn có thể triển khai **Ops Agent** theo nhiều cách khác nhau:

* Tự động cài đặt khi **tạo VM**
* Cài đặt hàng loạt bằng **gcloud** hoặc các công cụ tự động hóa như:

  * Ansible
  * Chef
  * Puppet
  * Terraform
* Quản lý bằng **Agent policies thông qua CLI**
* **Cài đặt thủ công** trên từng VM

---

## 📝 Cấu hình bằng YAML

Ops Agent sử dụng **file cấu hình YAML**:

* Cấu hình đơn giản và linh hoạt
* Dễ dàng tùy chỉnh việc **thu thập, phân tích và lọc log**

---

# Tính năng Logging

🚀 **Hiệu năng tốt hơn** so với logging agent cũ

📂 Có thể thu thập log từ:

* **System logs**

  * `/var/log/syslog`
  * `/var/log/messages`
* **File log**

  * hỗ trợ đường dẫn tùy chỉnh
* **TCP protocol streams**
* **Forward protocol**

  * Fluent Bit
  * Fluentd

### 🛠 Xử lý log linh hoạt

* Chuyển **log không có cấu trúc** thành **JSON có cấu trúc**
* Phân tích log bằng **Regex**
* Loại bỏ log bằng **labels hoặc regex**

### 🔌 Hỗ trợ ứng dụng bên thứ ba

Ops Agent có thể thu thập log từ nhiều ứng dụng phổ biến như:

* Apache Kafka
* Nginx
* Hadoop
* MongoDB
* MySQL
* Redis
* Oracle Database
* SAP HANA
* và nhiều ứng dụng khác

---

# Tính năng Monitoring

📊 **Thu thập các system metrics sẵn có**

* CPU
* Disk
* Memory
* Processes
* Networking
* Swap

Hỗ trợ thêm:

* **GPU (Linux)**
* **IIS, MSSQL, Pagefile (Windows)**

---

### 🔌 Tích hợp với ứng dụng bên thứ ba

Có thể giám sát các ứng dụng như:

* Kafka
* Nginx
* MariaDB
* MongoDB
* Redis
* WildFly
* và nhiều ứng dụng khác

---

### 📡 Thu thập metrics từ Prometheus

Ops Agent hỗ trợ thu thập **Prometheus metrics** từ các ứng dụng đang chạy trên Compute Engine.

---

### 🎮 Giám sát GPU NVIDIA

Có thể theo dõi GPU NVIDIA thông qua **tích hợp DCGM (Data Center GPU Manager)**.

---

# Tổng kết

Nếu bạn đang chạy workload trên **Google Compute Engine**, việc sử dụng **Ops Agent** là lựa chọn rất hợp lý:

✅ Một agent duy nhất cho cả **logs và metrics**
✅ Được **phát triển và hỗ trợ lâu dài**
✅ **Hiệu năng tốt hơn** và hỗ trợ nhiều ứng dụng bên thứ ba
✅ **Triển khai linh hoạt** ở quy mô lớn

Google đã khẳng định rõ: hãy **chuyển các workload sang Ops Agent ngay từ bây giờ** để có khả năng **quan sát hệ thống (observability) tốt hơn** cho hạ tầng của bạn.
