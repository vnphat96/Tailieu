# Quy ước soạn LaTeX VNPmath (`D:\Tailieu`)

Khi tạo, chỉnh sửa hoặc biên dịch bất kỳ file `.tex` nào trong dự án này, luôn kích hoạt skill `vnpmath-latex` (và tra cứu `CheatSheet-LaTeX-VNPmath.md` khi cần chi tiết lệnh).

## Quy tắc bắt buộc
- **Trình biên dịch:** Chỉ dùng **pdfLaTeX** (`latexmk -pdf -synctex=1 -interaction=nonstopmode <main>.tex`). Không dùng XeLaTeX hay LuaLaTeX.
- **Thư mục `ans/`:** Do LaTeX tự sinh qua `\Opensolutionfile` / `\Closesolutionfile` — tuyệt đối không sửa tay.
- **Cú pháp `ex_test.sty` v3.3.4:** Tuỳ chọn danh sách câu đặt **trước** tên môi trường (`\showansEX[1,3]{vd}`, `\hideansEX[3,7]{ex}`), không dùng cú pháp cũ v3.1.
- **Thứ tự preamble:** `\def\kieumau` và `\def\leftnote` phải đặt trước `\input{cautrucDT/Khai-bao-cho-file-mainDT}`.
- **Quy ước toán học & văn bản:**
  - Dùng `\dfrac` cho phân số trong câu, `\heva{}` / `\hoac{}` thay cho `cases`, `\mathscr{D}` cho tập xác định.
  - Dấu phẩy thập phân viết `{,}` (ví dụ `2{,}4`, bắt buộc trong `\shortans{$2{,}4$}`); dấu cách hàng nghìn viết `3\,200`; ngoặc kép dùng `\lq\lq ... \rq\rq`.
  - Mỗi `\choice` có đúng 1 `\True` và không có dòng trống giữa các phương án; `\shortans` tối đa ~4 ký tự.
  - Giữ đủ 3 nhóm comment `%[Nguồn]%[Người soạn]%[Mã ID]` trên cùng dòng `\begin{ex}`.
- **Chế độ trang:** Chỉ đặt `\NOTE` / `\FULLWIDTH` ở đầu phần/đầu trang; không gọi `\notename` sau `\FULLWIDTH`.
