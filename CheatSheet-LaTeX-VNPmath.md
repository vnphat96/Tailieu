# Cheat sheet — Soạn LaTeX theo bộ tài liệu VNPmath (`D:\Tailieu`)

> Tài liệu này tóm tắt **đúng quy ước đang dùng trong `D:\Tailieu`** để copy–paste khi soạn file mới.
> Nền tảng: `ex_test.sty` **v3.3.4** + preamble nội bộ `cautrucDT/`.
> Đọc thêm khi cần chi tiết:
> - `Hướng dẫn sử dụng ex-test v3.3.pdf`
> - `cautrucDT/Khai-bao-cho-file-mainDT.tex` (toàn bộ lệnh riêng của bộ tài liệu)
> - Mẫu thực tế: `data/10/CK1/0-HK1-CD-1-SoBacNinh-2324.tex` (đề 3 phần), `data/12/2D1-1-Dang1.tex` (bài giảng theo dạng)

---

## 1. Bản đồ thư mục

| Đường dẫn | Vai trò |
|---|---|
| `9.tex`, `10CD.tex`, `11.tex`, `12.tex` | **File main** (lớp `book`) — mỗi khối/mỗi bộ sách một main |
| `input9.tex … input12.tex` | File "mục lục công tắc": mọi `\input{data/...}` bị comment, chỉ mở đúng phần cần in |
| `cautrucDT/colorN\|Y\|P\|B.tex` | Bảng màu (`N` = đen–trắng, `Y` = nhiều màu, `P`, `B`) |
| `cautrucDT/Khai-bao-cho-file-mainDT.tex` | **Preamble dùng chung**: geometry + cột Quick Note, header/footer TikZ, titlesec, theorem, `dang`, `name`, công tắc ẩn/hiện |
| `data/9?`, `data/10/`, `data/11KNTT/`, `data/12/`, `dataDT/`, `dataTHPT/`, `dataDeTHPT/`, `datak40/` | Nội dung (bài giảng, đề) |
| `ans/` | **File đáp án do LaTeX sinh ra** — không sửa tay |
| `logo/`, `QRcode/`, `bia/`, `images/`, `img/`, `hinhve/` | Tài nguyên chèn ảnh, bìa, QR |
| `ex_test.sty`, `casiovn.sty`, `casio580x.sty`, `tkz-tab.sty`, `tikz-3dplot.sty`, `titledot.sty`, `CASIO580.ttf` | Gói/font cục bộ |
| `*.fdb_latexmk`, `*.fls`, `*.synctex.gz` | Dấu vết build bằng `latexmk` + pdfLaTeX (có synctex) |

**Luồng 3 lớp:** `main.tex` → `inputNN.tex` (mở/đóng bằng comment) → `data/....tex` (nội dung).

**Quy ước đặt tên file nội dung:** `2D1-1-Dang1.tex`, `2D1-B1-2.tex`, `0-KNTT-C1B2-X.tex`, `De_1.tex`, `0-HK1-CD-1-SoBacNinh-2324.tex`, `2-OTC5-De1.tex`.
**PDF xuất ra:** `12D1OTC.pdf` (đề) và `12D1OTC-LG.pdf` (lời giải).

---

## 2. Template file main

```latex
\documentclass[10pt,a4paper,onecolumn,titlepage,twoside,openany]{book}

%%%%%%%%%%%%%% KIỂU MÀU + ĐỘ RỘNG CỘT NOTE (phải đặt TRƯỚC \input{Khai-bao...})
\def\kieumau{N}          % N: đen-trắng | Y: màu | P | B
\def\leftnote{5}         % bề rộng cột Quick Note (cm)
\usepackage{xcolor}

%%%%%%%%%%%%%% KHAI BÁO BẮT BUỘC
\input{cautrucDT/color\kieumau}              % MÀU
\input{cautrucDT/Khai-bao-cho-file-mainDT}   % KHAI BÁO CƠ BẢN
\usepackage{currfile}
\usepackage{tkz-euclide,circuitikz}
\usepackage{chessfss}
% \usepackage{wrapfig}

%%%%%%%%%%%%%% ĐIỀU KHIỂN LỜI GIẢI / DÒNG CHẤM / ĐÁP SỐ  (bỏ comment khi cần)
% \dotlinefull{ex}                   % thay \loigiai bằng dòng chấm dài bằng lời giải
% \dotlineans{5}{ex}                 % thay \loigiai bằng 5 dòng kẻ
% \hideansEX{vd}                     % ẩn lời giải của môi trường vd
% \dongchamEXS{1,...,12}{1}          % câu 1..12: 1 dòng chấm (mọi đề)
% \dongchamEX{1/3,2/5,3/7}           % câu/dòng chấm cho từng câu (mọi đề)
% \DEdongchamEXS{3}{1,...,20}{2}     % chỉ áp dụng cho ĐỀ số 3
% \DEdongchamEX{3}{1/3,2/5}
% \showansEX[1,3,7,8,9]{vd}   % v3.3.4: list ĐẶT TRƯỚC tên môi trường
\exitdapso                            % ẩn đáp số (bật)
% \renewcommand{\indapan}[2]{}       % ẩn cả bảng đáp án

%%%%%%%%%%%%%% KHUNG NAME
\def\hoten{Gọi tôi là:}
\def\ngaylamde{Ngày làm đề: ...../...../........}   % để {} nếu không muốn
\def\tenchude{TÊN CHUYÊN ĐỀ}          % in ở header
\def\tendethi{TÊN ĐỀ THI}
\def\tentruong{LỚP TOÁN THẦY PHÁT}
\def\thoigian{Thời gian làm bài: 90 phút}

%%%%%%%%%%%%%% HEADER & FOOTER
\def\diachi{VNPmath - 0962940819}
\def\tenchuyende{\tenchude}
\def\tentacgia{GV.VŨ NGỌC PHÁT}
\def\chamngon{\lq\lq  It's not how much time you have, it's how you use it.\rq\rq}
\def\QRde{12D1OTC}                    % dùng ảnh QRcode/<QRde>.png

%%%%%%%%%%%%%% ĐÁP ÁN & ĐỀ
\khoanhtrondapan          % khoanh đáp án đúng; \khongkhoanhtrondapan nếu không khoanh
\renewcommand{\arraystretch}{1.5}
\OPTN{kindTF=t,dapanTF=a,boldTF=1,phatbieu=Mệnh đề,viettat=1}
% \OPTN{kindSA=oly,ketquaSA=KQ:,widthSA=4,heightSA=0.9,dapanSA=a}

%================= BẮT ĐẦU TÀI LIỆU =================
\begin{document}
\renewcommand{\chaptername}{Chương}
\pagenumbering{arabic}

% -------- ĐỀ BÀI (trang có cột Quick Note)
\NOTE \anLG \anDA
% \tableofcontents
% \includepdf{bia/bia12D4.pdf}
% \setcounter{deso}{5}
\input{input12.tex}

% -------- LỜI GIẢI CHI TIẾT (trang full bề rộng)
\FULLWIDTH \hienLG \hienDA
% \chap{LỜI GIẢI CHI TIẾT} \setcounter{dang}{0} \setcounter{section}{0}
% \input{input12.tex}
\end{document}
```

**Biến cần sửa cho mỗi tài liệu:** `\kieumau`, `\leftnote`, `\tenchude`, `\tendethi`, `\tentruong`, `\thoigian`, `\QRde`, các dòng điều khiển ẩn/hiện, và `\input{inputNN.tex}`.

---

## 3. Template file `inputNN.tex` (mục lục công tắc)

```latex
%Chương I. Hàm số
%%Bài 1. Đơn điệu, Cực trị
% \input{data/12/2D1-B1-1.tex}
% \input{data/12/2D1-B1-2.tex}
%%Bài 2. Max-min
% \input{data/12/2D1-B2-1.tex}

%Chương II. Vector trong KG
% \chap{CHƯƠNG II. VECTƠ VÀ HỆ TRỤC TOẠ ĐỘ TRONG KHÔNG GIAN}
% \setcounter{deso}{0}
% \input{datak40/2H2/2H2-B1-1.tex}

% ----- PHẦN ĐANG IN -----
\input{data/12/2H5/2-OTC5-De1.tex}
\input{data/12/2H5/2-OTC5-De2.tex}
% \foreach \i in {1,2,...,7} {\input{data/12/C5B2/C5B2CD\i.tex}}
```

Quy ước: **mọi dòng để comment `%`**, chỉ bỏ comment đúng phần cần build; giữ cấu trúc chương/bài bằng comment để tái dùng.

---

## 4. Hai chế độ trang (đổi bằng `\NOTE` / `\FULLWIDTH`)

| Lệnh | Tác dụng |
|---|---|
| `\NOTE` | Sang trang mới, khôi phục cột phải (Quick Note: logo, `\chamngon`, ô ĐIỂM, QUICK NOTE + 60 dòng chấm), geometry `right=\phai+\leftnote` |
| `\FULLWIDTH` | Sang trang mới, bỏ cột note (`\def\notename{}`), geometry full bề rộng — dùng cho phần **lời giải chi tiết** |
| `\notename` | Vẽ khối cột note (được gọi tự động trong `\name`; gọi tay ở đầu file bài giảng). **Sau `\FULLWIDTH` lệnh này không in gì** |
| `\myfancyhead`, `\myfancyfoot`, `\headmucluc` | Header/footer TikZ (tự đổi trái–phải theo trang chẵn/lẻ) |

> Cả hai lệnh đều gọi `\newpage` + `\newgeometry` → **chỉ đặt ở đầu phần/đầu trang**, không đặt giữa đoạn văn.

---

## 5. Template bài giảng / chuyên đề

```latex
\notename
\chap{ỨNG DỤNG CỦA ĐẠO HÀM}
\section{SỰ ĐỒNG BIẾN, NGHỊCH BIẾN CỦA HÀM SỐ}
\Opensolutionfile{ans}[ans/ansCD2D1-1]

\subsection{Tóm tắt lý thuyết}
\subsubsection{Định nghĩa}
Cho hàm số $f(x)$ xác định trên khoảng $K$.
\begin{note}
Hàm số đồng biến hoặc nghịch biến trên $K$ gọi chung là hàm số đơn điệu trên $K$.
\end{note}

\dn Sách KNTT định nghĩa ...
\dl Cho hàm số $f$ có đạo hàm trên $I$, khi đó ...
\tc ...
\hq ...
\nx ...

\subsection{Phân loại và phương pháp giải bài tập}
\begin{dang}{Xét tính đơn điệu của hàm số cho bởi công thức}
	\viduminhhoa
	\begin{vd}%[2D1B1-2]%Câu 1.
		Tìm các khoảng đồng biến, nghịch biến của hàm số $y=\dfrac{x+1}{x+3}$.
		\loigiai{
			Tập xác định $\mathscr{D}=\mathbb{R}\setminus\{-3\}$.\\
			$y'=\dfrac{2}{(x+3)^2}>0,\forall x\neq-3$.}
	\end{vd}
	\baitaptn
	\begin{ex}%[2D1B1-2]%Câu 2.
		...
		\choice
		{...}
		{\True ...}
		\loigiai{...}
	\end{ex}
\end{dang}

\Closesolutionfile{ans}
```

**Đặc điểm cần nhớ**
- `\section{...}` tự in thành **“Bài n.”**; `\subsection` → **A.**; `\subsubsection` → **1.**; `\chap{...}` là lệnh chương riêng (căn giữa + vào mục lục).
- `\begin{dang}{Tên dạng}` **tự tăng bộ đếm `dang`** và **reset `vd`, `ex`, `bt`, `subsubsection`**.
- `\begin{vd}` → “VÍ DỤ”, `\begin{bt}` → “BÀI”, `\begin{ex}` → “CÂU” (đếm liên tục trong cả tài liệu nếu không reset).
- Các tiện ích: `\viduminhhoa`, `\baitaptl`, `\baitaptn`, `\muccon{...}`, `\begin{phantich}...\end{phantich}`, `\khung{...}`, `\boxmini{...}`.

---

## 6. Template đề thi (3 phần)

```latex
%========================================== KHUNG TÊN ĐỀ
\begin{name}
	{ÔN TẬP KIỂM TRA CUỐI HỌC KÌ 1}
	{TOÁN 10}
	{LỚP TOÁN THẦY PHÁT}
	{\thoigian}
\end{name}
% #1: tiêu đề chính | #2: môn-khối (tự thêm "-- ĐỀ n") | #3: trường | #4: thời gian
% Mỗi \begin{name} tự động sang trang mới (khi deso > 0) và thêm 1 dòng vào mục lục

%========================================== PHẦN I. 4 phương án
\caulc
\Opensolutionfile{ans}[ans/0-HK1-CD-1-SoBacNinh-2324-Phan-I]
%%%============EX_1==============%%%
\begin{ex}%[0-HK1-CD-1-SoBacNinh-2324]%[VN-MT-6, Tống Văn Ký]%[0D3H1-2]
	Tập xác định của hàm số $y=\dfrac{2}{\sqrt{x+1}}$ là
	\choice
	{$\mathbb{R}\setminus \{-1\}$}
	{$[-1;+\infty)$}
	{\True $(-1;+\infty)$}          % \True đánh dấu đáp án đúng
	{$(1;+\infty)$}
	\loigiai{
		Hàm số xác định $\Leftrightarrow x+1>0 \Leftrightarrow x>-1$.\\
		Vậy tập xác định là $(-1;+\infty)$.
	}
\end{ex}
% --- câu có hình: dùng \immini{đề bài}{\begin{tikzpicture}...\end{tikzpicture}}
% --- câu nhiều hơn 4 phương án: \choiceN ; câu 2 cột: \choice[2]
\Closesolutionfile{ans}

%========================================== PHẦN II. Đúng/Sai
\cauds
\Opensolutionfile{ans}[ans/0-HK1-CD-1-SoBacNinh-2324-Phan-II]
\begin{ex}%[0-HK1-CD-1-SoBacNinh-2324]%[VN-MT-6, Tống Văn Ký]%[0D3H1-2]
	Cho hàm số $y=\sqrt{3-2x}$ có tập xác định là $\mathscr{D}$.
	\choiceTF
	{$\mathscr{D}=\left[\dfrac{3}{2};+\infty\right)$}
	{\True $x_0=1\in \mathscr{D}$}
	{\True $(-3;0) \subset \mathscr{D}$}
	{$\left(\dfrac{3}{2};3\right) \cap \mathscr{D}=\left\{\dfrac{3}{2}\right\}$}
	\loigiai{
		\begin{itemchoice}
		\itemch {\bf Sai.}\\ Điều kiện: $3-2x\ge 0\Leftrightarrow x\le \dfrac{3}{2}$.
		\itemch {\bf Đúng.}\\ ...
		\itemch {\bf Đúng.}\\ ...
		\itemch {\bf Sai.}\\ ...
		\end{itemchoice}
	}
\end{ex}
\Closesolutionfile{ans}

%========================================== PHẦN III. Trả lời ngắn
\caukq
\Opensolutionfile{ans}[ans/0-HK1-CD-1-SoBacNinh-2324-Phan-III]
\begin{ex}%[0-HK1-CD-1-SoBacNinh-2324]%[VN-MT-6, Tống Văn Ký]%[0D3N1-3]
	Cho hàm số $y=\heva{&\dfrac{2}{x-1}, &x \in(-\infty; 0) \\ &\sqrt{x+1}, &x \in[0; \infty)}$.
	Giá trị $f(0)+f(-1)$ bằng bao nhiêu?
	\shortans{$0$}                      % tối đa ~4 ký tự, dùng dấu phẩy thập phân {,}
	\loigiai{
		Với $x=0$ ta có $f(0)=1$; với $x=-1$ ta có $f(-1)=-1$. Vậy $f(0)+f(-1)=0$.}
\end{ex}
\Closesolutionfile{ans}
```

**Câu ghép cặp (nếu dùng):** `\match{...}{...}{...}{...} \with [3]{...}{...}[1]{...}[2,4]{...}`
**Bài tự luận:** `\begin{bt}[nguồn] ... \dapso{đáp số} \loigiai{...} \end{bt}`
**Nhóm câu chung giả thiết:** `\sochc{<số câu>}` + `\begin{chc}...\end{chc}`

---

## 7. File trong `ans/` — máy tự sinh

Sinh ra bởi cặp lệnh `\Opensolutionfile{ans}[ans/<tên>]` … `\Closesolutionfile{ans}`.
**Không sửa tay.** Dạng nội dung:

```latex
% Phần I — \choice
\begin{Solution}{1}
C
\end{Solution}

% Phần II — \choiceTF  (\circT = đúng, \circF = sai)
\begin{Solution}[\nameex ]{13}
\circF{a}\circT{b}\circT{c}\circF{d}
\end{Solution}

% Phần III — \shortans
\begin{Solution}[\nameex ]{17}
\dapsoSA{$-2$}
\end{Solution}
```

In bảng đáp án:
`\inputansbox{10}{ans/<file Phần I>}` · `\inputansbox{4}{ans/<file Phần II>}` · `\inputansbox{6}{ans/<file Phần III>}` · `\inputansbox{5}{ans/<file tự luận>}`
(`\inputansbox[0]`, `[1]` đổi kiểu hiển thị đáp án Đ/S; `\inputanstab{cols}{file}` cho dạng biểu bảng).
Trong bộ tài liệu, `\hienDA` / `\anDA` đã bọc sẵn hai lệnh này.

> Nếu chưa có file trong `ans/` → chạy build **1 lần**, LaTeX sẽ sinh file rồi lần sau mới in được bảng đáp án.

---

## 8. Bảng tra lệnh

### 8.1 Môi trường

| Môi trường | Dùng cho |
|---|---|
| `ex` | Câu trắc nghiệm / câu hỏi (đếm `ex`) |
| `vd` / `bt` | Ví dụ / Bài |
| `dn` `dl` `tc` `hq` `nx` | Định nghĩa / Định lí / Tính chất / Hệ quả / Nhận xét |
| `note` | Khung "Lưu ý" (icon tam giác, chữ nghiêng, không nền) |
| `dang` | Khung Dạng toán (tự tăng số dạng, reset bộ đếm con) |
| `phantich` | Khung "PHÂN TÍCH" (nền cyan, viền trái) |
| `khung` `boxmini` `boxdn` | Khung/hộp phụ trợ |
| `name` | Khung tên đề (4 tham số) |
| `itemchoice` + `\itemch` | Lời giải 4 ý Đúng/Sai |
| `listEX` `enumEX` `tasks` | Danh sách chia cột (ex_test) |
| `Solution` | Chỉ trong file `ans/` |

### 8.2 Lệnh tắt hay dùng

| Lệnh | Ý nghĩa |
|---|---|
| `\hoac{&a\\&b}` / `\heva{&a\\&b}` | Hệ **hoặc** / hệ **và** (đã canh `aligned`) |
| `\vec{AB}`, `\overrightarrow{AB}` | Vectơ (`esvect`) |
| `\indam{}`, `\indamm{}`, `\inden{}`, `\ind{}` | In đậm sans (định nghĩa / định lí / chấm) |
| `\tron{n}` | Số trong hình tròn (dùng cho mục lục) |
| `\muccon{}` | Mục con tự đếm (1., 2., …) |
| `\caulc` = `\TN`, `\cauds` = `\TNTF`, `\caukq` = `\TNSA`, `\TL` | In câu dẫn "Phần I / II / III / IV" |
| `\BTTN` `\BTTF` `\BTTL` | Khung "BÀI TẬP TRẮC NGHIỆM 4 PHƯƠNG ÁN / ĐÚNG SAI / TỰ LUẬN TRẢ LỜI NGẮN" |
| `\viduminhhoa` `\baitaptl` `\baitaptn` | Subsubsection "Ví dụ minh hoạ / Bài tập tự luận / Bài tập trắc nghiệm" |
| `\damEX` `\dam` `\damTT` `\fontchap` | Font tiêu đề (`ugqb8v`) ở các cỡ |
| `\qedEX` | Dấu kết thúc chứng minh |
| `\loigiaiEX` / `\loigiaiEXS` | Nhãn "Lời giải" / "Lời giải + dòng chấm" |
| `\tickv` `\itemKN` `\itemCI` `\iconGN` `\iconNS` `\iconQS` `\iconMT` `\iconX` `\iconCH` `\iconVD` `\iconCV` | Icon (fontawesome) |
| `\nameex` | Nhãn câu ("CÂU") — đổi bằng `\renewcommand{\nameex}` |
| `\tatsode` / `\batsode` | Tắt/bật lại chế độ có số đề + tự sang trang khi ghép nhiều đề liền nhau |
| `\hienEXS{41}{50}` | Chỉ in câu 41→50 của mỗi đề |
| `\setcounter{deso}{n}` | Đặt đề bắt đầu là số n |
| `\setcounter{ex}{k}` | Bắt đầu lại đánh số câu từ k+1 |

### 8.3 Ẩn/hiện & dòng chấm

| Lệnh | Tác dụng |
|---|---|
| `\hienLG` / `\anLG` | Hiện / ẩn **lời giải** |
| `\hienDA` / `\anDA` | Hiện / ẩn **bảng đáp án** |
| `\exitdapso` / `\showdapso` | Ẩn / hiện **đáp số** ở cuối câu |
| `\hideansEX[3,7,12]{ex}` · `\showansEX[1,3]{vd}` | Ẩn/hiện lời giải theo **danh sách câu** |
| `\exithideansEX{ex}` · `\exitshowansEX{ex}` | Ngắt áp dụng ẩn/hiện |
| `\hidetextchoice` / `\showtextchoice` | Ẩn/hiện dòng "Chọn đáp án" cuối lời giải |
| `\hidechoice[3,7]{ex}` | Ẩn 4 phương án của (các) câu chỉ định |
| `\dotlinefull[cols]{ex}` | Thay lời giải bằng dòng chấm dài bằng lời giải |
| `\dotlineans[cols]{số dòng}{ex}` | Thay lời giải bằng đúng số dòng kẻ |
| `\dotlineEX[số cột]{số dòng}` | Dòng chấm tại chỗ (không cần `\loigiai`) |
| `\dotlineEXS{n}`, `\dongcham{n}`, `\cham{n}` | Biến thể dòng chấm của bộ tài liệu |
| `\dongchamEX{1/3,2/5}` · `\dongchamEXS{1,...,12}{1}` | Dòng chấm theo từng câu / theo nhóm câu |
| `\DEdongchamEX{3}{1/3}` · `\DEdongchamEXS{3}{1,...,20}{2}` | Như trên nhưng **chỉ 1 đề** |
| `\dotlineOPTN{dotsize=8pt,dotcolor=black,linespacing=1,linebetween=0.5pt,linebetweencolor=black}` | Định dạng dòng chấm |

> ⚠️ **Cú pháp `\hideansEX` / `\showansEX` đã đổi:** `ex_test.sty` trong thư mục là **v3.3.4** → danh sách câu là **tuỳ chọn đặt TRƯỚC**: `\hideansEX[3,7,12]{ex}`, `\showansEX[1,3]{vd}`.
> Các comment trong `9.tex`/`12.tex` lại viết theo **v3.1** (`\showansEX{vd}[1,3,...]`) — nếu bỏ comment theo kiểu cũ thì `[1,3,...]` sẽ bị in ra như chữ bình thường.
> `\dotlinefull` / `\dotlineans` cho phép liệt kê nhiều môi trường: `\dotlineans{5}{ex}{bt}`, `\dotlinefull{ex}{bt}{vd}`.

### 8.4 Lệnh của `ex_test`

`\begin{ex}[nguồn]`, `\choice[tuỳ chọn]`, `\choiceN` (chọn nhiều), `\choiceTF[tuỳ chọn]`, `\shortans[tuỳ chọn]{kq}`, `\match … \with`, `\drag/\drop`, `\True`, `\False`, `\loigiai{...}`, `\immini[thm]{chữ}{hình}`, `\imminiL{hình}{chữ}`, `\Opensolutionfile{ans}[đường dẫn]`, `\Closesolutionfile{ans}`, `\inputansbox[kiểu]{số cột}{file}`, `\inputanstab{cols}{file}`.

---

## 9. `\OPTN` — tuỳ chọn dùng chung (đặt gần `\begin{document}`)

**Đúng/Sai**
```latex
\OPTN{kindTF=t,dapanTF=a,boldTF=1,phatbieu=Mệnh đề,viettat=1}
```
| Khoá | Giá trị |
|---|---|
| `kindTF` | `0` dạng chuẩn đề 2025 · `t` tự chọn 1–2 bảng · `1t` 1 bảng · `2t` 2 bảng · `t0` bỏ dòng tiêu đề + bỏ cột Đ/S · `t01` bỏ tiêu đề, giữ 2 cột Đ/S · `t10` giữ tiêu đề, bỏ 2 cột Đ/S · `tDS` gộp 1 cột Đ/S · để trống = theo từng câu |
| `dapanTF` | `a` → a) b) c) d) · `1` → 1) 2) 3) 4) · `A` → A. B. C. D. |
| `sepTF` | dấu sau nhãn (mặc định `)`) |
| `boldTF` | `1` in đậm tiêu đề bảng |
| `phatbieu` | sửa chữ "Phát biểu" |
| `viettat` | `1` → "Đ"/"S"; `0` → "Đúng"/"Sai" |
| `cotSTT` `dongPB` `cotDS` | hiện/ẩn cột Thứ tự, dòng Tiêu đề, tách/gộp cột Đ–S |
| `saplai` | `1` tự kéo phát biểu ngắn lên cùng dòng |
| `addanswers` `addquestions` | tự thêm Đ/S hoặc đề bài vào `itemchoice` |
| `explain` `exbreak` | đưa trích dẫn nguồn xuống cuối câu / tự xuống dòng khi có nguồn |

**Trả lời ngắn**
```latex
\OPTN{kindSA=oly,ketquaSA=KQ:,widthSA=4,heightSA=0.9,dapanSA=a}
```
| Khoá | Giá trị |
|---|---|
| `kindSA` | `0` dạng chuẩn · `oly` 4 ô ly · `...` dấu chấm · `n` ô dài n cm |
| `ketquaSA` | chữ trước ô điền kết quả (mặc định `KQ:`) |
| `widthSA` `heightSA` | kích thước ô (mặc định 4 và 0.9) |
| `dapanSA` | `a` / `1` / `A` — kiểu đánh thứ tự ý nhỏ trong bảng đáp án |

---

## 10. Nhãn câu, nguồn và mã ID

```latex
\begin{ex}%[0-HK1-CD-1-SoBacNinh-2324]%[VN-MT-6, Tống Văn Ký]%[0D3H1-2]
```
Ba nhóm comment trên **cùng dòng `\begin{ex}`**:  → `%[Nguồn đề]` %[Người biên soạn, dự án] %[Mã ID].
Mã ID (tài liệu ex-test gọi là `ID6`) có dạng `<lớp><mạch D/H><chương><mức><đơn vị kiến thức>-<chỉ số>`, ví dụ `2D1B1-2`, `2H3K4-1`, `0H5V3-8`, `0D3N1-3`. **Cách an toàn nhất: giữ nguyên mã của file nguồn / copy từ đề gốc.**

Biến thể chính thức cũng dùng được: `\begin{ex}[nguồn câu hỏi]`.
Chú thích thêm đặt ngay sau: `%Câu 1.`, `%%%============EX_1==============%%%` (dùng để dễ tìm khi file dài).

---

## 11. Quy ước trình bày

### 11.1 Hình vẽ (TikZ)

```latex
\immini[thm]{Cho hàm số ... có đồ thị như hình vẽ. Mệnh đề nào sau đây đúng?
	\choice{$a>0$}{\True $a<0$}{$a=0$}{$a\ge 0$}
}{
	\begin{tikzpicture}[scale=1, font=\footnotesize, line join=round, line cap=round, >=stealth]
		\def\xmin{-3}\def\xmax{1}\def\ymin{-0.5}\def\ymax{2.5}
		\draw[->] (\xmin,0)--(\xmax,0) node[below] {$x$};
		\draw[->] (0,\ymin)--(0,\ymax) node[right] {$y$};
		\draw (0,0) circle (1pt) node [above left] {$O$};
		\clip (\xmin,\ymin) rectangle (\xmax,\ymax);
		\draw[smooth,samples=200,domain=\xmin:\xmax] plot (\x,{-1*((\x)^2)-2*\x+1});
	\end{tikzpicture}
}
```
- Dựng điểm theo kiểu `\path (0,0) coordinate (A) (5,0) coordinate (C) ...;` rồi mới `\draw`.
- Nhãn điểm: `\foreach \x/\g in {A/90,B/-90,M/60}{\fill (\x) circle (1pt)+(\g:3mm) node{$\x$};}`
- Góc vuông: `\pic[draw,angle radius=2mm]{right angle=E--J--B};`
- Cung/góc: `\begin{scope}\clip (B)--(M)--(A)--cycle;\draw (M) circle(3mm);\end{scope}` + `\node at ($(M)+(160:7mm)$) {$120^{\circ}$};`
- Bảng biến thiên (`tkz-tab`):
```latex
\begin{tikzpicture}[scale=1, font=\footnotesize, line join=round, line cap=round, >=stealth]
	\tkzTabInit[nocadre=false,lgt=1,espcl=2.5,deltacl=0.5]{$x$/.7 ,$y'$/.7,$y$/2}
	{$-\infty$ , $-1$ , $1$ , $+\infty$}
	\tkzTabLine{ , - , $0$ , + , $0$ , - , }
	\tkzTabVar{+/$0$ , -/$-\dfrac{1}{2}$ , +/$\dfrac{1}{2}$ , -/$0$}
\end{tikzpicture}
```
- Hình học phẳng: thêm `\usepackage{tkz-euclide}` (đã có trong main). 3D: `tikz-3dplot`.

### 11.2 Toán

- Ưu tiên `\dfrac` khi phân số đứng trong câu (đẹp hơn `\frac`).
- Hệ/hợp: `\heva{}` / `\hoac{}` thay cho `cases`.
- Tập xác định: `\mathscr{D}`; tập hợp số: `\mathbb{R}`, `\mathbb{N}`, `\mathbb{Z}`.
- **Dấu phẩy thập phân:** `2{,}4` (không viết `2,4` trần) — quan trọng trong `\shortans{$2{,}4$}`.
- Dấu cách nghìn: `3\,200`.
- Chữ trong công thức: `\text{ khi }`, `\text{d}x`, `\text{ (đvdt)}`.
- Vectơ: `\vec{AB}` hoặc `\overrightarrow{AB}` (đã alias).
- Kết thúc chứng minh/lời giải: `\qedEX`.
- Công thức dài nhiều dòng: `\begin{eqnarray*} a&=&b\\&=&c\\ \end{eqnarray*}`.

### 11.3 Chữ & trình bày

- Viết tiếng Việt có dấu, câu kết thúc bằng dấu chấm; mỗi ý trong lời giải xuống dòng bằng `\\`.
- Trích dẫn: `\lq\lq ... \rq\rq`.
- Mục con trong lời giải: `\begin{itemize}...\end{itemize}`, `\begin{enumerate}...`.
- Nội dung quan trọng: `\indam{...}` (đậm sans).
- Bullet mặc định của `itemize` là `\faCheckCircleO` màu `\mauitem`; `labelenumi` = `a)`.
- `\setlength{\parindent}{0pt}` — không thụt đầu dòng (đã đặt sẵn).

---

## 12. Build

```bash
cd /d D:\Tailieu
latexmk -pdf -synctex=1 -interaction=nonstopmode 12.tex
```

- Bộ tài liệu dùng **pdfLaTeX** (`\usepackage[utf8]{vietnam}`) → **không dùng XeLaTeX/LuaLaTeX**.
- Cần chạy **2–3 lần** (mục lục, bảng đáp án, header/footer TikZ `remember picture`).
- Lần build đầu tiên sinh các file trong `ans/` → nếu bảng đáp án trống, build lại.
- Dọn rác khi cần: `latexmk -c` (giữ PDF) hoặc `latexmk -C`.
- Lỗi header/footer hoặc dòng chấm lệch → xoá `.aux`, build lại từ đầu.
- `%OUTDIR%/` là thư mục đầu ra của lần build có `-outdir`; giữ lại các tệp `.tex` được sinh trong đó nếu còn cần, còn các tệp phụ biên dịch đã được bỏ qua bởi `.gitignore`.

---

## 13. Checklist trước khi build

- [ ] `\def\kieumau`, `\def\leftnote` nằm **trước** `\input{cautrucDT/Khai-bao-cho-file-mainDT}`.
- [ ] Đã cập nhật `\tenchude`, `\tendethi`, `\tentruong`, `\thoigian`, `\diachi`, `\QRde`.
- [ ] Mỗi nhóm câu được bọc bởi `\Opensolutionfile{ans}[ans/<tên>]` … `\Closesolutionfile{ans}` (đủ và khớp).
- [ ] Mỗi `\choice` có **đúng một** `\True`; mỗi `\choiceTF` đánh dấu `\True` cho từng phát biểu **đúng**.
- [ ] `\shortans{...}` ≤ 4 ký tự, dùng `{,}` cho số thập phân.
- [ ] Mỗi `\begin{ex}` có 3 nhóm comment `[...]` (nguồn / người soạn / mã ID) cùng dòng.
- [ ] File mới đã được mở trong `inputNN.tex`.
- [ ] Đã đặt `\NOTE \anLG \anDA` (in đề) và `\FULLWIDTH \hienLG \hienDA` (in lời giải) đúng chỗ.
- [ ] Hình TikZ có `\clip` khi vẽ đồ thị; nhãn điểm dùng `\foreach`; không tràn ra ngoài khung.
- [ ] Kiểm tra trang chẵn/lẻ: header/footer tự đảo bên theo `twoside`.

---

## 14. Lỗi thường gặp

| Hiện tượng | Nguyên nhân / cách xử lý |
|---|---|
| Bảng đáp án trống | Quên `\Closesolutionfile{ans}` hoặc mới build lần đầu (file `ans/*.tex` chưa sinh) |
| Mất cột Quick Note | Đang ở chế độ `\FULLWIDTH`, hoặc gọi `\notename` sau `\FULLWIDTH` |
| `\notename` không in gì | Bị `\FULLWIDTH` đặt `\def\notename{}` — gọi `\NOTE` trước |
| Lỗi "Dimension too large" / header lệch | Xoá `.aux`, `.toc`, build lại |
| Câu không đánh số lại | Chưa `\setcounter{ex}{...}` hoặc chưa vào `\begin{dang}` mới |
| Bảng biến thiên lỗi | Thiếu `tkz-tab.sty` / sai số phần tử giữa `\tkzTabInit`, `\tkzTabLine`, `\tkzTabVar` |
| `.pdf` không cập nhật | Đang mở PDF trong trình xem; đóng rồi build lại |
| Số thập phân trong `\shortans` lỗi | Phải viết `$2{,}4$` |
| Chữ Việt lỗi font | Đang build bằng XeLaTeX → chuyển về pdfLaTeX |
| `\choice` báo lỗi đếm dòng | Thừa/thiếu dấu `{}` một phương án, hoặc có dòng trống giữa các phương án |

---

*Cheat sheet tạo từ việc đọc trực tiếp `D:\Tailieu` (main files, `cautrucDT/`, `ex_test.sty`, file đề/bài giảng mẫu và 2 PDF hướng dẫn ex-test). Khi quy ước trong thư mục thay đổi, cập nhật lại file này cho khớp.*
