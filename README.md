# 🐥 SunnySpeak

Ứng dụng web một file giúp luyện nói tiếng Anh bản xứ mỗi ngày: học hội thoại, nối âm, phiên âm kiểu Việt và nhờ AI chấm điểm phát âm.

## Tính năng

- **Lộ trình 30 ngày** chia 3 giai đoạn: Nền tảng (A2), Giao tiếp tự tin (B1), Nâng cao và công việc (B2). Mỗi ngày có một chủ đề và một kỹ thuật nối âm riêng.
- **Bài học do AI soạn** bằng Google Gemini, kèm 15 chủ đề nhanh. Khi chưa có API Key, 7 chủ đề đầu có bài mẫu đầy đủ, các chủ đề còn lại dùng bài chung.
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

1. Đổi tên `Sunny_Speak.html` thành `index.html` rồi đưa lên repo cùng `README.md` và `favicon.svg`.
2. Vào **Settings → Pages**, chọn nhánh `main` và thư mục `/ (root)`, bấm **Save**.
3. Sau vài phút trang chạy tại `https://<tên-bạn>.github.io/<tên-repo>/`.

GitHub Pages dùng https nên micro hoạt động ổn định và nhớ quyền cho trang.

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
