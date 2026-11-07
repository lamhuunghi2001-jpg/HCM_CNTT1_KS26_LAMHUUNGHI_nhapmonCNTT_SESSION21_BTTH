## Phần 1. Googling đúng cách

### 1.1 Vì sao câu "văn nói" cho kết quả kém

Câu "tại sao em cài node xong mà gõ npm nó không nhận vậy thầy ơi" là lời nói với một người đang đứng trước mặt, không phải từ khóa cho máy tìm kiếm. Google so khớp từ khóa, nên các từ như "thầy ơi", "vậy", "nó" chỉ làm loãng kết quả. Câu này cũng thiếu ba thứ quan trọng nhất: mã lỗi nguyên văn, hệ điều hành và tên phần mềm chính xác ("node" chưa rõ là Node.js). Kết quả trả về vì thế là các bài blog chung chung, diễn đàn tiếng Việt lạc đề, không đúng lỗi đang gặp.

### 1.2 Bóc tách lỗi

- Tên phần mềm / Hệ điều hành: Node.js (lệnh npm), Windows 11, chạy trong Terminal của VS Code.
- Mã lỗi nguyên văn: `'npm' is not recognized as an internal or external command, operable program or batch file.`
- Thao tác đang làm: gõ `npm -v` để kiểm tra phiên bản npm sau khi vừa cài Node.js.

### 1.3 Ba câu truy vấn tối ưu (mỗi câu đúng 1 toán tử)

Giới hạn trong StackOverflow, dùng `site:`:

```
site:stackoverflow.com npm is not recognized windows node.js
```

Khớp lỗi nguyên văn, dùng ngoặc kép:

```
"'npm' is not recognized as an internal or external command" windows 11
```

Loại trừ bài cũ không liên quan, dùng dấu trừ:

```
npm is not recognized node.js windows -xp
```

## Phần 2. Thẩm định StackOverflow

### 2.1 So sánh hai câu trả lời

Vote và tích xanh:
- Câu trả lời A: +186 vote, có Accepted Answer (tích xanh).
- Câu trả lời B: +2 vote, không có tích xanh.

Thời gian đăng bài:
- Câu trả lời A: 2 năm trước, còn khá mới, hợp với Windows 11.
- Câu trả lời B: 9 năm trước, đã quá cũ.

Tags khớp hệ điều hành/phần mềm:
- Câu trả lời A: `[node.js] [windows] [npm]`, khớp cả phần mềm lẫn hệ điều hành.
- Câu trả lời B: `[node.js] [windows-xp]`, sai hệ điều hành (máy đang dùng Windows 11) và không có tag npm.

### 2.2 Kết luận

Áp dụng Câu trả lời A: thêm `C:\Program Files\nodejs\` vào biến môi trường PATH rồi mở lại Terminal. Cách này đúng nguyên nhân (Windows không tìm thấy npm vì thư mục cài đặt chưa nằm trong PATH), được cộng đồng xác nhận và đúng nền tảng.

Câu trả lời B là cái bẫy vì ba lý do. Nó viết cho Windows XP, mà máy của mình là Windows 11. Nội dung (cài lại driver mạng) không liên quan gì đến lỗi PATH. Và nếu làm theo, mình gỡ Node.js đang cài đúng, rồi cài một driver của hệ điều hành đời cũ, có thể gây xung đột hoặc làm mất kết nối mạng của máy, trong khi lỗi ban đầu vẫn còn nguyên.

### 2.3 Quy trình 30 giây đọc lướt khi mở một trang StackOverflow mới

1. Đọc tiêu đề câu hỏi, xem có đúng lỗi của mình không.
2. Nhìn Tags, xem có khớp phần mềm và hệ điều hành không.
3. Nhảy thẳng xuống câu trả lời có tích xanh hoặc vote cao nhất, không đọc dịch cả phần mô tả câu hỏi.
4. Kiểm tra thời gian đăng của câu trả lời đó.
5. Đọc các lệnh hoặc bước làm trong câu trả lời, rồi lướt qua bình luận bên dưới xem có ai báo lỗi không.

## Phần 3. Hỏi AI đúng cách và nhận diện rủi ro

### 3.1 Prompt theo công thức 3 thành phần

```
Ngữ cảnh: Tôi vừa cài Node.js trên laptop Windows 11. Mở Terminal trong VS Code và gõ "npm -v" để kiểm tra phiên bản. Tôi đã thêm đường dẫn C:\Program Files\nodejs\ vào biến môi trường PATH, đóng và mở lại Terminal nhưng lỗi vẫn còn.

Lỗi nguyên văn: 'npm' is not recognized as an internal or external command, operable program or batch file.

Yêu cầu: Hãy liệt kê các nguyên nhân có thể xảy ra và hướng dẫn tôi kiểm tra từng bước, theo thứ tự từ dễ đến khó. Mỗi lệnh đề xuất hãy giải thích nó làm gì. Chỉ đề xuất các cách an toàn, không đề xuất lệnh xóa file hay can thiệp vào thư mục hệ thống.
```

### 3.2 Đánh giá câu trả lời của AI ở mục 3e

Không, đây không phải câu trả lời an toàn để làm theo. Ba nguyên tắc an toàn dùng để kiểm tra (theo cách hiểu của mình về 3 Nguyên tắc An toàn Vàng):

1. Hiểu lệnh trước khi chạy. Lệnh `del /f /s /q C:\Windows\System32\*` xóa cưỡng bức, không hỏi lại, toàn bộ file trong thư mục System32, nơi chứa các file lõi của Windows. Máy có thể không khởi động được nữa.
2. Cẩn thận với lệnh có quyền cao và lệnh xóa. Lệnh này đòi chạy bằng quyền Admin và mang tính phá hủy không thể hoàn tác.
3. Đối chiếu độ khớp và nguồn. Lỗi của mình là thiếu đường dẫn PATH, còn AI lại bảo xóa "cache hệ thống". System32 không phải cache, và xóa nó không liên quan gì đến PATH. Lời hứa "lỗi PATH sẽ tự hết" cũng không có nguồn nào chứng minh.

### 3.3 Mình sẽ phản hồi AI thế nào

Mình không chạy lệnh đó. Mình sẽ hỏi lại:

```
Lệnh del /f /s /q C:\Windows\System32\* sẽ xóa chính xác những gì? Vì sao nó liên quan đến lỗi PATH của npm? Bạn có nguồn chính thức nào (tài liệu Microsoft hoặc Node.js) nói cách này sửa được lỗi không? Hãy đề xuất các cách chỉ đọc, không xóa gì, để kiểm tra xem thư mục cài Node.js đã nằm trong PATH chưa.
```

Nếu AI không giải thích được hoặc không có nguồn, mình bỏ qua đề xuất đó và quay lại kiểm tra PATH theo hướng an toàn.

## Phần 4. Viết Bug Report chuẩn để nhờ hỗ trợ

### 4.1 Tin nhắn kém

```
Thầy ơi máy em bị lỗi node, em làm mãi không được, thầy giúp em với.
```

Tin nhắn này không nói máy gì, lỗi gì, đã thử gì, nên người nhận phải hỏi lại nhiều lần.

### 4.2 Tin nhắn chuẩn theo Bug Reporting Framework

- Context: Em dùng laptop Windows 11, vừa cài Node.js. Em mở Terminal trong VS Code và gõ `npm -v` để kiểm tra phiên bản. Em đã làm theo câu trả lời được chọn (tích xanh, +186 vote) trên StackOverflow là thêm `C:\Program Files\nodejs\` vào biến môi trường PATH, rồi mở lại Terminal mới.
- Expected: Terminal hiện ra số phiên bản của npm.
- Actual: Terminal vẫn báo lỗi `'npm' is not recognized as an internal or external command, operable program or batch file.`
- Log & Screenshot: Em gửi kèm dòng lỗi nguyên văn ở trên và ảnh chụp toàn bộ cửa sổ Terminal lúc báo lỗi.

Tin nhắn hoàn chỉnh:

```
Chào thầy, em nhờ thầy hỗ trợ một lỗi ạ.
Máy em là laptop Windows 11, vừa cài Node.js. Em mở Terminal trong VS Code, gõ "npm -v" để kiểm tra phiên bản. Em đã thêm C:\Program Files\nodejs\ vào biến môi trường PATH theo câu trả lời tích xanh (+186 vote) trên StackOverflow rồi mở lại Terminal.
Em mong đợi Terminal hiện số phiên bản npm.
Nhưng thực tế vẫn báo: 'npm' is not recognized as an internal or external command, operable program or batch file.
Em gửi kèm ảnh chụp Terminal ở dưới ạ. Em cảm ơn thầy.
```

### 4.3 Chụp màn hình và bảo mật

Phím tắt chụp màn hình trên Windows: `Windows + Shift + S` (kéo chọn vùng cần chụp, ảnh được lưu vào clipboard, dán vào khung chat là gửi được).

Lưu ý bảo mật khi đính kèm ảnh: kiểm tra và che đi thông tin cá nhân trước khi gửi, như tên tài khoản máy trong đường dẫn `C:\Users\<tên>`, email, mật khẩu, token hoặc API key nếu có xuất hiện trong ảnh. Chỉ chụp vùng cần thiết, không chụp cả màn hình có nhiều cửa sổ khác.
