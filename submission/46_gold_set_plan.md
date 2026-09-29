# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide,
**không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có
normal và hard slice. “Gold set” ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference
ADASIND hoặc nhãn bạn vừa vẽ. Nếu cần, dùng `notebooks/day11-svm360-colab.ipynb` để thử tổng phân bổ; notebook
không làm thay phần lý do.

| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| front | Giao lộ đông đúc, ngược sáng (chói nắng), xe máy tạt ngang đầu. | Mật độ đối tượng quá dày gây che khuất (occlusion). Chói sáng làm mất ranh giới xe. | Giữ nguyên thông số ma trận bù méo (intrinsic). Chú ý giới hạn vẽ vùng bầu trời và vùng nắp capo. | Chéo 2 Senior QA review độc lập (Blind peer-review), đối chiếu khung hình trước/sau để xác định occlusion. |
| rear | Xe bám quá sát đuôi (tailgating), lóa sáng đèn pha (ban đêm). | Xe bám sát bị cắt xén (truncation) che mất bánh/biển số, đèn pha gây mù lóa cục bộ (blooming). | Giữ nguyên thông số vị trí (extrinsic) góc chúc xuống. Lưu ý vùng mù sát cốp xe. | Xem theo chuỗi video (multi-frame) để theo dõi xe từ xa tiến lại gần, tránh vẽ theo tưởng tượng. |
| left | Xe máy/xe đạp đi vào điểm mù sườn xe, méo fisheye cực mạnh ở 4 góc ảnh. | Đối tượng bị biến dạng hình học cong vòng, kích thước thay đổi đột ngột khi tiến sát camera sườn. | Tuân thủ tuyệt đối ranh giới viền đen (lens_border). Chú ý sai số ghép mí (seam) với camera trước/sau. | Đối chiếu ghép mí (Overlay cross-check) với ảnh từ camera front/rear cùng timestamp để xác định đúng loại xe. |
| right | Phương tiện đi sát lề (đặc thù tay lái bên trái/phải), xe vượt lên từ điểm mù. | Biến dạng fisheye mạnh, lẫn lộn giữa vỉa hè, người đi bộ và phương tiện đỗ sát lề. | Tương tự left, quản lý chặt ranh giới seam và độ méo cạnh. | Overlay cross-check với camera front/rear để đảm bảo ID và class không bị gán sai khi xe chạy ngang qua. |

- Khi nào cần refresh gold set (đổi camera, calibration hoặc rule): Phải làm mới Gold Set ngay khi có thay đổi về phần cứng (đổi loại ống kính, lệch góc lắp đặt camera ảnh hưởng đến calibration extrinsic/intrinsic), đổi thuật toán bù méo, hoặc có bản cập nhật Guideline cốt lõi làm thay đổi định nghĩa class/box (VD: rule gộp Rider).
- Một ca seam/cross-camera cần policy và evidence trước khi ghép hai box: Khi một chiếc xe bị xẻ nửa nằm vắt ngang giữa 2 camera, cần policy chỉ định rõ: vẽ 2 box riêng rẽ ở 2 ảnh rồi cấp chung 1 Tracking ID, hay vẽ 1 box 3D/BEV? Bằng chứng (evidence) bắt buộc phải có là ảnh từ cả 2 camera chụp tại cùng 1 timestamp chính xác và ma trận ghép nối camera để chứng minh đó là cùng 1 xe.
- Vì sao peer agreement hoặc quality report trên ảnh một camera chưa chứng minh gold set đúng cho cả bốn camera: Sự đồng thuận (peer agreement) cao trên 1 góc cam chỉ chứng tỏ annotator hiểu đúng luật vẽ 2D cục bộ của góc đó. Hệ thống 4 camera (SVM) là một không gian bao quát 360 độ, các lỗi nghiêm trọng nhất thường nằm ở vùng ghép mí (seam) bị trùng lặp hoặc mâu thuẫn ID giữa các camera. Nếu chỉ test trên 1 cam, ta hoàn toàn mù mờ về khả năng xử lý tương quan không gian (cross-camera) và độ méo dị biệt của từng loại ống kính.
