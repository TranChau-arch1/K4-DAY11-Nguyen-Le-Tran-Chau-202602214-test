# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B2 | ATTRIBUTE | 1 |
| center | B2 | BOX_GEOMETRY | 2 |
| center | B2 | MISSING | 3 |
| center | B2 | SPURIOUS | 7 |
| center | C0 | SPURIOUS | 3 |
| edge | B2 | MISSING | 5 |
| edge | B2 | SPURIOUS | 2 |
| edge | B2 | WRONG_CLASS | 1 |
| mid | B2 | MISSING | 5 |
| mid | B2 | SPURIOUS | 9 |
| mid | B2 | WRONG_CLASS | 1 |

## Top defects
- SPURIOUS: 21 (ví dụ frame adasind_019560.jpg)
- MISSING: 13 (ví dụ frame adasind_082170.jpg)
- BOX_GEOMETRY: 2 (ví dụ frame adasind_062370.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy: 
  - Lỗi **SPURIOUS** xảy ra nhiều nhất do 2 nguyên nhân:
    1. `E1_annotator_error`: Người gán nhãn vẽ các đối tượng quá nhỏ/sát viền, hoặc mắc lỗi vẽ tách rời người lái (Pedestrian) ra khỏi xe 2 bánh (Bike).
    2. `E4_model_domain`: AI model gợi ý sai (false positive) cực kỳ nhiều ở vùng rìa ảnh fisheye do hình ảnh bị méo.
- Cách sửa và ai nhận việc (`owner`): 
  - **annotator**: Phải tuân thủ quy tắc (`R-Edge`, `R-Class`) để tự rà soát và xóa bỏ các box thừa.
  - **ai_team**: Phải thu thập các ca AI đoán sai (`SPURIOUS M_only`) làm dữ liệu bổ sung huấn luyện lại model.
- Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule): 
  - Dòng findings: Có đến 21 lỗi `SPURIOUS` được phát hiện trong `findings.csv`.
  - Rule bị vi phạm nhiều: `R-Edge` và `R-Class`.
  - Bằng chứng trực quan: Frame `screenshots/adasind_019560_L7_error.jpg` (vẽ tách người và bám sát viền ảnh).
  