# Quan sát vạch ô đỗ

- Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh): Hai vạch sơn trắng phân định ô đỗ xe nằm ở khu vực gần camera nhất (nửa dưới của khung hình), nơi các vạch còn rất rõ nét.
- Một vạch/dấu sơn hoặc biên **không** vẽ, và vì sao: Các vạch kẻ ô đỗ xe ở khoảng cách quá xa (gần sát hàng rào cây phía sau) không được vẽ vì ảnh bị mờ, không đủ độ phân giải và độ tương phản để xác định chính xác ranh giới.
- Polygon `free_space` dừng ở đâu; có phần bị che nào không: Polygon bao phủ toàn bộ mặt đường nhựa, dừng lại ở lề đường/hàng rào phía xa và các cạnh của bức ảnh. Vùng `free_space` bị khoét một khoảng do bị che khuất bởi chiếc xe ô tô màu đỏ đỗ ở bên trái khung hình.
- Ca chưa chắc cần hỏi người soát (nếu không có, ghi “không có”): Có cần phải vẽ các vạch `parking_line` mờ nhạt, đứt đoạn ở khu vực giữa sân đỗ (middle ground) hay không?
