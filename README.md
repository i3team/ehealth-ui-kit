# UI Kit cho EHealth VN

### Danh sách
- [EHealthButton](#ehealth-button)
- [EHealthAjaxButton](#ehealth-ajax-button)
- [EHealthTextField](#ehealth-textfield)
- [Searchbox](#searchbox)
- [Tag](#tag)
- [Dropdown/AsyncDropdown](#dropdown)
- [EHealthCheckboxGroup](#checkbox)
- [EHealthTabContainer/EHealthTab](#ehealth-tab)
- [IconfulSelect/IconfulSelectItem](#iconful)
- [EHealthTreeview/EHealthTreeviewItem](#treeview)
- [EHealthSwitch](#switch)
- [TimelineItem/TimelineItemBody](#timeline)
- [EHealthPanel](#ehealth-panel)
- [EHealthUploadFiles](#upload-files)
- [EHealthCard](#card)
- [EHealthDateRangePicker](#ehealth-date-range-picker)
- [EHealthDatePicker](#ehealth-date-picker)
- [EHealthTimePicker](#ehealth-time-picker)

Hầu hết các UI Kit đều có prop là `margin`, cách dùng như dùng trên `I3Component`
Các component của prop là `options` thì mặc định, `options` là `array` của object có shape là `{label, value}`

<a name="ehealth-button"/>

### EHealthButton
##### Props
Name | Type | Default | Description
:--- | :--- | :--- | :---
`variant` | "solid" or "outlined" or "text" | "solid" | Kiểu button
`margin` | string in availableMarginAndPadding | |
`iconClassName` | string fontawesome | | icon
`width` | string | | width của button
`iconPosition` | "left" or "right" | "left" | icon nằm bên trái hay phải
`size` | string | "md" or "sm" | "md" | size cuar icon
`progressive` | boolean | false | nếu true thì sẽ có hiệu hứng "loading" khi click, khi đó onClick sẽ nhận thêm parameter thứ 2 là finishCallback
`text` | string |  | Tương tự children

Và các props còn lại của native button

##### Code
```jsx
<EHealthButton margin="sm" variant="outlined" iconClassName="fas fa-ambulance">
    Xuất viện
</EHealthButton
<EhealthButton
    text="Test button"
    variant="outlined"
    progressive
    onClick={(e, finish) => {
    	// code here
	finish(); // để kết thúc "loading"
    }}
/>
```


<a name="ehealth-ajax-button"/>

### EHealthAjaxButton
##### Props
Name | Type | Default | Description
:--- | :--- | :--- | :---
`ajaxMethod` | "post" or "get" | | Kiểu button
`ajaxOptions` | | | options của ajax (BasePage)
Và các props còn lại của `EHealthButton` (trừ props `onClick` và `progressive` đã bị override)

##### Code
```jsx
<EhealthAjaxButton
    text="Test ajax button"
    variant="outlined"
    ajaxMethod='get'
    ajaxOptions={{
        url: '/api/Ehealth/TestAjaxButton',
        success: ack => {
            console.log(ack);
        }
    }}
/>
```

<a name="ehealth-textfield"/>

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
`multiline` | bool | false |
`rows` | number | | số rows default
`rowsMax` | number | | số rows max
`startAdornment` | any | | 
`endAdornment` | any | | 
`numberControl` | Boolean | true | Nút tăng giảm giá trị, áp dụng cho type number


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
	onChange={value => {
		this.setState({ errorTestText: value });
	}}
	error={this.state.errorTestText.includes('1')}
	errorDescription="Không được gõ số 1"
	placeholder="Không được gõ số 1"
	label="Validation text field"
	iconClassName="far fa-calendar-day"
/>
```

<a name="searchbox"/>

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

<a name="tag"/>

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

<a name="dropdown"/>

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
`autoUpdate` | bool | false | Tự động update lên value , `onChange` sẽ trở thành callback sau khi đã update
`variant` | "button" or "textField" | "button" | 
`renderTags` | bool | false | render tags lên trên button dropdown (chỉ work ở variant là "textField")

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

<Dropdown
	placeholder="Select ..."
	margin="sm"
	options={options}
	value={options.filter(opt => testValues.some(i => opt.value == i))}
	autoUpdate // ko cần on Change
	label="Select multiple"
	multiple
/>
```

#### AsyncDropdown - sử dụng để select query từ back-end
##### Props
Name | Type | Default | Description
:--- | :--- | :--- | :---
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
	value={this.props.data.testValues}
	getValue={testValues}
	onChange={values => {
		this.overwriteList(this.props.data.testValues, values)
	}}
	label="Select multiple"
	multiple
/>
```

<a name="checkbox"/>

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

<a name="ehealth-tab"/>

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

<a name="iconful"/>

### IconfulSelect/IconfulSelectItem

#### IconfulSelectItem
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

#### IconfulSelect
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

<a name="treeview"/>

### EHealthTreeView/EHealthTreeViewItem

#### EHealthTreeViewItem
##### Props
Name | Type | Default | Description
:--- | :--- | :--- | :---
`color` | string |  "primary5" | Màu chữ
`hoverColor` | string | "primary1" | Màu khi hover
`haveIcon` | boolean | true | 
`expandIconClassName` | "string" | "far fa-chevron-down" | Class name của icon khi expanded
`collapseIconClassName` | "string" | "far fa-chevron-up" | Class name của icon collapsed
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

#### EHealthTreeView
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

<a name="switch"/>

### EHealthSwitch
Xem [Switch](https://material-ui.com/api/switch/) của [Material-UI](https://material-ui.com/)

<a name="timeline"/>

### TimelineItem/TimelineItemBody

#### TimelineItem
##### Props
Name | Type | Default | Description
:--- | :--- | :--- | :---
`title`* | `node` or `string` or `number` | | 
`body`* | `node` | | 1 hoặc nhiều `TimelineItemBody` được bọc trong `Fragment`
`badgeVariant`* | `"solid"` or `"outlined"` | `"solid"` | | 
`color` | | |

#### TimelineItemBody
##### Props
Name | Type | Default | Description
:--- | :--- | :--- | :---
`time`* | `string` | | 
`description` | `string` | | 
`canDownload`* | `boolean` | `false` | hiện nút tải phiếu
`onClickDownload` | `func` | | 
`color`* | | | 
`noPadding`* | `boolean` | false | nếu ko cần padding ở body
`children`* | | | nội dung timelineitembody

#### Code
```jsx
import TimelineItem from "~/components/ui-kit/timeline/TimelineItem";
import TimelineItemBody from "~/components/ui-kit/timeline/TimelineItemBody";
<TimelineItem
	color="primary"
	badgeVariant="outlined"
	title="Testing 1"
	body={(
		<Fragment>
			<TimelineItemBody color="primary" time="15:30" description="Nguyễn Văn Bác Sĩ">
				<div>Nội dung ở đây</div>
			</TimelineItemBody>
			<TimelineItemBody color="orange5" time="15:30" canDownload>
				<div>Nội dung ở đây</div>
			</TimelineItemBody>
		</Fragment>
	)}
/>
<TimelineItem
	color="orange5"
	title="Testing 2"
	body={(
		<Fragment>
			<TimelineItemBody color="orange5" time="15:30" canDownload>
				<div>Nội dung ở đây</div>
			</TimelineItemBody>
		</Fragment>
	)}
/>

```

<a name="ehealth-panel"/>

### EHealthPanel

#### Props
Name | Type | Default | Description
:--- | :--- | :--- | :---
`time`* | string | | tiêu đề
`rightAdornment` |  | | phần bên phải tiêu đề
`children` | any | `false` | nội dung

#### Code
```jsx
<EHealthPanel
	title="hàng đợi"
	rightAdornment={(
		<div>
			<I3Icon className="fal fa-expand-alt" />
		</div>
	)}
>
	<div>
		nội dung
	</div>
</EHealthPanel>
```


<a name="ehealth-time-picker"/>

### EHealthTimePicker
#### Props
Name | Type | Default | Description
:--- | :--- | :--- | :---
`value` | Date | |  chuổi giờ dạng 07:00:00
`onChange` | Function |  |  (date)=>{}
`switchToMinuteOnHourSelect` | Boolean | false |  Chuyển sang chọn phút sau khi chọn giờ
`closeOnMinuteSelect` | Boolean | false |  Tắt sau khi chọn phút


#### Code
```jsx
      const time = "07:00:00"
       <EHealthTimePicker
          value={time}
          onChange={(value) => {
            this.setState({ time: value });
          }}
        />
```

<a name="ehealth-date-picker"/>

### EHeathDatePicker
#### Props
Name | Type | Default | Description
:--- | :--- | :--- | :---
`value` | string |  |  chuổi ngày dạng 2020/07/17 07:00:00
`onChange` | Function |  |  (date)=>{}


#### Code
```jsx
      const date = "2020/07/17 07:00:00"
        <EHeathDatePicker
          value={date}
          onChange={(value) => {
            this.setState({ date: value });
          }}
        />
```

<a name="ehealth-date-range-picker"/>


### EHealthDateRangePicker
#### Props
Name | Type | Default | Description
:--- | :--- | :--- | :---
`range` | object |  | {fromDate:"2020/07/17 07:00:00", toDate:"2020/07/18 07:00:00"}
`margin` | array hoặc string |  | margin của Tag, truyền như I3Div
`iconClassName` | string | `far fa-calendar-day` | classname của icon
`hasFixedRange` | bool | `true` | có các lựa chọn số ngày nhanh ko
`fixedRange` | array | `[1, 3, 5, 7, 14, 30]` | các số ngày chọn nhanh mặc định,
`clearabble` | bool | `true` | Có button xóa raneg về null ko
`disabled` | bool | false |
`inputFormat` | string | `YYYY-MM-DD HH:mm:ss` | Chuổi định dạng ngày đầu vào
`displayFormat` | string | `YYYY-MM-DD HH:mm:ss` | Chuổi định dạng ngày hiển thị
`closeAfterSubmit` | bool | `true` | close popover chọn khoảng ngày sau khi submit
`closeAfterSubmit` | bool | `true` | close popover chọn khoảng ngày sau khi submit
`selectToDateFirst` | bool | `true` | Chọn ngày kết thúc đầu tiền
`minDate` | bool | | ngày nhỏ nhất
`maxDate` | bool | | ngày lớn nhất
`disablePast` | bool | `false` | Disabled các ngày trogn quá khứ
`disableFuture` | bool | `false` | Disabled các ngày trong tương lai
`singleMonthStep` | bool| `false` | Chỉ tăng giảm 1 tháng khi bấm nút chuyển tháng. Mặc định là tăng giảm 2 tháng
`selectFromDate` | bool | `false` | Chỉ chọn ngày bắt đầu
`selectToDate` | bool| `false` | Chỉ chọn ngày kết thúc

Chức năng nhập text tạm thời chưa phát triển.

#### Code
```jsx
         const range ={
	 		fromDate: "2020/07/1 07:00:00",
	 		toDate: "2020/07/18 07:00:00"
		};      
        <EHealthDateRangePicker
          range={range} 
	  onChange={(range) => {
            this.setState({ range: range });
          }}
          label="Ngày cho thuốc"
          selectToDateFirst
          singleMonthStep
        />
```
<a name="upload-files" />

### EHealthUploadFiles
#### Props

Name | Type | Default | Description | IsRequired
:--- | :--- | :--- | :--- | :---
`multiple` | bool | false | cho phép upload nhiều files 1 lần |
`accept` | string |  |  loại file được upload (xem html thuần) |
`onUploaded` | func |  | hàm callback sau khi upload xong, param trả ra là object hay list tùy thuộc vào giá trị của multiple | Yes
`renderComponent` | object |  | component render thay thế cho input mặc định của html, khi click vào component này sẽ mở hộp thoại chọn file |

#### Example
```jsx
_onUploaded = (data) => {
    console.log(data);
  }

  consumerContent() {
    return (
      <UploadFile 
        multiple={true}
        onUploaded={this._onUploaded} 
        renderComponent={(<div>Upload</div>)}
      />
    );
  }
```

<a name="card" />

### EHealthCard

#### Props

Name | Type | Default | Description | IsRequired
:--- | :--- | :--- | :--- | :---
`margin` | string | null | clean node margin
`padding` | string | "lg" | clean node padding

#### Code
```jsx
<EHealthCard margin="md">
	<div>content</div>
</EHealthCard>
```

### IconInformation
#### Props
Name | Type | Default | Description
:--- | :--- | :--- | :---
`iconClassName` | string | `far fa-info-circle` | classname font-awesome
`iconColor` | string | `grey3` | màu của icon
`iconHoverColor` | string | `grey5` | màu icon khi hover
`text` | string | | nội dung khi hover vao icon
`placement` | string | `right` | vị trí của đoạn text khi hover so với icon
`margin` | string | `sm` | margin của icon so với các thành phần khác

#### Code
```jsx
      <IconInformation
            iconColor="red"
            text="Hello, i'm IconInformation."
          />
```
