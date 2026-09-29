# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling
đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy
   tắc riêng? Vì sao? 
   **Trả lời:** Cần một quy tắc riêng chứ không thể coi là lỗi `DUPLICATE` đơn thuần. Lý do là trong hệ thống SVM 4 camera, một vật thể vật lý khi đi qua vùng giao thoa (seam) sẽ tất yếu bị thu hình đồng thời bởi 2 camera khác nhau. Đây là hiện tượng vật lý bình thường, không phải do annotator cố tình vẽ trùng. Do đó cần một chính sách cross-camera tracking đặc thù để quy định xem sẽ cấp chung 1 ID hay gộp chung thành 1 box BEV/3D duy nhất, thay vì đánh lỗi xóa đi.
2. Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái
   Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera. 
   **Trả lời:** 
   - **Giữ cùng track ID:** Khi vật thể di chuyển liên tục, còn nằm trong tầm nhìn (dù bị che khuất một phần - occluded).
   - **Thêm keyframe:** Khi quỹ đạo, kích thước hoặc hướng di chuyển của vật thể thay đổi rõ rệt (VD: xe tiến lại gần, bẻ lái).
   - **Trạng thái Outside:** Khi vật thể bị che khuất hoàn toàn 100% hoặc di chuyển hẳn ra khỏi viền của bức ảnh.
   - **Bằng chứng nối track qua 2 camera:** Cần xem xét ảnh Overlay của 2 camera tại cùng đúng một mốc thời gian (timestamp). Quỹ đạo đi ra của camera này phải khớp với quỹ đạo đi vào ở camera kia dựa trên ma trận calibration.
3. Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`),
   bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm? 
   **Trả lời:** Điển hình là box `L3` ở frame `adasind_069450.jpg`. Mình cho rằng đó là một đối tượng xe thật, nhưng Reference lại đánh lỗi vẽ thừa (`SPURIOUS`). Mình đã xử lý bằng cách chấp nhận sửa (`action=rework`) và log lỗi `E1_annotator_error` vì đối tượng vi phạm quy định kích thước chuẩn/độ rõ nét ở vùng rìa. Nếu làm lại slice này, mình sẽ bám thật chặt guideline (đặc biệt là `H=40` và các mép fisheye), đồng thời so sánh (play) vài frame trước sau để xem vật thể có hình thành rõ ràng không trước khi quyết định vẽ các chấm mờ ảo.
