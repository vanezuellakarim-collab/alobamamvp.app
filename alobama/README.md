# 📞 AloBaMa — MVP bản chạy thật (Node.js)

Ứng dụng kết nối **Ông/Bà** và **Con cháu**: đăng ký → nhận **2 tài khoản** → đăng nhập vai nào thấy giao diện vai đó. Server Node thuần (**không cần cài thư viện nào**), dữ liệu lưu trong `data.json`.

---

## 🛠️ KHÔNG CHẠY ĐƯỢC? Xem đây trước!

| Triệu chứng | Nguyên nhân | Cách xử lý |
|---|---|---|
| Gõ `node server.js` báo **"'node' is not recognized..."** | Chưa cài Node.js | Tải bản **LTS** tại [nodejs.org](https://nodejs.org) → cài xong **đóng & mở lại terminal** → chạy lại |
| Nhấn đúp `run.bat` cửa sổ đóng ngay | Lỗi hiển thị trước khi đọc được | Chạy lại `run.bat` — bản mới có `pause`, hiện rõ lỗi. Hoặc mở CMD tại thư mục → gõ `node server.js` xem lỗi |
| **"Cannot find module .../server.js"** | Đứng sai thư mục | Phải `cd` vào thư mục **chứa file server.js** (thư mục `alobama`, không phải `alobama/alobama`) |
| Mở browser **"Không thể truy cập trang web này"** | Server chưa chạy hoặc tắt rồi | Phải để cửa sổ terminal **đang chạy** `node server.js` rồi mới mở http://localhost:3000 |
| Mở file `index.html` trực tiếp (đúp chuột) | App cần server, mở trực tiếp sẽ lỗi | Không mở file trực tiếp — chạy `node server.js` trước |
| Đăng ký xong bị **quay lại trang đăng nhập** | Cookie bị chặn (đã vá) | Đã vá bản mới — nếu vẫn gặp, thử trình duyệt khác hoặc tắt "Chặn cookie của bên thứ 3" |
| Port **3000 bị chiếm** (EADDRINUSE) | App khác đang dùng cổng | Chạy cổng khác: `set PORT=3001 && node server.js` (Windows) / `PORT=3001 node server.js` (Mac) |
| Đang xem qua **preview trong chat**, tắt server | Process không sống mãi | Nhắn "chạy lại server" là được |

**Chạy nhanh nhất: nhấn đúp `run.bat` (Windows) hoặc `./run.sh` (Mac/Linux).**

---

## 🚀 Cách chạy (2 bước)

**Cần có Node.js ≥ 16** ([tải tại nodejs.org](https://nodejs.org) — chọn bản LTS).

```bash
# 1. Mở terminal (CMD / PowerShell / Terminal của VS Code) tại thư mục alobama
cd alobama

# 2. Chạy server
node server.js
```

Mở trình duyệt: **http://localhost:3000**

> Xong! Không cần `npm install` gì cả. Muốn đổi cổng: `set PORT=8080 && node server.js` (Windows) hoặc `PORT=8080 node server.js` (Mac/Linux).

## 💻 Chạy bằng VS Code

1. Mở VS Code → **File → Open Folder…** → chọn thư mục `alobama`
2. Mở terminal trong VS Code: **View → Terminal** (hoặc `Ctrl + ~`)
3. Gõ `node server.js` → nhấn Enter
4. `Ctrl + Click` vào link `http://localhost:3000` trong terminal
## 👀 Demo cho giám khảo

| Cách | Cách làm |
|---|---|
| **1 máy** | Đăng ký → vào app Con cháu → bấm nút 🔄 (Đổi vai) trên thanh trên để nhảy qua lại giữa 2 giao diện |
| **2 thiết bị (đẹp nhất!)** | Máy tính đăng nhập `demo / 1234` (Con cháu) · Điện thoại **cùng wifi** đăng nhập `ongba1234 / 1234` (Ông/Bà) → bấm **TÔI ỔN** trên điện thoại → máy tính nhận thông báo **ngay lập tức** |
| **Điện thoại cùng wifi** | Xem IP máy tính (`ipconfig` trên Windows / `ifconfig` trên Mac), điện thoại mở `http://192.168.x.x:3000` |

**Tài khoản demo có sẵn:** `demo / 1234` (Con cháu) · `ongba1234 / 1234` (Ông/Bà)

## 🔁 Luồng chính của app

1. **Đăng ký** (`/register`): nhập tên, SĐT, tên ông bà, chọn gói (**59.000đ/tháng** hoặc **500.000đ/năm**) → hệ thống cấp 2 tài khoản (bản MVP **chưa thu tiền**)
2. **Đăng nhập** (`/login`): vào tài khoản nào → giao diện vai đó
   - **Con cháu**: xem vị trí, gửi ghi chú, đặt nhắc lịch, gọi, nhắn tin, nhận cảnh báo SOS
   - **Ông/Bà**: nút **TÔI ỔN** khổng lồ, lời nhắn từ con cháu, việc cần làm, gọi/nhắn, trợ lý AI, **cờ tướng** + **ghép đôi**, nút **KHẨN CẤP**
3. Hai bên **tự đồng bộ** (poll 4 giây) — sự kiện bên này hiện ngay bên kia

## 📁 Cấu trúc thư mục

```
alobama/
├── server.js          # Server Node (API + đăng nhập + phục vụ trang)
├── data.json          # "Database" — tự tạo khi chạy lần đầu
├── package.json
└── public/
    ├── index.html     # Landing page (Hero, Lợi ích, Tính năng, Giá, CTA, FAQ)
    ├── register.html  # Đăng ký → nhận 2 tài khoản
    ├── login.html     # Đăng nhập
    ├── ongba.html     # App bên Ông/Bà
    ├── conchau.html   # App bên Con cháu
    ├── css/style.css
    └── js/ (common.js · auth.js · ongba.js · conchau.js)
```

## 🌐 Đưa lên web miễn phí

| Nền tảng | Cách |
|---|---|
| **Render** ([render.com](https://render.com)) | New → Web Service → kết nối GitHub repo → Start command: `node server.js` — free tier |
| **Railway** ([railway.app](https://railway.app)) | New Project → Deploy from GitHub |
| **Glitch** ([glitch.com](https://glitch.com)) | New Project → Import từ GitHub — chạy `node server.js` |

> Lưu ý: `data.json` lưu trên đĩa của nền tảng — một số free tier reset định kỳ (dữ liệu demo mất là bình thường). Muốn dữ liệu bền hơn thì chuyển sang SQLite/Postgres ở bước tiếp theo.

## ⚠️ Ghi chú bản demo

- Mật khẩu mã hóa SHA-256 + salt — **chỉ dùng demo, chưa phải chuẩn bảo mật production**
- Bản đồ, vị trí, cuộc gọi và trợ lý AI là **dữ liệu mô phỏng**
- Thanh toán **chưa kích hoạt** (theo đúng MVP) — hiển thị giá 59k/tháng · 500k/năm
