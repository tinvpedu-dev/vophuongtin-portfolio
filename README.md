# VÕ PHƯƠNG TÍN — Creative Portfolio v1.0

**Website portfolio Việt / Anh, vận hành bằng Cloudflare Pages + GitHub + Pages CMS.**

Đây là phiên bản chuẩn hóa **dựa trên `README.md` và `index(2).html` mà người dùng gửi**. Các thành phần mà README ban đầu đề cập nhưng không cung cấp (CSS, JS, các trang con, ảnh concept) được **xây dựng lại** để thành một website có thể build. Phần chữ mô tả và hướng art direction navy là cơ sở tham chiếu; không có khẳng định đã phục dựng y hệt bộ gốc. Xem `reference/` để đối chiếu hai file ban đầu.

## Kết quả hiện tại

- Home, Work, Project Detail, About, Expertise, Contact, Journal.
- Đầy đủ đường dẫn `vi` tại `/` và `en` tại `/en/`.
- Lưới dự án, lọc danh mục, gallery ảnh, menu mobile.
- Projects, Posts, Profile, Services có cấu hình trong `.pages.yml` cho [Pages CMS](https://app.pagescms.org/).
- Nội dung được lưu dưới dạng JSON, không chỉnh nội dung trực tiếp trong HTML.
- `npm run build` tạo bản web tĩnh trong `dist/`; `npm run check` kiểm tra một số lỗi tĩnh.
- Các ảnh vector demo mới được tạo ở `public/media/` (khác ảnh mô tả trong README gốc).
- Có sitemap, robots, canonical, hreflang và meta social.

**CHƯA SẴN SÀNG CÔNG BỐ LÀ PORTFOLIO THẬT:** NOVA, LUMA, KADO và FORM là **demo không phải dự án khách hàng**; email công việc, ảnh chân dung, socials và hồ sơ nghề nghiệp của bạn chưa được cung cấp. Có cảnh báo demo trên website.

## 1. Dùng thử trên máy (cần Node.js >= 20)

Mở Terminal tại thư mục này:

```bash
npm run build
npm run check
npm run preview
```

Mở `http://localhost:4173`. Không mở trực tiếp file `dist/index.html` bằng `file://` vì CSS/JS/ảnh dùng đường dẫn gốc `/assets/...`.

Không cần cài framework hay `npm install` ở phiên bản này: generator dùng Node.js built-in. Nếu thay đổi code hoặc JSON trực tiếp ở máy, chạy lại `npm run build`.

## 2. Cấu trúc dự án

```text
.
├── .pages.yml                    # Cấu hình CMS nhập liệu
├── content/
│   ├── site.json                 # Tên, bio, contact, cài đặt
│   ├── services.json             # Chuyên môn
│   ├── projects/*.json           # Mỗi dự án là một JSON
│   └── posts/*.json              # Mỗi bài viết là một JSON
├── public/
│   ├── assets/style.css          # Design system & responsive
│   ├── assets/main.js            # Lọc, menu, gallery, form mailto
│   └── media/                    # Ảnh công khai (CMS upload vào đây)
├── scripts/
│   ├── build.mjs                 # Sinh HTML tĩnh song ngữ
│   ├── check.mjs                 # Kiểm tra tĩnh
│   └── serve.mjs                 # Preview local
├── docs/                         # Hướng dẫn triển khai + quản trị
├── reference/                    # Hai file người dùng cung cấp
└── dist/                         # Website xuất bản, tạo lại khi build
```

**Không đưa thư mục `dist/` lên GitHub:** Cloudflare tạo lại từ repository khi deploy. `.gitignore` đã loại trừ `dist/`.

## 3. Tạo repo GitHub & xuất bản Cloudflare

Đọc **`docs/01-DEPLOY-CLOUDFLARE.md`** theo từng bước. Kết nối Cloudflare Pages bằng **Git integration**, không chọn Direct Upload nếu mục tiêu là sửa nội dung từ CMS rồi tự deploy.

Build command: `npm run build` · Output directory: `dist` · Node.js >= 20.

## 4. Quản trị nội dung

Đọc **`docs/02-QUAN-TRI-CMS.md`**. Sau khi đưa source lên GitHub, vào https://app.pagescms.org/ đăng nhập GitHub, cho quyền repository, mở mục Projects / Posts / Profile / Services. CMS sẽ commit nội dung vào GitHub; Cloudflare tự build lại.

- `published: false`: không có trang public, không xuất hiện trong Work/Journal.
- `featured: true`: dự án xuất hiện ở Home (nếu published).
- `demo: true`: gắn nhãn concept minh họa; **không phải công trình có thật**.
- `slug`: chỉ dùng chữ thường `a-z`, số và dấu gạch ngang, ví dụ `thuong-hieu-moi`.
- Các đường dẫn portfolio tự tạo: `/work/thuong-hieu-moi/` và `/en/work/thuong-hieu-moi/`.
- Nội dung VI và EN được lưu tách trường. Chưa có tự dịch; hãy biên tập từng ngôn ngữ.
- Media CMS được upload trong `public/media/`, tham chiếu dạng `/media/ten-anh.webp`.

**Lưu ý Markdown:** Rich-text được xuất thành Markdown; generator chỉ hỗ trợ định dạng phổ biến (đoạn văn, tiêu đề, in đậm/nghiêng, đường dẫn và ảnh Markdown cơ bản). Bảng, nhúng HTML và các block nâng cao chưa hỗ trợ. Không đưa script/HTML lạ vào bài viết.

## 5. Contact

Trang Contact hiện **không thể nhận email** do bạn chưa cung cấp địa chỉ liên hệ. Khi điền email hợp lệ tại Profile trong CMS, trang hiển thị link email và form `mailto:`; việc nhấn gửi **mở ứng dụng email của khách**, không gửi trực tiếp/lưu tin nhắn vào database.

Muốn form gửi trực tiếp trong website, cần triển khai Cloudflare Worker + Turnstile + dịch vụ chuyển tiếp email/API; đó là tính năng **chưa làm ở bản này**.

## 6. Trước khi ra mắt

Đọc `docs/03-CHECKLIST-RA-MAT.md`. Cần:
1. Thay toàn bộ concept demo bằng các dự án thật.
2. Thêm email công việc, bio chính xác, ảnh chân dung và link mạng xã hội.
3. Dùng ảnh bạn có quyền sử dụng; nén WebP/AVIF.
4. Tắt `demoMode` trong Profile **sau khi đã gỡ hoàn toàn nội dung minh họa**.
5. Test Pages CMS thực tế + Cloudflare deployment + mobile + liên hệ.
6. Kiểm tra tên miền, HTTPS, PageSpeed, SEO trên môi trường production.

## 7. Giới hạn đã biết

- Chưa đăng nhập thử Pages CMS vì cần bạn cấp quyền qua GitHub.
- Chưa deploy lên Cloudflare hoặc cấu hình DNS trên tài khoản của bạn.
- Chưa có form backend hoặc `/admin` riêng; CMS là một ứng dụng ngoài tại `app.pagescms.org`.
- Chưa thể kiểm chứng thực tế giao diện trên thiết bị vì trình duyệt kiểm thử của môi trường hiện tại bị chặn mở trang local; đã chạy build + static link checks thành công.
- Chưa kiểm thử tải cao, Core Web Vitals trên domain thật, lịch sử bảo mật của môi trường production.

Bạn là chủ sở hữu code, ảnh, nội dung và tên miền. Không chia sẻ mật khẩu, token bí mật hoặc credentials trong cuộc trò chuyện.
