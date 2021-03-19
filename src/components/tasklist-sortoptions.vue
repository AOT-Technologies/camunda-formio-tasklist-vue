<template>
    <b-list-group v-if="showSortListDropdown" class="cft-sortlist-options-dropdown">
        <b-list-group-item v-for="s in sortOptions" :key="s.sortBy" @click="addSort(s)">
        <div class="col-12"> {{s.label}}</div>
        </b-list-group-item>
    </b-list-group>
</template>

<script lang="ts">
import { Component, Emit, Prop, Vue } from "vue-property-decorator";

@Component
export default class TaskSortOptions extends Vue {
    @Prop() private sortOptions !: any;
    @Prop({default: 'true'}) private showSortListDropdown !: boolean;

  private sOptions: any = '';
  private showDrop = false;

  @Emit()
  addSort(sort: any){
    console.log(sort);
    return sort
  }

  // mounted() {
  //   this.sOptions = this.sortOptions;
  //   this.showDrop = this.showSortListDropdown;
  // }

}
</script>