# 272. CloudWatch Metrics
**Amazon CloudWatch** là dịch vụ giúp giám sát và theo dõi các dịch vụ AWS. **CloudWatch Metrics** cung cấp các chỉ số cho mọi dịch vụ trong AWS, giúp bạn theo dõi các hoạt động trong tài khoản của mình.

- **Metrics** là các biến mà bạn muốn giám sát, ví dụ:
    - **EC2 instance**: CPUUtilization, NetworkIn, v.v.
    - **Amazon S3**: Bucket size, v.v.
- **Namespaces**: Mỗi dịch vụ có một namespace riêng biệt, chứa các metric liên quan.
- **Dimensions**: Các thuộc tính của metric (instance id, env, ...), mỗi metric có thể có tối đa **30 dimensions**.
- **Time-based**: Metrics phải có một timestamp. Dữ liệu sẽ được ghi lại theo thời gian.

### Tính năng của CloudWatch Metrics

- Bạn có thể tạo **CloudWatch Dashboards** để xem tất cả metrics theo cách cụ thể.
- **CloudWatch Custom Metrics**: Bạn có thể tạo metric tùy chỉnh. Ví dụ, để theo dõi bộ nhớ (memory usage) của một **EC2 instance**.

### Streaming CloudWatch Metrics

- Bạn có thể **stream** CloudWatch Metrics ra ngoài CloudWatch:
    - Truyền dữ liệu gần như theo thời gian thực (**near real-time**) với độ trễ thấp.
    - Đích đến có thể là **Amazon Kinesis Data Firehose**, và từ đó bạn có thể gửi tới:
        - **Amazon S3**, **Amazon Redshift** (phân tích dữ liệu), **Amazon OpenSearch** (xây dựng dashboard hoặc phân tích dữ liệu).
        - Các dịch vụ của bên thứ ba như **Datadog**, **Dynatrace**, **New Relic**, **Splunk**, **Sumo Logic**, v.v.
        - **Third-party Services**: CloudWatch Metrics có thể gửi trực tiếp tới các nhà cung cấp dịch vụ bên thứ ba (Datadog, Splunk, v.v.) qua **HTTP endpoint integration**.

- **Lọc theo region, dimension, hoặc resource ID**: CloudWatch Metrics cho phép bạn lọc dữ liệu theo các thuộc tính khác nhau để dễ dàng theo dõi các thông tin liên quan.

---
