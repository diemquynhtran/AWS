# 89. RDS Read Replicas & Multi AZ
#### **Read Replicas**
- Mục đích: Giúp scale reads, mở rộng khả năng đọc của hệ thống.
- Cơ chế:
    - Asynchronous replication từ Master database instance.
    - Dữ liệu có thể có độ trễ nhẹ gọi là eventually consistent.
- Sử dụng:
    - Tối ưu cho các truy vấn SELECT, chỉ hỗ trợ đọc dữ liệu. Không hỗ trợ INSERT, UPDATE, DELETE -> not change database
    - Có thể sử dụng cho các trường hợp chạy báo cáo hoặc analytics mà không làm slow down database
    - Có thể promted 1 replica làm DB chính 
- Hỗ trợ tối đa 15 Read Replicas
- Hỗ trợ triển khai trong cùng Availability Zone, cross Availability Zone, hoặc cross Region.
  - Network cost (thường CÓ khi data di chuyển từ AZ -> AZ khác, exception thường có với các managed service):
    - Data transfer to same REGION is  free but cross REGION isn't free 

#### **Multi AZ**
- Mục đích: Đảm bảo Disaster Recovery, increase availability!
- Cơ chế:
    - Synchronous replication giữa Master database và Standby instance trong Availability Zone khác.
    - Cung cấp 1 DNS name sử dụng để automatic failover khi master DB gặp disaster  
    - VD: loss of AZ, loss of network, instance or storage failure 
    - Tự động chuyển đổi (No manual) miễn là hệ thống trying to connect DB 
- Sử dụng:
    - Standby database chỉ hoạt động như bản dự phòng, không thể read/write dữ liệu và không sử dụng cho mục đích scaling
    - Read Replicas có thể setup như 1 Multi AZ for DR 
- Chuyển đổi từ Single AZ sang Multi AZ là một thao tác zero downtime. Khi lựa chọn option này, AWS RDS tự động thực hiện snapshot và thiết lập Standby database.

#### **So sánh Read Replicas và Multi AZ**
- Read Replicas dùng để scale reads, hỗ trợ đọc dữ liệu từ các Replica, sử dụng asynchronous replication.
- Multi AZ dùng cho mục đích tăng tính sẵn sàng và Disaster Recovery, Standby database không hỗ trợ đọc hay ghi, sử dụng synchronous replication.
- Read Replica có thể được cấu hình như Multi AZ nếu cần. 


