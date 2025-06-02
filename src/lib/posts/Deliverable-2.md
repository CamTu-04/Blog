---
title: "Deliverable 2"
date: "2025-05-03"
updated: "2025-05-05"
author: "Hoàng Cẩm Tú"
categories:
  - "Distributed Systems"
  - "Processes and Threads"
  - "Concurrency"
coverImage: "/images/milvus.png"
coverWidth: 16
coverHeight: 9
excerpt: Đề xuất đề tài và mô tả dự án.
---
# Kiến trúc hệ thống tìm kiếm sản phẩm tương tự trong e-commerce

## 1. Bản vẽ kiến trúc hệ thống

![Sơ đồ kiến trúc hệ thống](/images/KTHT.png)

Sơ đồ trên mô tả tổng thể hệ thống phân tán với các thành phần chính bao gồm Load Balancer, các node FastAPI, vectorizer ResNet, cơ sở dữ liệu vector Milvus, và kho lưu trữ hình ảnh.

---

## 2. Mô tả chi tiết các thành phần trong hệ thống

### 2.1. Load Balancer (Nginx)

- **Vai trò:** Phân phối các yêu cầu từ người dùng đến các node FastAPI một cách đều đặn theo thuật toán Round Robin.
- **Cách hoạt động:** Khi người dùng gửi một truy vấn (ảnh), Nginx chọn một node FastAPI trong số ba node để xử lý, giúp cải thiện khả năng mở rộng và độ tin cậy.

### 2.2. FastAPI Nodes (Node 1, 2, 3)

- **Vai trò:** Mỗi node là một instance FastAPI xử lý các request từ người dùng.
- **Chức năng:** 
  - Nhận ảnh từ người dùng.
  - Gọi module ResNet để trích xuất đặc trưng (feature vector).
  - Truy vấn vector đến Milvus để tìm sản phẩm tương tự.
  - Truy xuất thông tin ảnh từ kho lưu trữ và trả kết quả về cho người dùng.
- **Fault Tolerance:** Nếu một node chết, Nginx sẽ loại bỏ node đó trong lần phân phối tiếp theo (giảm downtime).

### 2.3. ResNet (Vectorizer)

- **Vai trò:** Trích xuất đặc trưng (feature vector) từ ảnh đầu vào.
- **Cách hoạt động:** Sử dụng mô hình pretrained ResNet50 từ PyTorch để ánh xạ ảnh sang không gian vector 2048 chiều.

### 2.4. Milvus Lite (Vector Database)

- **Vai trò:** Lưu trữ và tìm kiếm các vector đại diện cho ảnh.
- **Sharding:** Do sử dụng Milvus Lite nên không có sharding theo cụm, nhưng trong bản cluster có thể chia dữ liệu theo `product category`.
- **Replication:** Có thể cấu hình replication trong bản Milvus cluster (nếu mở rộng).
- **Thuật toán tìm kiếm:** IVF_FLAT hoặc HNSW (theo cấu hình), dựa trên khoảng cách cosine hoặc L2.

### 2.5. Image Storage (images/, metadata.json)

- **Vai trò:** Lưu ảnh gốc và thông tin metadata của sản phẩm.
- **Truy xuất:** Khi nhận kết quả từ Milvus (ID vector), hệ thống dùng ID để lấy ảnh và thông tin mô tả từ kho lưu trữ.

---

## 3. Công nghệ và thư viện sử dụng

| Công nghệ | Mục đích | Lý do chọn |
|----------|---------|------------|
| **FastAPI** | Web framework | Hiệu suất cao, async support, đơn giản để build API |
| **Milvus Lite** | Vector database | Tối ưu cho tìm kiếm vector, dễ cài đặt với Docker |
| **PyTorch + ResNet** | Trích xuất vector từ ảnh | ResNet50 có độ chính xác cao và phổ biến |
| **Docker + Docker Compose** | Triển khai hệ thống | Dễ dàng cấu hình nhiều service, phù hợp môi trường dev/test/prod |
| **Nginx** | Load balancing | Nhẹ, cấu hình đơn giản, hỗ trợ Round Robin |
| **Pillow, OpenCV** | Xử lý ảnh | Tiền xử lý ảnh đầu vào |
| **Flask-JSONRPC** *(tuỳ chọn)* | Giao tiếp nội bộ | Giao diện RPC đơn giản giữa các service |

---

## 4. Mô hình dữ liệu (Database Model)

### Vector Storage (Milvus)

- Mỗi vector được lưu với ID tương ứng với ảnh sản phẩm.

- Dữ liệu có thể phân chia theo category (shard key) khi mở rộng.

## 5. Chiến lược triển khai và cấu hình hệ thống

### Công cụ triển khai
**Docker Compose: Dùng để quản lý các service: FastAPI, Milvus, Nginx, ResNet, image storage.**

### Cấu trúc thư mục

```bash
milvus/
├── app.py                   # Backend FastAPI xử lý tìm kiếm ảnh
├── embeddings_to_milvus.py  # Nhúng dữ liệu ảnh hàng loạt vào Milvus (offline)
├── MilvusCollection.py      # Tạo collection Milvus đơn giản từ dòng lệnh
├── FeatureExtractor.py      # Class trích xuất vector đặc trưng từ ảnh bằng ResNet34
├── templates/
│   └── index.html           # Giao diện web để tải ảnh và xem kết quả
├── docker-compose.yml       # Triển khai ứng dụng và Milvus bằng Docker Compose
├── Requirements.txt         # Danh sách các thư viện Python cần thiết
├── images/                  # Thư mục chứa ảnh sản phẩm
├── nginx.conf               # Load Balancing và Reverse Proxy
└── stress_test.sh           # Script kiểm tra hiệu năng hệ thống (stress test)
              
```

### Các bước triển khai



