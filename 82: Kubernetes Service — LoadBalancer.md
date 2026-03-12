**Phần 82: Kubernetes Service — LoadBalancer**

#kubernetes
#cloud
#devops
#learning
![](https://media2.dev.to/dynamic/image/width=1000,height=420,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fzpsskfj3f5umve40sd7l.png)
# Kubernetes Service

Khi triển khai ứng dụng trên **Kubernetes**, các **Pod** có vòng đời ngắn và **địa chỉ IP của chúng có thể thay đổi khi được lên lịch lại (rescheduled)**.
Để cung cấp **truy cập ổn định tới các Pod**, Kubernetes sử dụng **Service**.

s1

---

# Bước 01: Service trong Kubernetes là gì?

**Service** là một lớp trừu tượng định nghĩa **một tập hợp logic các Pod** và **chính sách để truy cập vào chúng**.
Nói cách khác, **Service cung cấp khả năng kết nối mạng ổn định cho các Pod**.
![](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fa8yqosr0w6mwxmh5tn84.png)
---

## Các loại Service trong Kubernetes

**ClusterIP (mặc định)**
→ Chỉ expose ứng dụng **bên trong cluster**

**NodePort**
→ Expose ứng dụng trên **IP của mỗi Node tại một cổng tĩnh**

**LoadBalancer**
→ Expose ứng dụng ra bên ngoài thông qua **Load Balancer của nhà cung cấp Cloud**

**Ingress**
→ Định tuyến thông minh và **kết thúc HTTPS (HTTPS termination)** phía trước nhiều Service

---

# Bước 02: LoadBalancer Service là gì?

Nếu muốn **truy cập ứng dụng từ Internet**, chúng ta sử dụng **LoadBalancer Service**.

Trong **GKE (Google Kubernetes Engine)**, khi tạo một **Service có type là LoadBalancer**, Kubernetes sẽ tự động cấp phát:

* **Google Cloud Load Balancer**
* **Một địa chỉ IP Public (External IP) của Google Cloud**

Địa chỉ **External IP** này chính là địa chỉ mà **người dùng sử dụng để truy cập ứng dụng**.

---

# Bước 03: Cách hoạt động

![](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fdmy3fnuds14xexr9qh9l.png)

Tham khảo sơ đồ trên. Hãy xem quy trình hoạt động từng bước:

---

## Người dùng truy cập

Người dùng mở trình duyệt và nhập **địa chỉ IP public**, ví dụ:

```
http://34.120.55.100
```

---

## Cloud Load Balancer

Yêu cầu này sẽ được gửi đến **Google Cloud Load Balancer**, được **Kubernetes tự động tạo khi triển khai LoadBalancer Service**.

---

## Kubernetes Service (Ánh xạ cổng)

Service lắng nghe tại một cổng (ví dụ: **80**) và định tuyến lưu lượng tới **Pod**.

Bên trong Service:

**port (cổng của ClusterIP)** sẽ chuyển tiếp lưu lượng tới **targetPort của Pod**.

Ví dụ:

**Service port: 80 → Pod containerPort: 80**

---

## Tầng Pod (Container)

Cuối cùng, yêu cầu sẽ tới **Pod đang chạy container NGINX tại cổng 80**.

Container xử lý yêu cầu và gửi phản hồi trở lại theo chuỗi:

**Pod → Service → Load Balancer → Trình duyệt của người dùng**.
