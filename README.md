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
`onChange` | func | | onChange = (value)=>{} not (e)=>{}
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
### IconfulSelectItems
##### Props
Name | Type | Default | Description
:--- | :--- | :--- | :---
`selected` | Boolean |  false | Có chọn hay không
`onSelect` | Function | | Hàm handle khi selec, trả về option.value và e
`iconClassName` | string | "fad fa-question-circle" | Class name của icon
`iconSelectedClassName` | string | "far fa-check" | Class name của icon khi selected
`iconColor` | string | "yellow7" | Màu của icon
`color` | string | "yellow7" | Màu nền khi chưa được chọn
`selectedColor` | string | "primary1"  | Màu nền khi được chọn
`minimal` | bool | false  | Kiểu Item, minimal of formal

Và các props còn lại của I3Div

##### Code
```jsx
<IconfulSelectItem
              selected={
                multiple
                  ? value.some((v) => v === option.value)
                  : value === option.value
              }
              disabled={option.disabled || disabled}
              onSelect={(e) => {
                this._onSelect(option, e);
              }}
              iconClassName={option.iconClassName}
              {...selectItemProps}
              key={option.value}
            >
              {option.label}
            </IconfulSelectItem>
```

### IconfulSelect
##### Props
Name | Type | Default | Description
:--- | :--- | :--- | :---
`options` | Array of IconfulSelectModel or similar model |  | Dữ liệu cho select
`onChange` | Function | |Hàm handle khi chọn trả về option.value
`disabled` | Boolean | false | Disabled or not
`value` | any | | Truyền vào value hoặc mảng value được chọn
`multiple` | Boolean | false | có multi select hay không
`selectItemProps` | object |  | Các props truyền cho IconfulSelectItems
`direction` | "horizontal" hoặc "vertical" | "horizontal" | Hướng của select

Và các props còn lại của I3Div

##### Code
```jsx
<IconfulSelect
          options={options}
          value={value1}
          direction="vertical"
          disabled={true}
          onChange={this._onChange1}
          multiple
          selectItemProps={{
            width: "100px",
            color: "yellow1",
            selectedColor: "yellow9",
            iconColor: "green7",
          }}
        />
```

### EHealthTreeViewItem
##### Props
Name | Type | Default | Description
:--- | :--- | :--- | :---
`color` | string |  "primary5" | Màu chữ
`hoverColor` | string | "primary1" | Màu khi hover
`haveIcon` | boolean | true | 
`expandIconClassName` | "string" | "far fa-chevron-down" | Class name của icon khi expanded
`collapseIconClassName` | "string" | "far fa-chevron-down" | Class name của icon collapsed
`label` | string |  | Title của item
`disabled` | boolean| false  | 
`expand` | boolean |  | 
`defaultExpand` | boolean | false |  
`haveCheckBox` | boolean| true  | 
`checked` | boolean |  |  
`defaultChecked` | boolean | false |
`iconRotatable` | boolean | false  | icon có quay khi đóng mở hay không
`degreeRotate` | number | 90 | độ quay của icon khi đóng mở
`expandDuration` | number | 200  | thời gian quay của icon và thời gian mở đóng của children
`onClick` | func |  | khi click vào item function(e){}
`onChangeCheckBox` | func |  | khi click vào checkbox function(e){}


Và các props còn lại của I3Div

##### Code
```jsx

        <EHealthTreeViewItem label="item1">
          <EHealthTreeViewItem label="item1.1"></EHealthTreeViewItem>
          <EHealthTreeViewItem label="item1.2">
            <EHealthTreeViewItem label="item1.2.1"></EHealthTreeViewItem>
            <EHealthTreeViewItem label="item1.2.2"></EHealthTreeViewItem>
          </EHealthTreeViewItem>
        </EHealthTreeViewItem>
```

### EHealthTreeView
##### Props
Name | Type | Default | Description
:--- | :--- | :--- | :---
`data` | object |  | 
`disabled` | boolean | false | 
`getLabel` |  | "far fa-chevron-down" | Class name của icon khi expanded
`getExpanded` | func |  | function(item){} trả về boolean
`getDisabled` | func |  | function(item){} trả về boolean
`getChecked` | func |   | function(item){} trả về boolean
`getChildrens` | func |  | function(item){} trả về Array children or underfied
`onClick` | func |  | khi click vào item function(item,e){}
`onChangeCheckBox` | func |  | khi click vào checkbox function(item,e){}

Và các props còn lại của I3Div và EHealthTreeViewItem

Lưu ý: Mặc định các trường disabled, checked, expand, childrens sẽ lấy theo các field tương ứng trong object item, Nếu muốn lấy khác thì sử dụng các hàm get tương ứng. Lưu ý dùng các field để render sẽ giúp hiệu năng tốt hơn.

##### Code
```jsx
       <EHealthTreeView
          data={data}
          onChangeCheckBox={this._onChangeCheckBox}
          onClick={this._onClick}
          getChildrens={(d) => d.items}
        />
```

### EHealthSwitch
Xem [Switch](https://material-ui.com/api/switch/) của [Material-UI](https://material-ui.com/)
