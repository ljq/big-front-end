<template>
  <el-date-picker
    v-model="rangeDate"
    :prefix-icon="prefixIcon"
    :type="rangeType"
    :picker-options="pickerOptions"
    :range-separator="rangeSeparator"
    :start-placeholder="startPlaceholder"
    :end-placeholder="endPlaceholder"
    :value-format="valueFormat"
    @change="changeDateFormat"
    :default-time="defaultTime"
  ></el-date-picker>
</template>
<script>
import moment from "moment";
export default {
  name: "DatePicker",
  props: {
    rangeType: {
      type: String,
      default: "daterange",
    },
    beginTime: {
      type: String,
      default: "",
    },
    endTime: {
      type: String,
      default: "",
    },
    defaultIntervalDays: {
      // Number of days between
      type: Number,
      default: 0,
    },
    defaultTime: {
      type: Array,
      default: ["00:00:00", "23:59:59"],
    },
    valueFormat: {
      type: String,
      default: "yyyy-MM-dd",
    },
    rangeSeparator: {
      type: String,
      default: "至",
    },
    startPlaceholder: {
      type: String,
      default: "请选择开始时间",
    },
    endPlaceholder: {
      type: String,
      default: "请选择结束时间",
    },
  },
  data() {
    return {
      prefixIcon: "el-icon-date",
      rangeDate: [], //日期范围
      pickerOptions: {
        disabledDate(time) {
          //设置选择今天以及今天以前的日期(截止范围自然日的当天23:59:59秒)
          let todayTimestamp =
            new Date(new Date().toLocaleDateString()).getTime() +
            (24 * 60 * 60 * 1000 - 1);
          let endDayTimeRange = time.getTime() >= todayTimestamp;
          return endDayTimeRange;
        },
        shortcuts: [
          {
            text: "今天",
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime());
              picker.$emit("pick", [start, end]);
            },
          },
          {
            text: "昨天",
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 1);
              picker.$emit("pick", [start, end]);
            },
          },
          {
            text: "最近一周",
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
              picker.$emit("pick", [start, end]);
            },
          },
          {
            text: "最近30天",
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
              picker.$emit("pick", [start, end]);
            },
          },
          {
            text: "最近90天",
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
              picker.$emit("pick", [start, end]);
            },
          },
          {
            text: "最近180天",
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 180);
              picker.$emit("pick", [start, end]);
            },
          },
          {
            text: "最近365天",
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 365);
              picker.$emit("pick", [start, end]);
            },
          },
        ],
      },
    };
  },
  methods: {
    setDefaultTime: function () {
      const end = new Date();
      const start = new Date();
      const rangeDefaultTime = 3600 * 1000 * 24 * this.defaultIntervalDays;
      start.setTime(start.getTime() - rangeDefaultTime);
      let startFormat = start ? moment(start).format("YYYY-MM-DD") : "";
      let endFormat = end ? moment(end).format("YYYY-MM-DD") : "";
      // daterange
      if (this.rangeType === "daterange") {
        this.rangeDate = [start, end];
        let paramsDate = {
          beginTime: startFormat,
          endTime: endFormat,
        };
        this.$emit("changeDateFormat", paramsDate);
        return;
      }
      // set default custom time(hour minute second)
      this.prefixIcon = "el-icon-time";
      let rangeTimes = this.customDefaultTime(start, end);
      if (rangeTimes) {
        this.rangeDate = [rangeTimes[0], rangeTimes[1]];
      }
      let params = {
        beginTime: startFormat + " " + this.defaultTime[0],
        endTime: endFormat + " " + this.defaultTime[1],
      };
      this.$emit("changeDateFormat", params);
    },
    // update parent daterange
    changeDateFormat: function () {
      // when v-model clear empty, reset beginTime and endTime.
      if (this.rangeDate == null) {
        this.beginTime = "";
        this.endTime = "";
      }
      //console.log('this.rangeDate:' + this.rangeDate)
      let params = {
        beginTime: this.beginTime,
        endTime: this.endTime,
      };
      this.$emit("changeDateFormat", params);
    },
    // set default custom time(hour minute second)
    customDefaultTime: function (start, end) {
      if (this.defaultTime.length < 0) {
        return [];
      }
      let startArr = this.defaultTime[0].split(":");
      let endArr = this.defaultTime[1].split(":");
      start.setHours(startArr[0], startArr[1], startArr[2], 0);
      end.setHours(endArr[0], endArr[1], endArr[2], 999);
      let rangeTimes = [start, end];
      return rangeTimes;
    },
  },
  watch: {
    rangeDate: function (val) {
      if (val !== null) {
        this.beginTime = val[0];
        this.endTime = val[1];
      } else {
        this.beginTime = "";
        this.endTime = "";
      }
    },
  },
  created() {
    if (this.rangeDate.length == 0) {
      this.setDefaultTime();
    }
  }
};
</script>