<template>
  <div class="page-filter-container">
    <div class="filter-item" v-for="(item, index) in getFieldConfigList()" :key="index">
      <label
        class="filter-label"
        v-if="item.type !== 'button' && item.type !== 'group-input' && item.label && !item.hiddenLabel"
      >{{item.label}}</label>
      <!-- 输入框 -->
      <el-input
        :class="`filter-${item.type}`"
        v-if="item.type === 'input'"
        :type="item.type"
        :disabled="item.disabled"
        :clearable="item.clearable === false ? item.clearable : true"
        :placeholder="getPlaceholder(item)"
        @focus="handleEvent(item.event)"
        v-model="searchQuery[item.value]"
      ></el-input>

      <!-- 组合文本框 -->
      <el-input
        class="input-with-select"
        :class="`filter-${item.type}`"
        v-if="item.type === 'group-input' && searchQuery[item.value]"
        :type="item.type"
        :disabled="item.disabled"
        :clearable="item.clearable === false ? item.clearable : true"
        :placeholder="getPlaceholder(item)"
        @focus="handleEvent(item.event)"
        v-model="searchQuery[item.value].value"
      >
        <el-select
          v-model="searchQuery[item.value].field"
          slot="prepend"
          @change="handleGroupFieldChange($event, item)"
        >
          <el-option
            v-for="(field, index) in item.fields"
            :key="index"
            :label="field.label"
            :value="field.value"
          ></el-option>
        </el-select>
      </el-input>

      <!-- 选择框 -->
      <el-select
        :class="`filter-${item.type}`"
        v-if="item.type === 'select'"
        v-model="searchQuery[item.value]"
        :disabled="item.disabled"
        @change="handleEvent(item.event)"
        :clearable="item.clearable === false ? item.clearable : true"
        :filterable="item.filterable === false ? item.filterable : true"
        :placeholder="getPlaceholder(item)"
      >
        <el-option
          v-for="(item ,index) in  listTypeInfo[item.list]"
          :key="index"
          :label="item.label"
          :value="item.value"
        ></el-option>
      </el-select>

      <!-- 时间选择框 -->
      <el-time-select
        :class="`filter-${item.type}`"
        v-if="item.type === 'time'"
        v-model="searchQuery[item.value]"
        :picker-options="item.TimePickerOptions"
        :clearable="item.clearable === false ? item.clearable : true"
        :disabled="item.disabled"
        :placeholder="getPlaceholder(item)"
      ></el-time-select>
      <!-- 日期选择框 -->
      <el-date-picker
        :class="`filter-${item.type}`"
        v-if="item.type === 'date'"
        v-model="searchQuery[item.value]"
        :picker-options="item.datePickerOptions || datePickerOptions"
        :type="item.dateType"
        :clearable="item.clearable === false ? item.clearable : true"
        :disabled="item.disabled"
        @focus="handleEvent(item.event)"
        :placeholder="getPlaceholder(item)"
      ></el-date-picker>
      <!-- 按钮 -->
      <el-button
        :class="`filter-${item.type}`"
        v-else-if="item.type === 'button'"
        v-waves
        :type="item.btType"
        :icon="item.icon"
        @click="handleClickBtn(item.event)"
      >{{item.label}}</el-button>
      <!-- 组合输入框 -->
      <el-row :class="`filter-${item.type}`" v-if="item.type === 'input-with-input'">
        <el-col :span="11">
          <el-input
            type="input"
            v-model.number="searchQuery[item.value[0]]"
            :disabled="item.disabled"
            :placeholder="getPlaceholder(item)"
          ></el-input>
        </el-col>
        <el-col :span="2">-</el-col>
        <el-col :span="11">
          <el-input
            type="input"
            v-model.number="searchQuery[item.value[1]]"
            :disabled="item.disabled"
            :placeholder="getPlaceholder(item)"
          ></el-input>
        </el-col>
      </el-row>
    </div>
  </div>
</template>
<script>
import { debug } from "util";
export default {
  name: "PageFilter",
  props: {
    // 自定义类名
    className: {
      type: String
    },
    // 参数
    query: {
      type: Object
    },
    // 拦截器列表
    filterList: {
      type: Array
    },
    // 属性值集合
    listTypeInfo: {
      type: Object,
      default: () => {
        return {};
      }
    },
    //是否显示label
    labelStatus: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      searchQuery: {},
      value: "3",
      flag: "inner", // 内 inner  外outside
      datePickerOptions: {
        // disabledDate (time) {
        //   // 大于当前的时间都不能选 (+十分钟让点击此刻的时候可以选择到当前时间)
        //   return time.getTime() > +new Date() + 1000 * 600 * 1
        // }
      }
    };
  },
  watch: {
    searchQuery: {
      handler: function(val) {
        // 传入参数修改，不派发
        if (this.flag === "outside") {
          this.flag = "inner";
          return;
        }
        // 修改传入进来的参数
        this.$emit("update:query", { ...this.query, ...val });
      },
      deep: true // 深度监听
    },
    query(val) {
      this.flag = "outside"; // 标识为传入参数修改
      this.searchQuery = val;
    }
  },
  mounted() {
    this.initParams();
  },
  methods: {
    initParams() {
      let obj = {};
      for (let key in this.query) {
        if (this.query[key]) {
          obj[key] = this.query[key];
        }
      }
      this.searchQuery = obj;
    },
    getPlaceholder(row) {
      let placeholder;
      if (
        row.type === "input" ||
        row.type === "textarea" ||
        row.type === "group-input" ||
        row.type === "input-with-input"
      ) {
        placeholder = "请输入" + row.label;
      } else if (
        row.type === "select" ||
        row.type === "time" ||
        row.type === "date"
      ) {
        placeholder = "请选择" + row.label;
      } else {
        placeholder = row.label;
      }
      return placeholder;
    },
    // 获取字段配置项列表
    getFieldConfigList() {
      return this.filterList.filter(
        item =>
          !item.hasOwnProperty("show") ||
          (item.hasOwnProperty("show") && item.show)
      );
    },
    // 绑定相关事件
    handleEvent(event) {
      if (event) this.$emit("handleEvent", event);
    },
    // 按钮点击事件
    handleClickBtn(event, data) {
      if (event) this.$emit("handleClickBtn", event, data);
    },
    handleGroupFieldChange(event, item) {
      let fields = item.fields;
      let temp = fields.filter(field => field.value === event);
      item.label = temp[0].label;
      this.searchQuery[item.value].field = event;
    }
  }
};
</script>
<style lang="scss" scoped>
.page-filter-container {
  padding: 5px 0;
  .filter-item {
    display: inline-flex;
    align-items: center;
    margin-bottom: 7px;
    margin-right: 10px;
  }
  .filter-label {
    padding-right: 5px;
    font-size: 14px;
    color: #606266;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  .filter-input,
  .filter-time,
  .filter-date,
  .filter-select {
    width: 180px;
  }
  .filter-button {
    background: #277ac0;
  }
  .input-with-select {
    /deep/ .el-select .el-input {
      width: 100px;
    }
    .el-input-group__prepend {
      background-color: #fff;
    }
  }
  .filter-input-with-input {
    display: flex;
    align-items: center;
    text-align: center;
  }
}
</style>
