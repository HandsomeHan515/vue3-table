<template>
  <b-table
    :data="data"
    :columns="columns"
    border
    stripe
    :height="200"
    @on-select="handleSelect"
    @on-select-all="handleSelectAll"
    @on-selection-change="handleSelectionChange"
    @on-sort-change="handleSortChange"
  ></b-table>
</template>

<script>
import Table from "./components/table/table.vue";

export default {
  name: "App",
  components: { "b-table": Table },
  data() {
    return {
      columns: [
        { type: "selection", width: 60, fixed: "left" },
        { type: "index", width: 50 },
        { key: "id", title: "ID", width: 100, sortable: true },
        { key: "nsrsbh", title: "纳税人识别号", width: 200 },
        { key: "dwdm", title: "单位代码", width: 200 },
        { key: "dwdm", title: "单位代码", width: 200 },
        { key: "dwdm", title: "单位代码", width: 200 },
        { key: "dwdm", title: "单位代码", width: 200 },
        { key: "dwdm", title: "单位代码", width: 200 },
        { key: "dwdm", title: "单位代码", width: 200 },
        { key: "dwdm", title: "单位代码", width: 200 },
        { key: "dwdm", title: "单位代码", width: 200 },
        { key: "dwmc", title: "操作", width: 140, fixed: "right" },
      ],
      data: [
        {
          id: 11,
          nsrsbh: "110108201809777",
          dwdm: "050101",
          dwmc: "北京东吴科技",
        },
        {
          id: 2,
          nsrsbh: "110108201809737",
          dwdm: "0501010001",
          dwmc: "北京科技",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
        {
          id: 1,
          nsrsbh: "110108201809747",
          dwdm: "05010101",
          dwmc: "北京东吴",
        },
      ],
    };
  },
  methods: {
    handleSelect(val) {
      // console.log("select", val);
    },
    handleSelectAll(val) {
      // console.log("select all", val);
    },
    handleSelectionChange(val) {
      console.log("selection change", val);
    },
    handleSortChange(val) {
      console.log("sort change", val);
    },
  },
};
</script>
