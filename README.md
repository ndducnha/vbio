# BioAge — Landing page

Trang landing BioAge: ước tính tuổi sinh học từ xét nghiệm máu thường quy và các đồng hồ sinh học dựa trên bằng chứng. HTML + React (dc-runtime) tĩnh, chạy trực tiếp không cần build step.

## Chạy thử ở máy
```bash
python3 -m http.server 8000
# mở http://localhost:8000
```
> Mở trực tiếp `index.html` cũng chạy, nhưng nên dùng http server để font/ảnh nạp ổn định.

## Deploy GitHub Pages
1. Đẩy toàn bộ thư mục này lên nhánh `main`.
2. **Settings → Pages → Build and deployment → Deploy from a branch**.
3. Chọn `main` / `/ (root)` → Save. Vài phút sau site chạy tại `https://<user>.github.io/<repo>/`.

File `.nojekyll` đã có sẵn để GitHub Pages phục vụ nguyên trạng.

## Cấu trúc (phẳng — tất cả cùng cấp)
```
index.html                     # trang landing (template + logic)
dc-runtime.js                  # runtime render (nạp React)
react.production.min.js        # React 18.3.1 (local)
react-dom.production.min.js
bioage-logo.png                # logo chữ + favicon
bioage-atlas.png               # ảnh Human Biomarker Atlas
f-*.woff2                      # font (Space Grotesk, Inter, JetBrains Mono)
```
React nạp local qua `window.__resources`, nên trang chạy được cả khi không có mạng.

## Miễn trừ
BioAge cung cấp kết quả ước tính dựa trên dữ liệu bạn cung cấp và mô hình tính toán. Kết quả không thay thế chẩn đoán, tư vấn hoặc điều trị từ chuyên gia y tế.
