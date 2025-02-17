# Scaling

Trong **Amazon ECS (Elastic Container Service)**, có thể **tăng giảm số lượng tasks** trong dịch vụ ECS của bạn tự động hoặc thủ công. Ngoài ra, hỗ trợ auto scaling:

1. **Auto Scaling của ECS Service**:
    - **AWS Application Auto Scaling** giúp tự động điều chỉnh số lượng task của ECS Service dựa trên các **chỉ số hiệu suất**.
    - Các chỉ số để thực hiện scale bao gồm:
        - **CPU Utilization**: Tỷ lệ sử dụng CPU của ECS Service.
        - **Memory Utilization**: Tỷ lệ sử dụng bộ nhớ (RAM) của ECS Service.
        - **ALB Request Count per Target**: Số lượng yêu cầu từ **Application Load Balancer (ALB)** tới từng target.
    - Bạn có thể thiết lập các loại **Auto Scaling**:
        - **Target Tracking**: Theo dõi các mục tiêu cụ thể cho các chỉ số trên.
        - **Step Scaling**: Cấu hình tăng hoặc giảm dựa trên các bước với các ngưỡng cụ thể.
        - **Scheduled Scaling**: Thiết lập lịch trình mở rộng dịch vụ ECS trước thời gian dự đoán.

2. **Tăng/giảm số lượng EC2 Instances trong ECS Cluster**:
    - Nếu bạn sử dụng **EC2 Launch Type**, bạn cần tự động điều chỉnh số lượng **EC2 instances** trong **ECS Cluster**.
    - Các phương pháp để mở rộng EC2 instances:
        - **Auto Scaling Group Scaling**: Tự động mở rộng nhóm EC2 instances dựa trên các chỉ số như CPU Utilization.
        - **ECS Cluster Capacity Provider**: Là tính năng mới và thông minh hơn, khi thiếu tài nguyên để triển khai task mới, **Capacity Provider** sẽ tự động điều chỉnh **Auto Scaling Group** để tạo thêm EC2 instances.
    - Tùy chọn **ECS Cluster Capacity Provider** được khuyến khích sử dụng vì tính năng tự động và thông minh hơn.

3. **Quy trình hoạt động của ECS Service Auto Scaling**:
    - Giả sử bạn có **Service A** với hai task, và có **CPU Usage** cao.
    - Nếu số lượng người dùng tăng, **CPU Usage** sẽ tăng và **CloudWatch Metric** sẽ theo dõi sự thay đổi này.
    - Khi **CloudWatch Alarm** được kích hoạt vì CPU usage vượt ngưỡng, hoạt động scaling sẽ được kích hoạt.
    - **Desired Capacity** sẽ được tăng lên và **task mới** sẽ được tạo ra.
    - Nếu bạn sử dụng **EC2 Launch Type**, **ECS Capacity Providers** sẽ tự động mở rộng **EC2 instances** trong **ECS Cluster** để đáp ứng nhu cầu tài nguyên.

# EKS
### Data Volumes
- Need to specify StorageClass manifest on eks cluster 
- Leverages a container storage interface (CSI) compliant driver 
- support: EBS, EFS(only work with fargate), FSx for lustre, FSx for netOnTAP

# APP2Container 
- migrate app JAVA/.NET to AWS

