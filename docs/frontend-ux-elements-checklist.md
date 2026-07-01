# Bộ tiêu chuẩn UX dành cho Frontend Developer

Dưới đây là **bộ tiêu chuẩn UX dành cho Frontend Developer** để xây dựng và review từng loại UI element. Có thể dùng trực tiếp làm checklist cho Design System, code review hoặc QA.

## 1. Tiêu chuẩn chung cho mọi UI element

Mỗi component cần đáp ứng 7 nhóm tiêu chí:

| Nhóm | Tiêu chuẩn |
|---|---|
| Dễ nhận biết | Người dùng hiểu element có chức năng gì mà không cần thử |
| Dễ thao tác | Kích thước đủ lớn, khoảng cách hợp lý, không dễ bấm nhầm |
| Phản hồi rõ ràng | Có trạng thái hover, focus, active, loading, success, error |
| Nhất quán | Cùng chức năng phải có cùng cách hiển thị và hành vi |
| Accessibility | Dùng được bằng bàn phím, screen reader và độ tương phản phù hợp |
| Responsive | Hoạt động tốt trên mobile, tablet và desktop |
| Chống lỗi | Ngăn người dùng gây lỗi và hướng dẫn cách sửa lỗi |

Mục tiêu tối thiểu nên là **WCAG 2.2 Level AA**.

---

## 2. Button

### Tiêu chuẩn UX

- Label phải thể hiện hành động: `Lưu thay đổi`, `Tạo tài khoản`, `Xóa sản phẩm`.
- Tránh label mơ hồ như `OK`, `Đồng ý`, `Tiếp tục` khi chưa rõ hành động.
- Mỗi khu vực chỉ nên có một primary action nổi bật.
- Không dùng màu làm dấu hiệu duy nhất để phân biệt trạng thái.
- Icon-only button phải có tooltip và accessible name.
- Button nguy hiểm phải thể hiện rõ hành động.

### Trạng thái bắt buộc

- Default
- Hover
- Focus-visible
- Active/pressed
- Disabled
- Loading
- Success hoặc error khi cần

Khi loading:

- Không cho bấm lặp lại.
- Giữ nguyên chiều rộng để layout không bị nhảy.
- Hiển thị trạng thái như `Đang lưu…`.
- Không vô hiệu hóa toàn bộ giao diện nếu không cần thiết.

### Accessibility

- Ưu tiên dùng `<button>` thay vì `<div onClick>`.
- Thao tác được bằng `Enter` và `Space`.
- Toggle button phải cung cấp trạng thái như `aria-pressed`.
- Khi button mở modal, focus cần chuyển vào modal.

```html
<button type="submit" disabled={isSubmitting}>
  {isSubmitting ? "Đang lưu…" : "Lưu thay đổi"}
</button>
```

---

## 3. Link

Link dùng để **đi đến một địa chỉ hoặc nội dung khác**. Button dùng để **thực hiện hành động**.

### Tiêu chuẩn

- Nội dung link phải mô tả được đích đến.
- Tránh `Xem thêm`, `Bấm vào đây` khi đứng riêng không có ngữ cảnh.
- Link trong đoạn văn nên có underline hoặc dấu hiệu khác ngoài màu.
- Link mở tab mới cần thông báo cho người dùng khi việc đó không thể đoán trước.
- External link có thể dùng icon nhưng không nên lạm dụng.

### Sai phổ biến

```html
<a href="#" onClick={saveData}>Lưu</a>
```

### Nên dùng

```html
<button onClick={saveData}>Lưu</button>
```

---

## 4. Text input

### Tiêu chuẩn

- Luôn có label hiển thị bên ngoài input.
- Placeholder chỉ dùng làm ví dụ, không thay thế label.
- Label nên mô tả dữ liệu, không mô tả thao tác.
- Chọn đúng `type`, `inputMode` và `autocomplete`.
- Không xóa dữ liệu người dùng đã nhập khi validation thất bại.
- Chiều rộng input nên phản ánh tương đối độ dài dữ liệu.

```html
<label for="phone">Số điện thoại</label>
<input
  id="phone"
  type="tel"
  inputmode="tel"
  autocomplete="tel"
/>
```

### Trạng thái

- Empty
- Filled
- Hover
- Focus
- Disabled
- Read-only
- Error
- Success khi thực sự cần xác nhận

### Error message

Thông báo lỗi cần trả lời được ba câu hỏi:

1. Lỗi nằm ở đâu?
2. Vì sao lỗi?
3. Người dùng phải sửa thế nào?

Không tốt:

> Dữ liệu không hợp lệ.

Tốt:

> Mật khẩu phải có ít nhất 8 ký tự.

Lỗi phải xuất hiện gần field, không chỉ đổi border sang màu đỏ.

---

## 5. Textarea

Ngoài các tiêu chuẩn của input:

- Cho phép resize theo chiều dọc, trừ khi layout có lý do rõ ràng để khóa.
- Hiển thị giới hạn ký tự trước khi người dùng vượt quá.
- Không dùng textarea quá thấp cho nội dung dài.
- Giữ nội dung khi submit lỗi.
- Có thể auto-grow nhưng phải giới hạn chiều cao tối đa.
- Với nội dung quan trọng, cân nhắc autosave hoặc cảnh báo khi rời trang.

Ví dụ:

> 186/500 ký tự

---

## 6. Select, dropdown và combobox

### Dùng đúng loại

- Ít hơn khoảng 5 lựa chọn: cân nhắc radio.
- Danh sách trung bình: select.
- Danh sách dài hoặc cần tìm kiếm: combobox/autocomplete.
- Chọn nhiều: multi-select hoặc checkbox list.
- Không dùng dropdown để chứa các hành động nếu bản chất là menu.

### Tiêu chuẩn

- Có label rõ ràng.
- Giá trị mặc định không được vô tình trở thành lựa chọn của người dùng.
- Placeholder như `Chọn quốc gia` không nên là giá trị hợp lệ.
- Có thể nhập để tìm với danh sách dài.
- Không đóng dropdown khi người dùng chưa hoàn tất thao tác multi-select.
- Trạng thái selected phải dễ nhận biết.
- Hỗ trợ `Arrow Up`, `Arrow Down`, `Enter`, `Escape`.

---

## 7. Checkbox

Dùng checkbox khi người dùng có thể chọn **0, 1 hoặc nhiều lựa chọn độc lập**.

### Tiêu chuẩn

- Label phải click được.
- Không đặt checkbox đơn độc mà không có label.
- Hỗ trợ trạng thái:
  - Unchecked
  - Checked
  - Indeterminate
  - Disabled
  - Focus
- Không tự động submit ngay khi checkbox thay đổi nếu hậu quả lớn.
- Checkbox chấp nhận điều khoản không nên được chọn sẵn.
- Với `Chọn tất cả`, cần xử lý trạng thái indeterminate.

```html
<label>
  <input type="checkbox" name="newsletter" />
  Nhận bản tin hàng tuần
</label>
```

---

## 8. Radio button

Dùng radio khi người dùng phải chọn **chính xác một lựa chọn trong một nhóm**.

### Tiêu chuẩn

- Các lựa chọn phải loại trừ lẫn nhau.
- Có group label bằng `fieldset` và `legend`.
- Nên hiển thị toàn bộ lựa chọn khi số lượng ít.
- Không dùng radio cho lựa chọn bật/tắt.
- Có thể đặt mặc định khi lựa chọn đó an toàn và hợp lý.
- Hỗ trợ điều hướng bằng phím mũi tên.

```html
<fieldset>
  <legend>Phương thức giao hàng</legend>

  <label>
    <input type="radio" name="delivery" value="standard" />
    Giao hàng tiêu chuẩn
  </label>
</fieldset>
```

---

## 9. Switch / toggle

Switch phù hợp với thiết lập có hiệu lực gần như ngay lập tức.

### Tiêu chuẩn

- Label mô tả trạng thái hoặc tính năng, không ghi hai hành động đối lập.
- Trạng thái bật/tắt phải nhận biết được ngoài màu sắc.
- Thay đổi nên có hiệu lực ngay.
- Nếu cần nhấn `Lưu`, checkbox thường phù hợp hơn switch.
- Có accessible state bằng checkbox hoặc `role="switch"` và `aria-checked`.

---

## 10. Form

### Cấu trúc

- Chia form dài thành các section có heading.
- Thứ tự field theo logic của người dùng, không theo cấu trúc database.
- Chỉ yêu cầu dữ liệu thực sự cần thiết.
- Đánh dấu field tùy chọn bằng `(Không bắt buộc)`.
- Không yêu cầu nhập lại thông tin đã cung cấp.
- Không reset toàn bộ form vì một field lỗi.

### Validation

- Validate khi blur hoặc submit.
- Không báo lỗi khi người dùng vừa nhập ký tự đầu tiên.
- Có error summary ở đầu form dài.
- Khi submit lỗi, focus vào summary hoặc field lỗi đầu tiên.
- Với giao dịch quan trọng, phải cho người dùng review hoặc undo.

### Submit

- Ngăn submit trùng.
- Hiển thị trạng thái đang xử lý.
- Thông báo thành công rõ ràng.
- Không chuyển trang đột ngột mà không có feedback.
- Khi server error, giữ nguyên dữ liệu đã nhập.

---

## 11. Modal / dialog

- Có title rõ ràng.
- Có nút đóng dễ tìm.
- `Escape` đóng modal, ngoại trừ một số quy trình bắt buộc đặc biệt.
- Click backdrop có thể đóng đối với tác vụ không nguy hiểm.
- Không đóng khi người dùng click backdrop nếu có nguy cơ mất dữ liệu.
- Focus chuyển vào modal khi mở.
- Focus bị giữ trong modal.
- Khi đóng, focus quay lại element đã mở modal.
- Nội dung phía sau không được tương tác.
- Không mở modal chồng modal.
- Trên mobile, nội dung dài nên cân nhắc full-screen dialog hoặc page riêng.

---

## 12. Alert, toast và notification

| Loại | Khi sử dụng |
|---|---|
| Inline message | Feedback liên quan đến một khu vực cụ thể |
| Toast | Xác nhận hành động ngắn, không cần xử lý ngay |
| Banner | Thông tin quan trọng ở cấp trang |
| Alert dialog | Cần người dùng quyết định trước khi tiếp tục |

### Tiêu chuẩn toast

- Không dùng toast cho lỗi cần người dùng sửa.
- Nội dung ngắn, nêu rõ kết quả.
- Không biến mất quá nhanh.
- Có nút đóng nếu tồn tại lâu.
- Có action như `Hoàn tác` khi phù hợp.
- Không hiển thị quá nhiều toast cùng lúc.
- Screen reader cần nhận được status message.

---

## 13. Tooltip

- Chỉ chứa nội dung bổ sung, không chứa thông tin bắt buộc.
- Hiển thị khi hover và keyboard focus.
- Có thể đóng bằng `Escape`.
- Không che element đang thao tác.
- Không chứa form hoặc hành động phức tạp.
- Nội dung ngắn.
- Icon-only button nên có accessible name ngay cả khi đã có tooltip.

---

## 14. Tabs

- Tab active phải dễ nhận biết.
- Không chỉ dùng màu để thể hiện active.
- Tab order hợp lý.
- Hỗ trợ phím mũi tên giữa các tab.
- Mỗi tab liên kết đúng với tab panel.
- Không dùng tabs khi người dùng cần so sánh các phần nội dung cùng lúc.
- Trên mobile, tránh hàng tabs quá dài.

---

## 15. Accordion

- Header accordion phải là button.
- Toàn bộ vùng header có thể click.
- Có icon thể hiện expand/collapse.
- Cung cấp `aria-expanded`.
- Hỗ trợ bàn phím.
- Không ẩn thông tin quan trọng mà người dùng cần xem ngay.
- Không dùng accordion lồng quá nhiều cấp.

---

## 16. Navigation và menu

- Vị trí navigation nhất quán giữa các trang.
- Trang hiện tại được đánh dấu rõ.
- Label ngắn và dễ hiểu.
- Không có quá nhiều cấp menu.
- Dropdown menu không chỉ hoạt động bằng hover.
- Có thể mở bằng click và bàn phím.
- `Escape` đóng menu.
- Click bên ngoài đóng menu.
- Mobile menu không làm mất context khi đóng/mở.
- Logo dẫn về trang chủ nếu đây là convention của sản phẩm.

---

## 17. Table

- Có header rõ ràng.
- Căn trái cho text; căn phải cho số liệu.
- Đơn vị phải nhất quán.
- Sorting phải thể hiện cột và chiều sort.
- Filter đang áp dụng phải nhìn thấy được.
- Empty state phải giải thích vì sao chưa có dữ liệu.
- Không dùng table cho layout.
- Hành động trên từng row cần dễ tìm nhưng không gây nhiễu.
- Bulk action chỉ xuất hiện khi có row được chọn.
- Với mobile, cân nhắc scroll ngang, card hoặc ưu tiên cột quan trọng.

---

## 18. Pagination và infinite scroll

### Pagination phù hợp khi

- Người dùng cần biết tổng số trang.
- Cần quay lại một vị trí cụ thể.
- Dữ liệu có tính quản trị hoặc tra cứu.
- URL cần chia sẻ được.

### Infinite scroll phù hợp khi

- Người dùng chủ yếu khám phá nội dung.
- Không cần truy cập footer thường xuyên.
- Không cần nhớ chính xác vị trí.

### Tiêu chuẩn

- Giữ filter, sort và page trong URL.
- Back browser phải quay lại đúng trạng thái.
- Có loading skeleton hoặc indicator.
- Không mất scroll position khi quay lại.
- Với infinite scroll nên có phương án `Tải thêm`.

---

## 19. Card

- Có hierarchy rõ: image, title, metadata, action.
- Không biến mọi thành phần trong card thành link riêng.
- Nếu toàn card click được, vẫn cần semantics và focus rõ ràng.
- Không đặt button bên trong một link bao toàn card.
- Các card trong cùng danh sách nên có cấu trúc nhất quán.
- Không che nội dung quan trọng khi text dài.
- Skeleton phải gần với layout thật để tránh layout shift.

---

## 20. Image, icon và avatar

### Image

- Có `alt` mô tả mục đích ảnh.
- Ảnh trang trí dùng `alt=""`.
- Không nhúng text quan trọng trong ảnh.
- Khai báo `width` và `height`.
- Dùng responsive image và lazy loading hợp lý.
- Không lazy-load ảnh hero quan trọng đầu trang.

### Icon

- Icon không phổ biến phải có label.
- Icon-only action cần tooltip và accessible name.
- Không dùng cùng một icon cho nhiều ý nghĩa.
- Stroke, kích thước và alignment phải nhất quán.
- Decorative icon nên được ẩn khỏi screen reader.

### Avatar

- Có fallback initials hoặc placeholder.
- Không phụ thuộc avatar để định danh duy nhất.
- Với user list, luôn hiển thị tên bên cạnh khi có thể.

---

## 21. Loading state và skeleton

- Dưới khoảng 300 ms thường không cần loading indicator.
- Loading lâu hơn cần feedback rõ ràng.
- Skeleton nên phản ánh tương đối cấu trúc nội dung thật.
- Không dùng spinner toàn màn hình cho một thao tác cục bộ.
- Giữ nội dung cũ trong lúc refresh nếu có thể.
- Với tác vụ dài, hiển thị progress có ý nghĩa.
- Cho phép hủy khi thao tác có thể mất nhiều thời gian.

---

## 22. Empty state

Phân biệt ít nhất bốn trường hợp:

1. Chưa từng có dữ liệu.
2. Không có kết quả tìm kiếm.
3. Không có dữ liệu do filter.
4. Có lỗi khi tải dữ liệu.

### Empty state nên gồm

- Điều gì đang xảy ra.
- Vì sao chưa có dữ liệu.
- Người dùng có thể làm gì tiếp theo.
- Một primary action khi phù hợp.

---

## 23. Search

- Placeholder có thể là `Tìm theo tên hoặc mã đơn hàng`.
- Có nút xóa nhanh nội dung.
- `Enter` thực hiện tìm kiếm.
- Với search-as-you-type, dùng debounce.
- Không gửi request cho từng keystroke nếu không cần thiết.
- Hiển thị query trong trang kết quả.
- Có số lượng kết quả.
- Cho phép sửa query dễ dàng.
- Search suggestion phải hỗ trợ phím mũi tên, `Enter` và `Escape`.

---

## 24. Date picker và time picker

- Cho phép nhập bằng bàn phím.
- Hiển thị format ngay trong label hoặc helper text.
- Tự động format nhưng không làm con trỏ nhảy khó đoán.
- Ngày không hợp lệ phải bị disable và có lý do khi cần.
- Range picker phải thể hiện rõ ngày bắt đầu và kết thúc.
- Có shortcut như `Hôm nay`, `7 ngày qua` khi phù hợp.
- Tôn trọng locale và timezone.
- Hiển thị ngày tháng đầy đủ tại nơi dễ gây nhầm.

---

## 25. File upload

- Cho phép click và drag-and-drop nhưng không phụ thuộc drag-and-drop.
- Ghi rõ định dạng, kích thước tối đa và số lượng file.
- Validate trước khi upload khi có thể.
- Có progress cho từng file.
- Cho phép retry và remove.
- Không xóa toàn bộ danh sách nếu một file lỗi.
- Dropzone phải sử dụng được bằng bàn phím.

---

## 26. Responsive và mobile UX

- Không tạo horizontal scroll ngoài các vùng có chủ ý.
- Không phụ thuộc hover.
- Nội dung chính phải đọc được ở zoom 200%.
- Keyboard mobile không che field đang nhập.
- Khi focus field, tự scroll sao cho field và error vẫn nhìn thấy.
- Dùng đúng input keyboard.
- Sticky header/footer không được chiếm quá nhiều viewport.
- Safe area phải được xử lý trên thiết bị có notch.
- Không khóa orientation trừ trường hợp thật sự cần thiết.

---

## 27. Keyboard và focus

- `Tab` di chuyển theo thứ tự logic.
- `Shift + Tab` quay lại đúng.
- `Enter` và `Space` kích hoạt đúng loại control.
- `Escape` đóng modal, menu và popup.
- Focus indicator luôn nhìn thấy.
- Không dùng `outline: none` mà không có thay thế.
- Focus không bị sticky header, modal hoặc overlay che.
- Khi thêm/xóa nội dung động, focus không bị mất.
- Có skip link cho trang có navigation dài.

```css
:focus-visible {
  outline: 2px solid currentColor;
  outline-offset: 3px;
}
```

---

## 28. Màu sắc và typography

### Màu sắc

- Text thường cần contrast tối thiểu `4.5:1`.
- Text lớn cần tối thiểu `3:1`.
- UI control và trạng thái focus nên có contrast rõ với nền.
- Không truyền đạt trạng thái chỉ bằng màu.
- Kiểm tra cả light mode và dark mode.
- Disabled vẫn cần đọc được nhưng không được trông giống enabled.

### Typography

- Body text thường nên từ `16px`.
- Line-height khoảng `1.4–1.6`.
- Độ dài dòng lý tưởng khoảng `45–75` ký tự.
- Không dùng quá nhiều font size và font weight.
- Heading phải theo hierarchy.
- Không viết toàn bộ câu dài bằng chữ in hoa.
- Text phải reflow hợp lý khi zoom hoặc tăng font size.

---

## 29. Animation và motion

- Animation phải giải thích sự thay đổi trạng thái.
- Transition phổ biến nên ngắn, khoảng `150–300 ms`.
- Không trì hoãn hành động của người dùng chỉ để chạy animation.
- Tránh chuyển động lớn, flashing hoặc parallax gây khó chịu.
- Hỗ trợ `prefers-reduced-motion`.
- Skeleton và loading animation không nên gây phân tâm.

```css
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    scroll-behavior: auto;
    animation-duration: 0.01ms;
    animation-iteration-count: 1;
    transition-duration: 0.01ms;
  }
}
```

---

## 30. Definition of Done cho một component

Một component chỉ nên được xem là hoàn thiện khi:

- Có đầy đủ visual states.
- Dùng đúng semantic HTML.
- Hoạt động bằng chuột, touch và bàn phím.
- Có accessible name, role và state.
- Focus rõ và đúng thứ tự.
- Hoạt động ở mobile, tablet, desktop.
- Không vỡ khi nội dung dài hoặc bản dịch dài.
- Có loading, empty, error và disabled state khi phù hợp.
- Không phụ thuộc duy nhất vào màu hoặc icon.
- Không gây layout shift đáng kể.
- Có unit test cho logic quan trọng.
- Có interaction test cho keyboard.
- Được kiểm tra bằng screen reader ở các flow quan trọng.
- Đáp ứng WCAG 2.2 AA cho sản phẩm mục tiêu.

---

## Checklist review nhanh

Trước khi merge một giao diện, hãy trả lời:

1. Người dùng có hiểu element này làm gì không?
2. Họ có biết element đang ở trạng thái nào không?
3. Khi thao tác, hệ thống có phản hồi ngay không?
4. Khi có lỗi, họ có biết cách sửa không?
5. Có thao tác hoàn toàn bằng bàn phím được không?
6. Có sử dụng được trên màn hình nhỏ không?
7. Text dài, dữ liệu rỗng và request lỗi đã được xử lý chưa?
8. Người dùng có thể hoàn tác hoặc xác nhận hành động nguy hiểm không?
9. Trạng thái có được truyền đạt ngoài màu sắc không?
10. Back button, refresh và deep link có giữ đúng trạng thái không?
