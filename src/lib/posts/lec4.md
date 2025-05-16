---
title: "Định danh"
date: "2025-05-02"
updated: "2025-05-12"
author: "Hoàng Cẩm Tú"
categories:
  - "Distributed Systems"
  - "Processes and Threads"
  - "Concurrency"
coverImage: "/images/dinhdanh.png"
coverWidth: 16
coverHeight: 9
excerpt: Định danh trong hệ thống phân tán là phương pháp gán tên hoặc ID cho các nút, tài nguyên hoặc dịch vụ, giúp chúng có thể được xác định và truy cập một cách hiệu quả trong môi trường mạng phân tán.
---
# Bài Tập 1: Tìm Hiểu Quy Trình Duyệt Một Trang Web

## Quy Trình Duyệt Trang Web www.motvidu.com

Khi người dùng nhập địa chỉ website `www.motvidu.com` vào trình duyệt, quá trình diễn ra như sau:

### 1. Tra cứu DNS
- **DNS Lookup:** Trình duyệt sẽ tra cứu địa chỉ IP của tên miền `www.motvidu.com`. Quá trình này bắt đầu với việc kiểm tra bộ nhớ cache DNS của hệ thống. Nếu địa chỉ đã được lưu trữ, trình duyệt sẽ sử dụng địa chỉ đó.

### 2. Resolving DNS
- **Recursive Resolver:** Nếu địa chỉ không có trong cache, yêu cầu sẽ được gửi đến một máy chủ DNS (recursive resolver). Máy chủ này sẽ tìm kiếm thông tin qua các máy chủ DNS khác.
- **Root Server:** Máy chủ DNS sẽ đầu tiên hỏi máy chủ root để biết địa chỉ của máy chủ DNS quản lý miền cấp cao hơn (TLD server).
- **TLD Server:** Sau đó, máy chủ sẽ truy vấn máy chủ TLD (ví dụ: `.com`) để tìm địa chỉ máy chủ DNS của miền `motvidu.com`.
- **Authoritative Name Server:** Cuối cùng, máy chủ sẽ hỏi máy chủ DNS có thẩm quyền cho `motvidu.com` để lấy địa chỉ IP.

### 3. Caching
- **Caching DNS:** Khi có được địa chỉ IP, máy chủ DNS sẽ lưu trữ thông tin này trong cache để sử dụng cho các yêu cầu sau, giúp tăng tốc độ truy cập cho các lần truy cập tiếp theo.

### Tài Liệu Tham Khảo
- [How DNS Works](https://www.cloudflare.com/learning/dns/how-dns-works/)
- [What is DNS Resolution?](https://www.dnsimple.com/blog/what-is-dns-resolution/)



# Bài Tập 2: Thuật Toán Chord

## Giới Thiệu về Chord
Chord là một thuật toán phân tán để tìm kiếm và truy cập dữ liệu trong một mạng peer-to-peer (P2P). Nó sử dụng cấu trúc vòng tròn để tổ chức các nút, cho phép tìm kiếm dữ liệu một cách hiệu quả.

## Ví Dụ Cụ Thể về Chord
Giả sử chúng ta có một mạng P2P với các nút có ID từ 0 đến 7. Nếu một nút muốn tìm dữ liệu có ID là 3, nó sẽ sử dụng thuật toán Chord để tìm kiếm một cách hiệu quả.

## Code Thực Nghiệm

### Cài Đặt Thư Viện
Trước tiên, bạn cần cài đặt thư viện `hashlib` để mã hóa:

```bash
pip install hashlib

import hashlib

class Node:
    def __init__(self, identifier):
        self.id = identifier
        self.data = {}

    def store_data(self, key, value):
        self.data[key] = value

    def find_data(self, key):
        return self.data.get(key, None)

def hash_key(key):
    return int(hashlib.sha1(key.encode()).hexdigest(), 16) % 8

# Ví dụ sử dụng
node1 = Node(1)
node2 = Node(2)

node1.store_data(hash_key("data1"), "value1")
print(node1.find_data(hash_key("data1")))  # Kết quả: value1
```
Dưới đây là một test case cho thuật toán Chord:
```bash
python

def test_chord():
    node = Node(1)
    node.store_data(hash_key("test_key"), "test_value")
    assert node.find_data(hash_key("test_key")) == "test_value"
    assert node.find_data(hash_key("non_existent_key")) is None

test_chord()
print("Tất cả test case đã chạy thành công!")
```