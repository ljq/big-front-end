## COMPONENTS 组件

### 组件列表

* DateTimePicker by Element UI

#### DateTimePicker

###### 功能: 日期时间范围组件
* 使用场景:
    * 支持时间类型
    * 日期范围
    * 日期时间范围
    * 默认时间
    * 默认间隔天数选择等

```
import DateTimePicker from './DateTimePicker'
```

###### 组件调用说明:
时间格式(yyyy-MM-dd | yyyy-MM-dd yyyy-MM-dd HH:mm:ss),根据rangeType自动处理。
* rangeType：daterange｜datetimerange 时间类型
* valueFormat: yyyy-MM-dd | yyyy-MM-dd yyyy-MM-dd HH:mm:ss 日期格式
* beginTime: 开始时间 
* endTime: 结束时间
* startPlaceholder: 范围选择时开始时间的占位内容
* endPlaceholder： 范围选择时结束时间的占位内容
* defaultIntervalDays: 默认赋值时间范围（0 当天）
* defaultTime：类型datetimerage时默认赋值时间
* @changeDateFormat：更新父组件 daterage|datetimerange 选择时间范围值
* 备注：可选时间范围默认设置选择今天以及今天以前的日期(截止范围为自然日的当天23:59:59秒)
```
<date-time-picker 
    rangeType="" 
    valueFormat="" 
    beginTime="" 
    endTime="" 
    startPlaceholder="开始时间"
    endPlaceholder="结束时间"
    defaultDays="0"
    defaultTime="['00:00:00', '23:59:59']"
    @changeDateFormat=""
></date-time-picker>

# vue: components callback function
// update daterage|datetimerange values
changeDateFormat(params) {
    // params.beginTime, 
    // params.endTime
}
```
