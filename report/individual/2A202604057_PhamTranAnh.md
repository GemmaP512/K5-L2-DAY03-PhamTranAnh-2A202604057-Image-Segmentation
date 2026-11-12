# Báo cáo thực hành Day 3 — Image Segmentation

- Mã học viên: 2A202604057
- Họ tên: Pham Tran Anh
- Ngày thực hành và rà soát: 11–12/11/2026
- Công cụ: CVAT local, Polygon, công cụ kiểm ZIP và bộ tự chấm với ground truth.

## 1. Bài đã nộp

Số ảnh dưới đây là số ảnh có dữ liệu gán nhãn trong ZIP đã xuất.

| Task | File ZIP đúng tên | Số ảnh đã gán nhãn | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | `easy_semantic.zip` | 3 / 3 | 20 |
| medium_instance | `medium_instance.zip` | 3 / 3 | 32 |
| hard_panoptic | `hard_panoptic.zip` | 2 / 2 | 30 |
| cp1_holes | `cp1_holes.zip` | 1 / 1 | 3 |
| cp2_slice | `cp2_slice.zip` | 1 / 1 | 3 |
| cp5_occlusion | `cp5_occlusion.zip` | 1 / 1 | 3 |
| cp3_thin | `cp3_thin.zip` | 1 / 1 | 3 |
| cp4_curb | `cp4_curb.zip` | 1 / 1 | 3 |
| cp6_coverage | `cp6_coverage.zip` | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

## 2. Một quyết định trước khi dùng gợi ý

- Đối tượng Medium đầu tiên tự vẽ trước khi dùng gợi ý: person số 12
- Quyết định gán nhãn có thể kiểm tra trong bản export: ở ảnh `000000181542.jpg`, người phía sau thùng xe tải là một đối tượng `person` riêng. Polygon đi theo đầu, vai và phần thân lộ ra. Biên dưới dừng ở mép thùng xe vì phần thân dưới bị che.
- Cách thực hiện ca này: vẽ polygon theo ảnh gốc trong CVAT, sau đó đối chiếu bằng bộ tự chấm.

## 3. Một lỗi tôi tìm thấy và sửa

- Task/ảnh/vùng: `medium_instance`, ảnh `000000181542.jpg`, nhóm người phía sau xe tải.
- Loại lỗi: thiếu đối tượng.
- Bằng chứng: có các bóng người lộ đầu và vai phía trên mép thùng xe mà bản export trước chưa có mask tương ứng.
- Cách sửa: vẽ polygon riêng cho từng người theo phần nhìn thấy, dừng tại mép xe che khuất, rồi Save và export lại dưới dạng COCO 1.0.
- Kết quả kiểm tra: lượt sửa đầu thêm ba mask người. Hai mask mới đạt ngưỡng ghép IoU 0,50 và số đối tượng ghép được trên cả ba ảnh tăng từ 46 lên 48. Sau các lượt sửa tiếp theo, ZIP được chọn để nộp ghép được 54/71 đối tượng.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| Easy, `81ae7cbb-6bc63a4a.jpg`, mép phải ở nửa dưới ảnh | Dải sáng sát đường là vỉa hè hay phần giáp bãi cỏ có tuyết? | Mặt bê tông kéo dài dọc đường và có mép rõ với phần cỏ. | Gán `sidewalk` cho dải bê tông nhìn thấy và dừng ở ranh với bãi cỏ. |
| Medium, `000000181542.jpg`, phía sau thùng xe tải | Bỏ qua bóng người nhỏ hay gán mỗi người thành một đối tượng? | Một số bóng có đầu và vai tách biệt, còn thân dưới bị thùng xe che. | Gán `person` riêng cho từng người phân biệt được và dừng mask tại mép xe. |
| Medium, `000000458325.jpg`, hàng xe bên phải sau người trượt ván | Phần nóc bạc là một phần của xe phía trước hay thuộc xe khác? | Nóc và kính bạc có đường viền riêng, bị che bởi người và xe đỗ phía trước. | Gán phần nhìn thấy cho một đối tượng `car` riêng. |
