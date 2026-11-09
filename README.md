# Lab0911: Quản Lý Phụ Thuộc, Khóa Phiên Bản Và Robot Điều Phối Tự Động

Bài thực hành môn học về Quản lý cấu hình phần mềm (SCM / DevOps).

## 📌 Mục Tiêu Bài Thực Hành
1. **Manifest File (`package.json`)**: Định danh gói phần mềm, khai báo khoảng phiên bản SemVer với toán tử Caret (`^`).
2. **Lockfile (`package-lock.json`)**: Cơ chế đóng băng môi trường, ghi nhận phiên bản chính xác và mã băm mật mã (`integrity hash`) để đảm bảo tính tái lập (Reproducible Builds).
3. **Đóng gói Artifact & Giả lập phát hành**:
   - Sử dụng `npm pack` để tạo gói `.tgz` nén mã nguồn sạch rác.
   - Sử dụng `npm publish --dry-run` để giả lập kiểm tra phát hành lên npm Registry toàn cầu.
4. **Robot Điều Phối Tự Động Dependabot**:
   - Cấu hình file `.github/dependabot.yml`.
   - Giám sát chuỗi cung ứng mã nguồn mở (Software Supply Chain Security), phát hiện lỗ hổng và tự động mở Pull Request nâng cấp thư viện.

---

## 🛠️ Cấu Trúc Dự Án
```text
LAB0911/
├── .github/
│   └── dependabot.yml       # Bản thiết kế cấu hình Robot Dependabot
├── node_modules/             # Thư mục chứa thư viện phụ thuộc (được gitignore)
├── .gitignore               # Tệp loại trừ rác và node_modules
├── index.js                 # Mã nguồn chính của thư viện
├── package.json             # Manifest file định danh gói và dependencies
├── package-lock.json        # Lockfile khóa phiên bản và mã băm integrity
├── dut-core-math-utils-1.0.0.tgz # Gói Artifact được đóng gói bởi npm pack
└── README.md                # Tài liệu hướng dẫn dự án
```

---

## 🚀 Hướng Dẫn Thực Thi Từng Bước

### Bước 1: Khởi tạo Manifest & Cài đặt thư viện phụ thuộc
```bash
# Khởi tạo Manifest
npm init -y

# Cài đặt lodash ở phiên bản cũ (4.17.15) có cảnh báo bảo mật
npm install lodash@4.17.15
```
- Toán tử Caret `^4.17.15` cho phép cập nhật các bản PATCH (`4.17.x`) và MINOR (`4.x.x`), nhưng không tự nhảy lên MAJOR (`5.0.0`).
- Tệp `package-lock.json` lưu giữ chính xác phiên bản đang chạy và mã băm `sha512-...`. Đồng nghiệp khi clone dự án chỉ cần chạy `npm ci` để đồng bộ 100%.

### Bước 2: Đóng gói Artifact và giả lập phát hành
```bash
# Đóng gói sản phẩm thành file .tgz
npm pack

# Chạy giả lập quy trình phát hành lên npm registry
npm publish --dry-run
```

### Bước 3: Kích hoạt Robot Dependabot
Tệp `.github/dependabot.yml` được cấu hình để định kỳ quét hệ sinh thái `npm` hàng tuần (`weekly`) và `github-actions` hàng tháng (`monthly`). Khi phát hiện gói có lỗ hổng (như `lodash@4.17.15`), Dependabot sẽ tự động tạo Pull Request với nhãn `security-patch`.

---

## 👨‍💻 Tác giả
- Sinh viên: Kim Tai
- GitHub: [ZeronKT205](https://github.com/ZeronKT205)
- Repository: [Lab0911](https://github.com/ZeronKT205/Lab0911.git)
