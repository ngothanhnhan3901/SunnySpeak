# 🐥 SunnySpeak

Ứng dụng web một file giúp luyện nói tiếng Anh bản xứ mỗi ngày: học hội thoại, nối âm, phiên âm kiểu Việt và nhờ AI chấm điểm phát âm.

## Tính năng

- **Lộ trình 360 ngày, 12 giai đoạn** (mỗi giai đoạn 30 ngày): Giai đoạn 1 (A2–B2, 15 câu/ngày), Giai đoạn 2–12 (B1+ đến C1, 25 câu/ngày): công việc, du lịch, kể chuyện, tranh luận, thuyết trình, phỏng vấn, văn hóa và tin tức, công nghệ và kinh doanh, tiếng lóng và thành ngữ, ôn tập và thi thử. Hoàn thành đủ 30 ngày thì mở giai đoạn kế tiếp, có huy hiệu cho từng giai đoạn.
- **Bài học do AI soạn** bằng Google Gemini, mỗi ngày 15 câu hội thoại liền mạch, kèm 15 chủ đề nhanh. Khi chưa có API Key, 7 chủ đề đầu có bài mẫu đầy đủ, các chủ đề còn lại dùng bài chung.
- **Mỗi câu có đủ**: tiếng Anh, IPA, cách đọc kiểu Việt (ví dụ Hello → Hé-lô), cách nối âm và nghĩa tiếng Việt.
- **Ghi âm và chấm điểm**: ghi giọng của bạn, gửi lên Gemini để nhận dạng và chấm độ chính xác, nối âm, trôi chảy.
- **Tick câu đã học**, tự tick khi đạt từ 70 điểm. Hoàn thành hết câu trong ngày thì được tính xong ngày.
- **Giọng đọc mẫu** tiếng Anh, chọn được giọng và tốc độ (1.0x hoặc 0.75x).
- **XP, chuỗi ngày, huy hiệu và nhật ký học tập**. Dữ liệu lưu trong trình duyệt.

## Chạy thử trên máy

1. Tải file `index.html`.
2. Chạy một máy chủ cục bộ để trình duyệt nhớ quyền micro:

   ```bash
   python -m http.server 8000
   ```

3. Mở `http://localhost:8000/index.html` bằng Chrome hoặc Edge.

Mở file trực tiếp (`file://`) vẫn học được nhưng trình duyệt sẽ hỏi quyền micro lại sau mỗi lần tải trang.

## Đăng lên GitHub Pages

1. Đổi tên `Sunny_Speak.html` thành `index.html` rồi đưa lên repo cùng `README.md`, `manifest.webmanifest`, `apple-touch-icon.png`, `icon-192.png`, `icon-512.png` và `favicon.png`.
2. Vào **Settings → Pages**, chọn nhánh `main` và thư mục `/ (root)`, bấm **Save**.
3. Sau vài phút trang chạy tại `https://<tên-bạn>.github.io/<tên-repo>/`.

GitHub Pages dùng https nên micro hoạt động ổn định và nhớ quyền cho trang.

## Ngôn ngữ

Ô chọn ngôn ngữ ở đầu trang cho phép đổi giữa **Tiếng Anh** (360 ngày, 12 giai đoạn) và **Tiếng Pháp** (60 ngày, 2 giai đoạn, từ A1 đến B1). Mỗi ngôn ngữ có tiến độ, XP, chuỗi ngày, nhật ký và bài đã tạo riêng. Dữ liệu đồng bộ Firebase cũng tách riêng (`users/{uid}/sunnyspeak/main` cho tiếng Anh, `.../fr` cho tiếng Pháp), nên Rules hiện tại không cần đổi. Giọng đọc dùng giọng `fr-FR` của máy: trên iPhone cần tải giọng Français trong Cài đặt → Trợ năng → Nội dung được đọc → Giọng nói.

## Đồng bộ dữ liệu

Dữ liệu học (XP, chuỗi ngày, nhật ký, bài đã tạo) lưu trong trình duyệt của từng thiết bị. Đăng nhập Google ở tab **Nhật ký** để dữ liệu tự lưu trên Firebase và dùng chung giữa điện thoại và máy tính. API Key không bao giờ được đưa lên đám mây.

### Thiết lập Firebase (một lần, miễn phí)

1. Vào [console.firebase.google.com](https://console.firebase.google.com) → **Add project**.
2. Trong dự án, thêm app **Web (`</>`)** và sao chép đoạn `firebaseConfig`.
3. **Authentication → Sign-in method**: bật **Google**.
4. **Authentication → Settings → Authorized domains**: thêm tên miền trang (ví dụ `ten-ban.github.io`).
5. **Firestore Database → Create database**, sau đó vào tab **Rules** và dán:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{db}/documents {
       match /users/{uid}/{doc=**} {
         allow read, write: if request.auth != null && request.auth.uid == uid;
       }
     }
   }
   ```

6. Mở trang, vào tab **Nhật ký** → khung **Đồng bộ đám mây**, dán `firebaseConfig` rồi bấm **Lưu cấu hình** và **Đăng nhập bằng Google**.

Cách gộp dữ liệu: XP lấy giá trị cao nhất, danh sách câu đã học, ngày đã xong và nhật ký được gộp từ hai nơi. Một ghi chú xóa trên thiết bị này có thể xuất hiện lại nếu thiết bị kia còn giữ nó.

## Gemini API Key

1. Lấy key miễn phí tại [Google AI Studio](https://aistudio.google.com/app/apikey).
2. Bấm **Cài đặt AI** ở đầu trang, dán key, chọn model (mặc định `gemini-3.5-flash`), bấm **Kiểm tra kết nối** rồi **Lưu thiết lập**.

Lưu ý bảo mật:

- Key chỉ lưu trong `localStorage` của trình duyệt bạn, không nằm trong mã nguồn. **Không dán key vào file rồi đẩy lên GitHub.**
- Nên đặt giới hạn hạn mức cho key trong Google AI Studio.
- Âm thanh ghi âm được gửi trực tiếp từ trình duyệt đến Google Gemini để xử lý.

## Lỗi thường gặp

| Hiện tượng | Cách xử lý |
| --- | --- |
| Lỗi HTTP 404 khi kiểm tra kết nối | Model đã ngừng hoạt động. Chọn `gemini-3.5-flash`. |
| Lỗi HTTP 429 | Key hết hạn mức hoặc gọi quá nhanh. Đợi một lúc rồi thử lại. |
| Lỗi HTTP 400 hoặc 403 | Key sai hoặc chưa được cấp quyền. |
| Micro bị chặn | Bấm biểu tượng ổ khóa cạnh thanh địa chỉ, cho phép Microphone rồi tải lại trang. |
| Giọng đọc nghe như người Việt | Chọn một giọng English (US) ở ô chọn giọng. Máy chưa có giọng tiếng Anh thì cài thêm trong cài đặt Giọng nói của hệ điều hành. |

## Công nghệ

HTML, JavaScript thuần, [Tailwind CSS](https://tailwindcss.com) (CDN), [Lucide](https://lucide.dev) (icon), Web Speech API (giọng đọc), MediaRecorder (ghi âm), Google Gemini API (soạn bài, nhận dạng và chấm điểm). Phông chữ Be Vietnam Pro và Noto Sans từ Google Fonts. Cần kết nối mạng để tải các thư viện này.

## Trình duyệt hỗ trợ

Chrome và Edge là tốt nhất. Safari và Firefox dùng được phần học và ghi âm bằng Gemini, nhưng không có nhận dạng giọng nói có sẵn khi chưa nhập key.

## Giấy phép

Chưa chọn giấy phép. Thêm file `LICENSE` (ví dụ MIT) nếu bạn muốn chia sẻ mã nguồn.
