# Hiệu suất hệ thống với Apache JMeter

## 1. Cài đặt công cụ
Tải và cài đặt Apache JMeter từ trang chủ: [JMeter Official Site](https://jmeter.apache.org/)

## 2. Thiết lập kịch bản kiểm thử hiệu suất
- Trang web kiểm thử: **[the-internet.herokuapp.com](https://the-internet.herokuapp.com/)**
- Mô phỏng **50 người dùng đồng thời** truy cập trang web trong **5 phút**.
- Gửi **yêu cầu HTTP** đến trang chủ của website.
- Đo thời gian phản hồi và xác định bottleneck nếu có.

### **Các bước thiết lập trong JMeter**
1. **Tạo Test Plan** trong JMeter.
2. **Thêm Thread Group** và cấu hình:
   - Number of Threads (Users): **50**
   - Ramp-up Period: **60** giây
   - Loop Count: **Forever** hoặc đặt thời gian **5 phút**
3. **Thêm HTTP Request**:
   - Server Name: `the-internet.herokuapp.com`
   - Method: `GET`
   - Path: `/`
4. **Thêm HTTP Header Manager**:
   - Key: `Content-Type`
   - Value: `Content-Type	application/x-www-form-urlencoded`
5. **Thêm Listener**:
   - View Results Tree
   - Summary Report
   - Aggregate Report
   - Graph Results

## 3. Thực hiện kiểm thử tải (Load Testing)
- Chạy kiểm thử với số lượng người dùng tăng dần từ **10 → 50**.
- Ghi nhận **thời gian phản hồi trung bình** và **tỷ lệ lỗi (Error Rate)**.

## 4. Kiểm thử khả năng chịu tải (Stress Testing)
- Tiếp tục tăng số người dùng lên **100** để xác định giới hạn chịu tải của hệ thống.
- Quan sát thời điểm hệ thống bắt đầu **giảm hiệu suất đáng kể**.

## 5. Phân tích kết quả
### **Kết quả kiểm thử hiệu suất**
#### **Số lượng mẫu (Samples)**
- HTTP Request: **1,129** mẫu  
- Thread Group:HTTP Request: **18,527** mẫu  
- Tổng cộng: **19,656** mẫu  
→ Hệ thống đã xử lý gần **20,000 yêu cầu**, cho thấy mức độ tải thử nghiệm tương đối lớn.

#### **Thời gian phản hồi (Response Time - Average, Min, Max)**
- **Trung bình (Average)**: ~563ms  
- **Nhỏ nhất (Min)**: 27ms  
- **Lớn nhất (Max)**: 1,112ms  
→ Hệ thống có thời gian phản hồi trung bình khoảng **563ms**, khá ổn định. Tuy nhiên, thời gian phản hồi cao nhất lên đến **1,112ms**, điều này có thể cho thấy hệ thống có một số điểm nghẽn hoặc bị quá tải tại một số thời điểm.

#### **Độ lệch chuẩn (Standard Deviation - Std. Dev.)**
- **Khoảng 47 - 66ms**  
→ Độ lệch chuẩn thấp cho thấy hệ thống phản hồi tương đối ổn định, không có sự biến động lớn giữa các yêu cầu.

#### **Tỷ lệ lỗi (Error Rate)**
- Mức lỗi dao động từ **0.27% - 0.35%**  
→ Tỷ lệ lỗi thấp, cho thấy hệ thống xử lý yêu cầu khá tốt mà không gặp quá nhiều vấn đề.

#### **Thông lượng (Throughput)**
- **84.52 requests/sec** (trung bình tổng)
- **88.43 requests/sec** cho nhóm Thread Group
- **47.81 requests/sec** cho HTTP Request riêng lẻ
→ Đây là thông số quan trọng, cho thấy hệ thống có thể xử lý khoảng **84 - 88 yêu cầu mỗi giây**, một con số khá tốt tùy thuộc vào mục tiêu hiệu suất của bạn.

#### **Dữ liệu truyền tải**
- **Received KB/sec**: 426.18 KB/s  
- **Sent KB/sec**: 36.49 KB/s  
→ Hệ thống có thể tiếp nhận **~426 KB/s** dữ liệu từ server và gửi **~36 KB/s** dữ liệu, phản ánh lưu lượng mạng của hệ thống.

### **Kết luận về hiệu suất hệ thống**
✅ **Hệ thống hoạt động ổn định** với mức tải từ 10 đến 50 user.
✅ **Thời gian phản hồi trung bình tốt (~563ms), độ lệch chuẩn thấp**, cho thấy tính nhất quán trong phản hồi.
✅ **Tỷ lệ lỗi thấp (<0.35%)**, cho thấy hệ thống xử lý yêu cầu ổn định.
✅ **Thông lượng cao (~84-88 request/sec)**, chứng tỏ hệ thống có khả năng xử lý số lượng lớn yêu cầu đồng thời.
⚠️ **Thời gian phản hồi tối đa khá cao (1,112ms)**, có thể cần tối ưu thêm để giảm độ trễ cao nhất.


## 6. Câu hỏi thảo luận
1. **Tại sao kiểm thử phi chức năng lại quan trọng trong phần mềm?**  
   - Kiểm thử phi chức năng giúp đảm bảo hệ thống có thể hoạt động ổn định, đáp ứng yêu cầu về hiệu suất, bảo mật và khả năng mở rộng.
2. **Các thông số quan trọng cần theo dõi trong kiểm thử hiệu suất là gì?**  
   - Thời gian phản hồi (Response Time), Thông lượng (Throughput), Tỷ lệ lỗi (Error Rate), Độ lệch chuẩn (Standard Deviation).
3. **Nếu hệ thống không đáp ứng yêu cầu hiệu suất, bạn sẽ đề xuất giải pháp gì?**  
   - Cải thiện phần cứng (CPU, RAM, Network).
   - Tối ưu hóa truy vấn Database.
   - Sử dụng Load Balancer hoặc Scale hệ thống theo chiều ngang.
   - Triển khai caching để giảm tải cho server.


