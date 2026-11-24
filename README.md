# Quản lý chi tiêu — dự án iOS

Dự án Capacitor này bọc giao diện trong `www/index.html` thành app iOS. Phần giao diện hoạt động offline; giao dịch được lưu cục bộ trên thiết bị.

## Tạo IPA khi chỉ có Windows (GitHub Actions)

Project kèm workflow build IPA không ký trên runner macOS của GitHub Actions. Không cần máy Mac. Làm theo các bước:

1. Tạo một repository GitHub (có thể để riêng tư), giải nén project này trên Windows.
2. Mở PowerShell trong thư mục vừa giải nén và chạy các lệnh sau (thay URL bằng URL repository của bạn):

   ```powershell
   git init -b main
   git add .
   git commit -m "Add Quản lý chi tiêu app"
   git remote add origin https://github.com/TEN-CUA-BAN/TEN-REPO.git
   git push -u origin main
   ```

3. Mở repository trên GitHub, vào **Actions → Build unsigned iOS IPA → Run workflow**.
4. Khi workflow chạy xong, mở lần chạy đó và tải artifact **QuanLyChiTieu-unsigned-IPA** xuống. Bên trong là `QuanLyChiTieu-unsigned.ipa`.
5. Đưa IPA đó vào Sideloadly để ký bằng Apple ID và cài lên iPhone.

Workflow build trên macOS runner do GitHub cung cấp, tự đồng bộ Capacitor và đóng gói app chưa ký để Sideloadly ký. Lần đầu chạy có thể mất vài phút.

## Build trực tiếp trên máy Mac

1. Cài Node.js LTS và Xcode trên máy Mac.
2. Giải nén thư mục dự án rồi mở Terminal tại thư mục `quan-ly-chi-tieu-ios`.
3. Chạy:

   ```sh
   npm install
   npx cap add ios
   npx cap sync ios
   npx cap open ios
   ```

4. Trong Xcode chọn **Any iOS Device (arm64)**, vào **Product → Archive**, rồi xuất ứng dụng dạng IPA. Có thể dùng bản development signing để cài cá nhân.
5. Trên Windows, mở Sideloadly, chọn IPA vừa xuất, kết nối iPhone và ký/cài bằng Apple ID.

Nếu Xcode báo Bundle Identifier đã được dùng, đổi `appId` trong `capacitor.config.json` sang một ID duy nhất rồi chạy lại `npx cap sync ios`.

## Lưu ý dữ liệu

Giao dịch hiện lưu trong bộ nhớ cục bộ của ứng dụng. Gỡ ứng dụng có thể xóa dữ liệu; chưa có đồng bộ iCloud hay sao lưu tự động.
