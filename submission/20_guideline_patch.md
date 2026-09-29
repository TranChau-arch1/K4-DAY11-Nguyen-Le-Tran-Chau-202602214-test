# Guideline patch

- **Rule mới đề xuất:** Thêm quy định rõ ràng: Tuyệt đối không gán nhãn `Pedestrian` độc lập cho người đang ngồi điều khiển xe 2/3 bánh. Toàn bộ người và xe phải được gộp chung vào một bounding box duy nhất và gán nhãn là `Bike` (hoặc `ThreeWheeler`).
- **Áp dụng cho:** class `Bike`, `ThreeWheeler` và `Pedestrian`.
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** Luật hiện tại (mục `R-Class`) chưa nói rõ cách xử lý đối với người điều khiển phương tiện (rider). Do đó, annotator dễ bị nhầm lẫn và vẽ tách rời người (`Pedestrian`) khỏi xe (`Bike`), gây ra lỗi SPURIOUS hoặc WRONG_CLASS.
- **`rules_version` mới:** v1.1.0
- **Hiệu lực từ:** Vòng `rework` hiện tại và các batch gán nhãn tiếp theo.
