# Báo cáo Day 5 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên VLearn. Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 202602214
- Ngày / CVAT local: 17/09/2026
- Công cụ đã dùng: Polygon, Smart Segmentation

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh VLearn đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh `000000181542.jpg`, chiếc xe máy (`motorcycle`) ở khu vực trung tâm / nửa phải ảnh (bbox x≈354, y≈220).
- Class và quy tắc tôi dùng để chọn biên: Class `motorcycle`. Quy tắc chọn biên: bám sát phần thực tế nhìn thấy của thân xe, bánh xe, gương chiếu hậu và tay lái; dừng ranh giới ngay tại mép tiếp xúc với mặt đường và mép che khuất của người lái/vật cản, tuyệt đối không vẽ lấn vào bóng đổ xe trên mặt đường và không tự đoán phần thân sau bị che khuất.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: Sau khi thử dùng công cụ gợi ý/AI, mask tự động nhận diện tương đối tốt khung xe nhưng bị lem xuống phần bóng đen trên mặt đường và bỏ sót tay phanh/gương nhỏ. Tôi đã dùng Brush/Polygon ở chế độ trừ vùng để xóa phần bóng đổ và vẽ bù bổ sung chi tiết tay phanh/gương chiếu hậu để mask ôm sát thân xe thật.
- Nếu không dùng gợi ý: không dùng; tôi vẽ thủ công bằng Polygon và tinh chỉnh biên bằng Brush để kiểm soát chính xác từng điểm ảnh.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: Task `cp2_slice`, ảnh `000000017627.jpg`, khu vực hai xe ô tô đỗ liền kề sát nhau ở phía bên trái đường.
- Lỗi thuộc loại: sai lớp / thiếu-thừa vật / gộp-tách / biên / phủ vùng / khác: gộp-tách (gộp 2 vật thể riêng biệt cùng lớp thành 1 mask instance duy nhất).
- Bằng chứng tôi nhìn thấy: Do hai xe cùng màu và đỗ san sát nhau, mask ban đầu bao trùm cả hai xe thành một khối duy nhất, vi phạm nguyên tắc của Instance Segmentation. Phóng to thấy rõ khe hở mép cản sau của xe phía trước và đầu xe phía sau.
- Quy tắc và hành động sửa: Quy tắc `cp2_slice`: "adjacent same-class vehicles must be separate instances". Tôi đã dùng công cụ cắt/tách đa giác theo khe sáng giữa hai xe, phân chia thành 2 annotation độc lập có category `car`.
- Sau sửa đã Save và export lại chưa? Đã Save trong CVAT và export lại thành file `cp2_slice.zip` đặt trong thư mục `submissions/`.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): chưa có điểm (chưa có ground truth được phát trong repo fork; cả 9 file ZIP đã được chạy tự kiểm tra bằng `inspect_submissions.py` và đều đạt 100% hợp lệ cấu trúc contract). Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. `cp4_curb` (`7d83710e-4697c3b2.jpg`), dải bó vỉa phân cách mặt đường và vỉa hè | Gán bó vỉa vào `road` (vì bề mặt có màu xám nhựa đường tương đồng) hay gán vào `sidewalk` (phần phân chia cho người đi bộ). | Bó vỉa có cao độ nổi cao hơn mặt đường và đóng vai trò phân giới bảo vệ người đi bộ (quy tắc ranh giới chức năng trong guideline). | Quyết định gán gờ bó vỉa vào `sidewalk`. Câu hỏi cho coach: Tại các điểm dốc hạ vỉa hè cho xe lên xuống phẳng với mặt đường, nên lấy ranh giới thẳng hay uốn theo mép dốc? |
| 2. `cp1_holes` (`000000144300.jpg`), phần nan hoa bánh xe máy và kính chắn gió | Khoét rỗng (tạo lỗ/hole) các khe nhìn xuyên qua nan hoa/kính hay giữ nguyên bao phủ liền trong mask `motorcycle`. | Quy tắc của checkpoint `cp1_holes`: "Holes: windows/gaps stay inside the mask — do NOT cut them out". | Quyết định giữ liền khối mask của xe máy, không khoét lỗ xuyên qua nan hoa và kính; chỉ trừ các khoảng trống lớn hoàn toàn nằm ngoài kết cấu thân xe. |
| 3. `cp5_occlusion` (`000000336232.jpg`), xe ô tô bị người đi bộ che cắt đôi thành hai phần nhìn thấy | Tách thành 2 instance `car` riêng biệt hay gộp thành 1 instance `car` duy nhất gồm hai đa giác (multi-polygon). | Quy tắc Occlusion: Vật thể bị che khuất vẫn chỉ là MỘT thực thể duy nhất. Không vẽ đè lên vật che nhưng gom chung ID. | Quyết định gán cả hai phần nhìn thấy vào cùng một instance ID của chiếc ô tô đó, không tạo 2 ID riêng và không vẽ xuyên qua thân người. |
