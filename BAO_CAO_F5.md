# Lab F5 – Danh sách và lưới trong Flutter

## 1. Sản phẩm đã làm

- Tab **Danh sách** dùng `ListView.builder`, `itemCount`, `itemBuilder`, `Card` và `ListTile`.
- `SanPham` tách dữ liệu khỏi giao diện; giá được lưu bằng `int` và định dạng có dấu chấm phân cách nghìn.
- Nút `+` thêm sản phẩm lên đầu. Vuốt từ phải sang trái để xóa; `Dismissible` dùng key theo đúng đoạn code mẫu.
- Tìm sản phẩm theo tên khi nhập từ khóa; có nút xóa từ khóa.
- Khi xóa hiện SnackBar có nút **Hoàn tác**, chèn sản phẩm về vị trí cũ. Danh sách rỗng có thông báo và nút thêm.
- Chạm sản phẩm hiện SnackBar xác nhận lựa chọn, đúng như code mẫu.
- Tab **Lưới** dùng `GridView.builder` với hai cột, ảnh `BoxFit.cover` và nội dung có giới hạn dòng.
- `Image.asset` có `errorBuilder`; nếu chưa thêm ảnh app vẫn chạy và hiện ô xám.

## 2. Cách thêm ảnh và chạy

1. Tải sáu ảnh sản phẩm, mỗi ảnh dưới 200 KB, đặt trong `assets/images/` với tên chính xác `p1.jpg`, `p2.jpg`, `p3.jpg`, `p4.jpg`, `p5.jpg`, `p6.jpg`.
2. Mở Terminal tại thư mục `f5_danhsach` và chạy `flutter pub get`.
3. Chạy `flutter run`. Nếu ứng dụng đang mở khi vừa thêm ảnh, dừng và chạy lại (Hot Restart).
4. Nếu xoay ngang trên giả lập, bật xoay màn hình của giả lập trước. Lưới giữ hai cột theo đề bài.

Đường dẫn ảnh trong model đã khớp tên file. Không cần sửa Dart khi dùng đúng tên. Thư mục hiện có `README.txt` để được giữ trong Git trước khi bạn tải ảnh về.

## 3. Bảng 7 kịch bản kiểm thử

| # | Thao tác | Kết quả mong đợi | Kết quả thực tế |
|---|---|---|---|
| 1 | Mở ứng dụng ở tab Danh sách | Sáu sản phẩm có ảnh, tên, mô tả, giá định dạng | Đạt: đủ sáu tên và giá; ảnh hiện ô xám chờ tải ảnh thật |
| 2 | Cuộn nhanh lên xuống | Cuộn mượt, không có cảnh báo tràn | Chưa kiểm tra riêng |
| 3 | Bấm nút `+` | Sản phẩm mới xuất hiện đầu danh sách | Đạt: “Sản phẩm mới 1” lên đầu |
| 4 | Vuốt một dòng sang trái | Nền đỏ và biểu tượng xóa; dòng biến mất | Đạt: dòng được vuốt biến mất và hiện SnackBar |
| 5 | Vuốt xóa một dòng ở giữa | Đúng sản phẩm bị xóa; Hoàn tác khôi phục về vị trí cũ | Chưa kiểm tra sau khi thêm chức năng Hoàn tác |
| 6 | Xóa hết sản phẩm | Hiện “Danh sách trống” và nút thêm | Chưa kiểm tra riêng |
| 7 | Mở tab Lưới và xoay ngang | Hai cột đều, ảnh không méo, không tràn nội dung | Đạt trên chiều dọc: lưới hai cột; chưa kiểm tra xoay ngang |

## 4. Đối chiếu RecyclerView (Lab A9/A10)

| Việc cần làm | Android (Java) | Flutter (Dart) |
|---|---|---|
| Hiển thị danh sách dài | `RecyclerView` + `LayoutManager` | `ListView.builder` |
| Mô tả một dòng | Adapter + ViewHolder + layout item | Hàm `itemBuilder` trả về widget |
| Số phần tử | `getItemCount()` | `itemCount` |
| Gán dữ liệu cho dòng | `onBindViewHolder(holder, position)` | `itemBuilder(context, index)` |
| Báo dữ liệu thay đổi | `notifyItemInserted` / `notifyDataSetChanged` | `setState` |
| Lưới | `GridLayoutManager` | `GridView.builder` + `gridDelegate` |
| Vuốt để xóa | `ItemTouchHelper` | `Dismissible` |

Flutter gom Adapter, ViewHolder và layout item vào hàm tạo widget nên phần mã hiển thị ngắn hơn. Android tách rõ các vai trò, thuận tiện phân chia và kiểm soát khi dự án lớn.

## 5. Câu hỏi phân tích

1. **Vì sao mỗi Dismissible cần key?** Flutter dùng key để phân biệt từng dòng khi danh sách thay đổi. Mẫu đang dùng tên sản phẩm ghép với vị trí trong danh sách hiển thị để tạo key.
2. **Nếu đổi sang `SliverGridDelegateWithMaxCrossAxisExtent(maxCrossAxisExtent: 180)`?** Flutter tự tính số cột để mỗi ô không rộng quá khoảng 180 logical pixels. Khi màn hình ngang rộng hơn, thường sẽ tăng số cột; cấu hình hiện tại cố định hai cột theo yêu cầu đề.
3. **`ListView.builder` có tạo hết mọi dòng ngay khi mở không?** Không. Nó tạo những dòng cần hiển thị và các dòng lân cận khi cuộn. Có thể thêm `debugPrint('build item $index')` trong `itemBuilder`, mở màn hình và cuộn để quan sát các chỉ số được dựng dần.

## 6. Bài nâng cao đã làm

- **NC1 – Tìm kiếm:** lọc theo tên ngay khi nhập; không phân biệt chữ hoa/chữ thường.
- **NC3 – Hoàn tác:** sau khi vuốt xóa, bấm **Hoàn tác** để chèn sản phẩm trở lại vị trí cũ.

## 7. Trước khi nộp

- Thêm đủ sáu ảnh và xác nhận tên file đúng chữ thường.
- Chạy `flutter pub get`, sau đó chạy `flutter analyze`.
- Hoàn tất cột “Kết quả thực tế” sau khi tự chạy bảy kịch bản; ghi hình demo tối đa 2 phút.
- Nộp project gồm `lib/`, `pubspec.yaml`, `assets/` và báo cáo; không cần đưa `build/` hay `.dart_tool/` vào gói nộp.
