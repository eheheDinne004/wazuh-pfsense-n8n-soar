## Tổng Quan Hệ Thống Giám Sát & Phản Ứng An Ninh Mạng Không Dây

Hệ thống được xây dựng nhằm giải quyết bài toán giám sát lưu lượng và tự động hóa phản ứng trước các nguy cơ an ninh trong mạng không dây (Wireless Network). Sự kết hợp giữa **Wazuh SIEM**, **pfSense Firewall** và **n8n SOAR** tạo nên một cơ chế phòng thủ đa lớp, cho phép phát hiện và ngăn chặn các hành vi truy cập trái phép hoặc tấn công mạng theo thời gian thực.

### Vai Trò Các Thành Phần Trong Mạng Không Dây

* **pfSense (Wireless Gateway & Firewall):** Đóng vai trò là Trạm quản lý truy cập (Gateway/Captive Portal) và cấp phát IP (DHCP) cho các thiết bị không dây. pfSense chịu trách nhiệm ghi nhận toàn bộ nhật ký kết nối, lưu lượng gói tin và thực thi các chính sách chặn (Block) truy cập.

* **Wazuh SIEM (Phân Tích & Phát Hiện):** Thu thập và phân tích dữ liệu log từ pfSense. Nhờ tập luật tùy chỉnh (`local_rules.xml`), Wazuh có khả năng nhận diện sớm các hình thức tấn công phổ biến trong môi trường không dây như:
  * Dò quét mạng (`Nmap`)
  * Tấn công dò mật khẩu (`Brute Force`)
  * Thiết bị lạ gắn trái phép vào mạng (`Rogue Devices / Unknown MAC`)

* **n8n (Tự Động Hóa Phản Ứng - SOAR):** Đóng vai trò trung tâm điều phối. Khi nhận cảnh báo từ Wazuh qua Webhook, n8n tự động phân loại, gửi thông báo khẩn cấp tới Telegram và thực thi lệnh kết nối SSH tới pfSense để cách ly (Block) địa chỉ IP của kẻ tấn công ngay lập tức.

---

### Luồng Hoạt Động Xử Lý Sự Cố

<img width="599" height="317" alt="image" src="https://github.com/user-attachments/assets/1e3b226f-99ea-40c6-a3fe-779950effcf0" />

