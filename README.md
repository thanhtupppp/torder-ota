BƯỚC 1: Đẩy mã nguồn chính lên GitHub (Main Repository)
Mã nguồn của bạn hiện tại đã được commit đầy đủ ở máy local (bao gồm các thay đổi về OTA và sửa lỗi import). Bạn hãy chạy lệnh sau trong terminal để đẩy code lên repo quản lý chính:

powershell


git push origin main
BƯỚC 2: Quy trình phát hành bản cập nhật OTA lên Repo torder-ota
Khi bạn muốn đóng gói và phát hành phiên bản mới (ví dụ khi nâng cấp tính năng hoặc sửa lỗi), hãy làm theo quy trình 6 bước sau trên cửa sổ PowerShell:

1. Cập nhật số phiên bản (Version)
Mở tệp 
package.json
 và tăng số phiên bản tại trường "version" (Ví dụ: từ "1.0.0" lên "1.0.1").

2. Thiết lập 3 biến môi trường cho session PowerShell hiện tại
Bạn chạy các dòng lệnh sau trong cửa sổ PowerShell (thay thế token GitHub cá nhân của bạn vào):

powershell


$env:GH_TOKEN="MÃ_GH_TOKEN_CỦA_BẠN"
$env:OTA_GH_OWNER="thanhtupppp"
$env:OTA_GH_REPO="torder-ota"
IMPORTANT

GH_TOKEN cần là GitHub Personal Access Token (PAT) có quyền ghi (write:packages, repo) vào kho chứa torder-ota. Để bảo mật, không commit token này vào mã nguồn.

3. Kiểm tra xem biến môi trường đã được set đúng chưa
Chạy lệnh kiểm tra nhanh:

powershell


"GH_TOKEN set? " + [bool]$env:GH_TOKEN
"OTA_GH_OWNER=" + $env:OTA_GH_OWNER
"OTA_GH_REPO=" + $env:OTA_GH_REPO
4. Chạy kiểm tra tiền phát hành (Preflight Check)
Chạy lệnh sau để hệ thống tự động xác thực các biến môi trường và chạy thử quy trình biên dịch sản xuất:

powershell


npm run preflight:release
Nếu bước này báo thành công, mã nguồn của bạn đã sẵn sàng để phát hành.

5. Chạy lệnh phát hành chính thức
Chạy lệnh đóng gói và upload lên GitHub:

powershell


npm run release:win:public
Hệ thống sẽ biên dịch dự án, đóng gói bộ cài Windows dạng .exe, tạo blockmaps, file phiên bản latest.yml và tự động tải lên phần Releases của repo thanhtupppp/torder-ota.

6. Công bố Release trên GitHub
Truy cập kho lưu trữ: https://github.com/thanhtupppp/torder-ota/releases.
Bản cập nhật mới được tải lên sẽ ở dạng Draft (bản nháp). Bạn chỉ cần nhấn Edit, điền thông tin mô tả bản cập nhật (Changelog) nếu muốn, và nhấn Publish release để công bố rộng rãi.
Ngay sau đó, tất cả các máy client đang chạy phiên bản cũ sẽ nhận được thông báo cập nhật thời gian thực thông qua Modal cập nhật siêu đẹp mà chúng ta đã thiết kế!
