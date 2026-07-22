## Tổng Quan Hệ Thống Giám Sát & Phản Ứng An Ninh Mạng Không Dây

  Hệ thống được thiết kế để giải quyết bài toán giám sát lưu lượng và tự động hóa phản ứng trước các nguy cơ an ninh trong môi trường mạng không dây (Wireless Network). 

  Bằng cách tích hợp Wazuh SIEM, pfSense Firewall và nền tảng n8n SOAR, dự án tạo ra một kiến trúc phòng thủ đa lớp toàn diện—chuyển đổi từ mô hình giám sát thụ động sang khả năng phát hiện, phân tích và ngăn chặn các mũi tấn công theo thời gian thực.

---

### Vai Trò Các Thành Phần Cốt Lõi

- **pfSense (Wireless Gateway & Firewall)**
  Đóng vai trò là "người gác cổng" (Gateway/Captive Portal) và trung tâm cấp phát cấu hình (DHCP) cho các thiết bị kết nối không dây. Trách nhiệm chính của pfSense là ghi nhận chi tiết nhật ký lưu lượng mạng (Syslog) và trực tiếp thực thi các lệnh cách ly (Block) ở tầng mạng.

- **Wazuh SIEM (Phân Tích & Nhận Diện Đe Dọa)**
  Hoạt động như trung tâm phân tích tập trung. Wazuh thu thập log từ pfSense, sau đó đối chiếu với các tập luật phát hiện xâm nhập tùy chỉnh (`local_rules.xml`) để nhận diện sớm các hình thức tấn công không dây phổ biến:
  * Rà quét lỗ hổng và cổng mạng (`Nmap`)
  * Tấn công bạo lực dò tìm mật khẩu (`Brute Force`)
  * Thiết bị trái phép/lạ cố gắng truy cập hệ thống (`Rogue Devices / Unknown MAC`)

- **n8n (Tự Động Hóa Phản Ứng - SOAR Engine)**
  Hoạt động như "trung tâm điều phối chiến dịch". Ngay khi nhận được sự kiện vi phạm an ninh từ Wazuh thông qua Webhook, n8n lập tức kích hoạt chuỗi hành động tự động khép kín: phân loại mức độ nghiêm trọng, phát thông báo khẩn cấp (Telegram) và kết nối SSH ngược về pfSense để chặn đứng (Block) địa chỉ IP độc hại ngay lập tức.

---

### Luồng Hoạt Động Xử Lý Sự Cố (Workflow)

> *Sơ đồ dưới đây minh họa luồng dữ liệu khép kín: từ bước thiết bị đầu cuối tương tác với mạng, tiến trình phân tích log, cho đến khi hệ thống tự động đưa ra quyết định ngăn chặn và cảnh báo.*

<div align="center">
  <img width="700" alt="Sơ đồ kiến trúc luồng xử lý phản ứng an ninh mạng" src="https://github.com/user-attachments/assets/1e3b226f-99ea-40c6-a3fe-779950effcf0" />
</div>
