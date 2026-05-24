# toder-ota

## Hướng dẫn lưu trữ release OTA trên GitHub

Đây là hướng dẫn dùng GitHub Release làm nơi lưu trữ bản firmware / package OTA và metadata để thiết bị có thể tải về cập nhật.

### 1. Chuẩn bị bản OTA

1. Biên dịch hoặc đóng gói bản firmware/app mới.
2. Đặt tên file rõ ràng, ví dụ `firmware-v1.0.0.exe` hoặc `ota-package-v1.0.0.zip`.
3. Tạo checksum (SHA256/MD5) cho file:
   - `sha256sum firmware-v1.0.0.exe`
   - `md5sum firmware-v1.0.0.exe`
4. Nếu cần, tạo thêm file metadata JSON:
   ```json
   {
     "version": "1.0.0",
     "url": "https://github.com/<owner>/<repo>/releases/download/v1.0.0/firmware-v1.0.0.exe",
     "checksum": "...",
     "notes": "Release OTA version 1.0.0"
   }
   ```

### 2. Tạo Release trên GitHub

1. Mở repo trên GitHub.
2. Chọn tab `Releases` -> `Draft a new release`.
3. Chọn một tag mới, ví dụ `v1.0.0`.
4. Nhập `Release title` và `Description`.
5. Kéo thả hoặc upload các asset cần thiết:
   - firmware/package
   - metadata file
   - checksum nếu muốn
6. Click `Publish release`.

### 3. Dùng GitHub CLI để tạo release tự động

Nếu bạn muốn làm nhanh trên dòng lệnh:

```bash
gh auth login
gh release create v1.0.0 path/to/firmware-v1.0.0.bin --title "v1.0.0" --notes "OTA update"
gh release upload v1.0.0 path/to/metadata-v1.0.0.json
```

### 4. Lấy URL download cho thiết bị

- Với repo public, asset release có thể truy cập trực tiếp bằng URL.
- Với repo private, thiết bị cần dùng token GitHub hoặc proxy để tải về.
- Lấy URL từ trang Release hoặc bằng API GitHub.

### 5. Thiết kế metadata và version

Nên lưu metadata để thiết bị kiểm tra version trước khi tải:

- `version`: version hiện tại
- `url`: link tải file OTA
- `checksum`: dùng để xác thực
- `notes`: ghi chú cập nhật

Ví dụ metadata:

```json
{
  "version": "1.0.0",
  "url": "https://github.com/<owner>/<repo>/releases/download/v1.0.0/firmware-v1.0.0.bin",
  "checksum": "abc123...",
  "notes": "Cập nhật OTA bản đầu tiên"
}
```

### 6. Quy trình cập nhật OTA cho thiết bị

1. Thiết bị kiểm tra phiên bản hiện tại.
2. So sánh với `metadata.version` từ GitHub Release.
3. Nếu có version mới, tải file từ `metadata.url`.
4. Kiểm tra checksum.
5. Thực hiện cài đặt và khởi động lại nếu cần.

### 7. Gợi ý tự động hóa

- Có thể thêm workflow GitHub Actions để build và publish release tự động.
- Workflow thường gồm:
  - build firmware
  - tạo tag version
  - tạo release
  - upload asset

### 8. Lưu ý

- GitHub Release phù hợp cho lưu trữ OTA nhỏ đến trung bình.
- Nếu file quá lớn hoặc cần tải xuống nhiều thiết bị, cân nhắc dùng CDN hoặc dịch vụ lưu trữ chuyên dụng.
- Với repo private, nhớ cấp quyền truy cập phù hợp cho token.
