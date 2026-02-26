# Tạo 2 checkbox ảo trong Elementor (WordPress) và kiểm tra mở khóa form

Bạn có thể làm theo cách dưới đây để:
- Mỗi checkbox lưu trạng thái `true/false`.
- Khi click button có `id="check-form-unlock"`, nếu cả 2 checkbox đều `true` thì log `"mở khóa form"`, ngược lại log `"khóa form"`.

## 1) Thêm HTML widget trong Elementor
Kéo widget **HTML** vào vị trí bạn muốn hiển thị, rồi dán đoạn này:

```html
<div style="display:flex;flex-direction:column;gap:10px;">
  <label style="display:flex;align-items:center;gap:8px;cursor:pointer;">
    <input type="checkbox" id="virtual-checkbox-1" />
    <span>Điều kiện 1</span>
  </label>

  <label style="display:flex;align-items:center;gap:8px;cursor:pointer;">
    <input type="checkbox" id="virtual-checkbox-2" />
    <span>Điều kiện 2</span>
  </label>

  <!-- 2 trường ẩn để lưu true/false -->
  <input type="hidden" id="virtual-checkbox-value-1" name="virtual_checkbox_value_1" value="false" />
  <input type="hidden" id="virtual-checkbox-value-2" name="virtual_checkbox_value_2" value="false" />

  <!-- Nút kiểm tra mở khóa form -->
  <button type="button" id="check-form-unlock">Kiểm tra mở khóa form</button>
</div>
```

## 2) Thêm JavaScript để gán true/false và kiểm tra khi click nút
Thêm script ngay trong widget HTML (hoặc chèn bằng plugin custom code):

```html
<script>
document.addEventListener('DOMContentLoaded', function () {
  const checkbox1 = document.getElementById('virtual-checkbox-1');
  const checkbox2 = document.getElementById('virtual-checkbox-2');
  const hiddenField1 = document.getElementById('virtual-checkbox-value-1');
  const hiddenField2 = document.getElementById('virtual-checkbox-value-2');
  const checkButton = document.getElementById('check-form-unlock');

  function syncCheckboxValue(checkbox, hiddenField) {
    hiddenField.value = checkbox.checked ? 'true' : 'false';
  }

  // Đồng bộ giá trị mặc định ban đầu
  syncCheckboxValue(checkbox1, hiddenField1);
  syncCheckboxValue(checkbox2, hiddenField2);

  // Khi người dùng check/uncheck
  checkbox1.addEventListener('change', function () {
    syncCheckboxValue(checkbox1, hiddenField1);
  });

  checkbox2.addEventListener('change', function () {
    syncCheckboxValue(checkbox2, hiddenField2);
  });

  // Khi click nút kiểm tra
  checkButton.addEventListener('click', function () {
    const isChecked1 = hiddenField1.value === 'true';
    const isChecked2 = hiddenField2.value === 'true';

    if (isChecked1 && isChecked2) {
      console.log('mở khóa form');
    } else {
      console.log('khóa form');
    }
  });
});
</script>
```

## 3) Kết quả
- Checkbox 1 check => `virtual_checkbox_value_1 = true`, bỏ check => `false`.
- Checkbox 2 check => `virtual_checkbox_value_2 = true`, bỏ check => `false`.
- Click `#check-form-unlock`:
  - Nếu cả 2 đều `true` => `console.log('mở khóa form')`
  - Nếu thiếu 1 trong 2 => `console.log('khóa form')`

## 4) Nếu dùng trong Elementor Form
Nếu bạn dùng **Elementor Pro Form**:
- Tạo 2 field kiểu **Hidden** với name tương ứng:
  - `virtual_checkbox_value_1`
  - `virtual_checkbox_value_2`
- Đảm bảo `id` thực tế trên form khớp với script (hoặc sửa script theo id thực tế).

## 5) Lưu ý nhanh
- Nếu trang có cache/minify JS, nhớ xóa cache sau khi cập nhật script.
- Tránh trùng `id` nếu có nhiều form trên cùng 1 trang.
