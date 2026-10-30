# Tài liệu LaTeX VNPmath

Kho mã nguồn LaTeX cho bài giảng, đề kiểm tra và tài liệu ôn tập Toán. Nội dung được tổ chức theo lớp, bộ sách và chủ đề; giữ nguyên cấu trúc hiện tại để không làm hỏng các đường dẫn `\input`.

## Bản đồ nhanh

| Thư mục/tệp | Nội dung |
|---|---|
| `9.tex`, `10CD.tex`, `11.tex`, `12.tex` | Các tệp LaTeX chính |
| `input9.tex`–`input12.tex` | Danh sách phần nội dung bật/tắt cho từng tệp chính |
| `data/`, `dataDT/`, `dataDeTHPT/`, `dataTHPT/`, `datak40/` | Bài giảng, đề và dữ liệu câu hỏi |
| `ans/` | Các tệp đáp án được sinh trong quá trình biên dịch |
| `cautrucDT/`, `*.sty`, `*.ttf`, `*.tfm` | Cấu hình, gói và font LaTeX dùng chung |
| `bia/`, `images/`, `img/`, `hinhve/`, `logo/`, `QRcode/` | Bìa và tài nguyên hình ảnh |
| Các tệp PDF ở thư mục gốc | Đề, lời giải, tài liệu hướng dẫn và bản biên dịch |

## Biên dịch

Chọn các dòng `\input{...}` cần dùng trong tệp `inputNN.tex`, sau đó biên dịch tệp chính tương ứng, ví dụ:

```powershell
latexmk -pdf -synctex=1 -interaction=nonstopmode 12.tex
```

Tài liệu quy ước và mẫu LaTeX chi tiết nằm trong [CheatSheet-LaTeX-VNPmath.md](./CheatSheet-LaTeX-VNPmath.md). Các tệp PDF hiện có được giữ lại; chỉ các tệp phụ do công cụ biên dịch tạo ra mới được dọn.
