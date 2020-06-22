# UI Kit cho EHealth VN

### Getting started
- Hầu hết các UI Kit đều có prop là `margin`, cách dùng như dùng trên `I3Component`
- Các component của prop là options thì mặc định, options là array của object có shape là {label, value}

### EHealthButton
##### Props
Name | Type | Default | Description
:--- | :--- | :--- | :---
`variant` | "solid" or "outlined" or "text" | "solid" | Kiểu button
`margin` | string in availableMarginAndPadding | |
`iconClassName` | string fontawesome | | icon
`width` | string | | width của button
`iconPosition` | "left" or "right" | "left" | icon nằm bên trái hay phải

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
`errorDescription` | string | | description đưojc hiển thị nếu `error == true`
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
`onChange`| func | | Khác với `onChange` của input thông thường, `onChange` của Searchbox có parameter là text, không phải event.
`margin` | | | 
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

### Tag

##### Props
Name | Type | Default | Description
:--- | :--- | :--- | :---
`label` | string | | 
`margin` | | | 
`iconClassName` | | | 
`onDelete` | func |  | nếu truyền function này thì sẽ hiện dấu "X" 
`margin` | | | 

##### Code
```jsx
<Tag
	margin="sm"
	label="Tag without icon"
/>
<Tag
	margin="sm"
	iconClassName="far fa-calendar-day"
	label="Tag with icon"
/>
<Tag
	margin="sm"
	onDelete={() => { }}
	iconClassName="far fa-calendar-day"
	label="Tag with icon and delete"
/>
```

### Dropdown / AsyncDropdown 
#### Dropdown - sử dụng để select khi đã có options
##### Props
Name | Type | Default | Description
:--- | :--- | :--- | :---
`renderOption` | function | `option => option.label;` | Render label
`onChange` | function | | callback function khi change, parameter là list khi `multiple == true`, object khi `multiple == false`
`search` | boolean | `false` | `true` nếu cần filter options
`multiple` | boolean | `false` |
`label` | string | `"Select"` | Label (placeholder)
`options` | array | | list of object of {label, value}
`value` | array or object | | array nếu multiple, ngược lại
`noOptionText` | string | `"No options"` | text display khi không có options
`margin` | | | 

##### Code
```jsx
import Dropdown from "~/components/ui-kit/dropdown/EHealthSelect";
<Dropdown
	placeholder="Select ..."
	margin="sm"
	options={options}
	value={options.filter(opt => testValues.some(i => opt.value == i))}
	onChange={values => {
		this.overwriteList(this.props.data.testValues, values.map(i => i.value))
	}}
	label="Select multiple"
	multiple
/>
```

#### AsyncDropdown - sử dụng để select query từ back-end
##### Props
Name | Type | Default | Description
:--- | :--- | :--- | :---
`getValue` | function | | lấy value cho select dựa vào options, xem VD dưới
`getInit` | boolean | `false` | `true` nếu cần lấy sẵn options 
`apiUrl`* | string | | api query options từ back-end, được nối vào với searchValue, xem VD dưới
`extractOptionsFromApi`* | function | | Trích xuất options từ `ack`, xem VD dưới

và các props khác của `Dropdown`

##### Code
```jsx
import AsyncDropdown from "~/components/ui-kit/dropdown/AsyncDropdown";
<AsyncDropdown
	placeholder="Select ..."
	margin="sm"
	apiUrl="/api/Doctor/GetCLSOptions?searchValue="
	extractOptionsFromApi={ack => ack.data.items}
	getValue={options => options.filter(opt => testValues.some(i => opt.value == i))}
	onChange={values => {
		this.overwriteList(this.props.data.testValues, values.map(i => i.value))
	}}
	label="Select multiple"
	multiple
/>
```

### EHealthCheckboxGroup
Checkbox list

##### Props
Name | Type | Default | Description
:--- | :--- | :--- | :---
`value`* | array or string/number |  | array nếu `isMulti == true`
`options`* | array | | 
`onChange`* | func | | callback function với param là array nếu `isMulti == true` (như prop `value`)
`isMulti` | bool | `false` | 
`margin` | | | 

##### Code
```jsx
<EHealthCheckboxGroup
	options={options}
	value={testValue}
	onChange={e => {
		this.updateObject(this.props.data, { testValue: e });
	}}
/>
<EHealthCheckboxGroup
	options={options}
	isMulti
	value={testValues}
	onChange={e => {
		this.updateObject(this.props.data, { testValues: e });
	}}
/>
```
### EHealthTabContainer / EHealthTab
#### Code
```jsx
// primary tab
<EHealthTabContainer
	primary={true}
	value={tabValue}
	onChange={newValue => {
		this.setState({ tabValue: newValue })
	}}
>
	<EHealthTab label="Tab 1" />
	<EHealthTab label="Tab 2" />
	<EHealthTab label="Tab 3" />
	<EHealthTab label="Tab 4" />
	<EHealthTab label="Tab 5" />
</EHealthTabContainer>

// secondary tab
<EHealthTabContainer
	primary={false}
	value={tabValue}
	onChange={newValue => {
		this.setState({ tabValue: newValue })
	}}
>
	<EHealthTab label="Tab 1" value={11} iconClassName="far fa-calendar-day" />
	<EHealthTab label="Tab 2" value={21} />
	<EHealthTab label="Tab 3" value={56} iconClassName="far fa-calendar-day" />
	<EHealthTab label="Tab 4" value={34} />
	<EHealthTab label="Tab 5" value={65} />
</EHealthTabContainer>
```
#### Props
##### EHealthTabContainer
Name | Type | Default | Description
:--- | :--- | :--- | :---
`primary` | bool | true | 
`margin` | | | 
`value` | any | | giá trị của tab đang được active
`onChange` | func | |

##### EHealthTab
Name | Type | Default | Description
:--- | :--- | :--- | :---
`label` | string |  | label của tab
`value` | any | index của tab (starts from zero) | giá trị của tab
`iconClassName` | string | | icon bên trái của tab (hiện chỉ có tab secondary mới có icon, tức primary sẽ ignore icon)

#### Ví dụ về một trường hợp sử dụng tab cơ bản
```jsx
_renderTabs = () => {
    let { tabValue } = this.state;
    switch (tabValue) {
        case 0:
            return <Tab1Content />
        case 1:
            return <Tab2Content />
        case 2:
            return <Tab3Content />
        default: 
            throw "Invalid tab value";
    }
}

render(){
    return (
        <Fragment>
            <div>
                <EHealthTabContainer
                    value={tabValue}
                    onChange={newValue => {
                        this.setState({ tabValue: newValue })
                    }}
                >
                    <EHealthTab label="Tab 1" />
                    <EHealthTab label="Tab 2" />
                    <EHealthTab label="Tab 3" />
                </EHealthTabContainer>
            </div>
            <div>
                {this._renderTabs()}
            </div>
        </Fragment>
    )
}
```

### EHealthSwitch
Xem [Switch](https://material-ui.com/api/switch/) của [Material-UI](https://material-ui.com/)
