---
title: "Deliverable 1"
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
### 📚 Tổng quan Thư viện và Kế hoạch Dự án Giữa Kỳ
💡 Mục đích của Thư viện Milvus
### 🔍 Milvus là gì?
#### Milvus là một thư viện mã nguồn mở dùng để lưu trữ và tìm kiếm vector (vector database). Nó đặc biệt hữu ích trong các ứng dụng AI như:

##### 🖼️ Tìm kiếm hình ảnh, văn bản, âm thanh tương tự nhau

🧠 Hệ thống gợi ý

🧍‍♂️ Nhận diện khuôn mặt

📊 Trích xuất đặc trưng từ dữ liệu phi cấu trúc

### ❓ Milvus có thể giải quyết vấn đề gì?
Trong các hệ thống AI, dữ liệu thường được chuyển thành vector embedding. Việc tìm kiếm các vector gần nhất trong hàng triệu vector là một bài toán phức tạp.

### Milvus hỗ trợ:

✅ Lưu trữ vector hiệu quả (từ hàng triệu đến hàng tỷ vector)

✅ Tìm kiếm vector theo thời gian thực với độ trễ cực thấp

✅ Hỗ trợ nhiều thuật toán Approximate Nearest Neighbor (ANN) như:

+ Faiss

+ HNSW

### 🤖 Vì sao Milvus sử dụng ANN?
Các thuật toán ANN giúp:

⚡ Tìm kiếm cực nhanh (chỉ vài mili giây)

💰 Tối ưu chi phí phần cứng

🎯 Đủ chính xác cho ứng dụng thực tế (accuracy > 95%)

🌟 Ưu - Nhược điểm của Milvus
✅ Điểm mạnh
🔌 Dễ tích hợp với hệ thống AI

⚡ Hiệu năng cao

📈 Scalable (kiến trúc phân tán mở rộng dễ dàng)

### 🤖 Tối ưu cho các ứng dụng AI

#### ❌ Điểm yếu
Không hỗ trợ sẵn các tính năng nâng cao như clustering, retry logic

Phải xử lý reconnect và error handling thủ công

```
🔁 So sánh với các thư viện Message Queue khác
| Thư viện     | Ưu điểm                              | Nhược điểm                               |
| ------------ | ------------------------------------ | ---------------------------------------- |
| **pika**     | Nhẹ, dễ dùng cho người mới           | Thiếu tính năng nâng cao                 |
| **kombu**    | Abstraction cao, tích hợp Celery tốt | Nặng hơn, phức tạp hơn                   |
| **aio-pika** | Hỗ trợ async/await hiện đại          | Khó tích hợp vào hệ thống đồng bộ (sync) |
```
📡 flask-jsonrpc – Giao tiếp RPC qua JSON

🎯 Mục đích sử dụng: Gọi hàm từ xa (Remote Procedure Call) qua JSON

Nhẹ hơn XML-RPC, dễ đọc, dễ debug

#### ✅ Điểm mạnh
Dễ tích hợp với Flask

Cấu hình đơn giản

Hỗ trợ type hinting

#### ❌ Điểm yếu
Thiếu nhiều tính năng so với gRPC (streaming, code generation)

Hệ sinh thái chưa mạnh như REST/GraphQL

🔁 So sánh với các framework RPC khác
```
| Framework     | Giao thức         | Ưu điểm                       | Nhược điểm                      |
| ------------- | ----------------- | ----------------------------- | ------------------------------- || flask-jsonrpc | JSON-RPC          | Nhẹ, dễ dùng                  | Không hỗ trợ stream full-duplex |
| gRPC          | HTTP/2 + ProtoBuf | Nhanh, type-safe, đa ngôn ngữ | Cần biên dịch, setup phức tạp   |
| xmlrpc.client | XML-RPC           | Đơn giản, có sẵn trong Python | Định dạng XML nặng, lỗi thời    |
``` 
📌 Kế hoạch Dự kiến Bài Giữa Kỳ
### 🧪 Đề tài: Tìm kiếm sản phẩm tương tự trong hệ thống e-commerce
#### 📌 1. Vấn đề cần giải quyết
Người dùng muốn được gợi ý sản phẩm tương tự dựa trên:

📝 Mô tả văn bản sản phẩm

🖼️ Hình ảnh sản phẩm

⚙️ Thuộc tính kỹ thuật: màu, hãng, kích thước...

#### 2. 🛠️ Kế Hoạch Triển Khai
```

| Tuần       | Công Việc Chính                                                                                                                                  |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Tuần 1** | 🔍 Phân tích yêu cầu hệ thống và loại dữ liệu cần thiết <br>📁 Lên danh sách thuộc tính sản phẩm (mô tả, ảnh, màu sắc, kích thước,...)           |
| **Tuần 2** | 📦 Thu thập và tiền xử lý dữ liệu <br>📌 Dataset từ Kaggle hoặc thu thập thủ công <br>🧹 Làm sạch và chuẩn hóa dữ liệu                           |
| **Tuần 3** | 🧠 Triển khai mô hình xử lý văn bản và hình ảnh <br>📝 Vector hóa văn bản bằng TF-IDF / Word2Vec <br>🖼️ Trích đặc trưng ảnh bằng ResNet (Torch) |
| **Tuần 4** | 🔍 Tích hợp hệ thống tìm kiếm tương tự (Milvus) <br>⚙️ Kết hợp vector ảnh và văn bản để gợi ý <br>📤 Kết nối API bằng Flask + flask-jsonrpc      |
| **Tuần 5** | 🧪 Kiểm thử toàn hệ thống và demo <br>🧾 Viết báo cáo tổng kết <br>🗣️ Chuẩn bị slide thuyết trình giữa kỳ                                     |
                                     |
```
#### 🚀 3. Ứng dụng của dự án
🛒 Gợi ý sản phẩm liên quan khi người dùng xem sản phẩm

🔁 Tăng tương tác và doanh thu

🤖 Là nền tảng cho hệ thống gợi ý thông minh hơn sau này
#### 🧰 4. Thư viện sử dụng
```
| Thư viện        | Mục đích sử dụng                                    |
| --------------- | --------------------------------------------------- |
| `milvus`        | Lưu trữ và tìm kiếm vector nhanh chóng              |
| `scikit-learn`  | Vector hóa văn bản (TF-IDF), tính Cosine Similarity |
| `torch`         | Trích đặc trưng ảnh bằng mô hình ResNet             |
| `flask-jsonrpc` | Cung cấp API gợi ý sản phẩm                         |