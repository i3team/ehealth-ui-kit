# UI Kit cho EHealth VN

### Getting started
- Hầu hết các UI Kit đều có prop là `margin`, cách dùng như dùng trên `I3Component`

### EHealthButton
##### Props
Name | Type | Default | Description
:--- | :--- | :--- | :---
`variant` | "solid" or "outlined" or "text" | "solid" | Kiểu button
`margin` | string in availableMarginAndPadding | |
`iconClassName` | string fontawesome | | icon
`width` | string | | width của button

Và các props còn lại của native button

##### Code
```jsx
<EHealthButton margin="sm" variant="outlined" iconClassName="fas fa-ambulance">
    Xuất viện
</EHealthButton>
```

### EHealthTextField
##### Props
Name | Type | Default | Description
:--- | :--- | :--- | :---
`value` | string or number |  | native input value
`onChange` | func | | native input onChange
`error` | bool | false | true thì sẽ chuyển thành màu đỏ và hiển thị description (nếu có)
`errorDescription` | string | | description đưojc hiển thị nếu `error == true``
`iconClassName` | string fontawesome | | icon
`label` | string | |
`width` | string | | width của textfield nếu cần
`fullWidth` | bool | false | full width
`margin` | string in availableMarginAndPadding | |

Và các props còn lại của native input

##### Code
```jsx
<EHealthTextField
	margin="sm"
	width="250px"
	label="Textfield label"
	placeholder="Placeholder"
/>
<EHealthTextField
	margin="sm"
	width="250px"
	label="Textfield with icon label"
	placeholder="Placeholder"
	iconClassName="far fa-calendar-day"
/>
<EHealthTextField
	margin="sm"
	value={this.state.errorTestText}
	onChange={ev => {
		this.setState({ errorTestText: ev.target.value });
	}}
	error={this.state.errorTestText.includes('1')}
	errorDescription="Không được gõ số 1"
	placeholder="Không được gõ số 1"
	label="Validation text field"
	iconClassName="far fa-calendar-day"
/>
```

### Searchbox
Về cơ bản không khác gì một component text field, ngoài việc:
- Có UI dành cho việc search 
- Có thêm chức năng debounce
- Không nhận prop `value` như input thông thường, chỉ cần `onChange`

##### Props
Name | Type | Default | Description
:--- | :--- | :--- | :---
`debounce` | bool | true | `onChange` được invoke sau khi dừng nhập một khoảng (350ms)

Và các props khác của `EHealthTextField` (không bao gồm `value`)

##### Code
```jsx
<Searchbox
	margin="sm"
	width="420px"
	debounce
	placeholder="Debounced search box"
	onChange={text => {
		console.log(text);
	}}
/>
```


