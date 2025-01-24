# VPC


# LB  
- Chỉ có NLB cung cấp cả static DNS name và static IP
- ALB cung cấp 1 static DNS name. Reason: AWS muốn ELB có thể được access như một static endpoint ngay cả khi infra của AWS thay đổi 
- cookie names are reserved by the ELB (AWSALB, AWSALBAPP, AWSALBTG).
## ALB  

## NLB  
- Hoạt động ở layer 4 có thể xử lý hàng triệu req/s 
- 



# S3

# RDS
- Use case: RDBMS/OLTP, perform SQL queries, transactions
### security 
- Data at rest: use  KMS
- Data in transit: SSL
  - RDS tạo 1 SSL cert and install cert trên DB instance 
  - Đối với MySQL, bạn khởi chạy máy khách mysql bằng tham số --ssl_ca để tham chiếu khóa công khai nhằm mã hóa các kết nối. 
  - Đối với SQL Server, tải xuống khóa công khai và nhập chứng chỉ vào hệ điều hành Windows của bạn. 
  - RDS dành cho Oracle sử dụng mã hóa mạng gốc Oracle với phiên bản CSDL. Bạn chỉ cần thêm tùy chọn mã hóa mạng gốc vào một nhóm tùy chọn và liên kết nhóm tùy chọn đó với phiên bản CSD

# Aurora 

# Storage 

# Route53

# Queue

# EFS 
- Amazon Elastic File System (Amazon EFS) provides a simple, scalable, fully managed elastic NFS file system for use with AWS Cloud services and on-premises resources.

# NAT
![1.jpg](image/1.jpg)  


# Kinesis 
- Amazon Kinesis is a fully managed, scalable service that can ingest, buffer, and process streaming data in real-time.

# AWS Gateway 
- A Gateway Endpoint is a gateway that you specify as a target for a route in your route table for traffic destined to a supported AWS service. This cannot help in throttling or buffering of requests


# IAM 
