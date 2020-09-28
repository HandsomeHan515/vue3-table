<template>
  <div :class="wrapClasses" :style="styles">
    <div :class="classes">
      <div :class="[`${prefixCls}-title`]" v-if="showSlotHeader" ref="title">
        <slot name="header"></slot>
      </div>
      <div :class="[`${prefixCls}-header`]" v-if="showHeader" ref="header">
        <table-head
          :prefix-cls="prefixCls"
          :style="tableStyle"
          :columns="cloneColumns"
          :obj-data="objData"
          :columns-width="columnsWidth"
          :data="rebuildData"
        >
        </table-head>
      </div>
      <div
        :class="[`${prefixCls}-body`]"
        :style="bodyStyle"
        ref="body"
        v-show="rebuildData.length > 0"
        @scroll="handleBodyScroll"
      >
        <table-body
          ref="tbody"
          :prefix-cls="prefixCls"
          :style="tableStyle"
          :columns="cloneColumns"
          :data="rebuildData"
          :columns-width="columnsWidth"
          :obj-data="objData"
        >
        </table-body>
      </div>
      <div v-show="rebuildData.length === 0" :class="[`${prefixCls}-tip`]">
        <table cellspacing="0" cellpadding="0" border="0">
          <tbody>
            <tr>
              <td :style="{ height: bodyStyle.height }">
                {{ noDataText }}
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      <div
        :class="[`${prefixCls}-fixed`]"
        :style="fixedTableStyle"
        v-if="isLeftFixed"
      >
        <div :class="[`${prefixCls}-fixed-header`]" v-if="showHeader">
          <table-head
            fixed="left"
            :prefix-cls="prefixCls"
            :style="fixedTableStyle"
            :columns="leftFixedColumns"
            :obj-data="objData"
            :columns-width="columnsWidth"
            :data="rebuildData"
          >
          </table-head>
        </div>
        <div
          :class="[`${prefixCls}-fixed-body`]"
          :style="fixedBodyStyle"
          ref="fixedBody"
        >
          <table-body
            fixed="left"
            :prefix-cls="prefixCls"
            :style="fixedTableStyle"
            :columns="leftFixedColumns"
            :obj-data="objData"
            :columns-width="columnsWidth"
            :data="rebuildData"
          >
          </table-body>
        </div>
      </div>
      <div
        :class="[`${prefixCls}-fixed-right`]"
        :style="fixedRightTableStyle"
        v-if="isRightFixed"
      >
        <div :class="[`${prefixCls}-fixed-header`]" v-if="showHeader">
          <table-head
            fixed="right"
            :prefix-cls="prefixCls"
            :style="fixedRightTableStyle"
            :columns="rightFixedColumns"
            :obj-data="objData"
            :columns-width="columnsWidth"
            :data="rebuildData"
          >
          </table-head>
        </div>
        <div
          :class="[`${prefixCls}-fixed-body`]"
          :style="fixedBodyStyle"
          ref="fixedRightBody"
        >
          <table-body
            fixed="right"
            :prefix-cls="prefixCls"
            :style="fixedRightTableStyle"
            :columns="rightFixedColumns"
            :obj-data="objData"
            :columns-width="columnsWidth"
            :data="rebuildData"
          >
          </table-body>
        </div>
      </div>
      <div :class="[`${prefixCls}-footer`]" v-if="showSlotFooter" ref="footer">
        <slot name="footer"></slot>
      </div>
    </div>
  </div>
</template>
<script>
import TableHead from "./table-head.vue";
import TableBody from "./table-body.vue";
import { getScrollBarSize, deepCopy, getStyle } from "./util";
const prefixCls = "b-table";

export default {
  name: "b-table",
  components: { TableHead, TableBody },
  props: {
    data: {
      type: Array,
      default: () => [],
    },
    columns: {
      type: Array,
      default: () => [],
    },
    width: [Number, String],
    height: [Number, String],
    stripe: {
      type: Boolean,
      default: false,
    },
    border: {
      type: Boolean,
      default: false,
    },
    showHeader: {
      type: Boolean,
      default: true,
    },
    highlightRow: {
      type: Boolean,
      default: false,
    },
    rowClassName: {
      type: Function,
      default: () => "",
    },
    noDataText: {
      type: String,
      default: () => "暂无数据",
    },
  },
  data() {
    return {
      ready: false,
      tableWidth: 0,
      columnsWidth: {},
      prefixCls,
      objData: this.makeObjData(), // checkbox or highlight-row
      rebuildData: {}, // for sort or filter
      cloneColumns: this.makeColumns(),
      showSlotHeader: false,
      showSlotFooter: false,
      bodyHeight: 0,
      bodyRealHeight: 0,
      scrollBarWidth: getScrollBarSize(),
    };
  },
  computed: {
    wrapClasses() {
      return [
        `${prefixCls}-wrapper`,
        {
          [`${prefixCls}-hide`]: !this.ready,
          [`${prefixCls}-with-header`]: this.showSlotHeader,
          [`${prefixCls}-with-footer`]: this.showSlotFooter,
        },
      ];
    },
    classes() {
      return [
        `${prefixCls}`,
        {
          [`${prefixCls}-border`]: this.border,
          [`${prefixCls}-stripe`]: this.stripe,
          [`${prefixCls}-with-fixed-top`]: !!this.height,
        },
      ];
    },
    styles() {
      let style = {};
      if (this.height) {
        const height =
          this.isLeftFixed || this.isRightFixed
            ? parseInt(this.height) + this.scrollBarWidth
            : parseInt(this.height);
        style.height = `${height}px`;
      }
      if (this.width) style.width = `${this.width}px`;
      return style;
    },
    tableStyle() {
      let style = {};
      if (this.tableWidth !== 0) {
        let width = "";
        if (this.bodyHeight === 0) {
          width = this.tableWidth;
        } else {
          if (this.bodyHeight > this.bodyRealHeight) {
            width = this.tableWidth;
          } else {
            width = this.tableWidth - this.scrollBarWidth;
          }
        }
        style.width = `${width}px`;
      }
      return style;
    },
    bodyStyle() {
      let style = {};
      if (this.bodyHeight !== 0) {
        // add a height to resolve scroll bug when browser has a scrollBar in fixed type and height prop
        const height =
          this.isLeftFixed || this.isRightFixed
            ? this.bodyHeight + this.scrollBarWidth
            : this.bodyHeight;
        style.height = `${height}px`;
      }
      return style;
    },
    fixedTableStyle() {
      let style = {};
      let width = 0;
      this.leftFixedColumns.forEach((col) => {
        if (col.fixed && col.fixed === "left") width += col._width;
      });
      this.width = `${width}px`;
      return style;
    },
    fixedRightTableStyle() {
      let style = {};
      let width = 0;
      this.rightFixedColumns.forEach((col) => {
        if (col.fixed && col.fixed === "right") width += col._width;
      });
      width += this.scrollBarWidth;
      style.width = `${width}px`;
      return style;
    },
    fixedBodyStyle() {
      let style = {};
      if (this.bodyHeight !== 0) {
        let height = this.bodyHeight + this.scrollBarWidth - 1;
        if (this.width && this.width < this.tableWidth) {
          height = this.bodyHeight;
        }
        style.height =
          this.scrollBarWidth > 0 ? `${height}px` : `${height - 1}px`;
      }
      return style;
    },
    leftFixedColumns() {
      let left = [];
      this.cloneColumns.forEach((col) => {
        if (col.fixed && col.fixed === "left") {
          left.push(col);
        }
      });
      return left;
    },
    rightFixedColumns() {
      let right = [];
      this.cloneColumns.forEach((col) => {
        if (col.fixed && col.fixed === "right") {
          right.push(col);
        }
      });
      return right;
    },
    isLeftFixed() {
      return this.columns.some((col) => col.fixed && col.fixed === "left");
    },
    isRightFixed() {
      return this.columns.some((col) => col.fixed && col.fixed === "right");
    },
  },
  methods: {
    handleResize() {
      this.$nextTick(() => {
        const allWidth = !this.columns.some((cell) => !cell.width);
        if (allWidth) {
          this.tableWidth = this.columns
            .map((cell) => cell.width)
            .reduce((a, b) => a + b);
        } else {
          this.tableWidth = parseInt(getStyle(this.$el, "width")) - 1;
          console.log("111", this.tableWidth);
        }
        this.columnsWidth = {};
        this.$nextTick(() => {
          let columnsWidth = {};
          let autoWidthIndex = -1;
          if (allWidth)
            autoWidthIndex = this.cloneColumns.findIndex((cell) => !cell.width);
          if (this.data.length) {
            const $td = this.$refs.tbody.$el
              .querySelectorAll("tbody tr")[0]
              .querySelectorAll("td");
            for (let i = 0; i < $td.length; i++) {
              const column = this.cloneColumns[i];
              let width = parseInt(getStyle($td[i], "width"));
              if (i === autoWidthIndex) {
                width = parseInt(getStyle($td[i]) - 1);
              }
              if (column.width) width = column.width;
              this.cloneColumns[i]._width = width;
              columnsWidth[column._index] = { width };
            }
            this.columnsWidth = columnsWidth;
          }
        });
        this.bodyRealHeight = parseInt(
          getStyle(this.$refs.tbody.$el, "height")
        );
      });
    },
    handleBodyScroll(event) {
      if (this.showHeader)
        this.$refs.header.scrollLeft = event.target.scrollLeft;
      if (this.isLeftFixed)
        this.$refs.fixedBody.scrollTop = event.target.scrollTop;
    },
    getSelection() {
      let selectionIndexes = [];
      for (let i in this.objData) {
        if (this.objData[i]._isChecked) selectionIndexes.push(parseInt(i));
      }
      return JSON.parse(
        JSON.stringify(
          this.data.filter(
            (data, index) => selectionIndexes.indexOf(index) > -1
          )
        )
      );
    },
    toggleSelect(_index) {
      let data = {};
      for (let i in this.objData) {
        if (parseInt(i) === _index) {
          data = this.objData[i];
        }
      }
      const status = !data._isChecked;
      this.objData[_index]._isChecked = status;
      const selection = this.getSelection();
      if (status) {
        this.$emit(
          "on-select",
          selection,
          JSON.parse(JSON.stringify(this.data[_index]))
        );
      }
      this.$emit("on-selection-change", selection);
    },
    selectAll(status) {
      for (const data of this.rebuildData) {
        if (this.objData[data._index]._isDisabled) {
          continue;
        } else {
          this.objData[data._index]._isChecked = status;
        }
      }
      const selection = this.getSelection();
      if (status) {
        this.$emit("on-select-all", selection);
      }
      this.$emit("on-selection-change", selection);
    },
    fixedHeader() {
      if (this.height) {
        this.$nextTick(() => {
          const titleHeight =
            parseInt(getStyle(this.$refs.title, "height")) || 0;
          const headerHeight =
            parseInt(getStyle(this.$refs.header, "height")) || 0;
          const footerHeight =
            parseInt(getStyle(this.$refs.footer, "height")) || 0;
          this.bodyHeight =
            this.height - titleHeight - headerHeight - footerHeight;
        });
      } else {
        this.bodyHeight = 0;
      }
    },
    makeObjData() {
      let data = {};
      this.data.forEach((row, index) => {
        const newRow = deepCopy(row); // todo 直接替换
        newRow._isHover = false;
        if (newRow._disabled) {
          newRow._isDisabled = newRow._disabled;
        } else {
          newRow._isDisabled = false;
        }

        if (newRow._checked) {
          newRow._isChecked = newRow._checked;
        } else {
          newRow._isChecked = false;
        }

        if (newRow._highlight) {
          newRow._isHighlight = newRow._highlight;
        } else {
          newRow._isHighlight = false;
        }
        data[index] = newRow;
      });
      return data;
    },
    makeColumns() {
      let columns = deepCopy(this.columns);
      let left = [];
      let right = [];
      let center = [];

      columns.forEach((column, index) => {
        column._index = index;
        column._width = column.width || ""; // update in handleResize()
        column._sortType = "normal";

        // ...
        if (column.fixed && column.fixed === "left") {
          left.push(column);
        } else if (column.fixed && column.fixed === "right") {
          right.push(column);
        } else {
          center.push(column);
        }
      });
      return left.concat(center).concat(right);
    },
    sortData(data, type, index) {
      const key = this.cloneColumns[index].key;
      data.sort((a, b) => {
        if (this.cloneColumns[index].sortMethod) {
          return this.cloneColumns[index].sortMethod(a[key], b[key], type);
        } else {
          if (type === "asc") {
            return a[key] > b[key] ? 1 : -1;
          } else if (type === "desc") {
            return a[key] < b[key] ? 1 : -1;
          }
        }
      });
      return data;
    },
    handleSort(index, type) {
      const key = this.cloneColumns[index].key;
      if (this.cloneColumns[index].sortable) {
        this.rebuildData = this.sortData(this.rebuildData, type, index);
      }
      this.cloneColumns[index]._sortType = type;
      this.$emit("on-sort-change", {
        column: JSON.parse(
          JSON.stringify(this.columns[this.cloneColumns[index]._index])
        ),
        key: key,
        order: type,
      });
    },
    makeData() {
      // add _index in data field
      let data = deepCopy(this.data);
      data.forEach((row, index) => (row._index = index));
      return data;
    },
    makeDataWithSort() {
      let data = this.makeData();
      let sortType = "normal";
      let sortIndex = -1;
      let isCustom = false;

      for (let i = 0; i < this.cloneColumns.length; i++) {
        if (this.cloneColumns[i]._sortType !== "normal") {
          sortType = this.cloneColumns[i]._sortType;
          sortIndex = i;
          isCustom = this.cloneColumns[i].sortable === "custom";
          break;
        }
      }

      if (sortType !== "normal" && !isCustom)
        data = this.sortData(data, sortType, sortIndex);
      return data;
    },
  },
  created() {
    if (!this.content) this.content = this.$parent;
    this.rebuildData = this.makeDataWithSort();
  },
  mounted() {
    this.handleResize();
    this.fixedHeader();
    this.$nextTick(() => (this.ready = true));
    window.addEventListener("resize", this.handleResize, false);
  },
  beforeUnmount() {
    window.removeEventListener("resize", this.handleResize, false);
  },
  watch: {
    data: {
      handler() {
        this.objData = this.makeObjData();
        this.rebuildData = this.makeDataWithSort();
        this.handleResize();
      },
      deep: true,
    },
    columns: {
      handler() {
        this.cloneColumns = this.makeColumns();
        this.rebuildData = this.makeDataWithSort();
        this.handleResize();
      },
      deep: true,
    },
    height() {
      this.fixedHeader();
    },
  },
};
</script>