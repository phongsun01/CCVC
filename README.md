# 🏢 CCVC (Quản lý Công văn & Điều hành Công việc)

[![Playwright](https://img.shields.io/badge/Playwright-Automated_Scraping-green.svg)](https://playwright.dev/)
[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![Standard](https://img.shields.io/badge/N%C4%9030%2F2020%2FN%C4%90--CP-H%C3%A0nh_ch%C3%ADnh_Vi%E1%BB%87t_Nam-orange.svg)](https://thuvienphapluat.vn/)
[![License](https://img.shields.io/badge/License-MIT-lightgrey.svg)](LICENSE)

Bộ công cụ và kịch bản tự động hóa quản lý công văn đến/đi trên **Cổng thông tin Chính quyền điện tử tỉnh Quảng Ninh** (`congchuc.quangninh.gov.vn`).

Dành cho các cơ quan, đơn vị, bệnh viện trực thuộc (như **Bệnh viện Sản Nhi Quảng Ninh**, **Sở Y tế Quảng Ninh**), hỗ trợ tích hợp trực tiếp vào **AI Agent (Hermes / Claw / trợ lý ảo)** hoặc sử dụng độc lập qua **CLI**, **Zalo Bot**, **Telegram Bot**.

---

## 🌟 Tính năng nổi bật

### 1. Đồng bộ & Tra cứu Công văn đến (`/cc list`, `/cc list web`)
- **Đọc cache siêu tốc (`/cc list`)**: Truy xuất dữ liệu từ state JSON cục bộ gần như tức thì, phân trang rõ ràng, không làm nghẽn cổng SSO.
- **Quét trực tiếp thời gian thực (`/cc list web`)**: Tự động đăng nhập qua OpenAM SSO, đồng bộ danh sách mới nhất từ cổng thông tin với cơ chế cooldown 30s chống dồn request.
- **Tự động chuyển đúng đơn vị**: Khắc phục triệt để lỗi tài khoản đa đơn vị bị mặc định chuyển nhầm; tự động chuyển sang đơn vị chỉ định (mặc định ID `2256` - Bệnh viện Sản Nhi Quảng Ninh hoặc ID `226` - Sở Y tế) trước khi mở lưới xử lý.

### 2. Kết thúc xử lý văn bản siêu tốc (`/cc end`, `/cc end all`)
- **Kết thúc văn bản đơn lẻ (`/cc end <số> [lý do]`)**: Mở form kết thúc, điền lý do xử lý, postback và xác minh trực tiếp trên lưới danh sách trước khi đánh dấu hoàn thành.
- **⚡ Native Bulk End (`/cc end all`)**:
  - Không cần lặp qua từng văn bản tốn hàng chục phút như trước.
  - Tự động chuyển hiển thị sang **50 bản ghi/trang** (`PageSizeComboBox`).
  - Tích chọn ô **Chọn tất cả** (`ClientSelectColumnSelectCheckBox`).
  - Kích hoạt nút **Kết thúc** trên toolbar hệ thống và tự động xác nhận hộp thoại xác thực `radconfirm`.
  - **Thời gian xử lý rút gọn từ 30+ phút xuống chỉ còn ~10-15 giây** cho toàn bộ danh sách 30-50 văn bản.

### 3. Tải đính kèm & Tóm tắt bằng AI (`/cc tai`, `/cc tomtat`)
- **Tự động tải tệp (`/cc tai <số>`)**: Tải trọn bộ các tệp đính kèm văn bản (PDF, DOCX, RAR, ZIP) về thư mục lưu trữ cục bộ.
- **Tóm tắt chuyên sâu (`/cc tomtat <số>`)**: Đọc trực tiếp nội dung văn bản hành chính bằng Python, gọi mô hình ngôn ngữ lớn (LLM) để trích xuất:
  - Mục đích văn bản & cơ quan ban hành.
  - Các nội dung chỉ đạo trọng tâm.
  - Thời hạn báo cáo/thực hiện (Deadline).
  - Đơn vị/chuyên viên chủ trì tham mưu.
  - Gợi ý hướng xử lý văn bản.

### 4. Soạn thảo & Hiệu đính chuẩn thể thức (`/cc duthao`, `/cc hieudinh`, `/cc sualoi`)
- **Soạn dự thảo văn bản (`/cc duthao <số>`)**: Tự động phân tích công văn đến và lập dự thảo công văn/tờ trình trả lời theo đúng thể thức kỹ thuật trình bày văn bản hành chính quy định tại **Nghị định 30/2020/NĐ-CP**.
- **Hiệu đính & Soát lỗi (`/cc hieudinh`, `/cc sualoi`)**: Kiểm tra chính tả, ngữ pháp, chuẩn hóa xưng hô và văn phong công vụ; xuất 2 bản Word đối chiếu (Bản chuẩn hóa & Bản tối ưu).

### 5. Tra cứu văn bản đi (`/cc vbdi`, `/cc theodoi`)
- Tìm kiếm nhanh chóng văn bản phát hành theo số ký hiệu, trích yếu hoặc đơn vị soạn thảo.
- Đăng ký theo dõi tiến độ phát hành văn bản đi.

### 6. Khóa đa tiến trình tập trung (`lock_playwright.py`)
- Cổng OpenAM SSO của tỉnh giới hạn nghiêm ngặt 1 active session/tài khoản.
- Kỹ năng tích hợp cơ chế khóa tập trung `launch_browser_locked` (sử dụng non-blocking polling lock với timeout), triệt tiêu hoàn toàn các lỗi:
  - Lỗi vỡ pipe `write EPIPE` / Chromium crash.
  - Tranh chấp phiên khiến dữ liệu dòng bị nhảy sang văn bản khác.
  - Lỗi màn hình cảnh báo đổi mật khẩu (`btnContinue`).

---

## 📂 Cấu trúc dự án

```
CCVC/
├── SKILL.md                               # Hướng dẫn chi tiết định tuyến lệnh cho AI Agent
├── README.md                              # Tài liệu hướng dẫn sử dụng và kiến trúc
├── .env.example                           # Mẫu cấu hình biến môi trường
├── .gitignore                             # Bộ lọc loại trừ file log, cache và tệp nhạy cảm
│
├── scripts/                               # Toàn bộ mã nguồn Python tự động hóa
│   ├── congchuc_action.py                 # Xử lý kết thúc (Single, Batch, Native Bulk End), chuyển phòng ban
│   ├── congchuc_scrape.py                 # Đăng nhập, đồng bộ văn bản đến, tải file đính kèm
│   ├── congvan_status.py                  # Quản lý trạng thái theo dõi (new, read, wip, done)
│   ├── congvan_detail.py                  # Tra cứu chi tiết thông tin văn bản
│   ├── congchuc_summarize.py              # Đọc file văn bản và gọi LLM tóm tắt
│   ├── congchuc_draft.py                  # Soạn dự thảo công văn chuẩn NĐ 30/2020/NĐ-CP
│   ├── congchuc_editor.py                 # Hiệu đính, chuẩn hóa văn phong và chính tả
│   ├── congchuc_vbdi_scrape.py            # Đồng bộ văn bản đi từ cổng
│   ├── congchuc_vbdi_search.py            # Tìm kiếm văn bản đi theo từ khóa
│   ├── lock_playwright.py                 # Context manager khóa tập trung điều phối Playwright
│   ├── fix_state.py                       # Tiện ích sửa lỗi và đồng bộ state file
│   └── inspect_*.py                       # Các công cụ kiểm tra DOM, quy trình, tài khoản đơn vị
│
├── lib/                                   # Thư viện tích hợp hệ thống
│   ├── cc_router.py                       # Bộ định tuyến điều hướng lệnh callback từ bot
│   └── inline_menu_payload.py             # Bộ sinh payload nút bấm tương tác nhanh Zalo/Telegram
│
├── references/                            # Cẩm nang kỹ thuật & Hướng dẫn xử lý sự cố
│   ├── kethuc-playwright-pitfalls.md      # Tổng hợp kinh nghiệm khắc phục lỗi Playwright SSO
│   ├── tomtat-detail.md                   # 3 phương pháp trích xuất nội dung văn bản
│   ├── congchuc-scrape-reference.md       # Cấu trúc dữ liệu và logic bóc tách HTML RadGrid
│   ├── vbdi-scrape-fix.md                 # Hướng dẫn xử lý dữ liệu văn bản đi
│   ├── soan-congvan-word.md               # Quy cách sinh văn bản Word theo NĐ 30
│   └── gioi-thieu-cc-skill.md             # Tài liệu giới thiệu tính năng ứng dụng
│
└── templates/                             # Biểu mẫu hành chính
    └── gopy-template.md                   # Mẫu khung văn bản tham gia ý kiến/góp ý
```

---

## 📋 Bảng tra cứu lệnh nhanh

| Lệnh | Chức năng | Mô tả thực thi |
|---|---|---|
| `/cc list` | Xem danh sách VB mới | Đọc nhanh từ cache `vbden_state.json` |
| `/cc list web` | Quét live từ cổng web | Kết nối trực tiếp cổng công chức, đồng bộ mới nhất (cooldown 30s) |
| `/cc list --page <N>` | Chuyển trang danh sách | Xem tiếp trang thứ `<N>` khi danh sách dài |
| `/cc end <số> [lý do]` | Kết thúc 1 văn bản | Xử lý kết thúc văn bản đơn lẻ và đồng bộ `done` |
| `/cc end all` | **Kết thúc hàng loạt** | Kích hoạt **Native Bulk End** (50 bản ghi/lần, ~15s) |
| `/cc tai <số>` | Tải tệp đính kèm | Tải file đính kèm của số đến về thư mục máy chủ |
| `/cc tomtat <số>` | Tóm tắt văn bản bằng AI | Đọc PDF/DOCX, trích xuất mục đích, thời hạn, phân công |
| `/cc duthao <số>` | Soạn thảo dự thảo | Tạo văn bản trả lời chuẩn Nghị định 30/2020/NĐ-CP |
| `/cc hieudinh <số>` | Hiệu đính văn bản | Soát lỗi chính tả, tối ưu hóa văn phong công vụ |
| `/cc vbdi <từ khóa>` | Tìm văn bản đi | Tra cứu văn bản phát hành theo số hiệu, trích yếu |
| `/cc theodoi <số>` | Theo dõi văn bản đi | Đăng ký nhận thông báo khi văn bản đi được phát hành |
| `/cc help` | Hướng dẫn sử dụng | Hiển thị bảng trợ giúp lệnh nhanh |

---

## ⚙️ Cài đặt & Cấu hình

### 1. Yêu cầu hệ thống
- **Hệ điều hành**: Linux, macOS hoặc Windows 10/11 (chạy tốt trong Docker hoặc môi trường native).
- **Python**: Phiên bản `3.10` trở lên.
- **Trình duyệt không đầu (Headless Browser)**: Chromium của Playwright.

### 2. Cài đặt thư viện phụ thuộc
```bash
# Cài đặt các package cần thiết
pip install playwright python-docx beautifulsoup4 python-dotenv pymupdf

# Cài đặt trình duyệt cho Playwright
playwright install chromium
```

### 3. Thiết lập biến môi trường
Sao chép tệp `.env.example` thành `.env` và điền thông tin tài khoản:
```bash
cp .env.example .env
```

Nội dung `.env`:
```env
# Thông tin đăng nhập Cổng công chức Quảng Ninh
CONGVAN_USER=tai_khoan_dang_nhap
CONGVAN_PASS=mat_khau_cua_ban

# Đường dẫn URL xử lý văn bản
CONGVAN_URL=https://congchuc.quangninh.gov.vn/Default.aspx?tabid=1126

# Mã đơn vị xử lý (2256: BV Sản Nhi Quảng Ninh, 226: Sở Y tế)
CONGVAN_UNIT=2256,226

# Tùy chọn đường dẫn thư mục lưu trữ trạng thái (mặc định ~/.hermes)
HERMES_HOME=/opt/data
```

---

## 💻 Ví dụ chạy dòng lệnh trực tiếp (CLI)

### Kiểm tra danh sách văn bản chờ xử lý:
```bash
python scripts/congvan_status.py list --status new
```

### Quét live và cập nhật trạng thái trực tiếp từ web:
```bash
python scripts/congvan_status.py list --status new --web
```

### Kết thúc văn bản đơn lẻ:
```bash
python scripts/congchuc_action.py kethuc 4387 "Đã triển khai thực hiện theo chỉ đạo."
```

### Kết thúc toàn bộ văn bản tồn đọng bằng Native Bulk End:
```bash
python scripts/congchuc_action.py kethuc all "Đã hoàn thành xử lý toàn bộ văn bản."
```

### Tóm tắt nhanh văn bản:
```bash
python scripts/congchuc_summarize.py --so-den 4387
```

---

## 🛡️ Nguyên tắc an toàn & Lưu ý quan trọng

1. **Thao tác ghi không thể hoàn tác**:
   - Các hành động `end`, `end all`, chuyển phòng ban sẽ ghi nhận trực tiếp lên Cổng thông tin Chính quyền điện tử.
   - Luôn xác nhận danh sách văn bản và nội dung trước khi thực hiện.
2. **Không ghi đè bừa bãi `HERMES_HOME`**:
   - Tránh đặt `HERMES_HOME=/tmp` vì sẽ làm mất file cấu hình `.env` và file lưu trạng thái `.playwright_storage.json`.
3. **Bảo mật thông tin công vụ**:
   - Không commit tệp `.env`, tệp lưu cookie `.playwright_storage.json` hoặc các tài liệu đính kèm nội bộ lên kho lưu trữ mã nguồn công khai.

---

## 👨‍💻 Tác giả & Đóng góp
- **Tác giả**: Nguyễn Huy Phong ([@phongsun01](https://github.com/phongsun01)) — Bệnh viện Sản Nhi Quảng Ninh.
- **Dự án**: Hỗ trợ tích hợp trợ lý AI điều hành hành chính công vụ.
