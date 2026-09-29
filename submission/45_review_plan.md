# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| B2-edge / adasind_082170.jpg & 102750.jpg | Đa số là `SPURIOUS` (vẽ thừa) và `MISSING` (bỏ sót), `WRONG_CLASS` | Khu vực rìa ảnh (edge) bị méo hình học mạnh (fisheye), AI và labeler dễ nhầm lẫn nhất (VD: nhầm ThreeWheeler thành Car). Cần ưu tiên để chốt chuẩn méo. | Ảnh chụp màn hình lỗi ở góc, các dòng log lỗi `E4_model_domain` và `E1_annotator_error` |
| C0 / adasind_019560.jpg | `SPURIOUS` (vi phạm quy tắc `R-Class` và `R-Edge`) | Lỗi do hiểu sai luật cốt lõi (Guideline gap - không gộp Rider vào Bike). Nếu không sửa ngay sẽ làm toàn bộ các round gán nhãn sau bị sai dây chuyền. | Ảnh screenshot `adasind_019560_L7_error.jpg`, escalation ticket 1 và quyết định log D01 |

Giới hạn của kết luận từ ba frame ADASIND: Chỉ mang tính chất quan sát cục bộ trên một tập mẫu quá nhỏ (3 frame), không mang tính đại diện thống kê cho toàn bộ bộ dữ liệu hàng triệu frame. Các lỗi phát hiện có thể chỉ là ngẫu nhiên cục bộ của một đoạn đường nhất định.

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv` (kể cả tránh đếm nhiều frame liền nhau trong cùng cảnh
như nhiều ca độc lập), và vì sao kế hoạch đó chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi: Việc lấy mẫu 200 frame cần thực hiện phân tán, rải rác đều qua các cụm thời gian và các góc camera khác nhau, tuyệt đối tránh bốc các frame liên tiếp sát nhau (VD cách nhau 0.1s) vì cảnh vật gần như không đổi, làm lãng phí mẫu. Tuy nhiên, việc lấy mẫu (Sampling) theo cách phân lớp/chọn lọc này chỉ mang tính chất thăm dò (Exploratory) để khoanh vùng và phát hiện xem hệ thống đang mắc các loại lỗi "bệnh lý" gì, ở camera nào. Nó không tuân theo nguyên lý lấy mẫu ngẫu nhiên hoàn toàn (Simple Random Sampling), do đó ta không thể dùng con số lỗi của 200 frame này để ngoại suy ra một tỷ lệ lỗi chính xác (ví dụ "hệ thống lỗi 5%") cho toàn bộ tập dữ liệu gốc được.
