<template>
  <div id="search">
    <div class="search-header" v-loading="loading" :element-loading-background="loadingBackground">
      <el-form :inline="true">
        <el-select v-model="target" placeholder="search" class="search-target">
          <el-option
            v-for="item in searchTargets"
            :key="item.target"
            :label="item.label"
            :value="item.target">
          </el-option>
        </el-select>
        <input v-model="query" placeholder="keyword" class="search-keyword" v-shortkey="['enter']" @shortkey="search" autofocus></input>
        <div class="clearfix"></div>
      </el-form>
    </div>
    <div class="search-result">
      <search-account></search-account>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex'
import SearchAccount from './Search/Account'

export default {
  name: 'search',
  components: { SearchAccount },
  data () {
    return {
      target: 'account',
      searchTargets: [
        {
          target: 'account',
          label: 'Account'
        }
      ],
      query: ''
    }
  },
  computed: {
    ...mapState({
      loading: state => state.TimelineSpace.Contents.Search.loading,
      loadingBackground: state => state.App.theme.wrapper_mask_color
    })
  },
  methods: {
    search () {
      switch (this.target) {
        case 'account':
          this.$store.dispatch('TimelineSpace/Contents/Search/Account/search', this.query)
            .catch(() => {
              this.$message({
                message: 'Could not search',
                type: 'error'
              })
            })
          break
        default:
          break
      }
    }
  }
}
</script>

<style lang="scss" scoped>
#search {
  border-top: 1px solid var(--theme-border-color);

  .search-header {
    background-color: var(--theme-selected-background-color);
    padding: 8px 12px;

    .search-target /deep/ {
      float: left;
      width: 20%;

      .el-input__inner {
        background-color: var(--theme-background-color);
        border: none;
        border-radius: 4px 0 0 4px;
        color: var(--theme-primary-color);
      }
    }

    .search-keyword {
      float: left;
      width: 80%;
      background-color: var(--theme-background-color);
      border: none;
      border-radius: 0 4px 4px 0;
      color: var(--theme-primary-color);
      line-height: 40px;
      height: 40px;
      padding: 0 15px;
      outline: 0;
      font-size: 14px;
      box-sizing: border-box;
    }
  }
}
</style>
