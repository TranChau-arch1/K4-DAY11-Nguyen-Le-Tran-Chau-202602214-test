# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 6 | 1 | 2 | 1 | 3 | SPURIOUS (2) |
| mid | 6 | 1 | 1 | 4 | 8 | WRONG_CLASS (1) |
| edge | 6 | 2 | 1 | 4 | 1 | MISSING (1) |

## Nhận xét

- Zone nào người (L) và model (M) gãy nhiều nhất, dẫn số ở bảng trên: 
  - **Người (L):** Gãy nhiều nhất ở vùng `edge` (rìa ảnh) với 2 ca `MISSING` và 1 ca `SPURIOUS` (tổng 3 lỗi). Vùng `center` cũng gặp 2 lỗi `SPURIOUS`.
  - **Model (M):** Gãy "thảm họa" nhất ở vùng `mid` với 4 ca `MISSING` và tới 8 ca vẽ thừa (`M thừa`). Kế tiếp là vùng `edge` với 4 ca `MISSING`.
- Giả thuyết vì sao (méo fisheye, box lỏng, thiếu `ego_body`, ...) và giới hạn của slice ba frame: 
  - **Giả thuyết:** Lỗi Model (M) tập trung rất cao ở `mid` và `edge` là do đặc thù ống kính mắt cá (fisheye) làm vật thể bị biến dạng hình học cực mạnh. Model có xu hướng nhận diện quá đà (quá nhạy) sinh ra 8 box ảo ở vùng `mid`. Ngược lại, người gán nhãn (L) lại có xu hướng thận trọng ở vùng `edge` (do vướng quy tắc H=40 hoặc mờ nhòe) nên dẫn đến việc bỏ sót (`MISSING` x2). Lỗi `SPURIOUS` của người ở vùng `center` khả năng cao là do nhầm lẫn quy tắc gộp Rider vào Bike (vi phạm `R-Class`).
  - **Giới hạn của slice:** Dữ liệu chỉ đếm trên đúng 3 frame ảnh, số lượng mẫu `n_ref` quá nhỏ (chỉ 6 mẫu mỗi zone). Các con số này hoàn toàn không mang ý nghĩa thống kê đại diện cho toàn bộ độ đo (metrics) của mô hình. Nó chỉ có tác dụng thăm dò (exploratory) để khoanh vùng các lỗi "bệnh lý" (như méo fisheye hay luật Rider) nhằm viết Guideline/Escalation.
