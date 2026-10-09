# MikDrop releases

Bản dựng sẵn của **[MikDrop](https://github.com/duyminh-bostrap/MikDrop)**: chia sẻ ảnh và tệp ngang hàng (P2P) giữa iPhone, Android, Mac, Windows và Linux qua Wi-Fi, chỉ cần trình duyệt, không cần cài app trên điện thoại. Giao diện lấy cảm hứng từ AirDrop.

> Prebuilt binaries of **[MikDrop](https://github.com/duyminh-bostrap/MikDrop)**: peer-to-peer file sharing between iPhone, Android, Mac, Windows and Linux over Wi-Fi, right in the browser. English summary at the [bottom](#english).

## Tải về

Vào **[Releases → Latest](https://github.com/duyminh-bostrap/MikDrop-releases/releases/latest)** và tải file đúng với máy chạy MikDrop (không cần cài Node.js, chạy hoàn toàn offline):

| File | Dùng cho |
|---|---|
| `MikDrop.exe` | Windows 64-bit |
| `mikdrop-linux-x64` | Linux x64 (Ubuntu, Debian, Fedora, Arch...) |
| `mikdrop-linux-arm64` | Linux ARM64, ví dụ Raspberry Pi |
| `SHA256SUMS.txt` | Mã kiểm tra toàn vẹn (SHA-256) |

Chỉ máy **chạy MikDrop** (PC, laptop, mini PC, Raspberry Pi...) mới cần tải file. iPhone, Android và các máy khác chỉ cần mở địa chỉ trong trình duyệt.

Muốn dùng ngay không cần tải gì? Có bản online chạy thử tại **https://mikdrop.onrender.com** (có HTTPS, các thiết bị cùng Wi-Fi tự thấy nhau, khác mạng thì dùng chung mã phòng).

## Cách dùng nhanh

1. Chạy MikDrop trên một máy (xem bên dưới). Tất cả thiết bị phải **cùng một mạng Wi-Fi** (hoặc cùng router).
2. Trên máy đó trình duyệt tự mở `http://localhost:3000` và hiện **mã QR**. Dùng iPhone/Android quét mã QR, hoặc mở địa chỉ `http://<IP-của-máy>:3000` trên thiết bị khác.
3. Các thiết bị hiện trên radar. Chạm vào thiết bị cần gửi, chọn ảnh/tệp (hoặc kéo thả trên máy tính) rồi bấm **Gửi**.
4. Bên nhận bấm **Chấp nhận**. Tệp đi thẳng giữa hai thiết bị, được lưu tự động.

### Windows (`MikDrop.exe`)

1. Bấm đúp `MikDrop.exe`. MikDrop chạy nền, không hiện cửa sổ terminal.
2. Windows hỏi về tường lửa: tick **Mạng riêng tư (Private networks)** rồi bấm **Cho phép**, để các thiết bị khác truy cập được.
3. Đóng hết các trang MikDrop thì khoảng 15 giây sau nó tự thoát. Có thể thoát ngay ở **Cài đặt → Thoát MikDrop**.
4. Nhật ký chạy nằm trong `MikDrop.log` cạnh file exe.

File exe chưa được ký số nên Windows SmartScreen có thể cảnh báo *"Windows protected your PC"*. Bấm **More info → Run anyway**. Muốn chắc chắn file nguyên vẹn, hãy [kiểm tra SHA-256](#kiểm-tra-tính-toàn-vẹn).

### Linux (`mikdrop-linux-x64`, `mikdrop-linux-arm64`)

```bash
chmod +x mikdrop-linux-x64
./mikdrop-linux-x64
```

- Trên máy có desktop: tự mở trình duyệt (cần `xdg-open`) và tự thoát khi đóng hết trang web. Mã QR cũng in ngay trong terminal. Nhấn `Ctrl+C` để tắt.
- Trên máy chủ không có desktop (SSH, dịch vụ): chạy liên tục đến khi bạn dừng.
- Mở tường lửa nếu cần: `sudo ufw allow 3000/tcp` (Ubuntu/Debian) hoặc `sudo firewall-cmd --permanent --add-port=3000/tcp && sudo firewall-cmd --reload` (Fedora).
- File dựng cho các bản Linux dùng glibc, **không chạy trên Alpine** (musl).
- Chạy như dịch vụ systemd, tự khởi động cùng máy: xem [`deploy/mikdrop.service`](https://github.com/duyminh-bostrap/MikDrop/blob/main/deploy/mikdrop.service) ở repo chính.

### Tuỳ chọn dòng lệnh

| Lệnh | Tác dụng |
|---|---|
| `--port 8080` | Chọn cổng. Mặc định 3000, nếu bận tự thử 3001, 3002... |
| `--no-open` | Không tự mở trình duyệt |
| `--keep-alive` | Không tự thoát khi đóng hết trang web |
| `--https` | HTTPS tự ký, để iPhone có nút **Lưu vào Ảnh** (chứng chỉ lưu trong thư mục `.cert` cạnh file) |

Ví dụ: `MikDrop.exe --port 8080 --keep-alive` hoặc `./mikdrop-linux-x64 --https`.

## Tính năng

- Tự phát hiện thiết bị cùng mạng (radar), truyền **P2P bằng WebRTC**, server không lưu tệp.
- Xác nhận trước khi nhận: bên nhận thấy *"X muốn gửi 3 hình ảnh"* kèm ảnh xem trước.
- Chọn nhiều ảnh từ Thư viện ảnh (iOS/Android) hoặc kéo thả (Windows/macOS/Linux).
- Có đường dự phòng khi mạng chặn P2P, tự chọn đường nhanh khi một đầu là máy chạy MikDrop (khoảng 110 MB/s đo trên một máy).
- Tệp lớn được ghi thẳng ra ổ đĩa trên Chrome/Edge, gửi được tệp nhiều GB.
- Hai ngôn ngữ **Tiếng Việt / English**, tên hiển thị mặc định theo tên máy.
- Chia sẻ từ nút **Chia sẻ** của điện thoại: iPhone qua app Phím tắt, Android qua cài app (cần bản online có HTTPS). Hướng dẫn trong **Cài đặt → Chia sẻ từ app khác**.
- Đo tốc độ Wi-Fi ngay trong app.

Chi tiết, xử lý sự cố, triển khai lên Internet (Render, Docker) và biến môi trường: xem [README của repo chính](https://github.com/duyminh-bostrap/MikDrop#readme).

## Kiểm tra tính toàn vẹn

Mỗi release (từ 1.4.0) có `SHA256SUMS.txt`. Tải cùng thư mục với file cần dùng rồi chạy:

```bash
# Linux
sha256sum -c SHA256SUMS.txt --ignore-missing
```

```powershell
# Windows PowerShell: so khớp kết quả với dòng tương ứng trong SHA256SUMS.txt
Get-FileHash .\MikDrop.exe -Algorithm SHA256
```

Các bản cũ (1.0.0 đến 1.3.0) chỉ có `MikDrop.exe`, chưa có `SHA256SUMS.txt`.

## Về repo này

Repo này chỉ chứa **file phát hành**. Các release được đồng bộ từ repo chính, nơi có mã nguồn, tài liệu và báo lỗi: **https://github.com/duyminh-bostrap/MikDrop**. Vui lòng gửi issue ở repo chính.

## English

**MikDrop** shares photos and files peer-to-peer between iPhone, Android, Mac, Windows and Linux over your local Wi-Fi, in any browser, no phone app needed.

1. Download the file for the computer that will run MikDrop from [Releases → Latest](https://github.com/duyminh-bostrap/MikDrop-releases/releases/latest): `MikDrop.exe` (Windows x64), `mikdrop-linux-x64` or `mikdrop-linux-arm64` (Linux). No Node.js needed.
2. Run it (on Linux: `chmod +x mikdrop-linux-x64 && ./mikdrop-linux-x64`). On Windows allow **Private networks** in the firewall prompt.
3. A browser opens `http://localhost:3000` with a QR code. Scan it with your phone, or open `http://<computer-IP>:3000` on any device on the same Wi-Fi.
4. Tap a device, pick photos or files, send. The receiver taps **Accept**.

Options: `--port N`, `--no-open`, `--keep-alive`, `--https`. The Windows exe is unsigned, so SmartScreen may warn: choose *More info → Run anyway*. Verify downloads with `SHA256SUMS.txt`. Source code, full docs and issues live in the [main repository](https://github.com/duyminh-bostrap/MikDrop).
