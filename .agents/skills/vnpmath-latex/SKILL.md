---
name: vnpmath-latex
description: >-
  Author, edit, format, and build VNPmath LaTeX documents (lectures, 3-part exams,
  TikZ figures, and answer sheets) in D:\Tailieu using ex_test.sty v3.3.4 and
  cautrucDT/. Use whenever creating or modifying .tex files, TikZ diagrams,
  inputNN.tex switches, or compiling PDFs in this workspace.
---

# VNPmath LaTeX Authoring & Build Skill

Tài liệu tham chiếu đầy đủ (bảng lệnh, `\OPTN`, mẫu đầy đủ):
- [CheatSheet-LaTeX-VNPmath.md](../../../CheatSheet-LaTeX-VNPmath.md)
- Preamble nội bộ: `cautrucDT/Khai-bao-cho-file-mainDT.tex`

## 1. Kiến trúc 3 lớp
`main.tex` (`9.tex`, `10CD.tex`, `11.tex`, `12.tex`) → `inputNN.tex` (mục lục công tắc: comment `%` mọi dòng, chỉ mở đúng phần cần in) → `data/.../*.tex` (nội dung).
- **Không sửa tay** các file trong `ans/` (do `\Opensolutionfile{ans}[ans/<tên>]` … `\Closesolutionfile{ans}` tự sinh).

## 2. Quy trình soạn nội dung mới

### A. Bài giảng / chuyên đề
- Đầu file: `\notename`, `\chap{...}`, `\section{...}` (tự in "Bài n."), `\Opensolutionfile{ans}[ans/<tên>]`.
- Phân dạng: `\begin{dang}{Tên dạng} ... \end{dang}` (tự tăng `dang` và reset `vd`, `ex`, `bt`, `subsubsection`).
- Môi trường lý thuyết: `\dn`, `\dl`, `\tc`, `\hq`, `\nx`, `\begin{note}...\end{note}`.
- Môi trường bài tập: `\viduminhhoa` + `\begin{vd}`, `\baitaptn` + `\begin{ex}`, `\baitaptl` + `\begin{bt}`.
- Cuối file: `\Closesolutionfile{ans}`.

### B. Đề thi 3 phần
1. Khung tên đề: `\begin{name}{Tiêu đề}{Môn-Khối}{Trường}{\thoigian}\end{name}`.
2. **Phần I (4 phương án):** `\caulc` + `\Opensolutionfile{ans}[ans/<tên>-Phan-I]` + `\begin{ex}...\choice{...}{...}{\True ...}{...}\loigiai{...}\end{ex}` + `\Closesolutionfile{ans}`.
   - Đúng 1 `\True` cho mỗi `\choice`; không để dòng trống giữa các phương án.
3. **Phần II (Đúng/Sai):** `\cauds` + `\Opensolutionfile{ans}[ans/<tên>-Phan-II]` + `\choiceTF` (đánh `\True` cho từng phát biểu đúng) + lời giải dùng `\begin{itemchoice}\itemch {\bf Đúng./Sai.}\\ ...\end{itemchoice}` + `\Closesolutionfile{ans}`.
4. **Phần III (Trả lời ngắn):** `\caukq` + `\Opensolutionfile{ans}[ans/<tên>-Phan-III]` + `\shortans{$...$}` (≤ 4 ký tự, số thập phân bắt buộc dùng `{,}` như `$2{,}4$`) + `\Closesolutionfile{ans}`.

### C. Nhãn câu & Mã ID
- Giữ 3 nhóm comment trên **cùng dòng `\begin{ex}`**:
  `\begin{ex}%[Nguồn đề]%[Người soạn, dự án]%[Mã ID6]`
- Ưu tiên giữ nguyên mã ID6 từ file nguồn.

### D. Hình vẽ TikZ & Bảng biến thiên
- Chèn hình cạnh đề: `\immini[thm]{đề bài}{\begin{tikzpicture}...\end{tikzpicture}}`.
- Tùy chọn chuẩn: `[scale=1, font=\footnotesize, line join=round, line cap=round, >=stealth]`.
- Dựng điểm bằng `\path ... coordinate (A) ...;` trước khi `\draw`; vẽ nhãn điểm bằng `\foreach \x/\g in {...}`; luôn dùng `\clip` khi vẽ đồ thị hàm số.
- Bảng biến thiên dùng `tkz-tab` (`\tkzTabInit`, `\tkzTabLine`, `\tkzTabVar`), đảm bảo khớp số phần tử giữa các dòng.

## 3. Lưu ý phiên bản & chế độ trang
- **Bắt buộc `ex_test.sty` v3.3.4:** Danh sách câu đặt **trước** tên môi trường: `\hideansEX[3,7]{ex}`, `\showansEX[1,3]{vd}` (KHÔNG dùng cú pháp cũ v3.1 `\showansEX{vd}[...]`).
- Khai báo `\def\kieumau{...}` và `\def\leftnote{...}` phải đặt **trước** `\input{cautrucDT/Khai-bao-cho-file-mainDT}`.
- `\NOTE` (in đề, có cột Quick Note) và `\FULLWIDTH` (in lời giải, full bề rộng) chỉ đặt ở đầu phần/đầu trang; không gọi `\notename` sau `\FULLWIDTH`.

## 4. Build & Kiểm tra
```powershell
latexmk -pdf -synctex=1 -interaction=nonstopmode <main>.tex
```
- Chỉ dùng **pdfLaTeX** (`\usepackage[utf8]{vietnam}`), **không dùng XeLaTeX/LuaLaTeX**.
- Nếu bảng đáp án trống ở lần build đầu (do mới sinh `ans/*.tex`), chạy build lại thêm 1–2 lần.
- Nếu lỗi header/footer lệch hoặc "Dimension too large", xóa `.aux`/`.toc` rồi build lại.

## 5. Checklist trước khi hoàn tất
- [ ] Đủ cặp `\Opensolutionfile{ans}[...]` … `\Closesolutionfile{ans}`.
- [ ] `\choice` có đúng 1 `\True`, không có dòng trống giữa các phương án.
- [ ] `\shortans` ≤ 4 ký tự, dùng `{,}` cho dấu phẩy thập phân.
- [ ] Công thức dùng `\dfrac`, `\heva`/`\hoac`, `\mathscr{D}`, `3\,200`, `\lq\lq ... \rq\rq`.
- [ ] File mới đã được mở trong `inputNN.tex` (nếu người dùng yêu cầu build ngay).
