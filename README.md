# VBio — Landing page

Landing page giới thiệu VBio: phân tích chỉ số sinh học, ước tính **tuổi sinh học** và lộ trình cải thiện. HTML + React (dc-runtime) tĩnh, chạy trực tiếp không cần build step.

## Chạy thử ở máy
```bash
python3 -m http.server 8000
# mở http://localhost:8000
```
> Mở trực tiếp `index.html` bằng `file://` cũng chạy, nhưng nên dùng http server để font/ảnh nạp ổn định.

## Deploy GitHub Pages
1. Đẩy toàn bộ thư mục này lên nhánh `main`.
2. **Settings → Pages → Build and deployment → Deploy from a branch**.
3. Chọn `main` / `/ (root)` → Save. Vài phút sau site chạy tại `https://<user>.github.io/<repo>/`.

File `.nojekyll` đã có sẵn để GitHub Pages phục vụ nguyên trạng thư mục `assets/`.

## Cấu trúc (phẳng — tất cả cùng cấp, không thư mục con)
```
index.html                    # trang landing (template + nội dung)
dc-runtime.js                 # runtime render (nạp React)
react.production.min.js       # React 18.3.1 (local, không phụ thuộc CDN)
react-dom.production.min.js
vbio-mark.png                 # icon / favicon
vbio-lockup.png               # logo chữ
vbio-src.png                  # logo gốc
f-*.woff2                     # font (Space Grotesk, Inter, JetBrains Mono)
```
Chỉ cần kéo-thả tất cả file vào repo GitHub là chạy.
React được nạp local qua `window.__resources`, nên trang chạy được cả khi không có mạng tới unpkg.

## Miễn trừ
VBio cung cấp kết quả ước tính dựa trên dữ liệu bạn cung cấp và mô hình tính toán. Kết quả không thay thế chẩn đoán, tư vấn hoặc điều trị từ chuyên gia y tế.
