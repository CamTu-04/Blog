---
title: "Tiến trình & Luồng"
date: "2025-05-03"
updated: "2025-05-06"
author: "Hoàng Cẩm Tú"
categories:
  - "Distributed Systems"
  - "Processes and Threads"
  - "Concurrency"
coverImage: "/images/tien-trinh-va-luong-1.jpg"
coverWidth: 16
coverHeight: 9
excerpt: Blog này giúp bạn hiểu rõ về tiến trình và luồng trong hệ thống phân tán, sự khác biệt giữa chúng, và cách chúng ảnh hưởng đến hiệu năng của máy tính khi xử lý các tác vụ đồng thời.
---

# 1. Dựa vào bài học, check CPU, GPU, RAM, giải thích về hiệu năng của máy tính mà em đang dùng?

Phân Tích Hiệu Năng Máy Tính HP Pavilion Laptop 15-eg2xxx

| Thành phần | Thông số chi tiết | Đánh giá hiệu năng |
|------------|------------------|---------------------|
| *CPU* | Intel Core i5-1240P (12th Gen, 10 nhân, 16 luồng, base 1.7GHz, turbo ~4.4GHz) <br> Cache: L1: 768KB, L2: 12MB, L3: 12MB | Bộ xử lý thế hệ mới với kiến trúc hiệu suất cao, hỗ trợ đa luồng mạnh mẽ. Phù hợp cho tác vụ lập trình, xử lý dữ liệu và chạy các ứng dụng nặng như máy ảo và game nhẹ. Có hỗ trợ ảo hóa cho các ứng dụng như VirtualBox. |
| *RAM* | 8GB DDR4, bus 3200MT/s <br> Đang dùng: ~7.68GB, Còn trống: ~320MB | Đủ cho các tác vụ văn phòng, lập trình và chạy nhiều ứng dụng cùng lúc. Tuy nhiên, nếu sử dụng nhiều tiến trình nặng hoặc chạy máy ảo, RAM có thể trở thành yếu tố giới hạn. |
| *GPU* | Intel Iris Xe Graphics (tích hợp) <br> Driver: Cập nhật mới nhất | GPU tích hợp cho phép xử lý đồ họa 2D, lập trình giao diện và xem video 4K. Không phù hợp cho các tác vụ AI nặng hoặc game 3D cao cấp. |

## Sử dụng hiện tại:
- **CPU:** 15% sử dụng  
- **GPU:** 3% sử dụng  
- **RAM:** 7.68GB/8GB đang dùng  
- **Số tiến trình:** 250  
- **Số luồng:** 3200  

### 🔎 Kết luận:

Máy tính HP Pavilion 15-eg2xxx có khả năng đáp ứng tốt cho:

- Viết code với các IDE như VSCode, chạy server backend nhẹ.
- Học tập về đa tiến trình và đa luồng qua thực hành.
- Sử dụng ảo hóa với hỗ trợ VT-x, thích hợp cho các tác vụ lập trình và phát triển ứng dụng.
- Không nên dùng cho các tác vụ AI deep learning nặng hoặc xử lý video phức tạp.

Máy có hiệu năng ổn định và phù hợp cho học tập cũng như công việc văn phòng hàng ngày.

# 2. Nghiên cứu tìm hiểu, liệt kê ít nhất 12 bài toán phổ biến trong ngành CNTT của em đang học, những bài toán này sử dụng đa luồng, đa tiến trình ở đâu?

Trong ngành công nghệ thông tin, có nhiều bài toán đụng phải việc xử lý đồng thời với đa tiến trình và đa luồng. Dưới đây là một số ví dụ:

1. **Xử lý hình ảnh**  
   - **Ứng dụng:** Tăng tốc các thao tác như lọc, biến đổi và nén ảnh.  
   - **Đa luồng:** Xử lý từng phần của hình ảnh song song.

2. **Tìm kiếm dữ liệu**  
   - **Ứng dụng:** Tìm kiếm thông tin trong cơ sở dữ liệu lớn.  
   - **Đa tiến trình:** Chia nhỏ các truy vấn và xử lý đồng thời.

3. **Máy chủ web**  
   - **Ứng dụng:** Xử lý nhiều yêu cầu từ người dùng cùng lúc.  
   - **Đa luồng:** Mỗi yêu cầu được xử lý trong một luồng riêng.

4. **Phân tích dữ liệu lớn**  
   - **Ứng dụng:** Xử lý và phân tích khối lượng dữ liệu khổng lồ.  
   - **Đa tiến trình:** Chia dữ liệu thành các phân đoạn và xử lý đồng thời.

5. **Mô phỏng**  
   - **Ứng dụng:** Mô phỏng hệ thống phức tạp như khí hậu hoặc tài chính.  
   - **Đa luồng:** Chạy nhiều mô hình mô phỏng song song.

6. **Trò chơi máy tính**  
   - **Ứng dụng:** Xử lý logic trò chơi, đồ họa và âm thanh đồng thời.  
   - **Đa luồng:** Tách biệt các thành phần để tăng tốc độ xử lý.

7. **Xử lý video**  
   - **Ứng dụng:** Chỉnh sửa và nén video.  
   - **Đa luồng:** Xử lý nhiều khung hình song song.

8. **Hệ thống nhúng**  
   - **Ứng dụng:** Quản lý nhiều thiết bị và cảm biến trong thời gian thực.  
   - **Đa tiến trình:** Chạy nhiều tác vụ nhỏ trên các thiết bị khác nhau.

9. **Chạy máy ảo**  
   - **Ứng dụng:** Tạo và quản lý nhiều máy ảo trên một máy chủ.  
   - **Đa tiến trình:** Mỗi máy ảo có thể chạy một hệ điều hành khác nhau.

10. **Web scraping**  
    - **Ứng dụng:** Thu thập dữ liệu từ nhiều trang web.  
    - **Đa luồng:** Gửi nhiều yêu cầu HTTP đồng thời.

11. **Tính toán phân tán**  
    - **Ứng dụng:** Chia nhỏ bài toán tính toán và phân phối cho nhiều nút.  
    - **Đa tiến trình:** Các nút xử lý các phần khác nhau của bài toán song song.

12. **Hệ thống điều khiển tự động**  
    - **Ứng dụng:** Quản lý và điều khiển các quy trình tự động trong sản xuất.  
    - **Đa luồng:** Xử lý nhiều tín hiệu và dữ liệu từ các cảm biến đồng thời.

# 3. Liệt kê các trường hợp nào thì nên dùng thread, trường hợp nào nên dùng process, khi nào thì nên dùng cả hai. Viết dưới dạng table và đưa ví dụ các bài toán

So Sánh Sử Dụng Thread và Process

| Trường hợp sử dụng        | Nên dùng Thread                       | Nên dùng Process                       | Nên dùng Cả Hai                     |
|---------------------------|---------------------------------------|----------------------------------------|--------------------------------------|
| **Tài nguyên chia sẻ**    | Khi cần chia sẻ tài nguyên (RAM) nhanh chóng. | Khi cần tách biệt hoàn toàn tài nguyên (bảo mật). | Khi một phần cần chia sẻ, phần khác cần tách biệt. |
| **Tốc độ**                | Khi cần xử lý nhanh và hiệu suất cao. | Khi tính ổn định và bảo mật quan trọng hơn tốc độ. | Khi cần tối ưu hóa cả tốc độ và bảo mật. |
| **Giao tiếp**             | Giao tiếp dễ hơn giữa các thread.   | Giao tiếp giữa các process phức tạp hơn (IPC). | Khi cần cả giao tiếp nhanh và bảo mật. |
| **Quản lý lỗi**           | Nếu một thread gặp lỗi, có thể ảnh hưởng đến toàn bộ ứng dụng. | Mỗi process độc lập, lỗi trong một process không ảnh hưởng đến process khác. | Khi cần xử lý lỗi độc lập nhưng vẫn cần chia sẻ tài nguyên. |
| **Tình huống cụ thể**     | - Game: xử lý logic và đồ họa. <br> - Ứng dụng web: xử lý yêu cầu đồng thời. | - Ứng dụng tính toán nặng: xử lý dữ liệu lớn. <br> - Chạy máy ảo: từng máy ảo độc lập. | - Hệ thống điều khiển: xử lý cảm biến và điều khiển hành động. |

## Ví dụ Bài Toán

1. **Dùng Thread:**
   - **Game 2D:** Xử lý logic trò chơi và đồ họa đồng thời.
   - **Máy chủ web:** Xử lý nhiều yêu cầu từ người dùng cùng lúc.

2. **Dùng Process:**
   - **Xử lý video:** Chỉnh sửa và nén video với các tiến trình độc lập.
   - **Máy ảo:** Chạy nhiều hệ điều hành trên một máy chủ mà không ảnh hưởng đến nhau.

3. **Dùng Cả Hai:**
   - **Hệ thống điều khiển tự động:** Xử lý dữ liệu từ cảm biến (process) và điều khiển hành động (thread).
   - **Phân tích dữ liệu lớn:** Sử dụng process để xử lý dữ liệu và thread để tối ưu hóa các tác vụ trong từng process.

# 4. Report, tìm hiểu 1. chatGPT training tập dữ liệu lớn bằng distributed system như thế nào. Đưa link nguồn tài liệu tham khảo từ google

Để huấn luyện các mô hình ngôn ngữ lớn như ChatGPT, các tổ chức thường sử dụng các hệ thống phân tán để xử lý khối lượng tính toán khổng lồ và yêu cầu về bộ nhớ. Dưới đây là các phương pháp và công cụ phổ biến được áp dụng.

## 🧠 Các phương pháp huấn luyện phân tán cho mô hình ngôn ngữ lớn

### 1. *Data Parallelism (Phân tán dữ liệu)*:
   - *Mô tả*: Dữ liệu được chia thành nhiều phần và mỗi phần được xử lý song song trên các nút khác nhau. Các cập nhật trọng số được đồng bộ hóa sau mỗi bước huấn luyện.
   - *Ví dụ*: Sử dụng *Horovod*, một thư viện hỗ trợ phân tán dữ liệu trên nhiều GPU và máy chủ.

### 2. *Model Parallelism (Phân tán mô hình)*:
   - *Mô tả*: Mô hình được chia thành nhiều phần và mỗi phần được đặt trên các thiết bị khác nhau. Phương pháp này hữu ích khi mô hình quá lớn để vừa vặn với bộ nhớ của một thiết bị duy nhất.
   - *Ví dụ*: *DeepSpeed*, một thư viện của Microsoft, hỗ trợ phân tán mô hình và tối ưu hóa bộ nhớ để huấn luyện các mô hình với hàng trăm tỷ tham số.

### 3. *Pipeline Parallelism (Phân tán theo đường ống)*:
   - *Mô tả*: Mô hình được chia thành các giai đoạn, và mỗi giai đoạn được xử lý trên các thiết bị khác nhau. Dữ liệu được chuyền qua các giai đoạn theo dạng "đường ống".
   - *Ví dụ*: *DeepSpeed* hỗ trợ phân tán theo đường ống, giúp tối ưu hóa hiệu suất huấn luyện.

### 4. *Tensor Parallelism (Phân tán theo tensor)*:
   - *Mô tả*: Các tensor (ma trận trọng số) được chia nhỏ và phân phối trên nhiều thiết bị, giúp giảm bớt yêu cầu về bộ nhớ và tăng tốc độ huấn luyện.
   - *Ví dụ*: *DeepSpeed* hỗ trợ phân tán theo tensor, cho phép huấn luyện các mô hình lớn hơn.

### 5. *Reinforcement Learning with Human Feedback (RLHF)*:
   - *Mô tả*: Sau khi huấn luyện sơ bộ, mô hình được tinh chỉnh bằng cách sử dụng phản hồi từ con người để cải thiện khả năng hiểu và phản hồi.
   - *Ví dụ*: *OpenAI* sử dụng phương pháp RLHF để huấn luyện các mô hình như *InstructGPT* và *ChatGPT*.

## 🛠️ Công cụ hỗ trợ huấn luyện phân tán

- *DeepSpeed*: Thư viện mã nguồn mở của Microsoft giúp tối ưu hóa huấn luyện mô hình lớn, hỗ trợ phân tán dữ liệu, mô hình và đường ống, đồng thời giảm thiểu yêu cầu về bộ nhớ.
- *Ray*: Một hệ thống phân tán hỗ trợ các tác vụ AI, bao gồm huấn luyện mô hình và triển khai ứng dụng.
- *Horovod*: Thư viện hỗ trợ huấn luyện phân tán trên nhiều GPU và máy chủ, tương thích với TensorFlow và PyTorch.

## 📚 Tài liệu tham khảo

[DeepSpeed: Easy, Fast and Affordable RLHF Training of ChatGPT-like Models at All Scales](https://arxiv.org/abs/2308.01320)
[Efficient Training of Large Language Models on Distributed Systems](https://arxiv.org/abs/2407.20018)
[How Ray, a Distributed AI Framework, Helps Power ChatGPT](https://thenewstack.io/how-ray-a-distributed-ai-framework-helps-power-chatgpt/)