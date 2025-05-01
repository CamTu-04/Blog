---
title: "Hệ thống phân tán"
date: "2023-04-28"
updated: "2023-04-28"
categories:
  - "sveltekit"
  - "markdown"
  - "svelte"
coverImage: "/images/jerry-zhang-ePpaQC2c1xA-unsplash.jpg"
coverWidth: 16
coverHeight: 9
excerpt: This post demonstrates how to include a Svelte component in a Markdown post.
---

# Hệ thống phân tán là gì?

Hệ thống phân tán (Distributed System) là tập hợp các máy tính độc lập, giao tiếp với nhau thông qua mạng, để đạt được một mục tiêu chung như một hệ thống duy nhất.

# Các ứng dụng của hệ thống phân tán

- Dịch vụ lưu trữ đám mây (Google Drive, Dropbox)
- Mạng xã hội (Facebook, Instagram)
- Các hệ thống tài chính (ngân hàng, ví điện tử)

# Các khái niệm chính của hệ thống phân tán

## Scalability
Khả năng mở rộng hệ thống để đáp ứng nhu cầu tăng trưởng.

## Fault Tolerance
Khả năng hệ thống tiếp tục hoạt động khi một thành phần bị lỗi.

## Availability
Khả năng hệ thống luôn sẵn sàng phục vụ người dùng.

## Transparency
Người dùng không nhận thấy sự phức tạp của hệ thống.

## Concurrency
Khả năng nhiều tiến trình chạy đồng thời mà không gây lỗi.

## Parallelism
Thực thi nhiều tác vụ cùng lúc để tăng tốc độ xử lý.

## Openness
Khả năng hệ thống mở rộng, tích hợp nhiều công nghệ khác nhau.

## Vertical Scaling
Tăng khả năng của một máy chủ (thêm CPU, RAM).

## Horizontal Scaling
Thêm nhiều máy chủ vào hệ thống.

## Load Balancer
Phân phối tải đều lên nhiều server.

## Replication
Sao chép dữ liệu để đảm bảo độ tin cậy và hiệu năng.

# Ví dụ

Một ứng dụng như Google Drive:

- Dùng **Replication** để sao lưu file người dùng ở nhiều data center.
- Dùng **Load Balancer** để phân phối truy cập người dùng đến các server ít tải hơn.
- Khi nhu cầu tăng, Google thực hiện **Horizontal Scaling** thêm máy chủ mới.
- Nếu một máy chủ hỏng, hệ thống vẫn hoạt động nhờ **Fault Tolerance**.

# Kiến trúc của hệ thống phân tán

Một số mô hình kiến trúc phổ biến:

- **Client-Server**: Khách gửi yêu cầu, server xử lý và trả lời.
- **Three-tier**: Phân thành 3 lớp: giao diện, xử lý logic, và dữ liệu.
- **Peer-to-Peer (P2P)**: Các node vừa gửi vừa nhận tài nguyên (ví dụ: BitTorrent).

**Mô hình mới** hiện nay:
- **Microservices Architecture**: Chia hệ thống thành các dịch vụ nhỏ, độc lập.
- **Serverless Computing**: Triển khai chức năng không cần quản lý server.
- **Edge Computing**: Xử lý dữ liệu gần người dùng hơn, giảm độ trễ.

# Ví dụ minh họa kiến trúc

<img src="/blog/phan-tan-architecture.png" alt="Kiến trúc hệ thống phân tán" />

Ví dụ: Netflix sử dụng Microservices, các dịch vụ video streaming, thanh toán, cá nhân hóa,... hoạt động độc lập.
