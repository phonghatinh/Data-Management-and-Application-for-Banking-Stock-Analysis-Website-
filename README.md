# Bảng Điều Khiển Tài Chính và Chứng Khoán Việt Nam

Ứng dụng web hiển thị dữ liệu chứng khoán trực tuyến với máy chủ FastAPI và giao diện tương tác cao


### Điểm Nổi Bật

- Hiệu suất cao: nhanh hơn 93-94% với bộ nhớ đệm Redis
- Thiết kế đáp ứng: ưu tiên thiết bị di động với chế độ tối
- Biểu đồ tương tác: nến Plotly.js và dữ liệu trực tuyến
- Cập nhật trực tuyến: WebSocket cho dữ liệu sống
- Ngăn xếp hiện đại: FastAPI + Jinja2 + Redis + Plotly
- Ứng dụng một trang: điều hướng không tải lại trang

## Kiến Trúc Hệ Thống

### Tổng Quan Ngăn Xếp Công Nghệ

Hệ thống được chia thành 4 lớp chính:

**Lớp Giao Diện**: HTML/CSS/JavaScript, Biểu đồ Plotly.js, Giao diện đáp ứng
**Lớp Giao Diện Lập Trình Ứng Dụng Máy Chủ**: Khung FastAPI, Bất đồng bộ/Chờ đợi, Mẫu Jinja2  
**Lớp Bộ Nhớ Đệm**: Bộ nhớ đệm Redis với Chiến lược Thời gian sống
**Nguồn Dữ Liệu**: Giao diện lập trình ứng dụng VNStock và Dữ liệu Tài chính

### Kiến Trúc Hiệu Suất

| Lớp | Công Nghệ | Thời Gian Phản Hồi | Chiến Lược Bộ Nhớ Đệm |
|-------|------------|---------------|----------------|
| Giao diện | HTML/CSS/JavaScript, Plotly.js | dưới 100 mili giây | Bộ nhớ đệm trình duyệt, tải lười |
| Lớp Giao Diện Lập Trình | FastAPI, Bất đồng bộ/Chờ đợi | 150-200 mili giây | Bộ nhớ đệm Redis lớp 2 |
| Nguồn Dữ Liệu | Giao diện lập trình ứng dụng VNStock | 2-3 giây (không có bộ nhớ đệm) | Thời gian sống 30 phút - 2 giờ |

## Cấu Trúc Dự Án

```
CSDL_FIXV1_backup/
├── main.py                    # Điểm khởi đầu ứng dụng FastAPI
├── requirements.txt           # Các gói phụ thuộc Python
├── app/                       # Logic máy chủ
│   ├── cache_manager.py       # Triển khai bộ nhớ đệm Redis
│   ├── controllers/           # Xử lý tuyến đường FastAPI
│   ├── services/              # Logic nghiệp vụ và xử lý dữ liệu
│   ├── models/                # Mô hình dữ liệu và lược đồ
│   └── config.py              # Cấu hình ứng dụng
├── templates/                 # Mẫu HTML Jinja2
│   ├── base.html              # Mẫu cơ sở với bố cục chung
│   ├── priceboard.html        # Bảng giá trực tuyến
│   ├── information.html       # Báo cáo tài chính
│   ├── analytics.html         # Phân tích PowerBI
│   ├── report.html            # Báo cáo và đánh giá
│   └── stock.html             # Chi tiết cổ phiếu với biểu đồ
└── static/                    # Tài sản giao diện
    ├── css/                   # Bảng kiểu đáp ứng
    │   ├── style.css          # Kiểu toàn cục với biến CSS
    │   ├── stock.css          # Kiểu riêng cho trang cổ phiếu
    │   ├── priceboard.css     # Kiểu bảng điều khiển với chế độ tối
    │   └── bieu_do_tron.css   # Kiểu riêng cho biểu đồ
    └── js/                    # Các mô-đun JavaScript
        ├── main.js            # Chức năng cốt lõi
        ├── stock.js           # Tương tác trang cổ phiếu
        └── charts.js          # Cấu hình biểu đồ
```

## Hướng Dẫn Cài Đặt và Chạy Ứng Dụng

### Bước 1: Chuẩn Bị Môi Trường

1. Sao chép kho lưu trữ và di chuyển vào thư mục dự án
2. Đảm bảo Python 3.8+ đã được cài đặt
3. Cài đặt máy chủ Redis (tùy chọn, có thể chạy mà không cần Redis)

### Bước 2: Cài Đặt Các Gói Phụ Thuộc

Cài đặt các gói Python cần thiết:
```
pip install -r requirements.txt
```

### Bước 3: Khởi Chạy Máy Chủ

Chạy máy chủ phát triển với tải lại nóng:
```
python main.py
```

Hoặc sử dụng uvicorn:
```
uvicorn main:app --reload --port 8000
```

### Bước 4: Truy Cập Ứng Dụng

Máy chủ sẽ chạy trên địa chỉ: http://localhost:8000

## Các Trang Chính của Ứng Dụng

### Trang Chủ (Bảng Giá)
- **Địa chỉ**: http://localhost:8000/ hoặc http://localhost:8000/priceboard
- **Mô tả**: Hiển thị tổng quan thị trường chứng khoán
- **Nội dung**: Chỉ số thị trường, Sơ đồ cây vốn hóa, Biểu đồ tròn tài chính, Bảng giá trực tuyến, Tin tức

### Trang Chi Tiết Cổ Phiếu  
- **Địa chỉ**: http://localhost:8000/stock?bank_code=VCB
- **Mô tả**: Thông tin chi tiết về từng mã cổ phiếu
- **Nội dung**: Hồ sơ công ty, Biểu đồ giá, Cơ cấu cổ đông, Ban lãnh đạo

### Trang Báo Cáo Tài Chính
- **Địa chỉ**: http://localhost:8000/information  
- **Mô tả**: Biểu mẫu lựa chọn mã cổ phiếu và loại báo cáo
- **Nội dung**: Hiển thị dữ liệu dạng bảng với khả năng xuất Excel

### Trang Báo Cáo và Đánh Giá
- **Địa chỉ**: http://localhost:8000/report
- **Mô tả**: Tích hợp khung nội tuyến PowerBI hiển thị báo cáo chỉ số hiệu suất chính
- **Nội dung**: Bảng điều khiển chỉ số hiệu suất chính và các chỉ số đánh giá

### Trang Phân Tích PowerBI
- **Địa chỉ**: http://localhost:8000/analytics
- **Mô tả**: Tích hợp khung nội tuyến PowerBI cho phân tích dữ liệu tài chính  
- **Nội dung**: Phân tích chuyên sâu và trí tuệ kinh doanh

### Tài Liệu Giao Diện Lập Trình Ứng Dụng
- **Địa chỉ**: http://localhost:8000/docs
- **Mô tả**: Giao diện người dùng Swagger tự động sinh từ FastAPI
- **Nội dung**: Tài liệu giao diện lập trình ứng dụng tương tác

## Kiến Trúc Ứng Dụng Một Trang

### Cách Thức Hoạt Động

Ứng dụng đã được nâng cấp để sử dụng kiến trúc ứng dụng một trang nhằm cải thiện trải nghiệm người dùng:

1. **Chặn Điều Hướng**: Khi nhấp vào thanh điều hướng, JavaScript sẽ chặn điều hướng mặc định
2. **Yêu Cầu Bất Đồng Bộ**: Gửi yêu cầu bất đồng bộ tới điểm cuối giao diện lập trình ứng dụng tương ứng
3. **Nội Dung Một Phần**: Nhận về nội dung HTML một phần thay vì toàn bộ trang
4. **Cập Nhật Mô Hình Đối Tượng Tài Liệu**: Cập nhật chỉ phần nội dung chính (id="main") 
5. **Quản Lý Lịch Sử**: Quản lý lịch sử trình duyệt để hỗ trợ nút quay lại/tiến tới

### Lợi Ích của Ứng Dụng Một Trang

- **Tốc độ**: Chỉ tải nội dung cần thiết, không tải lại đầu trang/thanh bên
- **Trải nghiệm**: Điều hướng mượt mà như ứng dụng gốc
- **Băng thông**: Tiết kiệm băng thông do không tải lại tài sản
- **Thân thiện với Tối Ưu Hóa Công Cụ Tìm Kiếm**: Vẫn hỗ trợ truy cập trực tiếp qua địa chỉ

### Hệ Thống Mẫu

Sử dụng Jinja2 với cấu trúc kế thừa và mẫu một phần:

- **base.html**: Mẫu chính chứa bố cục, đầu trang, thanh bên
- **Mẫu trang**: Mở rộng base.html và bao gồm nội dung một phần  
- **Mẫu một phần**: Chứa nội dung chính của từng trang cho ứng dụng một trang

## Kiến Trúc Giao Diện và Thành Phần Giao Diện Người Dùng

### Hệ Thống Mẫu - Kế Thừa Jinja2

Hệ thống mẫu được tổ chức theo cấu trúc kế thừa với base.html là mẫu chính chứa bố cục chung. Các mẫu con mở rộng base.html và ghi đè các khối cụ thể cho từng trang.

### Kiến Trúc CSS - Hệ Thống Thiết Kế

**Biến CSS (Mã Thông Báo Thiết Kế)**: Quản lý màu sắc, kiểu chữ, khoảng cách và bố cục thống nhất

**Hỗ Trợ Chế Độ Tối**: Thông qua biến CSS với chế độ sáng làm mặc định

**Thiết Kế Đáp Ứng**: Phương pháp ưu tiên thiết bị di động với các điểm ngắt được tối ưu

### Các Mô-đun JavaScript và Tích Hợp Biểu Đồ

**Tích Hợp Plotly.js**: Biểu đồ nến tương tác với thu phóng, di chuyển, công cụ vẽ

**WebSocket Trực Tuyến**: Cập nhật dữ liệu chứng khoán trực tuyến với tự động kết nối lại

**Khởi Tạo Thành Phần**: Khởi tạo lại cho từng trang khi điều hướng ứng dụng một trang

## Luồng Dữ Liệu và Tích Hợp Giao Diện Lập Trình Ứng Dụng

### Luồng Dữ Liệu Giao Diện

Tương Tác Người Dùng → Sự Kiện JavaScript → Gọi Giao Diện Lập Trình → Điểm Cuối FastAPI → Kiểm Tra Bộ Nhớ Đệm → Trả Về Dữ Liệu → Cập Nhật Mô Hình Đối Tượng Tài Liệu → Vẽ Biểu Đồ → Cập Nhật Giao Diện Người Dùng

### Chiến Lược Bộ Nhớ Đệm

**Bộ Nhớ Đệm Redis**: Chiến lược thời gian sống khác nhau cho từng loại dữ liệu

**Bộ Nhớ Đệm Trình Duyệt**: Tài sản tĩnh và tùy chọn người dùng

**Lưu Trữ Cục Bộ**: Cài đặt người dùng và tùy chọn chủ đề

## Tối Ưu Hóa Hiệu Suất

### Các Chỉ Số Hiệu Suất Hiện Tại

| Chỉ Số | Máy Tính Để Bàn | Di Động | Mục Tiêu | Trạng Thái |
|--------|---------|--------|--------|--------|
| Vẽ Nội Dung Đầu Tiên | 1.2 giây | 2.1 giây | dưới 2.5 giây | Đạt |
| Vẽ Nội Dung Lớn Nhất | 1.8 giây | 2.8 giây | dưới 3.0 giây | Đạt |
| Thay Đổi Bố Cục Tích Lũy | 0.05 | 0.08 | dưới 0.1 | Đạt |
| Thời Gian Tương Tác | 2.1 giây | 3.2 giây | dưới 3.5 giây | Đạt |

### Kỹ Thuật Tối Ưu Hóa

**Tối Ưu Hóa Tài Sản**: Tải trước, tải lười, và nén

**Chia Tách Mã**: Nhập động cho các thành phần nặng

**Tải Lười**: Quan sát giao điểm cho biểu đồ và hình ảnh

**Chiến Lược Bộ Nhớ Đệm**: Bộ nhớ đệm đa lớp từ trình duyệt đến Redis

## Thiết Kế Đáp Ứng và Tối Ưu Hóa Di Động

### Chiến Lược Điểm Ngắt

**Phương Pháp Ưu Tiên Di Động** với các điểm ngắt:
- Kiểu cơ sở: Di động (320px+)
- Máy tính bảng: 768px+ 
- Máy tính để bàn: 1024px+

### Giao Diện Người Dùng Thân Thiện với Cảm Ứng

**Mục Tiêu Cảm Ứng**: Tối thiểu 44px cho tương tác di động

**Cuộn Mượt**: Được tối ưu cho thiết bị di động

**Bảng Đáp Ứng**: Cuộn ngang với chỉ báo cảm ứng

## Xử Lý Lỗi và Trải Nghiệm Người Dùng

### Xử Lý Lỗi Giao Diện Lập Trình Ứng Dụng

**Trình Xử Lý Lỗi Toàn Cục**: Xử lý tất cả cuộc gọi giao diện lập trình với cơ chế dự phòng

**Thông Báo Thân Thiện với Người Dùng**: Thông báo lỗi bằng tiếng Việt

**Suy Giảm Duyên Dáng**: Dự phòng tải lại toàn trang khi ứng dụng một trang thất bại

### Trạng Thái Tải

**Chỉ Báo Tải**: Vòng quay và màn hình khung xương

**Tải Tiến Bộ**: Hiển thị nội dung theo từng phần

**Khôi Phục Lỗi**: Cơ chế thử lại và tùy chọn làm mới thủ công

## Các Tính Năng Chính

### Cập Nhật Dữ Liệu Trực Tuyến

**Kết Nối WebSocket**: Cập nhật dữ liệu chứng khoán trực tuyến

**Kết Nối Lại Tự Động**: Xử lý ngắt kết nối

**Đồng Bộ Dữ Liệu**: Đồng bộ dữ liệu giữa các thành phần

### Biểu Đồ Tương Tác

**Biểu Đồ Plotly.js**: Nến với công cụ nâng cao

**Tích Hợp Chart.js**: Biểu đồ đơn giản và trực quan hóa dữ liệu

**Biểu Đồ Đáp Ứng**: Tự động thay đổi kích thước theo màn hình

### Tích Hợp Dữ Liệu Tài Chính

**Giao Diện Lập Trình Ứng Dụng VNStock**: Dữ liệu chứng khoán Việt Nam

**Báo Cáo Tài Chính**: Báo cáo tài chính chi tiết

**Thông Tin Công Ty**: Hồ sơ công ty và ban lãnh đạo

### Tích Hợp PowerBI

**Khung Nội Tuyến Nhúng**: Báo cáo PowerBI trong ứng dụng

**Bảng Điều Khiển Chỉ Số Hiệu Suất Chính**: Các chỉ số đánh giá quan trọng

**Trí Tuệ Kinh Doanh**: Phân tích nâng cao và thông tin chi tiết

## Hỗ Trợ Trình Duyệt

Hỗ trợ các trình duyệt hiện đại:

- Chrome 90+
- Firefox 88+  
- Safari 14+
- Edge 90+
- iOS Safari 14+
- Chrome Mobile 90+

## Khắc Phục Sự Cố

### Các Lỗi Thường Gặp

**Biểu đồ không hiển thị**
- Kiểm tra Plotly.js đã tải
- Xác minh định dạng dữ liệu đúng
- Kiểm tra lỗi bảng điều khiển

**Cuộc gọi giao diện lập trình bị hết thời gian chờ**  
- Kiểm tra máy chủ đang chạy
- Xác minh kết nối mạng
- Kiểm tra kết nối Redis

**CSS không tải đúng**
- Xóa bộ nhớ đệm trình duyệt
- Kiểm tra đường dẫn tập tin
- Xác minh phục vụ tập tin tĩnh

**Vấn đề kết nối WebSocket**
- Kiểm tra điểm cuối WebSocket máy chủ
- Kiểm tra cài đặt tường lửa
- Xác minh khả năng truy cập cổng

### Vấn đề Hiệu Suất

**Tải trang chậm**
- Kiểm tra trạng thái bộ nhớ đệm
- Tối ưu kích thước hình ảnh  
- Giảm gói JavaScript

**Rò rỉ bộ nhớ**
- Dọn dẹp trình nghe sự kiện
- Đóng kết nối WebSocket
- Xóa các thể hiện biểu đồ không sử dụng

**Hiệu suất di động**
- Tối ưu tương tác cảm ứng
- Giảm độ phức tạp hoạt ảnh
- Nén tài sản

## Kế Hoạch Phát Triển Tương Lai

### Ngắn Hạn (1-2 tháng)

**Ứng Dụng Web Tiến Bộ**: Khả năng ngoại tuyến và trải nghiệm giống ứng dụng

**Thư Viện Thành Phần**: Thành phần giao diện người dùng có thể tái sử dụng cho tính nhất quán

**Chuyển Đổi TypeScript**: Độ an toàn kiểu tốt hơn và trải nghiệm nhà phát triển

**Đường Ống Xây Dựng**: Công cụ hiện đại với Webpack/Vite

### Trung Hạn (3-6 tháng)

**Khung Hiện Đại**: Chuyển đổi sang React/Vue cho khả năng bảo trì tốt hơn

**Thông Báo Thời Gian Thực**: Hệ thống thông báo dựa trên WebSocket

**Biểu Đồ Nâng Cao**: Tích hợp biểu đồ TradingView

**Vi Giao Diện**: Kiến trúc mô-đun cho khả năng mở rộng

### Dài Hạn (6+ tháng)

**Ứng Dụng Di Động**: React Native/Flutter cho trải nghiệm gốc

**Ứng Dụng Máy Tính Để Bàn**: Trình bao bọc Electron cho người dùng máy tính để bàn

**Tính Năng Trí Tuệ Nhân Tạo**: Cảnh báo thông minh và phân tích dự đoán

**Quốc Tế Hóa**: Hỗ trợ đa ngôn ngữ

## Thông Tin Liên Hệ và Hỗ Trợ

**Phát triển bởi**: Nhóm Bảng Điều Khiển Tài Chính  
**Phiên bản**: 1.2.0  
**Cập nhật cuối**: Tháng 12, 2024

### Mục Tiêu Hiệu Suất

- Vẽ Đầu Tiên: dưới 1.5 giây
- Tương Tác: dưới 2.5 giây  
- Điểm Di Động: trên 90
- Điểm Khả Năng Truy Cập: trên 95

### Hỗ Trợ Kỹ Thuật

**Vấn Đề GitHub**: Báo cáo lỗi và yêu cầu tính năng

**Wiki Tài Liệu**: Chi tiết triển khai và kiến trúc

**Hệ Thống Thiết Kế**: Hướng dẫn thành phần giao diện người dùng và thực hành tốt nhất

---

*Lưu ý: Tập tin README này tập trung vào mô tả tính năng và kiến trúc thay vì ví dụ mã để dễ đọc và bảo trì.* 