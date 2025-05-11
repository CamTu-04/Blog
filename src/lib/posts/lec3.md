---
title: "Trao đổi thông tin"
date: "2025-05-02"
updated: "2025-05-06"
author: "Hoàng Cẩm Tú"
categories:
  - "Distributed Systems"
  - "Processes and Threads"
  - "Concurrency"
coverImage: "/images/TraoDoiThongTin.jpg"
coverWidth: 16
coverHeight: 9
excerpt: Blog này giúp bạn hiểu rõ về trao đổi thông tin trong hệ thống phân tán.
---
# Bài Tập 1: Tìm Hiểu Về RabbitMQ
## 1. Cơ Chế và Chức Năng
RabbitMQ là một dịch vụ truyền thông điệp mã nguồn mở, dựa trên giao thức AMQP (Advanced Message Queuing Protocol). Nó cho phép các ứng dụng giao tiếp với nhau qua các hàng đợi, giúp tách biệt các thành phần của hệ thống và tối ưu hóa khả năng mở rộng.

### Cơ Chế Hoạt Động
- **Producer:** Gửi thông điệp đến hàng đợi.
- **Queue:** Lưu trữ thông điệp cho đến khi chúng được tiêu thụ.
- **Consumer:** Nhận thông điệp từ hàng đợi và xử lý chúng.
- **Exchange:** Chuyển tiếp thông điệp từ producer đến queue dựa trên các quy tắc (binding).

### Chức Năng
- **Đảm bảo độ tin cậy:** Lưu trữ thông điệp cho đến khi chúng được xử lý thành công.
- **Hỗ trợ nhiều giao thức:** AMQP, MQTT, STOMP.
- **Quản lý tải:** Phân phối tải giữa nhiều consumer.

## 2. Cài Đặt RabbitMQ

### Bước 1: Cài đặt RabbitMQ
1. **Cài đặt Erlang (được yêu cầu cho RabbitMQ)**
- **Trên Ubuntu/Debian:**
  ```bash
  sudo apt-get update
  sudo apt-get install erlang
2. **Tải RabbitMQ từ trang chính thức và thực hiện cài đặt**
Tải RabbitMQ từ trang chính thức [RabbitMQ Downloads].
- **Trên Ubuntu/Debian:**
  ```bash
  sudo apt-get install rabbitmq-server
3. **Khởi động RabbitMQ**
Khởi động dịch vụ RabbitMQ:
- **Trên Ubuntu/Debian:**
  ```bash
  sudo rabbitmq-server start
4. **Truy cập Giao diện quản lý**
Mở trình duyệt và truy cập http://localhost:15672. Đăng nhập với thông tin mặc định:
- Tên đăng nhập: guest
- Mật khẩu: guest
5. **Kiểm tra trạng thái**
Kiểm tra trạng thái RabbitMQ để đảm bảo nó đang chạy:
- **Trên Ubuntu/Debian:**
  ```bash
  sudo rabbitmqctl status

# Bài Tập 2: Code Hệ Thống Sử Dụng RabbitMQ

## Mục Tiêu
Xây dựng một ứng dụng đơn giản sử dụng RabbitMQ để gửi và nhận thông điệp.

## Cài Đặt Thư Viện
Trước tiên, bạn cần cài đặt thư viện `pika` để làm việc với RabbitMQ

## Dưới đây là Code Ví Dụ:
- **Producer**
```bash
python

import pika

# Thiết lập kết nối đến RabbitMQ
connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

# Đảm bảo rằng hàng đợi tồn tại
channel.queue_declare(queue='hello')

# Gửi thông điệp vào hàng đợi
channel.basic_publish(exchange='', routing_key='hello', body='Hello World!')
print(" [x] Sent 'Hello World!'")

# Đóng kết nối
connection.close()
```

- **Consumer**
```bash
python

import pika

# Hàm callback sẽ được gọi khi nhận được thông điệp
def callback(ch, method, properties, body):
    print(f" [x] Received {body}")

# Thiết lập kết nối đến RabbitMQ
connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

# Đảm bảo rằng hàng đợi tồn tại
channel.queue_declare(queue='hello')

# Đăng ký hàm callback
channel.basic_consume(queue='hello', on_message_callback=callback, auto_ack=True)

print(' [*] Waiting for messages. To exit press CTRL+C')
channel.start_consuming()
```

# Bài Tập 3: Tìm hiểu về JSON-RPC

## 1. Các Thư Viện RPC Khác
Ngoài thư viện xmlrpc, có nhiều thư viện khác hỗ trợ Remote Procedure Call (RPC) với định dạng JSON, bao gồm:
- **json-rpc**: Thư viện đơn giản cho JSON-RPC.
- **gRPC**: Thư viện mạnh mẽ với hỗ trợ nhiều ngôn ngữ.
## 2. Ví dụ sử dụng JSON-RPC
- **Cài đặt thư viện**
Để sử dụng json-rpc, bạn cần cài đặt thư viện:
```bash
pip install jsonrpc
```
- **Code ví dụ**

**Server**
```bash
python

from jsonrpc import Server

# Định nghĩa hàm sẽ được gọi qua RPC
def add(a, b):
    return a + b

# Tạo server
server = Server()
server.register_function(add, 'add')

# Chạy server
if __name__ == '__main__':
    server.serve_forever()
```
**Khách hàng gọi RPC**
```bash
python

import json
import requests

url = 'http://localhost:5000/'
payload = {
    "jsonrpc": "2.0",
    "method": "add",
    "params": [5, 3],
    "id": 1,
}

response = requests.post(url, json=payload)
print(response.json())
```