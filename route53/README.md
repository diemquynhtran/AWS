# 102. DNS 
### Tổng hợp về DNS (Domain Name System)

- DNS (Domain Name System) là hệ thống chuyển đổi **hostname** thân thiện với con người (vd: `www.google.com`) thành địa chỉ IP của server đích (vd: `172.217.11.46`).
- Là "backbone" của internet, giúp trình duyệt hoặc ứng dụng truy cập đúng server.

### **Cấu trúc phân cấp của DNS**
- **Root**: Ký hiệu là dấu chấm `.` cuối cùng trong **Fully Qualified Domain Name (FQDN)**.
- **TLD (Top-Level Domain)**: Các phần mở rộng như `.com`, `.org`, `.gov`, `.in`, v.v.
- **Second-Level Domain**: Là phần đứng trước TLD, ví dụ `google.com`, `amazon.com`.
- **Subdomain**: Là phần đứng trước Second-Level Domain, ví dụ `www.example.com`, `api.example.com`.

#### **Ví dụ về FQDN**
`http://api.www.example.com`
- **Root**: Dấu `.` cuối cùng.
- **TLD**: `.com`.
- **Second-Level Domain**: `example.com`.
- **Subdomain**: `www.example.com`.
- **FQDN**: `api.www.example.com`.
- **Protocol**: `HTTP`.

---

### **Các thành phần liên quan**
- **Domain Registrar**: Nơi đăng ký tên miền, ví dụ: **Amazon Route 53**, **GoDaddy**.
- **DNS Records**: Các loại bản ghi dùng để ánh xạ hostname với địa chỉ IP hoặc thông tin khác:
    - **A**: Ánh xạ tên miền với IPv4.
    - **AAAA**: Ánh xạ tên miền với IPv6.
    - **CNAME**: Ánh xạ một tên miền với tên miền khác.
    - **NS**: Chỉ định Name Server.
- **Zone File**: Chứa toàn bộ các bản ghi DNS cho một domain.
- **Name Servers**: Server chịu trách nhiệm xử lý truy vấn DNS.

---

### **Cách DNS hoạt động**
1. **Truy vấn từ trình duyệt**:  
   Khi bạn truy cập `example.com`, trình duyệt sẽ hỏi **Local DNS Server** (quản lý bởi ISP hoặc công ty).

2. **Truy vấn đệ quy qua các server**:
    - **Root DNS Server**: Trả về thông tin của **TLD (.com)**.
    - **TLD DNS Server**: Trả về thông tin của **Second-Level Domain**.
    - **Second-Level DNS Server**: Trả về địa chỉ IP cuối cùng.

3. **Kết quả trả về**:
    - Local DNS Server lưu trữ tạm thời (cache) kết quả để trả lời nhanh hơn cho các lần truy vấn sau.
    - Trình duyệt dùng IP được trả về để truy cập vào web server đích.

---

### **Route 53 và quản lý DNS**
- Sau khi hiểu cách DNS hoạt động, bạn có thể sử dụng **Amazon Route 53** để quản lý DNS cho domain của mình.
- Route 53 cung cấp các tính năng như:
    - Đăng ký domain.
    - Tạo và quản lý DNS Records.
    - Thiết lập **Health Check** để theo dõi trạng thái của server.

--- 

### Lợi ích của DNS
- Cho phép truy cập internet bằng tên miền dễ nhớ thay vì địa chỉ IP khó hiểu.
- Giảm độ trễ khi truy cập nhờ caching tại Local DNS Server.
- Hỗ trợ khả năng mở rộng và phân phối tải.  


# 103. DNS Overview

Amazon Route 53 là dịch vụ DNS managed service của AWS, có khả năng scalable và highly available. Dịch vụ này giúp bạn **quản lý DNS records**, xác định cách **route traffic** đến tên miền và các miền con của nó.

### Các tính năng chính của Route 53
- **Quản lý DNS**: Route 53 cho phép bạn cấu hình các **DNS records** cho tên miền của bạn, giúp chuyển đổi tên miền thành **IP addresses** (ví dụ: từ `example.com` sang `54.22.33.44`).
- **Đăng ký tên miền**: Route 53 cũng cung cấp chức năng **domain registrar**, cho phép bạn **register domain names** như `example.com`.
- **Kiểm tra tình trạng sức khỏe**: Route 53 có khả năng theo dõi **health checks** của các tài nguyên trong hệ thống và tự động điều chỉnh **traffic routing** nếu phát hiện sự cố.
- **SLA khả dụng 100%**: Route 53 là dịch vụ DNS duy nhất của AWS cung cấp **100% availability SLA**.

### Các loại bản ghi DNS quan trọng trong Route 53
- **A Record**: Dùng để ánh xạ **hostname** vào **IPv4 address**. Ví dụ: `example.com` -> `1.2.3.4`.
- **AAAA Record**: Tương tự như A Record, nhưng ánh xạ **hostname** vào **IPv6 address**.
- **CNAME Record**: Dùng để ánh xạ một **hostname** vào **hostname** khác. Ví dụ: `www.example.com` -> `example.com`. Lưu ý, bạn không thể tạo **CNAME** cho tên miền chính (ví dụ: `example.com`).
- **NS Record**: Chỉ định các **name servers** của **hosted zone**, giúp xác định các máy chủ DNS có thể trả lời các truy vấn cho tên miền đó.

### Các Hosted Zones trong Route 53

- **Public Hosted Zone**:  Đây là **public DNS zone**, có thể trả lời các truy vấn từ công chúng. Ví dụ: khi người dùng truy vấn `example.com`, hệ thống sẽ trả về địa chỉ IP công khai của máy chủ.
- **Private Hosted Zone**:  Đây là **private DNS zone**, chỉ có thể được truy vấn từ các tài nguyên trong **VPC** (Virtual Private Cloud). Ví dụ: nếu bạn có một dịch vụ nội bộ như `app.company.internal`, chỉ có các tài nguyên trong VPC của bạn mới có thể truy vấn được tên miền này.


### Lưu ý về chi phí sử dụng Route 53

- Mỗi **hosted zone** bạn tạo sẽ mất khoảng **50 cent/tháng**.
- Nếu bạn đăng ký một tên miền qua Route 53, chi phí bắt đầu từ **12 USD/năm**.
