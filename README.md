# Nối — Bài tập gọi video

Website tiếng Việt dùng Firebase Authentication (Google/GitHub), Firestore (danh sách tài khoản và lời mời gọi), Jitsi Meet IFrame API (video, micro, chia sẻ màn hình). Không cần bước build.

## Chạy thử

Chạy `npm run dev`, mở http://127.0.0.1:5173. Bắt buộc đăng nhập Google/GitHub để hiển thị và dùng các chức năng. Không có chế độ xem thử hoặc cấu hình trên giao diện. Chạy `npm run check` để kiểm tra cú pháp.

## Cấu hình để dùng thật

1. Tạo Firebase project và thêm Web App trong https://console.firebase.google.com. Sao chép **web config**, không sử dụng service account hoặc OAuth client secret trong mã web.
2. Authentication → Sign-in method: bật Google. Nếu dùng GitHub, tạo OAuth App trên GitHub, nhập Client ID/secret vào Firebase và dùng callback URL Firebase cung cấp.
3. Authentication → Settings → Authorized domains: thêm `localhost` và tên miền triển khai của website.
4. Tạo Firestore Database, xuất bản nội dung `firestore.rules` trong tab Rules. Không dùng chế độ mở quyền ghi công khai.
5. Trong `dist/config.js`, thay `firebase: null` bằng đối tượng web config; sửa `teamName` và `members` thành thông tin nhóm thật. Các giá trị này được triển khai chung cho mọi người.
6. Triển khai thư mục `dist` trên hosting HTTPS như Firebase Hosting, Netlify hoặc Cloudflare Pages; thêm miền đó vào Authorized domains. Nếu dùng Firebase Hosting, chọn public directory là `dist` và giữ index.html hiện có.

## Kiểm tra bài nộp

- Mở hai trình duyệt hoặc hai tài khoản trên hai máy. Đăng nhập Google/GitHub trên từng máy.
- Kiểm tra tên tài khoản kia xuất hiện, tìm theo tên và nhấn nút gọi. Người nhận chấp nhận lời mời khi đang mở trang.
- Cho phép micro/camera, kiểm tra tiếng/hình cả hai chiều, chia sẻ màn hình, tắt micro và rời phòng.
- Kiểm tra tạo phòng, sao chép liên kết, đăng nhập từ liên kết và tham gia bằng mã.
- Kiểm tra điện thoại và điền đúng tên nhóm, thành viên trước khi gửi link cho lớp.

## Giới hạn cần biết

Chưa cấu hình Firebase thì chưa có đăng nhập/danh sách/cuộc gọi thật. Không có tài khoản hoặc danh sách người dùng giả được dùng như dữ liệu thật.

Jitsi public tại meet.jit.si có thể yêu cầu chủ phòng đăng nhập thêm, và phụ thuộc chính sách dịch vụ. Đăng nhập Firebase chặn thao tác gọi trong ứng dụng, không thay thế cơ chế quyền truy cập của Jitsi: người có URL Jitsi có thể truy cập theo chính sách của Jitsi. Muốn kiểm soát phòng ở server, dùng Jitsi tự host/JaaS và backend cấp JWT.

Trạng thái hoạt động dựa trên heartbeat mỗi 30 giây; sau khi đóng tab có thể mất tới 90 giây mới hiển thị không hoạt động. Lời mời chỉ hiển thị khi người nhận đang mở trang. Danh sách là các tài khoản trong cùng Firebase project, không phải danh bạ Google/GitHub. Có thể cấu hình Firestore TTL cho `calls.createdAt` để dọn lời mời cũ.

## Tài liệu

- https://firebase.google.com/docs/auth/web/google-signin
- https://firebase.google.com/docs/auth/web/github-auth
- https://firebase.google.com/docs/rules/basics
- https://jitsi.github.io/handbook/docs/dev-guide/dev-guide-iframe/
