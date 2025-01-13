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


# 108. Time to live
TTL là thời gian mà một bản ghi DNS sẽ được lưu trữ trong bộ nhớ cache của client trước khi phải yêu cầu lại từ DNS server. Trong ví dụ dưới đây, khi một client truy vấn DNS từ Route 53, hệ thống DNS sẽ trả về một bản ghi A với địa chỉ IP và TTL, ví dụ 300 giây (5 phút). TTL cho biết client cần lưu trữ kết quả này trong thời gian đó.
- **Giảm tải cho DNS**: Mục đích của TTL là giảm thiểu số lần truy vấn đến DNS server, vì không kỳ vọng bản ghi DNS thay đổi thường xuyên. Điều này giúp tiết kiệm băng thông và giảm chi phí.
- Nếu bạn dự định thay đổi bản ghi, bạn có thể giảm TTL xuống thấp trong khoảng 24 giờ trước khi thay đổi bản ghi. Sau khi thay đổi, tăng TTL trở lại để giảm lưu lượng và chi phí.

TTL bắt buộc đối với mọi loại bản ghi trừ Alias record

# 109. Route53 CNAME and Alias
### **CNAME Record:**
CNAME (Canonical Name) là một bản ghi DNS cho phép bạn trỏ tên miền này tới một tên miền khác.
- CNAME có thể chỉ trỏ tới một tên miền khác (hostname), ví dụ: `app.mydomain.com` trỏ tới `blabla.anything.com`.
- CNAME **chỉ hoạt động với tên miền con**, không thể dùng cho tên miền gốc (root domain). Ví dụ, bạn không thể sử dụng CNAME cho `mydomain.com`, nhưng có thể sử dụng cho `something.mydomain.com`.
- **Hạn chế**:
    - Nó chỉ có thể ánh xạ đến các tên miền khác, không hỗ trợ ánh xạ trực tiếp đến các **tài nguyên** trong AWS.

---

### **Alias Record:**
Alias là tính năng đặc biệt của AWS Route 53 cho phép bạn trỏ một tên miền tới các tài nguyên AWS như Load Balancer, CloudFront, API Gateway, S3 Websites, v.v.
- **Alias có thể dùng cho cả tên miền gốc (root domain)** và tên miền con (non-root domain). Ví dụ, bạn có thể ánh xạ `mydomain.com` (root domain) tới một Load Balancer, điều mà CNAME không thể làm được.
- Alias Records có thể trỏ tới các tài nguyên AWS cụ thể, như Elastic Load Balancer (ELB), CloudFront Distributions, API Gateway, S3 Websites (không phải S3 Buckets), Elastic Beanstalk, và nhiều tài nguyên khác.
- **Miễn phí**: Không có chi phí khi thực hiện truy vấn với Alias Record, điều này giúp tiết kiệm chi phí DNS.
- **Health check**: Alias health check tài nguyên mà nó trỏ tới.
- **Tự động cập nhật**: Nếu IP của tài nguyên AWS (ví dụ như Load Balancer) thay đổi, Alias Record sẽ tự động nhận diện và cập nhật mà không cần can thiệp thủ công.
- **Hạn chế**:
    - Alias Records chỉ hoạt động với các tài nguyên trong AWS.
    - Không thể trỏ tới tên miền EC2 DNS (ví dụ: không thể tạo Alias cho DNS của EC2 instance).

# 110. Routing Policy

Routing policies giúp Route 53 trả lời các truy vấn DNS và không nên nhầm lẫn với thuật ngữ "routing" trong các hệ thống như load balancer. 
DNS không chuyển tiếp lưu lượng mà chỉ trả lời các truy vấn DNS và giúp client xác định cách thực hiện các truy vấn HTTP

Route 53 hỗ trợ các routing policies sau:
- **Simple Routing Policy**: Định tuyến lưu lượng đến một tài nguyên duy nhất.
- **Weighted Routing Policy**: Định tuyến lưu lượng đến các tài nguyên với tỉ lệ trọng số khác nhau.
- **Latency-based Routing Policy**: Định tuyến lưu lượng tới tài nguyên có độ trễ thấp nhất.
- **Failover Routing Policy**: Định tuyến lưu lượng dựa trên sự khả dụng của các tài nguyên.
- **Geolocation Routing Policy**: Định tuyến lưu lượng dựa trên vị trí địa lý của client.
- **Multi-value Answer Routing Policy**: Định tuyến với nhiều giá trị trả về từ DNS.
- **Geoproximity Routing Policy**: Định tuyến lưu lượng đến các tài nguyên dựa trên khoảng cách địa lý.

### **Simple Routing Policy**
Chính sách này định tuyến lưu lượng đến một tài nguyên duy nhất. Ví dụ, khi client yêu cầu truy cập vào `foo.example.com`, Route 53 sẽ trả về địa chỉ IP của tài nguyên đó.

- **A record** có thể chứa nhiều giá trị IP. Nếu nhiều giá trị được trả về, client sẽ chọn ngẫu nhiên một trong số đó.
- Nếu sử dụng alias record, bạn chỉ có thể chỉ định một tài nguyên AWS duy nhất.
- Chính sách này rất đơn giản và không hỗ trợ health checks.

VD:
- Tạo một bản ghi A cho `simple.stephanetheteacher.com` với giá trị là một instance tại khu vực `ap-southeast-1`.
- Thiết lập TTL (Time To Live) cho bản ghi là 20 giây.
- Khi thực hiện một truy vấn DNS (sử dụng công cụ như `dig`), hệ thống trả về một địa chỉ IP.
- Có thể cập nhật bản ghi để trả về nhiều địa chỉ IP từ các khu vực khác nhau, ví dụ như `ap-southeast-1` và `us-east-1`.
- Sau khi TTL hết hạn, client sẽ nhận lại nhiều địa chỉ IP và sẽ chọn ngẫu nhiên một trong số đó.

### Weighted Routing Policy
Cho phép bạn điều chỉnh phần trăm lưu lượng truy cập được chuyển tới từng tài nguyên dựa trên trọng số (weights) mà bạn chỉ định. 
Cụ thể, bạn có thể phân chia lưu lượng theo tỷ lệ phần trăm giữa các tài nguyên khác nhau. Mỗi tài nguyên có thể được chỉ định một trọng số, và tỷ lệ lưu lượng sẽ được điều chỉnh dựa trên các trọng số đó.

- Giả sử bạn có ba EC2 instance được gán các trọng số khác nhau: 70, 20 và 10.
- Điều này có nghĩa là 70% các phản hồi DNS từ Route 53 sẽ được chuyển đến EC2 instance đầu tiên, 20% sẽ chuyển đến EC2 instance thứ hai, và 10% sẽ chuyển đến EC2 instance thứ ba.
- Các trọng số này không cần phải cộng lại thành 100, chúng chỉ mang tính chất tương đối để xác định lưu lượng được phân phối giữa các tài nguyên.
- Để làm điều này hoạt động, các bản ghi DNS phải có cùng tên và loại, và bạn có thể liên kết chúng với các **health checks** (mặc dù phần này sẽ được tìm hiểu sau).

VD:
- **Load balancing**: Phân phối lưu lượng giữa các regions khác nhau.
- **Kiểm thử phiên bản ứng dụng mới**: Gửi một lượng lưu lượng nhỏ đến một phiên bản mới của ứng dụng.
- **Điều chỉnh lưu lượng**: Nếu trọng số của một bản ghi là 0, không có lưu lượng nào sẽ được gửi đến tài nguyên đó.


### Latency-based Routing Policy

Điều hướng lưu lượng truy cập đến tài nguyên có độ trễ thấp nhất, nghĩa là tài nguyên gần người dùng nhất. 
Đây là policy rất hữu ích khi độ trễ là yếu tố quan trọng đối với các trang web hoặc ứng dụng của bạn.
- Độ trễ được đo lường dựa trên thời gian kết nối nhanh nhất của người dùng đến một AWS region đã được xác định cho bản ghi DNS.
- Ví dụ, nếu người dùng ở Đức và tài nguyên ở Mỹ có độ trễ thấp nhất, thì người dùng sẽ được chuyển đến tài nguyên đó.
- Chính sách này có thể kết hợp với các health checks để đảm bảo tài nguyên hoạt động ổn định.

### Failover Routing Policy

Định tuyến lưu lượng mạng đến các tài nguyên khác nhau dựa trên các tiêu chí như latency, healthy, hoặc các tình huống dự phòng.

Đảm bảo rằng khi tài nguyên chính không khả dụng, lưu lượng sẽ được chuyển hướng đến tài nguyên dự phòng 

### Geolocation Routing Policy 
Chính sách Geolocation Routing trong Route 53 cho phép bạn định tuyến lưu lượng dựa trên vị trí 
địa lý của người dùng. Đây là một phương pháp rất khác biệt so với Latency-based Routing, 
bởi vì nó xác định lưu lượng dựa trên nơi người dùng thực sự đang ở, chẳng hạn như từ quốc gia, 
khu vực, hoặc thậm chí tiểu bang trong một quốc gia.

- Với Geolocation Routing, bạn có thể chỉ định rằng người dùng đến từ một khu vực cụ thể sẽ được chuyển hướng đến một tài nguyên (IP) cụ thể.
- Ví dụ, người dùng từ Đức có thể được chuyển đến một IP chứa phiên bản ứng dụng bằng tiếng Đức, trong khi người dùng từ Pháp sẽ được chuyển đến một IP khác chứa phiên bản ứng dụng bằng tiếng Pháp.
- Bạn cũng có thể xác định một bản ghi default để xử lý các trường hợp khi không có địa lý khớp với các quy tắc bạn đã tạo.
- Lợi ích của Geolocation Routing bao gồm: địa phương hóa trang web, hạn chế phân phối nội dung, và cân bằng tải dựa trên vị trí của người dùng.

### Geoproximity Routing

**Geoproximity Routing** là một tính năng trong AWS Route 53 giúp định tuyến lưu lượng dựa trên vị trí địa lý của người dùng và tài nguyên, với khả năng điều chỉnh mức độ lưu lượng chuyển hướng tới tài nguyên ở các vùng địa lý cụ thể bằng cách sử dụng **bias**.

#### Các điểm cần lưu ý về Geoproximity Routing:
**Định tuyến theo vị trí địa lý**:
- **Geoproximity Routing** cho phép bạn chuyển hướng lưu lượng đến tài nguyên dựa trên **vị trí địa lý của người dùng** và **vị trí của các tài nguyên** (có thể là các điểm cuối AWS hoặc trung tâm dữ liệu của bạn).
- Cách thức này được sử dụng để **tối ưu hóa lưu lượng**, đặc biệt khi bạn muốn điều chỉnh lượng người dùng truy cập vào một khu vực cụ thể.

**Bias**:
- **Bias** là một giá trị số giúp điều chỉnh mức độ lưu lượng chuyển tới các tài nguyên nhất định. Bias có thể được đặt là:
  - **Positive bias**: Thu hút nhiều lưu lượng hơn đến tài nguyên đó.
  - **Negative bias**: Giảm lượng lưu lượng được chuyển đến tài nguyên đó.
  - **Bias = 0**: Không có sự điều chỉnh và lưu lượng được phân phối đều.
- Ví dụ: Nếu bạn có một tài nguyên ở **us-west-1** và một tài nguyên khác ở **us-east-1**, bạn có thể điều chỉnh bias để hướng lưu lượng nhiều hơn đến **us-east-1** bằng cách tăng bias của nó.

**Quy trình hoạt động**:
- Ban đầu, nếu không có bias, lưu lượng sẽ được phân chia đồng đều giữa các tài nguyên, phụ thuộc vào vị trí địa lý của người dùng.
- Khi bạn **tăng bias** ở một vùng cụ thể, đường phân chia (line) giữa các khu vực sẽ di chuyển về phía **vùng có bias cao hơn**. Điều này có nghĩa là **nhiều người dùng sẽ được chuyển hướng đến tài nguyên đó**.
- Tương tự, khi **giảm bias**, lưu lượng sẽ ít hơn được chuyển đến tài nguyên đó.

**Ví dụ**:
- Giả sử bạn có hai tài nguyên ở hai khu vực **us-west-1** và **us-east-1**, và bạn muốn chuyển hướng nhiều lưu lượng đến **us-east-1**. Bạn có thể **đặt bias dương** cho **us-east-1** và **bias bằng 0** cho **us-west-1**. Điều này sẽ khiến lưu lượng từ người dùng ở phía **đông** của Hoa Kỳ được chuyển hướng đến **us-east-1**, còn lưu lượng từ người dùng phía **tây** sẽ tiếp tục đến **us-west-1**.
- Nếu bạn cần chuyển lưu lượng nhiều hơn nữa đến **us-east-1**, bạn có thể tăng bias cho **us-east-1** (ví dụ: 50) để khiến đường phân chia dịch chuyển về phía **us-west-1**.

**Tính năng mở rộng**:
- Geoproximity Routing không chỉ hoạt động với tài nguyên AWS, mà bạn cũng có thể định nghĩa **tọa độ (latitude, longitude)** cho các tài nguyên không phải AWS, ví dụ như **trung tâm dữ liệu của bạn** hoặc các tài nguyên ngoài AWS.
- Để sử dụng Geoproximity Routing, bạn cần cấu hình **Route 53 Traffic Flow** và xác định các tài nguyên và bias của chúng.

### IP-based Routing

Định tuyến lưu lượng dựa trên địa chỉ IP của khách hàng. Điều này giúp tối ưu hóa hiệu suất mạng hoặc giảm chi phí mạng khi bạn biết trước IP mà người dùng sẽ kết nối.

- **Định tuyến theo CIDR**: Bạn sẽ xác định một danh sách các dải địa chỉ IP (CIDR) và chỉ định một địa chỉ IP cụ thể cho mỗi dải. Khi một yêu cầu DNS đến từ một IP trong dải CIDR đó, lưu lượng sẽ được điều hướng đến tài nguyên (ví dụ: EC2 instance) tương ứng.
- **Mục đích sử dụng**:
    - **Tối ưu hóa hiệu suất** khi bạn biết trước khách hàng sẽ đến từ đâu.
    - **Giảm chi phí mạng** khi bạn biết nguồn lưu lượng và có thể chỉ định địa chỉ IP gần với người dùng hơn.

### Multi-Value Routing Policy

Điều hướng lưu lượng đến resources.   
Điều này giúp phân phối lưu lượng giữa nhiều tài nguyên khác nhau để cải thiện độ sẵn sàng và độ tin cậy của dịch vụ.
- **Định tuyến đến nhiều tài nguyên**: Route 53 trả về nhiều giá trị (địa chỉ IP) từ các bản ghi tài nguyên mà bạn đã cấu hình.
- **Kết hợp với Health Checks**: Bạn có thể gắn Health Checks với các bản ghi. Chỉ các tài nguyên có Health Check thành công (healthy) mới được trả về.
- **Khách hàng chọn tài nguyên**: Các khách hàng sẽ nhận được một danh sách các tài nguyên và tự chọn tài nguyên để kết nối.

Chính sách **Multi-Value** có thể trả về **tối đa 8 bản ghi** (có thể là địa chỉ IP của EC2, ví dụ) cho mỗi truy vấn DNS.

Ứng dụng:
- **Load balancing client-side**: Mặc dù chính sách này không thay thế hoàn toàn Elastic Load Balancing (ELB), nhưng nó cung cấp phương thức phân phối tải ở phía khách hàng.
- **Tăng tính sẵn sàng**: Nếu một trong các tài nguyên trở nên không khả dụng (unhealthy), sẽ có các tài nguyên khác để thay thế.

# 113. Health Checks
Giúp bạn kiểm tra tình trạng hoạt động của các tài nguyên, đặc biệt là các tài nguyên công cộng. 
Tuy nhiên, nó cũng hỗ trợ kiểm tra tài nguyên riêng tư, như các tài nguyên trong VPC 
hoặc các tài nguyên nội bộ khác, thông qua việc sử dụng các CloudWatch Alarm.

**Health Checks theo Endpoint**  
- Được sử dụng để kiểm tra các tài nguyên công cộng như **Load Balancer** hoặc máy chủ ứng dụng.
- AWS sử dụng khoảng 15 **health checkers** từ nhiều khu vực trên toàn cầu để gửi các yêu cầu đến điểm cuối của bạn. Nếu điểm cuối trả về mã trạng thái `2xx` hoặc `3xx`, tài nguyên được xem là **healthy** (hoạt động tốt).
- Bạn có thể thiết lập tần suất kiểm tra là **30 giây** (mặc định) hoặc **10 giây** (tăng chi phí).
- Bạn có thể kiểm tra nhiều giao thức như **HTTP, HTTPS, TCP** và thậm chí kiểm tra nội dung trong phản hồi để xác định tình trạng tài nguyên.
- Để health check hoạt động, bạn cần cho phép các yêu cầu từ **IP health checkers của Route 53**.

**Calculated Health Checks**
- Cho phép kết hợp kết quả của nhiều health check thành một health check duy nhất.
- Ví dụ, bạn có thể kết hợp ba health check cho ba EC2 instances và tạo một **parent health check** để theo dõi tất cả các child health check.
- Bạn có thể định nghĩa điều kiện như **AND**, **OR**, hoặc **NOT** để quyết định khi nào parent health check được coi là healthy.
- Điều này rất hữu ích khi bạn muốn bảo trì trang web mà không làm hỏng toàn bộ hệ thống kiểm tra sức khỏe.

**Health Checks cho Tài nguyên Riêng Tư**
- Route 53 health checkers không thể truy cập trực tiếp các tài nguyên trong VPC riêng tư.
- Để giải quyết vấn đề này, bạn có thể tạo **CloudWatch Metrics** để theo dõi tài nguyên trong VPC riêng và sử dụng **CloudWatch Alarm**. Khi **CloudWatch Alarm** báo động, trạng thái của health check sẽ chuyển sang unhealthy, giúp bạn theo dõi và quản lý tài nguyên riêng tư.

### Quy trình thiết lập:
- Tạo các **health checks** cho các tài nguyên công cộng như **ALB** trong các khu vực khác nhau (ví dụ `us-east-1`, `eu-west-1`).
- Liên kết các health checks này với các bản ghi DNS trong Route 53 để đảm bảo tự động **DNS failover** (chuyển hướng lưu lượng DNS khi tài nguyên không khả dụng).
- Với các tài nguyên riêng tư, bạn có thể sử dụng **CloudWatch Metrics** và **CloudWatch Alarms** để theo dõi sức khỏe của chúng và tích hợp với Route 53.

# 126.
### **Tăng tốc triển khai ứng dụng với EC2 Instances và các phương pháp tối ưu**

**Golden AMI**:
- Một **Golden AMI (Amazon Machine Image)** là một **image** đã được cài sẵn các ứng dụng, hệ điều hành, và các phụ thuộc cần thiết. Sau khi cài đặt tất cả mọi thứ và cấu hình xong, bạn sẽ tạo một AMI từ instance đó.
- **Ưu điểm**: Khi bạn cần khởi tạo các **EC2 instances** mới trong tương lai, bạn có thể **launch** trực tiếp từ **Golden AMI** mà không cần phải cài đặt lại các ứng dụng và cấu hình hệ điều hành. Điều này giúp khởi động **EC2 instances** nhanh hơn rất nhiều.

**EC2 User Data**:
- **Bootstrapping** là quá trình tự động cài đặt và cấu hình các ứng dụng và hệ điều hành khi một EC2 instance lần đầu được khởi động. **User Data** của EC2 cho phép bạn thực hiện bootstrapping, ví dụ như cài đặt ứng dụng hoặc lấy thông tin cấu hình như URL cơ sở dữ liệu hoặc mật khẩu.
- **Ưu điểm**: Bạn có thể kết hợp giữa **Golden AMI** và **User Data** để giảm thiểu thời gian cấu hình khi khởi động các instance mới.
**Elastic Beanstalk**:
- **Elastic Beanstalk** là một dịch vụ quản lý ứng dụng của AWS, sử dụng kết hợp giữa việc tái cấu hình AMI và việc thêm **user data** để tự động triển khai các ứng dụng. Đây là một cách hiệu quả để triển khai nhanh các ứng dụng với các môi trường được cấu hình sẵn.

**Sử dụng Snapshot cho RDS và EBS**:
- **RDS Snapshots**: Đối với các cơ sở dữ liệu RDS, thay vì phải chạy các câu lệnh **insert** mất nhiều thời gian để chèn dữ liệu, bạn có thể **khôi phục từ một snapshot**. Snapshot này sẽ có các schema và dữ liệu đã có sẵn, giúp tiết kiệm thời gian triển khai.
- **EBS Snapshots**: Tương tự với EBS, bạn có thể **khôi phục từ một snapshot** thay vì phải tạo mới và định dạng ổ đĩa từ đầu. Snapshot sẽ có dữ liệu cần thiết và đã được định dạng sẵn, giúp tiết kiệm thời gian khi khởi động các instance mới.

# 127. Elastic BeanStalk
Elastic Beanstalk là một dịch vụ quản lý ứng dụng của AWS giúp đơn giản hóa quá trình triển khai và quản lý ứng dụng web. Trước khi có Elastic Beanstalk, chúng ta phải tự xây dựng các thành phần cơ sở hạ tầng như **Load Balancer (LB)**, **Auto Scaling Group (ASG)**, **EC2 Instances**, **RDS Databases**, và các tầng khác của ứng dụng. Tuy nhiên, với Elastic Beanstalk, tất cả các thành phần này được quản lý tự động và bạn chỉ cần tập trung vào việc triển khai mã nguồn của ứng dụng.
giúp các nhà phát triển giảm thiểu công sức trong việc quản lý hạ tầng và triển khai mã nguồn. Khi sử dụng Elastic Beanstalk, bạn sẽ không phải lo về việc cấu hình **Load Balancers**, **Auto Scaling**, hay **RDS**. Các thành phần này được Elastic Beanstalk quản lý và tự động hoá, giúp việc triển khai và mở rộng ứng dụng trở nên dễ dàng hơn rất nhiều.

**Elastic Beanstalk** sẽ quản lý
- Tự động phân bổ tài nguyên cần thiết cho ứng dụng.
- Cấu hình và quản lý các bộ phân phối tải.
- Health check
- Quản lý các instance EC2 tự động.

### **Cấu trúc của Elastic Beanstalk**
Elastic Beanstalk được tổ chức thành các thành phần chính:
1. **Ứng dụng**: Là tập hợp các thành phần của Beanstalk, bao gồm môi trường, phiên bản và cấu hình.
2. **Phiên bản ứng dụng**: Là phiên bản của mã nguồn ứng dụng. Bạn có thể có nhiều phiên bản như phiên bản 1, 2, 3,...
3. **Môi trường**: Là tập hợp các tài nguyên đang chạy phiên bản ứng dụng cụ thể. Mỗi môi trường chỉ có thể chứa một phiên bản ứng dụng tại một thời điểm, nhưng bạn có thể cập nhật phiên bản ứng dụng trong môi trường từ phiên bản này sang phiên bản khác.
4. **Môi trường Web và Worker**:
    - **Web Tier**: Đây là kiến trúc truyền thống với một **Load Balancer** phân phối lưu lượng tới các **EC2 instances** được quản lý trong **Auto Scaling Group**. Mỗi EC2 instance sẽ chạy ứng dụng web của bạn.
    - **Worker Tier**: Trong trường hợp này, các **EC2 instances** không phục vụ trực tiếp người dùng. Thay vào đó, chúng sẽ lấy các **message** từ một **SQS queue** để xử lý. Môi trường worker sẽ tự động mở rộng dựa trên số lượng tin nhắn trong **SQS queue**.

### **Triển khai ứng dụng với Elastic Beanstalk**
Quá trình triển khai ứng dụng trên Elastic Beanstalk bao gồm các bước:
1. **Tạo một ứng dụng**.
2. **Tải lên mã nguồn** của ứng dụng.
3. **Khởi tạo môi trường** cho ứng dụng.
4. **Quản lý vòng đời môi trường** và cập nhật ứng dụng khi cần thiết.

### **Các ngôn ngữ hỗ trợ**
Elastic Beanstalk hỗ trợ nhiều ngôn ngữ lập trình khác nhau, bao gồm:
- **Go**, **Java SE**, **Java với Tomcat**, **.NET Core trên Linux**, **.NET trên Windows Server**, **Node.js**, **PHP**, **Python**, **Ruby**.
- Ngoài ra, Elastic Beanstalk còn hỗ trợ các ứng dụng chạy trong **Docker containers** (đơn và đa container).

### **Các chế độ triển khai Elastic Beanstalk**
Elastic Beanstalk có hai chế độ triển khai chính:
1. **Single Instance (Dành cho phát triển)**: Đây là môi trường triển khai đơn giản với một **EC2 instance** duy nhất, có thể kết hợp với **Elastic IP** và **RDS database**. Đây là lựa chọn lý tưởng cho phát triển và thử nghiệm.
2. **High Availability (Dành cho sản xuất)**: Trong môi trường sản xuất, bạn có thể sử dụng chế độ **High Availability** với **Load Balancer**, **Auto Scaling Group**, và **Multiple Availability Zones** (AZs). Bên cạnh đó, bạn có thể kết hợp với **RDS Multi-AZ** để đảm bảo tính sẵn sàng cao cho cơ sở dữ liệu.

### **Tóm tắt**
Elastic Beanstalk là một công cụ mạnh mẽ giúp các nhà phát triển triển khai và quản lý ứng dụng web một cách đơn giản và hiệu quả. Dịch vụ này tự động hoá nhiều công việc quản lý hạ tầng phức tạp như cấu hình **Load Balancer**, **Auto Scaling**, và **Database**. Với Elastic Beanstalk, các nhà phát triển chỉ cần tập t
rung vào việc phát triển mã nguồn và có thể triển khai ứng dụng với rất ít nỗ lực quản lý hạ tầng.