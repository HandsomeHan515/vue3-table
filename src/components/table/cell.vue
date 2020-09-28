<template>
  <div :class="classes">
    <template v-if="renderType === 'index'">{{ naturalIndex + 1 }}</template>
    <template v-if="renderType === 'selection'">
      <input
        type="checkbox"
        :checked="checked"
        :disabled="disabled"
        @change="toggleSelect"
      />
    </template>
    <template v-if="renderType === 'normal'">{{ row[column.key] }}</template>
  </div>
</template>
<script>
import { reactive, computed, toRef, onBeforeMount } from "vue";

export default {
  data() {
    return {
      renderType: "",
      uid: -1,
      content: "",
    };
  },
  props: {
    prefixCls: String,
    row: Object,
    column: Object,
    naturalIndex: Number,
    index: Number,
    checked: Boolean,
    disabled: Boolean,
    fixed: {
      type: [Boolean, String],
      default: false,
    },
  },
  created() {
    if (this.column.type === "index") {
      this.renderType = "index";
    } else if (this.column.type === "selection") {
      this.renderType = "selection";
    } else if (this.column.render) {
      this.renderType = "render";
    } else {
      this.renderType = "normal";
    }
  },
  computed: {
    classes() {
      return [
        `${this.prefixCls}-cell`,
        {
          [`${this.prefixCls}-hidden`]:
            !this.fixed &&
            this.column.fixed &&
            (this.column.fixed === "left" || this.column.fixed === "right"),
          [`${this.prefixCls}-cell-ellipsis`]: this.column.ellipsis || false,
        },
      ];
    },
  },
  methods: {
    toggleSelect() {
      this.$parent.$parent.toggleSelect(this.index);
    },
  },
};
</script>