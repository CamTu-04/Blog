---
title: "Milvus Image Search Project"
date: "2025-05-03"
updated: "2025-05-03"
author: "Hoàng Cẩm Tú"
categories:
  - "Distributed Systems"
  - "Processes and Threads"
  - "Concurrency"
coverImage: "/images/milvus.png"
coverWidth: 16
coverHeight: 9
excerpt: Xây dựng hệ thống tìm kiếm sản phẩm tương tự phân tán với Milvus, Flask và Deep Learning.
---
# I. Giới thiệu dự án

## 🏷️ Tên dự án: **Tìm kiếm sản phẩm tương tự trong hệ thống E-Commerce sử dụng Milvus**

## ❓ Vấn đề

Người dùng thường muốn **tìm sản phẩm tương tự** với một món hàng họ yêu thích – có thể là một chiếc áo, đôi giày, hay vật dụng gia đình nào đó.

Làm sao để họ có thể:

- Tải ảnh sản phẩm họ muốn tìm tương tự?
- Gõ một đoạn mô tả sản phẩm để tìm sản phẩm tương tự trên hệ thống?
- Tìm các sản phẩm có cùng loại, màu sắc, kích thước?

## 🎯 Mục tiêu

Dự án nhằm xây dựng một hệ thống có khả năng **gợi ý các sản phẩm tương tự** dựa trên:

- Ảnh sản phẩm
- Mô tả văn bản
- Các thuộc tính kỹ thuật (màu sắc, kích thước, loại sản phẩm...)

Bằng cách kết hợp **AI embedding models** (ResNet, BERT) với **hệ thống tìm kiếm vector phân tán Milvus**, người dùng có thể:

- Tải ảnh sản phẩm hoặc nhập mô tả
- Nhận được danh sách các sản phẩm tương tự đang có trên hệ thống
- Trải nghiệm phản hồi gần như thời gian thực


## 🔧 Tính năng chính

- **Tìm kiếm tương tự bằng ảnh:** sử dụng mô hình ResNet để trích xuất đặc trưng hình ảnh.
- **Tìm kiếm tương tự bằng mô tả văn bản:** sử dụng mô hình BERT hoặc TF-IDF/Word2Vec.
- **Tìm kiếm theo thuộc tính sản phẩm:** phân tích thuộc tính kỹ thuật để nâng độ chính xác.
- **Triển khai phân tán:** hệ thống được container hóa bằng Docker, hỗ trợ load balancing và fault tolerance.
- **Tích hợp Milvus:** dùng làm vector database để tìm kiếm các vector embedding hiệu quả.



## 🧪 Công nghệ sử dụng

| Thành phần               | Công nghệ                                     |
|--------------------------|-----------------------------------------------|
| Xây dựng API             | `Flask`                                       |
| Cơ sở dữ liệu vector     | `Milvus`                                      |
| Mô hình ảnh              | `ResNet`                                      |
| Mô hình văn bản          | `Word2Vec` / `BERT`                           |
| Container hóa & phân tán | `Docker Compose`, `Distributed Systems`       |
| Các khái niệm phân tán   | `Fault Tolerance`, `Sharding`, `Replication`, `Distributed Communication` |

---
## II. 📁 Tổng quan kiến trúc và cấu trúc thư mục dự án

### ✅ Kiến trúc hệ thống

**Sơ đồ các thành phần:**

- Giao tiếp giữa người dùng và hệ thống thông qua REST API được xây dựng bằng **Flask** (`app.py`).
- Dữ liệu được vector hóa thông qua các mô hình **ResNet50** (ảnh) và **Word2Vec/BERT** (văn bản).
- Các vector được gửi đến **Milvus** để tìm kiếm vector tương tự.
- Kết quả được trả về từ Milvus và phản hồi lại người dùng.
- Toàn bộ các thành phần được container hóa bằng **Docker Compose** để dễ triển khai và mở rộng.


### ✅ Cấu trúc thư mục

```bash
milvus/
├── app.py                    # Backend FastAPI xử lý tìm kiếm ảnh
├── embeddings_to_milvus.py  # Nhúng dữ liệu ảnh hàng loạt vào Milvus (offline)
├── MilvusCollection.py      # Tạo collection Milvus đơn giản từ dòng lệnh
├── FeatureExtractor.py      # Class trích xuất vector đặc trưng từ ảnh bằng ResNet34
├── templates/
│   └── index.html           # Giao diện web để tải ảnh và xem kết quả
├── docker-compose.yml       # Triển khai ứng dụng và Milvus bằng Docker Compose
├── Requirements.txt         # Danh sách các thư viện Python cần thiết
├── images/                  # Thư mục chứa ảnh sản phẩm
└── stress_test.sh           # Script kiểm tra hiệu năng hệ thống (stress test)
              
```

### ✅ Tiến trình hoạt động hệ thống tìm kiếm sản phẩm tương tự

**Bước 1: Người dùng gửi yêu cầu tìm kiếm ảnh**
- Người dùng gửi một ảnh lên từ giao diện web hoặc qua API POST request.

**Bước 2: Nginx Load Balancer nhận yêu cầu**
- Request đầu tiên đi vào Nginx – là load balancer.
- Nginx thực hiện phân phối tải (load balancing) đến một trong các FastAPI nodes (cụ thể là fastapi1, fastapi2 hoặc fastapi3).
- Cấu hình trong nginx.conf đã định nghĩa upstream fastapi_backend gồm fastapi1 và fastapi2.

**Bước 3: FastAPI xử lý ảnh**
- Node FastAPI được chọn sẽ:
1. Nhận ảnh từ người dùng
2. Gửi ảnh vào module feature extractor để trích xuất đặc trưng.
3. Sau đó, ảnh sẽ được vector hóa (dùng model như ResNet để chuyển thành vector 512 chiều).
4. Vector thu được sẽ được sử dụng để truy vấn tới cơ sở dữ liệu Milvus.

**Bước 4: Gửi truy vấn vector tới Milvus**
- FastAPI sử dụng thư viện pymilvus để gửi truy vấn vector search đến Milvus Lite DB.
- Milvus sẽ so sánh vector đầu vào với vector đã lưu trong DB để tìm ra các vector gần nhất (khoảng cách cosine hoặc L2).

**Bước 5: Milvus trả về danh sách ID sản phẩm tương tự**
- Milvus trả về danh sách các vector gần nhất kèm theo ID hoặc metadata (như file name).
- FastAPI dựa vào thông tin này để truy xuất hình ảnh tương ứng từ thư mục lưu trữ hoặc dịch vụ lưu ảnh (có thể là MinIO).

**Bước 6: FastAPI trả kết quả về client**
- FastAPI đóng gói kết quả và trả về danh sách các ảnh sản phẩm tương tự cho người dùng.

---
## III. 🧠 Vector hóa dữ liệu & tích hợp Milvus

### ✅ Mục tiêu

Để tìm được sản phẩm tương tự với sản phẩm mà người dùng cung cấp (thông qua **ảnh**), hệ thống phải **chuyển đổi ảnh thành vector đặc trưng (embedding)** rồi **tìm kiếm các vector gần nhất** trong một cơ sở dữ liệu vector.

Đây là lý do ta cần đến:

- **Mô hình học sâu (ResNet34)** để tạo vector cho ảnh.
- **Milvus** – hệ quản trị cơ sở dữ liệu chuyên cho dữ liệu vector – để lưu trữ và truy vấn nhanh chóng các vector ảnh.


### 🧠 Lợi ích & vai trò của Milvus trong hệ thống

| Mục đích | Milvus hỗ trợ |
|---------|----------------|
| Lưu trữ vector đặc trưng ảnh | Cho phép lưu hàng ngàn – hàng triệu vector |
| Tìm kiếm vector gần nhất | Hỗ trợ truy vấn nhanh bằng thuật toán Approximate Nearest Neighbor (ANN) |
| Tương thích với kiến trúc phân tán | Dễ dàng scale, hỗ trợ sharding, replication |
| Hỗ trợ nhiều loại khoảng cách | Trong dự án này dùng **Cosine Similarity** |


### 🧩 Các bước xử lý & vai trò từng tệp trong thư mục `milvus/`

> Các tệp mã nguồn dưới đây thực hiện toàn bộ quy trình: **vector hóa dữ liệu → lưu vào Milvus → truy vấn ảnh tương tự**.

#### 📄 `FeatureExtractor.py` – *Trích xuất vector ảnh*

- Định nghĩa class `FeatureExtractor` dùng mô hình ResNet34 (từ `timm`) để biến ảnh thành vector 512 chiều.
- Vector được **chuẩn hóa (normalize)** để phù hợp với việc so sánh bằng cosine.

```
python

output = self.model(input_tensor)
feature_vector = normalize(output.squeeze().numpy().reshape(1, -1), norm="l2")
```
✅ Tệp này được sử dụng bởi cả app.py và embeddings_to_milvus.py.
#### 📄 `embeddings_to_milvus.py` – *Nhúng Dữ Liệu Ảnh vào Milvus*
- Duyệt toàn bộ ảnh trong thư mục images/, images/train/, v.v.
- Gọi FeatureExtractor để trích xuất vector từ mỗi ảnh.
- Tạo collection trong Milvus nếu chưa tồn tại.
- Thực hiện insert theo lô để tối ưu hiệu suất ghi dữ liệu.
```
python

client.insert("image_embeddings", [{"vector": embedding, "filename": file_path}])
```
✅ Dùng khi cần đẩy toàn bộ ảnh vào Milvus trước khi chạy hệ thống.
#### 📄 `MilvusCollection.py` – *Tạo Nhanh Collection Milvus*
**Tệp giúp khởi tạo collection mới trong Milvus với:**
- Vector field tên vector
- Dimension = 512
- Metric = COSINE
```
python

client.create_collection(
    collection_name="image_embeddings",
    dimension=512,
    metric_type="COSINE",
)
```
✅ Có thể chạy riêng để chuẩn bị Milvus trước khi insert dữ liệu.
#### 📄 `app.py` – *Xử Lý Truy Vấn Tìm Kiếm & Giao Diện Web*
**Tệp chính của ứng dụng Flask, thực hiện các chức năng:**
- Hiển thị giao diện web tải ảnh.
- Nhận ảnh từ người dùng.
- Trích xuất vector từ ảnh và truy vấn Milvus để tìm ảnh tương tự nhất.
- Trả về kết quả hiển thị ảnh tương tự.
```
python

results = client.search(..., data=[embedding], limit=10)
```
✅ Là trung tâm giao tiếp giữa người dùng và Milvus.
#### 📄 `templates/index.html` 
- Giao diện web đơn giản cho phép người dùng tải ảnh và xem ảnh gợi ý tương tự nhất đã lưu trong Milvus.

✅ Phần front-end của hệ thống.
#### 📄 `images/` 
- Chứa dữ liệu ảnh gốc của hệ thống, có thể chia thành các thư mục con như train/, test/, exception/, object/.

✅ Nguồn dữ liệu cho quá trình index và truy vấn.
#### 📄 `stress_test.sh` 
**Script để kiểm tra hiệu năng hệ thống khi xử lý:**
- Truy vấn ảnh liên tục.
- Xử lý hàng loạt ảnh trong thời gian ngắn.

✅ Giúp phát hiện và kiểm tra khả năng chịu lỗi.

*Thông qua việc tích hợp Milvus và sử dụng các mô hình học sâu, hệ thống này không chỉ cung cấp khả năng tìm kiếm ảnh tương tự một cách hiệu quả mà còn cho phép mở rộng và tối ưu hóa quy trình xử lý dữ liệu ảnh.*

---
## IV. ⚙️ Triển khai hệ thống phân tán: Kiến trúc & Các tiêu chí kỹ thuật

Triển khai hệ thống gợi ý sản phẩm tương tự trên môi trường phân tán sử dụng **Docker Compose**, đảm bảo các tiêu chí kỹ thuật như giao tiếp phân tán, fault tolerance, phân mảnh dữ liệu, logging và stress test.s
### 1. 🔗 Giao tiếp phân tán (Distributed Communication)
- Các thành phần (Flask API, dịch vụ embedding, Milvus) giao tiếp thông qua **REST API nội bộ** hoặc **cổng gRPC của Milvus**.
- Dữ liệu ảnh/văn bản được vector hóa thành **vector JSON** và truyền giữa các container qua **Docker network nội bộ (`milvus-net`)**.
- Tất cả service nằm trên cùng mạng `milvus-net`, cho phép gọi nhau qua tên container như `milvus`, `embedding`, `etcd`, v.v.

##### 🧱 Kiến trúc giao tiếp giữa các thành phần:
```
plaintext

Người dùng
    ↓
Flask API (app.py)
    ↓            ↘
embedding service   → Milvus (proxy → querynode, datanode)
    ↓                    ↓
vector JSON         → Kết quả tìm kiếm
```
##### ✅ Cách các thành phần giao tiếp

| Thành phần         | Giao tiếp với     | Giao thức     | Dạng dữ liệu     |
|--------------------|-------------------|---------------|------------------|
| Người dùng         | Flask API         | HTTP          | form, file       |
| Flask API          | Embedding service | HTTP/Flask    | JSON             |
| Embedding service  | Milvus            | SDK gRPC      | vector           |
| Các node Milvus    | Lẫn nhau          | Docker nội bộ | Pulsar, gRPC     |


##### ✅ Thiết lập mạng Docker riêng cho hệ thống

Tạo một mạng nội bộ để các container (Milvus node, API, embedding) có thể giao tiếp bằng hostname:

```bash
docker network create milvus-net
```
*milvus-net là mạng nội bộ giúp các container gọi nhau bằng tên (etcd:2379, pulsar:6650, proxy:19530, v.v.)*

##### ✅ Cấu hình docker-compose để sử dụng mạng này
Trong docker-compose.yml, thêm cấu hình mạng vào mỗi service:
```
yaml

proxy:
  image: milvusdb/milvus:v2.4.0
  container_name: proxy
  command: ["milvus", "run", "proxy"]
  depends_on:
    - rootcoord
  ports:
    - "19530:19530"
  networks:
    - milvus-net
```
📌 Lưu ý: Bạn cần tạo mạng trước khi docker-compose up.

##### 🔌 Giao tiếp Flask ↔ Milvus
Trong Flask API (file app.py), bạn kết nối Milvus như sau:
```
python

from pymilvus import connections

connections.connect(host="proxy", port="19530")

```
→ Flask có thể gọi tới Milvus proxy container qua tên proxy nhờ vào mạng milvus-net.

### 2. 🛠️ Fault Tolerance trong hệ thống Milvus phân tán

**Mục tiêu:** Đảm bảo hệ thống vẫn hoạt động ổn định ngay cả khi một số thành phần (container) bị lỗi hoặc dừng đột ngột.

##### ✅ Tự khởi động lại container với Docker

Sử dụng `restart: always` trong file `docker-compose.yml` để các container tự khởi động lại khi gặp sự cố:

```yaml
embedding:
  image: your-embedding-service
  container_name: embedding
  restart: always
  networks:
    - milvus-net
```
Tương tự, áp dụng cho các service khác như proxy, querynode, api, …

📌 Điều này đảm bảo rằng nếu một container (ví dụ: Milvus hoặc embedding) bị dừng, Docker sẽ tự khởi động lại mà không ảnh hưởng toàn bộ hệ thống.

##### ✅ Flask API có xử lý ngoại lệ (try/except)

Trong app.py, các phần gọi tới Milvus hoặc embedding service cần được bọc bởi try/except để xử lý lỗi và tránh làm crash toàn bộ ứng dụng:

```
python

embedding:
  image: your-embedding-service
  container_name: embedding
  restart: always
  networks:
    - milvus-net
```
📌 Nếu một Milvus node chết, API có thể thông báo lỗi, thử lại, hoặc chuyển hướng truy vấn khi cần.

##### ✅ Hệ thống vẫn hoạt động khi một node lỗi

**Trong hướng dẫn bạn gửi, Milvus được triển khai với nhiều node như:**
- `datanode`
- `querynode`
- `proxy`
- `rootcoord`

*Nhờ kiến trúc phân tán và tách biệt, việc một node bị lỗi (ví dụ: tắt `embedding` hoặc `querynode`) không làm gián đoạn toàn bộ hệ thống.*

📌 Bạn có thể thử tắt container embedding hoặc một node Milvus → API và các thành phần khác vẫn có thể xử lý truy vấn hoặc hiển thị thông báo phù hợp.

*Hướng dẫn cài Milvus phân tán cho thấy rõ cách phân chia các node chuyên biệt (proxy, querynode, datanode, pulsar, etcd, ...) và ghép chúng lại bằng mạng Docker milvus-net.*

```
yaml

querynode:
  image: milvusdb/milvus:v2.4.0
  container_name: querynode
  command: ["milvus", "run", "querynode"]
  restart: always
  networks:
    - milvus-net
```
### 3. 🧩 Sharding/Replication trong hệ thống Milvus phân tántán

**Mục tiêu:** Tăng khả năng mở rộng và tính sẵn sàng của hệ thống bằng cách chia nhỏ hoặc nhân bản dữ liệu trên nhiều node hoặc collection Milvus.

##### 🔍 Định nghĩa
Trong các hệ thống phân tán hiện đại như Milvus, hai kỹ thuật thường được sử dụng để tăng khả năng mở rộng và độ tin cậy là:
- Sharding (Phân mảnh dữ liệu): Dữ liệu được chia thành nhiều phần (shard) dựa trên một tiêu chí cụ thể (ví dụ: danh mục sản phẩm). Mỗi shard được lưu trữ và xử lý riêng biệt, thường ở các node khác nhau.
- Replication (Sao chép dữ liệu): Dữ liệu được nhân bản và lưu trữ trên nhiều node, đảm bảo rằng nếu một node gặp lỗi thì node khác vẫn có thể tiếp tục xử lý yêu cầu → tăng fault tolerance.
##### ✅ Tại sao dùng Replication?

**Trong hệ thống Milvus bạn triển khai (theo hướng dẫn docker-compose), Replication là kỹ thuật chủ đạo, mang lại nhiều lợi ích:**

| Mục tiêu                                      | Lợi ích cụ thể                                                                    |
| --------------------------------------------- | --------------------------------------------------------------------------------- |
| 🔁 **Đảm bảo độ tin cậy (Reliability)**       | Nếu một node (datanode hoặc querynode) chết, node khác vẫn hoạt động bình thường. |
| ⚙️ **Tự động phân phối tải (Load Balancing)** | Các truy vấn được gửi tới proxy và Milvus sẽ tự quyết định node xử lý phù hợp.    |
| 📈 **Mở rộng dễ dàng (Scalability)**          | Có thể thêm `querynode` hoặc `datanode` vào cụm mà không cần dừng hệ thống.       |


##### ⚙️ Cách hoạt động
Hệ thống Milvus được cấu hình như sau:
```
yaml

services:
  datanode1:
    image: milvusdb/milvus:v2.4.0
    container_name: datanode1
    command: ["milvus", "run", "datanode"]
    networks:
      - milvus-net

  datanode2:
    image: milvusdb/milvus:v2.4.0
    container_name: datanode2
    command: ["milvus", "run", "datanode"]
    networks:
      - milvus-net

  querynode1:
    image: milvusdb/milvus:v2.4.0
    container_name: querynode1
    command: ["milvus", "run", "querynode"]
    networks:
      - milvus-net

  querynode2:
    image: milvusdb/milvus:v2.4.0
    container_name: querynode2
    command: ["milvus", "run", "querynode"]
    networks:
      - milvus-net
```
- Các node giống nhau được khởi tạo nhiều lần → tức là dữ liệu và truy vấn được sao chép & phân phối giữa các node.
- Giao tiếp giữa các node thông qua mạng nội bộ milvus-net, sử dụng gRPC/Pulsar.

##### 🔄 Quy trình xử lý với Replication

1. Người dùng gửi truy vấn đến proxy (cổng 19530).
2. Proxy forward truy vấn tới rootcoord.
3. RootCoord phân phối yêu cầu tới các querynode hoặc datanode sẵn sàng.
4. Các node tự quản lý dữ liệu được sao chép, đảm bảo truy vấn không bị gián đoạn nếu 1 node chết.

### 4. 📊 Logging & Monitoring trong hệ thống Milvus phân tán

Trong một hệ thống phân tán, **Logging & Monitoring** là yếu tố không thể thiếu để đảm bảo hệ thống hoạt động ổn định, dễ bảo trì và dễ mở rộng.

##### ✅ Ghi log truy vấn
Mỗi node (datanode, querynode, proxy, v.v.) trong cụm Milvus đều ghi log ra **console**. Trong `app.py`:
```
python

import logging
from flask import Flask, request

app = Flask(__name__)
logging.basicConfig(filename='app.log', level=logging.INFO)

@app.route("/search", methods=["POST"])
def search():
    try:
        image = request.files["file"]
        # ... Xử lý tìm kiếm ảnh ...
        logging.info("Request type: search | Status: success")
        return {"result": "ok"}
    except Exception as e:
        logging.error(f"Request type: search | Error: {str(e)}")
        return {"error": "failed"}, 500
```
 ##### ✅ Nội dung log ghi lại
 Các thông tin chính được ghi lại trong log của từng node:

 | Trường thông tin  | Mô tả                                                                 |
| ----------------- | --------------------------------------------------------------------- |
| `Request type`    | Kiểu truy vấn: insert, search, load, release, v.v.                    |
| `Thời gian xử lý` | Milvus ghi lại thời gian bắt đầu - kết thúc của mỗi truy vấn          |
| `Error (nếu có)`  | Nếu có lỗi (mất kết nối, không tìm thấy collection...), log sẽ ghi rõ |

Ví dụ đoạn log từ proxy:

```
css

[INFO] [2025-05-30 12:04:22] - Received search request on collection `fashion_vector`
[INFO] [2025-05-30 12:04:22] - Forwarding request to querynode1
[ERROR] [2025-05-30 12:04:23] - querynode1 timeout, retrying on querynode2
```

##### 🧩 Liên kết với bài làm

Trong file docker-compose.yml, mỗi container là một node độc lập (proxy, datanode1, querynode2...). Các node này chia sẻ cùng một mạng milvus-net và hoạt động song song.

```
yaml

proxy:
  image: milvusdb/milvus:v2.4.0
  container_name: proxy
  command: ["milvus", "run", "proxy"]
  depends_on:
    - rootcoord
  ports:
    - "19530:19530"
  networks:
    - milvus-net
```
Bạn có thể theo dõi log của node proxy bằng: *docker logs proxy*

### 5. 🔬 Stress Testing

##### ✅ Mục tiêu
**Mô phỏng tình huống nhiều người dùng truy vấn cùng lúc vào hệ thống Milvus thông qua proxy (port 19530), nhằm kiểm tra:**
- Độ ổn định của hệ thống
- Thời gian phản hồi trung bình
- Tỷ lệ lỗi và downtime (nếu có)

##### ✅ Công cụ đề xuất

| Công cụ                 | Mô tả ngắn gọn                                                               |
| ----------------------- | ---------------------------------------------------------------------------- |
| `Apache Benchmark (ab)` | Gửi số lượng lớn request HTTP tới Flask endpoint để kiểm tra tốc độ phản hồi |
| `Locust / JMeter`       | Mô phỏng nhiều người dùng với các luồng riêng biệt                           |
| `Python script`         | Dùng `pymilvus` để tạo nhiều truy vấn song song đến Milvus proxy             |

##### Ví dụ Stress Test với Python (sử dụng pymilvus)

```
python

from pymilvus import connections, CollectionSchema, FieldSchema, DataType, Collection
import threading
import random

def insert_task(index):
    connections.connect(host='localhost', port='19530')
    fields = [
        FieldSchema(name="id", dtype=DataType.INT64, is_primary=True, auto_id=True),
        FieldSchema(name="embedding", dtype=DataType.FLOAT_VECTOR, dim=128)
    ]
    schema = CollectionSchema(fields, description=f"stress_test_{index}")
    collection = Collection(name=f"stress_collection_{index}", schema=schema)
    data = [[random.random() for _ in range(128)] for _ in range(1000)]
    collection.insert([None, data])
    print(f"[{index}] Inserted 1000 vectors")

threads = []
for i in range(20):  # tạo 20 luồng đồng thời
    t = threading.Thread(target=insert_task, args=(i,))
    threads.append(t)
    t.start()

for t in threads:
    t.join()
```
🧠 Script này kết nối đến proxy (port 19530) trong Docker Compose của bạn, mỗi thread thực hiện insert vào một collection khác nhau.

##### ✅ Ghi lại kết quả
| Thông số              | Ý nghĩa                                                           |
| --------------------- | ----------------------------------------------------------------- |
| ⏱️ Thời gian phản hồi | Tổng thời gian xử lý truy vấn (ms hoặc giây)                      |
| ❌ Số lượng lỗi        | Số truy vấn gặp lỗi (mất kết nối, lỗi insert, lỗi tìm kiếm, v.v.) |
| 🛑 Downtime (nếu có)  | Thời gian hệ thống không phản hồi hoặc Milvus proxy bị treo       |

## V. 🧾 Kết luận

Hệ thống tìm kiếm sản phẩm tương tự sử dụng Milvus không chỉ mạnh mẽ về hiệu năng mà còn linh hoạt và mở rộng tốt nhờ kiến trúc phân tán. Thông qua việc triển khai với Docker Compose, ta dễ dàng cấu hình nhiều node Milvus (datanode, querynode, proxy…) để đạt được các mục tiêu như:
- 🔁 Fault Tolerance: hệ thống vẫn hoạt động khi một node gặp sự cố.
- 🧩 Replication: dữ liệu được sao lưu và phân phối trên nhiều node, tăng độ tin cậy.
- 📈 Monitoring & Logging: theo dõi tình trạng hệ thống theo thời gian thực.
- 🔬 Stress Testing: kiểm tra hiệu năng trong điều kiện tải cao.
