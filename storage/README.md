# 173. AWS Snowball
- **AWS Snowball** là một thiết bị di động, bảo mật cao, giúp thu thập, xử lý và di chuyển dữ liệu vào/ra AWS. Đây là giải pháp lý tưởng khi cần di chuyển khối lượng dữ liệu lớn (ví dụ: petabytes) nhưng gặp phải các vấn đề về băng thông và kết nối mạng.
- Snowball có hai loại thiết bị chính:
    - **Edge Storage Optimized**: Thiết bị này chuyên dùng cho lưu trữ dữ liệu, với dung lượng lên đến **210 terabytes**.
    - **Edge Compute Optimized**: Thiết bị này chuyên cho tính toán và chỉ có dung lượng lưu trữ là **28 terabytes**.

### Sử Dụng AWS Snowball cho Di Chuyển Dữ Liệu

- Việc truyền tải dữ liệu qua mạng có thể gặp nhiều vấn đề như băng thông thấp, kết nối không ổn định, hoặc chi phí mạng cao. Ví dụ, nếu bạn truyền 100 terabytes qua kết nối **1Gbps**, sẽ mất khoảng **12 ngày**.
- Trong trường hợp này, **AWS Snowball** là giải pháp hữu ích vì nó không yêu cầu sử dụng băng thông của bạn. Bạn chỉ cần:
    - Nhận một thiết bị Snowball vật lý.
    - Tải dữ liệu lên thiết bị.
    - Gửi thiết bị lại cho AWS.
    - AWS sẽ xử lý và chuyển dữ liệu lên các dịch vụ như **Amazon S3**.

### Sử Dụng AWS Snowball cho Edge Computing

- Một ứng dụng khác của **AWS Snowball** là **Edge Computing**, khi bạn cần xử lý dữ liệu ngay tại các điểm biên (edge) mà không có kết nối mạng ổn định hoặc băng thông lớn.
- Ví dụ: Các thiết bị có thể được sử dụng trong môi trường không có internet hoặc băng thông hạn chế, như các trạm khai thác mỏ, tàu trên biển, hoặc xe tải đang di chuyển.
- **Edge Compute Optimized** là loại thiết bị Snowball thích hợp cho các tác vụ tính toán, vì nó có khả năng chạy **EC2 instances** hoặc **Lambda functions** trực tiếp trên thiết bị.
    - Bạn có thể **tiền xử lý dữ liệu**, **máy học** hoặc **chuyển mã phương tiện** ngay trên thiết bị trước khi gửi về AWS.

### Kết Luận

- **AWS Snowball** giúp giải quyết các vấn đề khi cần di chuyển dữ liệu lớn mà không thể sử dụng kết nối mạng thông thường.
- **Edge Computing** trên Snowball cũng rất hữu ích khi xử lý dữ liệu ở các địa điểm xa xôi, không có kết nối internet ổn định.
- Tóm lại, AWS Snowball là công cụ tuyệt vời cho cả **data migration** và **edge computing** trong các tình huống cần xử lý dữ liệu lớn hoặc ở vùng không có kết nối mạnh mẽ.
![1.png](image/1.png)
--- 

# 176. Amazon FSx

Amazon FSx cho phép bạn triển khai các hệ thống tệp hiệu suất cao của bên thứ ba trên AWS dưới dạng *fully managed service*. Điều này tương tự như cách RDS cho phép bạn triển khai MySQL hoặc PostgreSQL, nhưng thay vì cơ sở dữ liệu, FSx chuyên về các hệ thống tệp.

FSx hỗ trợ các hệ thống tệp khác nhau, bao gồm:

1. FSx cho Windows File Server
2. FSx cho Lustre
3. FSx cho NetApp ONTAP
4. FSx cho OpenZFS

### 1. FSx cho Windows File Server

- Đây là dịch vụ Windows File Server được quản lý hoàn toàn, hỗ trợ giao thức SMB (Server Message Block) và Windows NTFS.
- Tính năng đặc biệt:
  - Tích hợp với Microsoft Active Directory để bảo mật người dùng.
  - 1Az -> multi Az
  - Hỗ trợ ACL (Danh sách kiểm soát truy cập) và user quotas.
  - Có thể gắn kết FSx trên các Linux EC2 instances.
  - Hỗ trợ kết nối với Windows File Server tại *on-premises* thông qua tính năng DFS (Distributed File System).
- Hiệu suất: Có thể mở rộng lên hàng chục gigabyte mỗi giây, hàng triệu IOPS và hàng trăm petabyte dữ liệu.
- Lưu trữ:
  - SSD: Cho các workload yêu cầu độ trễ thấp và hiệu suất cao, như cơ sở dữ liệu, xử lý phương tiện, phân tích dữ liệu.
  - HDD: Dành cho các workload rộng hơn, chẳng hạn như thư mục chính (home directories) hoặc CMS.
- Khả năng truy cập: Có thể truy cập từ cơ sở hạ tầng *on-premises* qua kết nối riêng tư, và có thể cấu hình Multi-AZ cho độ sẵn sàng cao.
- Sao lưu: Dữ liệu được sao lưu hàng ngày lên Amazon S3 cho mục đích phục hồi sau thảm họa.

### 2. FSx cho Lustre

- Lustre là hệ thống tệp phân tán dùng cho các ứng dụng tính toán hiệu suất cao (HPC) và máy học (ML).
- Tính năng đặc biệt:
  - Hỗ trợ các ứng dụng như xử lý video, mô phỏng tài chính, thiết kế điện tử tự động.
  - Có khả năng mở rộng cao với hàng trăm gigabyte dữ liệu mỗi giây, hàng triệu IOPS và độ trễ sub-millisecond.
  - Tích hợp liền mạch với Amazon S3, cho phép bạn đọc dữ liệu từ S3 như một hệ thống tệp và ghi kết quả tính toán từ FSx về S3.
  - Có thể sử dụng từ VPN/Direct connect 
- Lưu trữ:
  - SSD: Dành cho các workload yêu cầu độ trễ thấp, IOPS cao.
  - HDD: Dành cho các workload yêu cầu thông lượng cao và thao tác file tuần tự lớn.
- Các tùy chọn triển khai file system:
  - **Scratch File System**: Lưu trữ tạm thời, không có sao lưu, nhưng tối ưu hóa hiệu suất. Phù hợp với các workload ngắn hạn: Đây là lựa chọn lý tưởng cho các tác vụ tạm thời như phân tích dữ liệu, tính toán mô phỏng, hay các công việc không đòi hỏi bảo vệ dữ liệu lâu dài, optimize const 
  - **Persistent File System**: Lưu trữ lâu dài với sao lưu và tính sẵn sàng cao. Đây là lựa chọn phù hợp cho các tác vụ cần tính ổn định cao và không chấp nhận mất mát dữ liệu. Ví dụ như các ứng dụng tính toán khoa học, phân tích dữ liệu lớn, hay các hệ thống yêu cầu xử lý và lưu trữ dữ liệu lâu dài.

### 3. FSx cho NetApp ONTAP

- FSx cho NetApp ONTAP cung cấp một hệ thống tệp ONTAP được quản lý trên AWS.
- Tính năng đặc biệt:
  - Tương thích với các giao thức NFS, SMB và iSCSI.
  - Phù hợp để di chuyển các workload đang chạy trên hệ thống ONTAP hoặc NAS tại *on-premises* vào AWS.
  - Tính năng tự động điều chỉnh dung lượng lưu trữ, sao lưu, và sao chép dữ liệu.
  - Hỗ trợ các tính năng như nén dữ liệu, deduplication và **point-in-time cloning** cho phép bạn dễ dàng sao chép nhanh các hệ thống tệp để thử nghiệm hoặc tạo môi trường staging.
- Tương thích với nhiều hệ điều hành và dịch vụ như Linux, Windows, macOS, VMware Cloud on AWS, WorkSpaces, AppStream, EC2, ECS và EKS.

### 4. FSx cho OpenZFS

- FSx cho OpenZFS là một hệ thống tệp OpenZFS được quản lý trên AWS, tương thích với giao thức NFS.
- Tính năng đặc biệt:
  - Thích hợp để di chuyển các workload đang chạy trên ZFS vào AWS.
  - Hiệu suất cao với khả năng mở rộng lên đến 1 triệu IOPS và độ trễ dưới 0.5 ms.
  - Hỗ trợ các tính năng như sao lưu, nén dữ liệu và **point-in-time cloning**.
  - Tuy nhiên, không hỗ trợ tính năng deduplication như FSx cho NetApp ONTAP.
---

# 178. Storage Gateway
![6.png](image/6.png)

- Hybrid Cloud: Một phần cơ sở hạ tầng của bạn sẽ ở trên đám mây của AWS, phần còn lại sẽ vẫn lưu trữ tại chỗ (on-premises). Điều này có thể do nhiều lý do, như yêu cầu bảo mật, yêu cầu trước đây, hoặc chiến lược chỉ sử dụng đám mây cho elastic workloads.
- AWS Storage Gateway: Là cầu nối giữa dữ liệu tại chỗ và dữ liệu đám mây của AWS. Dịch vụ này giúp di chuyển dữ liệu giữa hạ tầng tại chỗ và đám mây.

### Các Loại Dịch Vụ Lưu Trữ Trên AWS

1. Block Storage: Ví dụ như Amazon EBS hoặc EC2 Instance Store.
2. File Systems: Bao gồm Amazon EFS và Amazon FSx.
3. Object-Level Storage: Chẳng hạn như Amazon S3 hoặc Amazon Glacier.

### Các Loại Storage Gateway

1. **Amazon S3 File Gateway**:
   ![2.png](image/2.png)

  - Chức năng: Kết nối Amazon S3 bucket với các máy chủ ứng dụng tại chỗ thông qua các giao thức NFS hoặc SMB.
  - Cách hoạt động: Sử dụng các giao thức này, Storage Gateway sẽ chuyển đổi các yêu cầu thành các yêu cầu HTTPS tới Amazon S3.
  - Lưu ý: Bạn có thể thiết lập các lifecycle policy để chuyển các đối tượng sang Glacier để lưu trữ lâu dài.
  - Tính năng: Dữ liệu được truy cập gần đây sẽ được lưu trữ tạm trong S3 File Gateway để truy cập nhanh hơn.

2. **Amazon FSx File Gateway**:
   ![3.png](image/3.png)

  - Chức năng: Cung cấp quyền truy cập gốc vào Amazon FSx cho Windows File Server.
  - Lý do sử dụng: Tạo một bộ nhớ đệm (cache) tại chỗ cho các dữ liệu hay được truy cập, giảm độ trễ khi truy cập dữ liệu quan trọng.
  - Tính tương thích: Hỗ trợ các giao thức SMB và Active Directory cho xác thực người dùng.

3. **Volume Gateway**:
  - Chức năng: Sử dụng iSCSI protocol để lưu trữ khối dữ liệu, được sao lưu bằng Amazon EBS snapshots.
  - Các loại:
    - Cached volumes: Truy cập nhanh dữ liệu gần đây.
    - Stored volumes: Dữ liệu hoàn toàn lưu trữ tại chỗ và sao lưu định kỳ lên Amazon S3.
  - Lý do sử dụng: Sao lưu các volumes từ các máy chủ ứng dụng tại chỗ.

4. **Tape Gateway**:
  - Chức năng: Dành cho các công ty sử dụng hệ thống sao lưu bằng băng từ (tape). Dữ liệu sao lưu sẽ được lưu trữ trên đám mây, sử dụng Amazon S3 hoặc Glacier.
  - Tính năng: Hỗ trợ Virtual Tape Library (VTL) và giao thức iSCSI để kết nối với các phần mềm sao lưu của bên thứ ba.
    ![4.png](image/4.png)

### Storage Gateway Hardware Appliance
- Cầu nối phần cứng: Nếu bạn không có máy chủ ảo hóa tại chỗ, bạn có thể sử dụng Storage Gateway Hardware Appliance từ AWS.
- Chức năng: Đây là một thiết bị phần cứng nhỏ được cài đặt tại cơ sở hạ tầng của bạn, giúp chạy các gateway như File Gateway, Volume Gateway, hoặc Tape Gateway mà không cần ảo hóa.

# 180. AWS Transfer Family
![7.png](image/7.png)  
AWS Transfer Family cho phép bạn truyền tải dữ liệu vào và ra từ Amazon S3 hoặc EFS mà không cần sử dụng S3 APIs hoặc EFS network file system, chỉ cần sử dụng giao thức FTP.

Dịch vụ này hỗ trợ ba giao thức:
- **FTP**: Giao thức truyền tải tệp không mã hóa.
- **FTPS**: FTP qua SSL, phiên bản mã hóa.
- **SFTP**: Giao thức truyền tải tệp an toàn.

### Tính Năng và Cách Thức Hoạt Động

- **AWS Transfer Family** là một hạ tầng quản lý hoàn toàn, có khả năng mở rộng, đáng tin cậy và có độ khả dụng cao. Bạn chỉ cần quản lý khả năng này mà không cần lo lắng về hạ tầng.
- **Giá cả**: Bạn sẽ trả phí theo giờ cho mỗi endpoint được cấp phát, cộng với phí truyền tải dữ liệu vào và ra khỏi dịch vụ.
- Bạn có thể lưu trữ và quản lý thông tin đăng nhập của người dùng trong dịch vụ hoặc tích hợp với hệ thống xác thực hiện có như **Active Directory**, **LDAP**, **Okta**, **Amazon Cognito** hoặc bất kỳ nguồn tùy chỉnh nào.

### Ứng Dụng

AWS Transfer Family rất hữu ích khi cần giao diện FTP vào Amazon S3 hoặc EFS để chia sẻ tệp, chia sẻ bộ dữ liệu công cộng, hoặc sử dụng cho các ứng dụng như CRM, ERP, v.v.

### Sơ Đồ Hoạt Động
- Dịch vụ Transfer Family hỗ trợ ba phiên bản, người dùng có thể truy cập trực tiếp vào các endpoint FTP hoặc có thể sử dụng **Route 53** để cung cấp tên miền riêng cho dịch vụ FTP.
- Khi đó, dịch vụ FTP của Transfer Family sẽ có một IAM role được giả định để gửi hoặc đọc tệp từ Amazon S3 hoặc Amazon EFS.
- Việc này diễn ra một cách minh bạch, bạn không cần thiết lập quá nhiều.

---

# 181. DataSync Overview 
![8.png](image/8.png) 
AWS DataSync là một dịch vụ rất phổ biến trong kỳ thi và rất đơn giản để hiểu. Nó được thiết kế để đồng bộ hóa dữ liệu, di chuyển khối lượng lớn dữ liệu từ các vị trí khác nhau, ví dụ như từ **on-premises** hoặc các đám mây khác vào AWS.

- **Dữ liệu và Metadata**: DataSync có khả năng giữ nguyên quyền truy cập tệp và metadata, tuân thủ các hệ thống tệp NFS POSIX và quyền SMB. Điều này rất quan trọng khi di chuyển dữ liệu từ một vị trí này sang vị trí khác, bảo đảm tính toàn vẹn và bảo mật của dữ liệu.
- **Băng thông**: Mỗi agent có thể xử lý lên đến 10 gigabits mỗi giây, nhưng bạn có thể thiết lập giới hạn băng thông nếu không muốn sử dụng hết băng thông mạng.
- Không liên tục
### Các Kịch Bản Sử Dụng

1. **Di chuyển từ on-premises vào AWS và ngược lại**:
![9.png](image/9.png) 
  - Cài đặt **DataSync agent** tại **on-premises** và kết nối với các máy chủ SMB hoặc NFS.
  - Sau đó, agent sẽ kết nối an toàn với dịch vụ DataSync và di chuyển dữ liệu đến các dịch vụ như S3, EFS, hoặc FSx.
  - DataSync có thể đồng bộ hóa dữ liệu từ AWS về **on-premises**, không chỉ đơn giản là di chuyển dữ liệu mà còn đồng bộ hóa metadata và quyền truy cập tệp.
  - Nếu không có đủ năng lực mạng, bạn có thể sử dụng **AWS Snowcone**, thiết bị với DataSync agent được cài sẵn. Snowcone sẽ thu thập dữ liệu tại **on-premises**, sau đó chuyển về AWS và đồng bộ hóa dữ liệu với các tài nguyên lưu trữ của AWS.

4. **Đồng bộ hóa giữa các dịch vụ AWS**:
  - DataSync có thể đồng bộ hóa giữa các dịch vụ lưu trữ AWS như S3, EFS, hoặc FSx mà vẫn **giữ nguyên/ bảo toàn** metadata và quyền truy cập tệp.

# Compare
![10.png](image/10.png)
