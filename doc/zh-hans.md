# vue-datepicker-mobile

移动端友好的日历，用于酒店入住，可选择范围

![e.g.](https://github.com/ikarosu/vue-datepicker-mobile/blob/master/git/eg.png?raw=true)
[Live Demo](https://ikarosu.github.io/datepicker/)
![demo](https://github.com/ikarosu/vue-datepicker-mobile/blob/master/git/qr.png?raw=true)

## 属性

| 参数 | 说明 | 类型 | 默认值 |
| :-: | :- | :-: | :-: |
| autoComplete | 是否选择日期后自动确认，false时将显示确认按钮，由用户手动点击确认。 | boolean | false |
| mondayFirst | 是否周一开头。默认周日开头 | boolean | false |
| reverseSelect | 是否允许往前反选日期 | boolean | false |
| beginningText | 选中的起点日期文本 | boolean | false |
| endText | 选中的终点日期文本 | boolean | false |

## Prop
| 参数 | 说明 | 类型 | 默认值 |
| :-: | :- | :-: | :-: |
| display | 显示这个日期选择器 | boolean | false |
| displayRange | 显示的范围，以月为长度 | number | 13 |
| displayRangeStart | 显示的开始月份，默认为当月。示例：`'2018-9-1'` | string | - |
| selectRangeStart | 可以选择的开始日期，默认为当日。格式同上。 | string | - |
| selectRangeEnd | 可以选择的结束日期，默认为显示结束。格式同上。 | string | - |
| restday | 自定义节假日，会有“休”字提示。是一个字符串数组。示例：`['2019.5.1', '2019.5.2', '2019.5.3']` | array | - |
| workday | 自定义工作日，会有“班”字提示。是一个字符串数组。示例：`['2019.5.4', '2019.5.5']` | array | - |
| customData | 自定义显示的文本数组，可以是一个字符串或者对象。 | array | - |

### customData
数据以当日为index 0，顺序往后加入到日历中。

当是字符串的时候，直接显示文本。 `['￥100']`

通过传入一个对象，来配置其他选项

| 参数 | 说明 | 类型 |
| :-: | :- | :-: |
| text | 文本 | string |
| highlight | 高亮显示文本 | boolean |
| bgcolor | 背景颜色。接受一个合法的颜色值 | string |

例如：`[{ highlight: true, text: '￥100' }]`

## Events

### `select({ start, end, range })`
选择后触发，有三个参数，start和end分别代表被选中的开始和结尾，是一个选中对象，而range是一个数组，表示出了开始和结尾中间被选中的部分，为选中对象的集合，如果开始和结尾没有间隔，得到一个空数组。
#### 选中对象
被选中的对象包含以下属性：

| 参数 | 说明 | 类型 |
| :-: | :- | :-: |
| year | 年份 | number |
| month | 月份 | number |
| day | 号数 | number |
| date | 完整日期 YYYY-M-D | string |
| odate | 完整日期 YYYY-MM-DD | string |
| rest | 是否为休息日，包含不是工作日的周末和指定公休日 | boolean |
| restday | 是否为指定的休息日 | boolean |
| workday | 是否为指定的工作日（补班） | boolean |
| disabled | 是否禁用，可选的都为false | boolean |
| customData | 自定义显示的文本。`{ highlight: boolean, text: string }` | object |
| begin | 是否为选中的开始 | boolean |
| active | 是否为选中中间部分 | boolean |
| end | 是否为选中的结束 | boolean |
### cancle()
取消时触发，用于手动隐藏窗口。

## 使用示例
```html
<input type="date" @touchstart="show = true" v-model="startDate" readonly>
-
<input type="date" @touchstart="show = true" v-model="endDate" readonly>
<DatePicker
  :restday="restday"
  :workday="workday"
  :display="show"
  :customData="data"
  @select="select"
  @cancle="cancle"
  autoComplete/>
```

```javascript
import DatePicker from './DatePicker'

export default {
  components: {
    DatePicker
  },
  data() {
    return {
      startDate: '',
      endDate: '',
      restday: [],
      workday: ['2018-9-29', '2018-9-30'],
      data: ['￥1300', '￥1345', { highlight: true, text: '￥888' }],
      show: true
    }
  },
  methods: {
    select({ start, end, range }) {
      this.startDate = start.odate
      this.endDate = end.odate
      console.log('range', range)
      this.show = false
    },
    cancle() {
      this.show = false
    }
  },
  created() {
    // it is working!
    setTimeout(() => {
      this.workday = ['2018-8-25', '2018-8-26', '2018-8-27']
      this.restday = ['2018-8-22', '2018-8-23', '2018-8-24']
      this.data = ['￥1999', { text:'￥2018',bgcolor: 'lightgreen' }, { highlight: true, text: '￥666' }, { highlight: true, text: '￥888',bgcolor: 'gold' }]
    }, 2000)
  }
}
```
