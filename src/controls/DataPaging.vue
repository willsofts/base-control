<template>
<div class="fschaptertablelayer">
<table class="fschaptertable">
	<tr class="fschapterrow"><td class="fschaptercolumn">
	<div class="fschapterlayer">
    <ul :class="pageCSS.ulCSS" >
        <li v-for="(item,index) in pager" :key="index" :class="[pageCSS.liCSS, item.css]">
            <A HREF="javascript:void(0)" @click="pageSelect(item)" :class="[pageCSS.aCSS, item.css]" :data-paging="item.page">{{ item.text }}</A>
        </li>
    </ul>
    </div>
	</td>
	</tr>
</table>
</div>
</template>
<script>
import { ref } from 'vue';
import { Paging, DEFAULT_PAGE_SETTINGS } from "../assets/js/Paging.js";

const defaultCSS = {
  ulCSS: "page-table-class pagination pagination-sm",
  liCSS: "page-item page-column-class",
  aCSS: "page-link pagenoclass fa-data-page"
};

export default {
  props: {
    settings: Object,
    css: Object,
  },
  emits: ["page-select"],
  setup(props) {
    let paging = new Paging(props.settings);
    let model = paging.buildPagingModel();
    let pager = ref(model);
    let pageCSS = {...defaultCSS, ...props.css};
    return { paging, pager, pageCSS };
  },
  methods: {
    clear() {
      this.reset(DEFAULT_PAGE_SETTINGS);
    },
    reset(newSettings) {
      if(newSettings) {
        this.paging.reset(newSettings);
        let model = this.paging.buildPagingModel();
        this.pager = {...model};
      }
    },
    pageSelect(item) {
      this.$emit('page-select', item);
    },
  }
};
</script>
