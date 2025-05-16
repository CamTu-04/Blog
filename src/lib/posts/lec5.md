---
title: "Các giao thức trong phân tán"
date: "2025-05-02"
updated: "2025-05-13"
author: "Hoàng Cẩm Tú"
categories:
  - "Distributed Systems"
  - "Processes and Threads"
  - "Concurrency"
coverImage: "/images/http.jpg"
coverWidth: 16
coverHeight: 9
excerpt: Các giao thức phân tán như HTTP, TCP/IP, và gRPC hỗ trợ truyền tải và xử lý dữ liệu trong mạng, mỗi giao thức có mục đích và tính năng riêng.
---
# 1. Tìm Hiểu Sự Tương Quan và Khác Biệt Giữa Các Giao Thức

## Giao Thức HTTP
- **Mục Đích Sử Dụng:** Giao thức truyền tải siêu văn bản, chủ yếu được sử dụng để truyền tải dữ liệu trên web.
- **Liên Quan:** HTTP thường được sử dụng trên TCP/IP để truyền tải dữ liệu.

## Giao Thức TCP/IP
- **Mục Đích Sử Dụng:** Giao thức cơ sở cho việc truyền tải dữ liệu trên Internet, đảm bảo dữ liệu được gửi và nhận chính xác.
- **Liên Quan:** Là nền tảng cho nhiều giao thức khác như HTTP, FTP, SMTP.

## Giao Thức UDP
- **Mục Đích Sử Dụng:** Giao thức truyền tải không kết nối, sử dụng cho các ứng dụng yêu cầu tốc độ hơn là độ tin cậy, như video streaming.
- **Liên Quan:** Cùng thuộc bộ giao thức TCP/IP nhưng không đảm bảo độ tin cậy như TCP.

## REST
- **Mục Đích Sử Dụng:** Kiến trúc để xây dựng dịch vụ web, dựa trên HTTP, cho phép tương tác giữa client và server.
- **Liên Quan:** Thường được sử dụng cùng với HTTP.

## GraphQL
- **Mục Đích Sử Dụng:** Ngôn ngữ truy vấn API, cho phép client yêu cầu chính xác dữ liệu cần thiết.
- **Liên Quan:** Thay thế cho REST trong nhiều ứng dụng web hiện đại.

## SOAP
- **Mục Đích Sử Dụng:** Giao thức truyền tải dữ liệu trong các dịch vụ web, sử dụng XML và thường đi kèm với WS-* standards.
- **Liên Quan:** Thường được so sánh với REST, nhưng phức tạp hơn.

## AJAX
- **Mục Đích Sử Dụng:** Kỹ thuật để tải dữ liệu từ server mà không cần tải lại trang, sử dụng JavaScript.
- **Liên Quan:** Thường được sử dụng với HTTP và RESTful APIs.

## RPC
- **Mục Đích Sử Dụng:** Giao thức cho phép một chương trình gọi hàm trên một máy khác như đang gọi hàm cục bộ.
- **Liên Quan:** Giao thức gốc cho việc gọi hàm từ xa.

## gRPC
- **Mục Đích Sử Dụng:** Giao thức RPC hiện đại, sử dụng Protocol Buffers, hỗ trợ nhiều ngôn ngữ lập trình.
- **Liên Quan:** Cải thiện hiệu suất và khả năng mở rộng so với RPC truyền thống.

### Tóm Tắt
Mặc dù các giao thức này phục vụ các mục đích khác nhau, nhưng chúng thường tương tác với nhau trong các ứng dụng web hiện đại, với TCP/IP là nền tảng cho nhiều giao thức truyền tải dữ liệu.



# 2. Nghiên Cứu Về Thư Viện OpenMPI

## Tính Năng
- **OpenMPI** là một thư viện mã nguồn mở cho phép lập trình song song và phân tán.
- Hỗ trợ giao tiếp giữa các máy tính trong một mạng lưới, cho phép chia sẻ dữ liệu và tài nguyên.

## Các Hàm Sử Dụng
- **MPI_Init:** Khởi tạo môi trường MPI.
- **MPI_Comm_size:** Lấy số lượng tiến trình.
- **MPI_Comm_rank:** Lấy thứ hạng của tiến trình.
- **MPI_Send:** Gửi dữ liệu từ một tiến trình tới tiến trình khác.
- **MPI_Recv:** Nhận dữ liệu từ một tiến trình khác.
- **MPI_Finalize:** Kết thúc môi trường MPI.

## Giải Pháp Sử Dụng Hệ Phân Tán
### Bài Toán: Tính 10,000,000 Số Nguyên Tố Đầu Tiên
- **Phân Chia Công Việc:** Chia số nguyên tố cần tính cho các máy, mỗi máy xử lý một phần dữ liệu.
- **Sử Dụng Nhiều Core:** Có thể điều chỉnh số lượng core sử dụng (8, 10, 12, 16) bằng cách chia đều công việc cho từng core trong mỗi máy.

### Kế Hoạch Thực Hiện
1. **Khởi Tạo OpenMPI:** Sử dụng `MPI_Init` để bắt đầu.
2. **Phân Phối Công Việc:** Tính toán khoảng số nguyên tố mà mỗi máy sẽ xử lý.
3. **Tính Toán Song Song:** Mỗi core tính toán số nguyên tố trong khoảng của nó.
4. **Gửi Kết Quả:** Sử dụng `MPI_Send` và `MPI_Recv` để gửi kết quả về máy chủ.
5. **Tổng Hợp Kết Quả:** Tổng hợp kết quả từ tất cả các máy và in ra kết quả cuối cùng.

### Kết Luận
Sử dụng OpenMPI cho phép tối ưu hóa quá trình tính toán số nguyên tố lớn với khả năng mở rộng linh hoạt theo số lượng core sử dụng, từ đó đạt được hiệu suất cao nhất.

### Tham Khảo
- [Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes)