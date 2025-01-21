# 131. S3 Security

**User-Based**: 
  - IAM policies để cấp quyền cho các API calls mà người dùng IAM cụ thể có thể thực hiện.

**Encryption keys**: để mã hóa các đối tượng.

**Resource-Based Security**: 
- Bucket policies (phổ biến) để áp dụng các quy tắc phạm vi rộng, có thể gán trực tiếp từ S3 Console. Ví dụ:
    - Một người dùng cụ thể hoặc người dùng từ tài khoản khác (cross-account) có thể truy cập vào S3 Buckets của bạn.
    - Đây là cách làm cho S3 Buckets trở thành công khai.
- Ví dụ về Bucket Policy cho Public Access:
  - Người dùng trên worldwide web muốn truy cập vào các tệp trong S3 Buckets của bạn. Bạn sẽ gắn S3 Bucket policy cho phép truy cập công khai.
  - Sau khi chính sách này được gắn vào S3 Bucket, người dùng có thể truy cập vào bất kỳ đối tượng nào trong đó.

- Object Access Control List:Cung cấp bảo mật chi tiết hơn cho từng đối tượng và có thể tắt tính năng này. Nếu quản lý ở cấp Bucket, bạn có thể dùng Buckets ACL, nhưng ít phổ biến và có thể tắt đi.

- IAM principle có thể truy cập vào một S3 object nếu IAM permissions hoặc resource policies cho phép và không có explicit deny.
- Nếu có một người dùng IAM trong tài khoản của bạn và muốn truy cập Amazon S3, bạn có thể gán IAM permissions cho người dùng đó thông qua chính sách. Nếu chính sách cho phép, người dùng sẽ có thể truy cập S3 Buckets.
- Nếu bạn có một EC2 instance và muốn cấp quyền truy cập từ EC2 instance vào S3 Buckets, cần sử dụng IAM roles thay vì IAM users.

- Cross-Account Access:
    - Nếu có một IAM user trong tài khoản AWS khác, bạn cần tạo S3 Bucket policy cho phép quyền truy cập Cross-Account Access. Người dùng sẽ có thể thực hiện API calls vào S3 Buckets của bạn.

- Block Public Access settings:
    - AWS cung cấp cài đặt này như lớp bảo mật để ngăn ngừa rò rỉ dữ liệu.
    - Nếu bạn bật các cài đặt này, S3 Bucket của bạn sẽ không bao giờ trở thành công khai dù chính sách S3 Bucket có cho phép.
    - Nếu bạn chắc chắn rằng không có S3 Buckets nào của bạn cần công khai, có thể thiết lập ở cấp tài khoản.


# 140. S3 Storage Classes
![1.jpg](image/1.jpg)
### **Amazon S3 Standard-General Purpose (S3 Standard)**
- Đây là lớp lưu trữ mặc định trong Amazon S3, được sử dụng cho dữ liệu truy cập thường xuyên.
- S3 Standard có độ bền cao với "11 nines" (99.999999999%), nghĩa là trung bình mỗi 10 triệu đối tượng lưu trữ, sẽ có một đối tượng bị mất mỗi 10.000 năm.
- Lớp này có **availability** là 99.99%, với **low latency** và **high throughput**.
- Usecase: phân tích dữ liệu lớn, ứng dụng di động, game, phân phối nội dung.

### **Amazon S3 Infrequent Access (S3-IA)**

- Dành cho dữ liệu ít được truy cập nhưng cần truy cập nhanh khi cần.
- Giá thấp hơn S3 Standard nhưng có phí khi truy xuất dữ liệu.
- **S3 Standard-IA**:
  - Availability là 99.9%.
  - Use case: disaster recovery, backups
- **S3 One Zone-IA**
  - Có độ bền cao nhưng chỉ trong một khu vực (AZ) duy nhất
  - **Availability** là 99.5%.
  - Thích hợp cho các bản sao lưu phụ hoặc dữ liệu có thể tái tạo.

### **Amazon S3 Glacier Storage Classes**
- Cost thấp -> phù hợp với archiving/backup
- Price = storage + retrieval cost 

- **Amazon S3 Glacier Instant Retrieval**: 
  - Dữ liệu có thể truy xuất trong vài mili giây 
  - Phù hợp với dữ liệu cần truy cập thỉnh thoảng (ví dụ: mỗi quý). 
  - Thời gian lưu trữ tối thiểu là 90 ngày.

- **Amazon S3 Glacier Flexible Retrieval**: 
  - Truy xuất dữ liệu trong khoảng thời gian từ 1 đến 12 giờ, tùy chọn (Expedited, Standard, Bulk).
  - Thời gian lưu trữ tối thiểu là 90 ngày.

- **Amazon S3 Glacier Deep Archive**: 
  - Lưu trữ dài hạn với chi phí thấp nhất
  - truy xuất dữ liệu trong khoảng thời gian từ 12 giờ đến 48 giờ. 
  - Thời gian lưu trữ tối thiểu là 180 ngày.

### **Amazon S3 Intelligent-Tiering**
- Tự động di chuyển object giữa các tầng lưu trữ dựa trên mẫu sử dụng.
- Không cần can thiệp của người dùng để tiết kiệm chi phí.
- Các tầng lưu trữ:
    - **Frequent Access**: Tầng truy cập thường xuyên mặc định.
    - **Infrequent Access**: Dành cho đối tượng không được truy cập trong vòng 30 ngày.
    - **Archive Instant Access**: Dành cho đối tượng không được truy cập trong vòng 90 ngày.
    - **Archive Access**: Tầng lưu trữ dài hạn, có thể cấu hình từ 90 đến hơn 700 ngày.
    - **Deep Archive Access**: Dành cho đối tượng không được truy cập trong vòng 180 đến hơn 700 ngày.

# 142. S3 Lifecycle Rules 
### Chuyển Đổi Giữa Các Lớp Lưu Trữ trong Amazon S3

- **Các lớp lưu trữ**: Bạn có thể di chuyển đối tượng giữa các lớp lưu trữ khác nhau như từ **Standard** sang **Standard IA**, **Intelligent Tiering**, **One-Zone IA**, và từ **One-Zone IA** sang **Flexible Retrieval** hoặc **Deep Archive**.
- **Di chuyển đối tượng**: Nếu đối tượng ít được truy cập, có thể chuyển sang **Standard IA**. Nếu cần lưu trữ lâu dài, chuyển sang **Glacier** hoặc **Deep Archive**.

### Lifecycle Rules

- **Quy tắc vòng đời** giúp tự động chuyển đổi đối tượng giữa các lớp lưu trữ hoặc xóa đối tượng sau một khoảng thời gian nhất định.
  - **Transition action**: Chuyển đối tượng sang lớp lưu trữ khác sau một khoảng thời gian nhất định (ví dụ: chuyển sang **Standard IA** sau 60 ngày, chuyển sang **Glacier** sau 6 tháng).
  - **Expiration action**: Xóa đối tượng sau một khoảng thời gian (ví dụ: xóa log truy cập sau 365 ngày, xóa phiên bản cũ nếu đã bật **versioning**, hoặc xóa các upload chưa hoàn thành sau 2 tuần).

- Quy tắc có thể được áp dụng cho:
  - Một **prefix** cụ thể trong bucket.
  - Một **path** trong bucket.
  - Các **object tags** cụ thể (ví dụ: chỉ áp dụng cho các đối tượng thuộc phòng ban tài chính).

### Các Kịch Bản Ví Dụ

1. **Kịch bản 1 - Ứng dụng EC2 tạo ảnh thu nhỏ (thumbnails)**:
  - **S3 source images**: Lưu trữ trong **Standard** với quy tắc vòng đời để chuyển sang **Glacier** sau 60 ngày.
  - **Thumbnails images**: Lưu trữ trong **One-Zone IA** và xóa sau 60 ngày vì chúng có thể dễ dàng tái tạo.

2. **Kịch bản 2 - Khôi phục đối tượng bị xóa**:
  - **S3 Versioning**: Bật **versioning** để giữ các phiên bản đối tượng bị xóa, và sử dụng **delete marker** để ẩn chúng. Các đối tượng không phải phiên bản hiện tại sẽ được chuyển sang **Standard IA** và sau đó chuyển sang **Glacier Deep Archive** để lưu trữ.

### S3 Analytics

- **Amazon S3 Analytics** cung cấp các đề xuất cho việc chuyển đổi đối tượng giữa **Standard** và **Standard IA**, nhưng không hỗ trợ cho **One-Zone IA** hoặc **Glacier**.
- **S3 Analytics** tạo ra báo cáo CSV hàng ngày giúp bạn hiểu cách tối ưu hóa quy tắc vòng đời và đưa ra các đề xuất về thời gian chuyển đổi.
- Báo cáo có thể mất từ 24 đến 48 giờ để cập nhật dữ liệu phân tích.


# 144. Requester Pays
- Các bucket owners sẽ trả tiền cho Amazon S3 storage và data-transfer costs liên quan đến các bucket của họ.
- Một requester tải xuống một file từ bucket.
- Networking cost sẽ được tính vào tài khoản của bucket owner.
- Nếu Requester Pays được bật, requester sẽ trả phí cho việc data download thay vì chủ sở hữu bucket.
- Chủ sở hữu vẫn trả storage costs của các đối tượng trong bucket.
- Requester phải được authenticated trong AWS để đảm bảo tính phí chính xác cho networking costs.
- Tính năng này hữu ích khi chia sẻ large data sets với các accounts khác.

# 145. S3 Event Notifications
- Events xảy ra trong Amazon S3 như: object được tạo, xóa, khôi phục, hoặc replication.
- Bạn có thể filter các events theo đối tượng, ví dụ chỉ xem các đối tượng có đuôi JPEG.
- Use case phổ biến: tự động phản hồi các sự kiện trong S3, ví dụ như tạo thumbnails cho tất cả hình ảnh tải lên S3.
- Các destination cho Event Notification: SNS topic, SQS queue, Lambda function.
- Events thường được gửi đến destinations trong vài giây, nhưng có thể lâu hơn.
- Để Event Notifications hoạt động, cần có **IAM policy** attach to target service -> we dont use IAM Role for S3. Instead, we define and attach policy to service
  - SNS: Cần **SNS resource access policy** cho phép S3 gửi message vào SNS topic.
  - SQS: Cần **SQS resource access policy** để S3 gửi message vào SQS queue.
  - Lambda: Cần **Lambda resource policy** để S3 có quyền invoke Lambda function.
  - Amazon EventBridge: Mọi sự kiện S3 sẽ được chuyển đến EventBridge.
    - EventBridge hỗ trợ filter nâng cao (metadata, object size, name).
    - Gửi events tới nhiều destinations (Step Functions, Kinesis, Firehose).
    - Tính năng của EventBridge: lưu trữ, phát lại, và đảm bảo delivery.

# 147. S3 Baseline Performance

- **Amazon S3** tự động mở rộng và có độ trễ rất thấp từ 100-200 ms để lấy byte đầu tiên từ S3.
- Request per second:
  - 3,500 **PUT/COPY/POST/DELETE** requests per second, per prefix.
  - 5,500 **GET/HEAD** requests per second, per prefix.
- **Prefix** là phần đường dẫn giữa bucket và file. Ví dụ: **/folder1/subfolder1** là prefix của một file trong thư mục đó.
  - Với mỗi prefix, có thể thực hiện 3,500 PUT và 5,500 GET requests per second.
  - Nếu chia đều các prefix, tổng số request có thể đạt 22,000 requests per second cho GET/HEAD.

### Optimizing S3 Performance

**Multi-part Upload**
  - Dành cho các tệp trên 100MB và bắt buộc cho tệp trên 5GB.
  - Tệp sẽ được chia thành các phần nhỏ và tải lên song song.
  - Sau khi tất cả các phần đã được tải lên, S3 sẽ tự động ghép lại thành một tệp hoàn chỉnh.

**S3 Transfer Acceleration**
  - Tăng tốc độ tải lên và tải xuống bằng cách chuyển tệp đến **AWS edge location** và sau đó chuyển đến S3 bucket trong vùng đích.
  - **Edge locations** giúp giảm độ trễ bằng cách sử dụng mạng riêng AWS thay vì internet công cộng.
  - Hỗ trợ multi-part upload, giúp tăng tốc độ truyền tệp.

**S3 Byte Range Fetches**
  - GETs song song bằng cách request các byte cụ thể trong tệp. -> tăng tốc độ download
  - Có thể retry các byte range nhỏ hơn trong trường hợp thất bại, tăng độ tin cậy.
  - Usecase: tải một phần của tệp (ví dụ: chỉ tải phần header của tệp).

---

# 148. S3 Batch Operations
**S3 Batch Operations** là một tính năng cho phép bạn thực hiện các thao tác hàng loạt trên nhiều đối tượng S3 chỉ với một yêu cầu duy nhất. Các thao tác phổ biến bao gồm:

- **Chỉnh sửa metadata** và thuộc tính của nhiều đối tượng S3.
- **Sao chép các đối tượng** giữa các bucket S3.
- **Mã hóa các đối tượng chưa được mã hóa** trong bucket S3 (đây là ví dụ thường gặp trong kỳ thi).
- **Chỉnh sửa ACL hoặc thẻ** của nhiều đối tượng cùng lúc.
- **Khôi phục nhiều đối tượng** từ S3 Glacier.
- **Gọi Lambda function** để thực hiện hành động tùy chỉnh trên từng đối tượng trong thao tác batch.

**Cách hoạt động của S3 Batch Operations**:
- Một **job** trong S3 Batch Operations bao gồm:
  - Danh sách các đối tượng.
  - Thao tác cần thực hiện.
  - Tham số tùy chọn cho thao tác.

**Lý do nên sử dụng S3 Batch Operations**:
- **Retry**: tự động
- **Theo dõi tiến độ**: Bạn có thể theo dõi tiến trình của các thao tác.
- **Thông báo hoàn thành**: Gửi thông báo khi quá trình hoàn tất.

**Cách tạo danh sách đối tượng cho S3 Batch Operations**:
- Sử dụng **S3 Inventory** để lấy danh sách đối tượng trong một bucket S3, sau đó sử dụng **Athena** để truy vấn danh sách này và lọc các đối tượng cần thiết cho thao tác batch.


# 149. S3 Storage Lens
**S3 Storage Lens** là một dịch vụ giúp bạn phân tích, tối ưu hóa và hiểu rõ hơn về việc sử dụng bộ nhớ trên toàn bộ AWS Organization của bạn.
- Phát hiện sự bất thường và xác định hiệu quả chi phí.
- **Áp dụng các phương pháp bảo vệ dữ liệu tốt nhất** trên toàn bộ tổ chức AWS của bạn.
- Cung cấp các **chỉ số sử dụng và hoạt động trong 30 ngày**.
- Cho phép bạn tổng hợp dữ liệu theo cấp độ tổ chức, tài khoản, vùng, bucket hoặc thậm chí theo prefix.
- Cung cấp các bảng điều khiển mặc định hoặc tùy chỉnh để hiển thị các báo cáo và chỉ số.
- **Xuất báo cáo** dưới định dạng CSV hoặc Parquet vào một S3 bucket.

### Các loại **metrics** trong S3 Storage Lens:
**Summary Metrics**:
  - Giúp bạn hiểu tổng quan về bộ nhớ S3 của mình, ví dụ như số byte lưu trữ và số lượng đối tượng.

**Cost Optimization Metrics**:
  - Cung cấp thông tin để tối ưu hóa chi phí lưu trữ, chẳng hạn như bao nhiêu không phải là phiên bản mới nhất hoặc bao nhiêu lưu trữ đang bị chiếm dụng bởi các upload chưa hoàn thành.

**Data Protection Metrics**:
  - Cung cấp thông tin về các tính năng bảo vệ dữ liệu, ví dụ như số lượng bucket có bật versioning hoặc MFA delete.

**Access Management Metrics**:
  - Giúp bạn hiểu rõ về quyền sở hữu đối tượng trong S3, giúp xác định các thiết lập quyền sở hữu của bucket.

**Event Metrics**:
  - Cung cấp thông tin về các sự kiện S3 như số lượng bucket có cấu hình thông báo sự kiện.

**Performance Metrics**:
  - Đánh giá hiệu suất của S3, chẳng hạn như bao nhiêu bucket có S3 Transfer Acceleration được bật.

**Activity Metrics**:
  - Thống kê về các yêu cầu đến S3 như số lượng yêu cầu GET, PUT, số byte đã tải xuống, v.v.

**HTTP Status Code Metrics**:
  - Cung cấp thông tin về các mã trạng thái HTTP như 200 OK, 403 Forbidden để hiểu cách sử dụng các bucket của bạn.

**Free Metrics**: Có sẵn cho tất cả khách hàng và bao gồm khoảng 28 chỉ số sử dụng với dữ liệu có sẵn trong 14 ngày.

**Paid Metrics**: Bao gồm các chỉ số chi tiết như hoạt động, tối ưu hóa chi phí nâng cao, bảo vệ dữ liệu nâng cao và trạng thái mã lỗi HTTP. Dữ liệu cho các chỉ số này có sẵn trong 15 tháng và có thể xuất bản vào CloudWatch.

# 150. S3 Object Encryption

### Các phương pháp mã hóa đối tượng trong S3:
### **Server-side encryption (SSE)** 
  - **SSE-S3 (Server-Side Encryption with Amazon S3-managed keys):
    - ** Mã hóa sử dụng khóa do AWS quản lý, sử dụng AES-256 cho mã hóa. 
    - Đây là phương pháp mặc định được bật cho các buckets và objects mới. 
    - Request phải bao gồm header `"x-amz-server-side-encryption": "AES256"`.
  - **SSE-KMS (Server-Side Encryption with AWS Key Management Service):
    - ** Mã hóa sử dụng khóa từ AWS KMS để quản lý khóa mã hóa. 
    - Bạn có thể create và manage khóa thông qua CloudTrail để theo dõi mọi hành động liên quan đến khóa. 
    - Việc sử dụng SSE-KMS yêu cầu quyền truy cập vào KMS key để mã hóa và giải mã object, đồng thời có thể ảnh hưởng đến KMS quotas khi có lưu lượng API cao. 
  - **SSE-C (Server-Side Encryption with Customer-provided keys):
    - ** Mã hóa với customer-provided key. 
    - Khóa được cung cấp từ phía khách hàng và không được AWS lưu trữ. Request phải bao gồm khóa mã hóa trong header HTTP và HTTPS phải được sử dụng để bảo mật quá trình truyền tải.

### **Client-Side Encryption:** 
- Dữ liệu được mã hóa ở phía khách hàng trước khi upload lên S3. 
- Quá trình decryption sẽ diễn ra ở phía khách hàng khi tải về từ S3. 
- Client hoàn toàn kiểm soát việc encryption và key management.

### Encryption in Transit
  - Mã hóa trong quá trình truyền tải (hoặc SSL/TLS) đảm bảo rằng dữ liệu được mã hóa khi truyền từ và đến Amazon S3.
  - S3 expose 2 endpoint: HTTP endpoint (không mã hóa) và HTTPS endpoint (mã hóa). Khuyến nghị sử dụng HTTPS để đảm bảo secure transmission.
  - Nếu sử dụng SSE-C, yêu cầu HTTPS để đảm bảo bảo mật khi truyền tải customer key.
  - Bạn có thể force encryption in transit bằng cách sử dụng bucket policy với điều kiện `"aws:SecureTransport": "false"` để từ chối mọi GetObject request không sử dụng HTTPS.

# 155. S3 CORS

# 156. S3 MFA Delete
Force user generate 1 code trên thiết bị trước khi doing important operation on S3
- Permanently delete một object version
- Suspend Versioning on the bucket

Để sử dụng, bucket bắt buộc đã bật Versioning. 

Chỉ bucket owner (root) có thể enable/disable MFA Delete 

# 162. S3 Glacier Vault Lock and S3 Object Lock


### **S3 Glacier Vault Lock**
S3 Glacier Vault Lock là một tính năng giúp bảo vệ dữ liệu lưu trữ trong Amazon S3 Glacier bằng cách áp dụng mô hình **WORM** (Write Once, Read Many). Mô hình này cho phép bạn ghi dữ liệu vào một đối tượng trong Glacier Vault nhưng không thể chỉnh sửa hay xóa nó sau khi đã lưu trữ.

- **Mục đích**: Bảo vệ dữ liệu khỏi bị thay đổi hoặc xóa bởi bất kỳ ai, kể cả người quản trị hoặc AWS.
- **Cách hoạt động**:
  - Bạn sẽ tạo một **Vault Lock Policy** cho Glacier Vault.
  - Sau khi chính sách này được thiết lập và khóa, không ai có thể thay đổi hoặc xóa nó, điều này rất hữu ích cho yêu cầu **compliance** (tuân thủ quy định) và **data retention** (bảo vệ dữ liệu).
  - Một khi dữ liệu đã được lưu trữ và Vault Lock Policy được áp dụng, dữ liệu sẽ không thể bị xóa hay thay đổi, ngay cả khi có quyền quản trị viên.

### **S3 Object Lock**
S3 Object Lock áp dụng cho các đối tượng trong **S3 Buckets** và có tính linh hoạt cao hơn.

- **Yêu cầu**: Trước khi bật S3 Object Lock, bạn cần bật **versioning** cho S3 Bucket.
- **Mô hình**: Cũng áp dụng mô hình **WORM** nhưng thay vì khóa toàn bộ bucket, bạn có thể khóa từng đối tượng riêng biệt.
- **Chế độ bảo vệ**:
  - **Compliance Mode**:
    - Giống như Glacier Vault Lock, trong chế độ này, các phiên bản đối tượng không thể bị thay đổi hoặc xóa, kể cả người dùng root.
    - Thời gian lưu trữ không thể bị rút ngắn hay thay đổi, điều này giúp đảm bảo tuân thủ các quy định nghiêm ngặt.
  - **Governance Mode**:
    - Cho phép người dùng có quyền quản trị (admin) thay đổi thời gian lưu trữ hoặc xóa đối tượng, nhưng hầu hết người dùng không thể thay đổi hoặc xóa dữ liệu.
    - Linh hoạt hơn Compliance Mode nhưng vẫn đảm bảo bảo vệ dữ liệu.

- **Retention Period**: Bạn có thể đặt một thời gian lưu trữ cố định cho đối tượng, và thời gian này có thể kéo dài nếu cần thiết.

- **Legal Hold**:
  - Đây là một tính năng đặc biệt, bảo vệ đối tượng vô thời hạn, không phụ thuộc vào thời gian lưu trữ hoặc chế độ retention đã thiết lập.
  - Khi áp dụng **Legal Hold**, đối tượng sẽ được bảo vệ cho tới khi bạn gỡ bỏ hold này. Tính năng này đặc biệt hữu ích khi đối tượng có liên quan đến các vấn đề pháp lý, ví dụ như vụ kiện.

- **Quyền IAM**: Để sử dụng Legal Hold, người dùng phải có quyền **S3 PutObjectLegalHold** để đặt hoặc gỡ bỏ Legal Hold trên các đối tượng.


# 163. S3 Access Point
S3 Access Points là một cách để quản lý truy cập dữ liệu trong **S3 Buckets** dễ dàng và hiệu quả hơn, đặc biệt khi bạn có nhiều dữ liệu và nhiều nhóm người dùng cần truy cập vào các phần dữ liệu khác nhau trong cùng một bucket.

- Nếu một S3 bucket có quá nhiều dữ liệu (ví dụ: dữ liệu tài chính, dữ liệu bán hàng) và nhiều người dùng hoặc nhóm người dùng cần truy cập vào các phần khác nhau của dữ liệu, việc tạo các **bucket policy** phức tạp có thể trở nên khó quản lý khi số lượng người dùng và dữ liệu tăng lên.

- S3 Access Points cho phép bạn tạo các điểm truy cập (access points) riêng biệt cho từng phần dữ liệu. Mỗi access point có thể có một chính sách truy cập riêng (tương tự như bucket policy).

#### Cách hoạt động của S3 Access Points**
- Bạn có thể tạo các **access points** cho từng nhóm dữ liệu cụ thể, chẳng hạn:
  - **Finance Access Point**: Truy cập dữ liệu tài chính. Bạn sẽ gán một chính sách truy cập cho point này, cho phép đọc và ghi vào phần dữ liệu tài chính.
  - **Sales Access Point**: Truy cập dữ liệu bán hàng với chính sách truy cập riêng biệt.
  - **Analytics Access Point**: Truy cập dữ liệu tài chính và bán hàng nhưng chỉ với quyền đọc.

- **Quản lý bảo mật**: Thay vì phải quản lý bảo mật ở cấp độ bucket, bạn sẽ quản lý bảo mật ở từng **access point**. Mỗi access point có một **Access Point Policy** riêng và có thể cấp quyền truy cập cho các nhóm người dùng khác nhau.

- **Quyền IAM**: Người dùng có quyền truy cập sẽ chỉ có thể kết nối với các access point tương ứng và truy cập vào phần dữ liệu phù hợp (ví dụ: người dùng tài chính chỉ truy cập vào tài chính, người dùng phân tích có thể truy cập vào cả tài chính và bán hàng).

#### **Quản lý bảo mật đơn giản hơn**
- **Tăng tính mở rộng**: S3 Access Points giúp bạn mở rộng việc quản lý bảo mật và truy cập cho các bucket một cách dễ dàng. Bạn không cần phải tạo một bucket policy phức tạp mà có thể tạo nhiều policy cho từng access point.
- **DNS Name riêng**: Mỗi access point có một tên DNS riêng, bạn có thể sử dụng nó để kết nối với access point.

#### **Kết nối qua Internet hoặc VPC**
- **Kết nối qua Internet**: S3 Access Points có thể được cấu hình để cho phép truy cập công khai từ Internet.
- **Kết nối qua VPC**: Bạn cũng có thể cấu hình để truy cập vào S3 Access Points qua một **VPC** (Virtual Private Cloud), giúp đảm bảo rằng lưu lượng truy cập không đi qua Internet, mà đi qua mạng riêng tư.

#### **VPC Endpoint**
- **VPC Endpoint**: Để truy cập S3 Access Points qua VPC, bạn cần tạo một **VPC Endpoint**, cho phép kết nối vào access point mà không cần ra ngoài Internet.
- **VPC Endpoint Policy**: Bạn cần định nghĩa một chính sách cho VPC Endpoint, cho phép EC2 instance trong VPC kết nối với S3 bucket và access points.

# 164. S3 Object Lambda
#### 1. **S3 Object Lambda là gì?**
S3 Object Lambda cho phép bạn **thay đổi dữ liệu** trong một đối tượng khi nó được truy xuất từ S3, mà không cần phải tạo nhiều phiên bản của đối tượng trong các bucket khác nhau. Điều này hữu ích khi bạn muốn xử lý hoặc biến đổi dữ liệu ngay trước khi nó được truy cập, thay vì phải tạo các bản sao khác nhau của cùng một đối tượng.

#### 2. **Cách hoạt động của S3 Object Lambda**
Giả sử bạn có một **S3 bucket** chứa các đối tượng, ví dụ như dữ liệu của một ứng dụng **E-commerce**. Ứng dụng này có thể truy cập trực tiếp vào bucket và lấy các đối tượng gốc.

- **Trường hợp cần biến đổi dữ liệu**: Một ứng dụng **analytics** có thể cần truy cập các đối tượng đã **xóa thông tin nhạy cảm (redacted)**, ví dụ như thông tin cá nhân. Thay vì tạo một bucket mới chứa các đối tượng đã chỉnh sửa, bạn có thể sử dụng **S3 Object Lambda**.

- **Quy trình hoạt động**:
  1. Tạo một **S3 Access Point** cho bucket.
  2. Kết nối Access Point này với một **Lambda function**. Lambda function sẽ xử lý đối tượng khi nó được truy xuất.
  3. Ứng dụng analytics sẽ truy cập vào **S3 Object Lambda Access Point**, kích hoạt Lambda function để chỉnh sửa dữ liệu và trả về đối tượng đã được biến đổi.

#### 3. **Các tình huống sử dụng S3 Object Lambda**
- **Redact dữ liệu (Xóa dữ liệu nhạy cảm)**: Ví dụ, bạn có thể xóa thông tin cá nhân (PII) từ dữ liệu khi nó được sử dụng cho mục đích phân tích hoặc trong các môi trường không phải sản xuất.

- **Enrich dữ liệu (Làm phong phú dữ liệu)**: Ví dụ, một ứng dụng marketing có thể cần truy cập các đối tượng đã được **làm phong phú** bằng dữ liệu từ cơ sở dữ liệu khách hàng (customer loyalty database) mà không cần phải tạo một bucket mới với các đối tượng đã làm phong phú.

- **Chuyển đổi dữ liệu**: Ví dụ như chuyển đổi định dạng dữ liệu từ **XML sang JSON** hoặc thay đổi cấu trúc của dữ liệu khi lấy ra từ S3.

- **Chỉnh sửa hình ảnh (on-the-fly)**: Ví dụ như thay đổi kích thước ảnh hoặc **thêm watermark** tùy chỉnh vào ảnh mỗi khi người dùng yêu cầu (chẳng hạn watermark có thể khác nhau cho từng người dùng).

#### 4. **Lợi ích của S3 Object Lambda**
- **Tiết kiệm tài nguyên**: Bạn không cần phải tạo nhiều bucket hoặc nhiều phiên bản của đối tượng cho các trường hợp khác nhau. S3 Object Lambda cho phép bạn xử lý dữ liệu trực tiếp khi truy xuất.
- **Linh hoạt**: Bạn có thể thực hiện nhiều loại xử lý hoặc thay đổi trên đối tượng mà không cần thay đổi cấu trúc lưu trữ dữ liệu trong bucket.

#### 5. **Tóm tắt**
- **S3 Object Lambda** cho phép bạn **thay đổi dữ liệu** trong một đối tượng khi nó được truy xuất từ S3 mà không cần phải tạo các bản sao mới.
- Các use case phổ biến bao gồm **redact dữ liệu** (xóa thông tin nhạy cảm), **enrich dữ liệu** (làm phong phú dữ liệu), và **chuyển đổi dữ liệu** (ví dụ: XML sang JSON, thay đổi kích thước ảnh).
- **Lambda function** được sử dụng để xử lý đối tượng khi người dùng truy cập thông qua **S3 Object Lambda Access Point**.

