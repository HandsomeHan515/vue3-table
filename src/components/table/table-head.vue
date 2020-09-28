<template>
  <table cellspacing="0" cellpadding="0" border="0" :style="styles">
    <colgroup>
      <col
        v-for="(column, index) in columns"
        :key="column.key"
        :width="setCellWidth(column, index, true)"
      />
    </colgroup>
    <thead>
      <tr>
        <th
          v-for="(column, index) in columns"
          :key="index"
          :class="alignCls(column)"
        >
          <div :class="cellClasses(column)">
            <template v-if="column.type === 'selection'">
              <input
                type="checkbox"
                :checked="isSelectAll"
                @change="selectAll"
              />
            </template>
            <template v-else>
              {{ column.title || "#" }}
              <span :class="[prefixCls + '-sort']" v-if="column.sortable">
                <i
                  :class="{ on: column._sortType === 'asc' }"
                  @click="handleSort(index, 'asc')"
                  >上</i
                >
                <i
                  :class="{ on: column._sortType === 'desc' }"
                  @click="handleSort(index, 'desc')"
                  >下</i
                >
              </span>
            </template>
          </div>
        </th>
      </tr>
    </thead>
  </table>
</template>
<script>
import Mixin from "./mixin";

export default {
  name: "table-head",
  mixins: [Mixin],
  props: {
    prefixCls: String,
    style: Object,
    columns: Array,
    data: Array,
    objData: Object,
    columnsWidth: Object,
    fixed: {
      type: [Boolean, String],
      default: false,
    },
  },
  computed: {
    styles() {
      const style = Object.assign({}, this.style);
      const width =
        this.$parent.bodyHeight === 0
          ? parseInt(this.style.width)
          : parseInt(this.style.width) + this.$parent.scrollBarWidth;
      style.width = `${width}px`;
      return style;
    },
    isSelectAll() {
      let isSelectAll = true;
      if (!this.data.length) isSelectAll = false;
      for (let i = 0; i < this.data.length; i++) {
        if (
          !this.objData[this.data[i]._index]._isChecked &&
          !this.objData[this.data[i]._index]._isDisabled
        ) {
          isSelectAll = false;
          break;
        }
      }
      return isSelectAll;
    },
  },
  methods: {
    cellClasses(column) {
      return [
        `${this.prefixCls}-cell`,
        {
          [`${this.prefixCls}-hidden`]:
            !this.fixed &&
            column.fixed &&
            (column.fixed === "left" || column.fixed === "right"),
        },
      ];
    },
    selectAll() {
      const status = !this.isSelectAll;
      this.$parent.selectAll(status);
    },
    handleSort(index, type) {
      this.$parent.handleSort(index, type);
    },
  },
};
</script>