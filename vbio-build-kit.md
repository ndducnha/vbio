# VBio — Build Kit (HTML/CSS/JS thuần cho GitHub Pages)

## 0. Cấu trúc thư mục
```
vbio/
├── index.html
├── style.css
├── script.js
└── assets/
    ├── vbio-mark.png     (icon V + DNA, nền trong suốt)
    └── vbio-lockup.png   (logo chữ "Vbio", nền trong suốt)
```
Deploy: đẩy repo lên GitHub → Settings → Pages → Deploy from branch → `main` / `/root`.

---

## 1. Design system
**Fonts (Google Fonts):** Space Grotesk (tiêu đề + số), Inter (body), JetBrains Mono (nhãn/nhỏ).
```
https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;600;700&family=Inter:wght@400;500;600&family=JetBrains+Mono:wght@400;500&display=swap
```
**Màu — Hero (nền tối):** nền `linear-gradient(180deg,#04130F,#050A15,#03060D)` + vệt sáng emerald/indigo/gold. Hạt: emerald `#34E6BE`, aqua `#60E0EA`, platinum `#EBF7FF`, gold `#E4C67E`, violet `#9684FF`.
**Màu — Body (nền sáng):** nền `#EEF3F1`; chữ `#13221E` / phụ `#5C716B` / mờ `#7E8D92`; accent xanh `#0FA37F`, cyan `#0C93A6`, tím `#6B4EE0`; cảnh báo `#DD6238`. Card: nền trắng, viền `rgba(18,45,40,.09)`.

---

## 2. Sơ đồ trang (1 trang cuộn)
`Hero (full màn hình, tối)` → `Phân tích/Upload` → `Form + Độ đầy đủ` → `Chạy phân tích (8 bước)` → `Kết quả` → `Yếu tố ảnh hưởng` → `Mô phỏng (slider)` → `Lộ trình 30/90/180` → `Báo cáo` → `Đội ngũ` → `Footer`.

---

## 3. Hero — animation kể chuyện (canvas, loop ~13s)
Tổng chu kỳ 13000ms, 6 cảnh (ms): `[2200, 2000, 2200, 2400, 2600, 1600]`.
1. **Digital Human** — ~520 hạt xếp thành hình người (đầu/thân/hông/2 chân/2 tay), có đường nối kiểu neural.
2. **Tan rã** — hạt tán ra vòng tròn; 17 chỉ số bay quanh: Vitamin D, HbA1c, Glucose, HDL, LDL, CRP, Ferritin, Insulin, Testosterone, Sleep, Heart Rate, Stress, BMI, Muscle Mass, Inflammation, Protein, Hormone.
3. **VBio Engine** — hạt hút về tâm thành quả cầu năng lượng xoay (neural + glow).
4. **DNA** — hạt xếp thành chuỗi xoắn kép quay chậm, có rung nối 2 sợi.
5. **Kết quả** — hạt mờ đi; hiện Tuổi thực **42** → Tuổi sinh học đếm động về **34.8**; Chênh lệch **−7.2 năm**, Tiềm năng **Xuất sắc**.
6. **Quay lại** hình người → loop.
Kỹ thuật: 1 mảng hạt dùng chung, mỗi cảnh đặt "vị trí đích" cho hạt rồi nội suy `p.x += (target-p.x)*0.12` → tự morph mượt. Vẽ bằng `globalCompositeOperation='lighter'` để phát sáng. Thêm twinkle (alpha dao động sin) + vài hạt "sao" loé chữ thập + vignette. Tôn trọng `prefers-reduced-motion`.

**Text hero:** logo `assets/vbio-lockup.png` + phụ đề "Giải mã sinh học. Hiểu quá trình lão hóa của bạn." + nút "Bắt đầu phân tích" / "Khám phá công nghệ".

---

## 4. Mô hình tính tuổi sinh học (JS) — để số Kết quả & Mô phỏng khớp nhau
```js
function clamp(x,a,b){return Math.max(a,Math.min(b,x));}
function computeBio(v){                     // v = {vitd,sleep,bodyfat,exercise,glucose,chol,stress,weight}
  const c=x=>clamp(x,0,1);
  const n={
    vitd:c((v.vitd-15)/25),
    sleep:c(1-Math.abs(v.sleep-7.5)/3.5),
    bodyfat:c((30-v.bodyfat)/12),
    exercise:c(v.exercise/5),
    glucose:c((115-v.glucose)/30),
    chol:c((230-v.chol)/60),
    stress:c((8-v.stress)/6),
    weight:c(1-Math.abs(v.weight-62)/25)
  };
  const w={vitd:1.4,sleep:1.6,bodyfat:1.6,exercise:1.4,glucose:1.2,chol:1.0,stress:1.2,weight:0.8};
  let s=0; for(const k in w) s+=n[k]*w[k];
  return clamp(40.2 - s, 29, 44);
}
// Giá trị hiện tại (ra ~36.8):
const current={vitd:18,sleep:5.8,bodyfat:29,exercise:2,glucose:105,chol:210,stress:6,weight:68};
// Mô phỏng: copy current, cho user kéo slider, gọi lại computeBio(sim).
// Cải thiện = computeBio(current) - computeBio(sim).
```
Slider mô phỏng (min/max/current): Vitamin D 10–60/18 · Sleep 4–9/5.8 · Body fat 15–40/29 · Buổi tập 0–7/2 · Glucose 80–140/105 · Cholesterol 150–260/210 · Stress 1–10/6 · Weight 50–95/68. Nút "kịch bản tốt nhất": `{vitd:42,sleep:7.5,bodyfat:22,exercise:5,glucose:92,chol:175,stress:3,weight:64}`.

---

## 5. Nội dung (tiếng Việt)
**Kết quả:** Tuổi sinh học 36.8 · Tuổi thực 42 · Chênh lệch 5.2 năm trẻ hơn · Tốc độ lão hóa 0.88 · Điểm sức khỏe 82/100 · Độ tin cậy: Cao.
**Yếu tố tích cực:** Đường huyết ổn định (−0.9), Chức năng gan tốt (−0.7), Vận động đều đặn (−0.6), Huyết áp ổn định (−0.5).
**Yếu tố thúc đẩy lão hóa:** Vitamin D thấp (18, ngưỡng 30–50, +0.8), Viêm cao (CRP 3.4, +0.7), Ngủ kém (5.8 giờ, mục tiêu 7–8, +0.9), Mỡ & stress cao (mỡ 29%, +1.1).
**8 bước xử lý:** Đọc tài liệu → Trích xuất chỉ số → Chuẩn hóa giá trị → So sánh khoảng tham chiếu → Chạy mô hình lão hóa → Tính tuổi sinh học → Xác định yếu tố → Tạo kịch bản cải thiện.
**Lộ trình:** 30 ngày (ngủ, vận động, kiểm tra Vitamin D, giảm đường) · 90 ngày (giảm mỡ, cải thiện viêm, theo dõi đường huyết, tim mạch) · 180 ngày (xét nghiệm mới, so sánh, cập nhật mô hình, điều chỉnh kế hoạch).
**Đội ngũ:** Dr. Đào Thị Mai Lan — Nhà sáng lập · Sinh học tế bào gốc & gen. Dr. Nguyễn Đình Đức Nhã (Tony) — Cố vấn kỹ thuật · AI & khoa học dữ liệu.
**Disclaimer (bắt buộc, luôn hiển thị ở footer/kết quả):** "VBio cung cấp kết quả ước tính dựa trên dữ liệu bạn cung cấp và mô hình tính toán. Kết quả không thay thế chẩn đoán, tư vấn hoặc điều trị từ chuyên gia y tế."

---

## 6. PROMPT (dán vào AI code)
> Dựng cho tôi một **landing page tên "VBio"** bằng **HTML + CSS + JavaScript thuần** (không framework, không bundler, không build step, dùng relative path), chia thành `index.html`, `style.css`, `script.js`, thư mục `assets/`, chạy được khi mở trực tiếp `index.html` và deploy thẳng GitHub Pages. Fonts Google: Space Grotesk, Inter, JetBrains Mono.
>
> **Hero full màn hình nền tối** với **1 canvas animation loop ~13 giây, 6 cảnh**: (1) hình người 3D tối giản tạo từ ~520 hạt sáng có đường nối neural; (2) tan thành hạt, 17 chỉ số sinh học bay quanh (Vitamin D, HbA1c, Glucose, HDL, LDL, CRP, Ferritin, Insulin, Testosterone, Sleep, Heart Rate, Stress, BMI, Muscle Mass, Inflammation, Protein, Hormone); (3) hạt hút về tâm thành quả cầu năng lượng AI xoay; (4) hạt xếp thành chuỗi DNA xoắn kép quay chậm; (5) hạt tụ thành kết quả: Tuổi thực 42 → Tuổi sinh học đếm động về 34.8, Chênh lệch −7.2 năm, Tiềm năng Xuất sắc; (6) quay lại hình người rồi lặp. Dùng 1 mảng hạt dùng chung, nội suy vị trí đích để morph mượt, vẽ `globalCompositeOperation='lighter'`, thêm hiệu ứng lấp lánh (twinkle + sao loé) và vignette. Tôn trọng `prefers-reduced-motion`. Bảng màu hạt: emerald #34E6BE, aqua #60E0EA, platinum #EBF7FF, gold #E4C67E, violet #9684FF. Hero hiển thị logo `assets/vbio-lockup.png` + phụ đề "Giải mã sinh học. Hiểu quá trình lão hóa của bạn." + nút "Bắt đầu phân tích", "Khám phá công nghệ".
>
> **Phần thân nền sáng** (#EEF3F1, chữ #13221E, accent #0FA37F/#0C93A6/#6B4EE0, card trắng viền mờ) gồm các mục: khu **upload kéo-thả** (nút Tải tệp / Nhập thủ công / Dùng dữ liệu mẫu; file mẫu blood_test_2026.pdf, annual_health_check.xlsx, smart_watch_data.csv chạy trạng thái Đã nhận → Đang đọc → Đang chuẩn hóa → Sẵn sàng); **form** thông tin sức khỏe + thanh "Độ đầy đủ dữ liệu %" tự tính; nút **Chạy phân tích VBio** hiện checklist 8 bước rồi đếm số về kết quả; **Kết quả** (Tuổi sinh học 36.8, Tuổi thực 42, −5.2 năm, tốc độ 0.88, điểm 82/100); **Yếu tố ảnh hưởng** (tích cực/thúc đẩy lão hóa kèm giá trị & khoảng tham chiếu); **Mô phỏng** với 8 slider cập nhật realtime + 1 chuỗi DNA nhỏ đổi màu theo kết quả; **Lộ trình 30/90/180 ngày**; **Báo cáo** (nút Tải/Lưu/So sánh/Chia sẻ); **Đội ngũ** 2 thẻ thông tin (Dr. Đào Thị Mai Lan — Nhà sáng lập; Dr. Nguyễn Đình Đức Nhã — Cố vấn kỹ thuật); **footer** kèm disclaimer.
>
> Công thức tuổi sinh học (dùng đúng để Kết quả và Mô phỏng khớp): [dán khối `computeBio` ở mục 4]. Toàn bộ giao diện **tiếng Việt**. Giọng điệu: wellness/ước tính, KHÔNG khẳng định y khoa; luôn hiển thị disclaimer: "VBio cung cấp kết quả ước tính dựa trên dữ liệu bạn cung cấp và mô hình tính toán. Kết quả không thay thế chẩn đoán, tư vấn hoặc điều trị từ chuyên gia y tế."

---

## 7. Assets
Tải 2 file logo (đã tách nền trong suốt) trong thư mục `assets/` kèm bộ này: `vbio-mark.png`, `vbio-lockup.png`. Bản gốc: `vbio-src.png`.
