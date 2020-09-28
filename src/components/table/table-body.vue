<template>
  <table cellspacing="0" cellpadding="0" border="0" :style="style">
    <colgroup>
      <col
        v-for="(column, index) in columns"
        :key="column.key"
        :width="setCellWidth(column, index, false)"
      />
    </colgroup>
    <tbody :class="[`${prefixCls}-tbody`]">
      <tr
        v-for="(row, index) in data"
        :key="index"
        :class="rowClasses(row._index)"
      >
        <td
          v-for="column in columns"
          :key="column.key"
          :class="alignCls(column, row)"
        >
          <Cell
            :fixed="fixed"
            :prefix-cls="prefixCls"
            :row="row"
            :column="column"
            :natural-index="index"
            :index="row._index"
            :checked="rowChecked(row._index)"
            :disabled="rowDisabled(row._index)"
          />
        </td>
      </tr>
    </tbody>
  </table>
</template>
<script>
import Cell from "./cell.vue";
import Mixin from "./mixin";

export default {
  name: "table-body",
  components: { Cell },
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
  methods: {
    rowClasses(_index) {
      return [
        `${this.prefixCls}-row`,
        {
          [`${this.prefixCls}-row-highlight`]:
            this.objData[_index] && this.objData[_index]._isHighlight,
          [`${this.prefixCls}-row-hover`]:
            this.objData[_index] && this.objData[_index]._isHover,
        },
      ];
    },
    rowChecked(_index) {
      return this.objData[_index] && this.objData[_index]._isChecked;
    },
    rowDisabled(_index) {
      return this.objData[_index] && this.objData[_index]._isDisabled;
    },
    rowClsName(_index) {
      return this.$parent.rowClassName(this.objData[_index], _index);
    },
  },
};
</script>