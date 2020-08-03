<template>
  <div class="q-pa-md">
    <q-card class="my-card">
      <q-card-section>
        <div class="text-h6">项目查询</div>
        <q-form class="search-box" @submit="onSubmit">
            <div class="q-gutter-md" style="display: inline-flex">
              <q-select square outlined v-model="model" :options="options" label="搜索条件" />
              <q-input square outlined v-model="search_text" label="search" />
              <q-btn label="搜索" outline type="submit" color="primary"/>
            </div>
        </q-form>
      </q-card-section>

      <q-markup-table>
        <thead>
        <tr>
          <th class="text-left"><q-checkbox v-model="fullSelect" label="全选"/></th>
          <th class="text-center">项目编号</th>
          <th class="text-center">项目名称</th>
          <th class="text-center">单价</th>
          <th class="text-center">来源网址</th>
          <th class="text-center">类型</th>
          <th class="text-center">业务</th>
          <th class="text-center">状态</th>
          <th class="text-center">已收藏</th>
        </tr>
        </thead>
        <tbody>
        <template v-if="searchResults">
          <tr v-for="items in searchResults" v-bind:key="items">
            <td class="text-center"></td>
            <td class="text-center">{{ items.split(',')[0].trim('|') }}</td>
            <td class="text-center">{{ items.split(',')[1] }}</td>
            <td class="text-center">{{ items.split(',')[2] }}</td>
            <td class="text-center"></td>
            <td class="text-center">{{ items.split(',')[3] }}</td>
          </tr>
        </template>
        <tr v-else>
          <td colspan="9" class="text-center">暂无数据</td>
        </tr>
        </tbody>
      </q-markup-table>
    </q-card>
  </div>
</template>

<style lang="sass" scoped>
.my-card
  width: 100%
  //max-width: 500px
</style>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      user_token: this.$q.cookies.get('user_token'),
      fullSelect: true,
      searchResults: '',
      model: '项目名称',
      search_text: '',
      options: [
        '模糊搜索', '项目名称', '项目编号', '网址'
      ]
    }
  },
  methods: {
    onSubmit () {
      const api = `http://dkh.nmgldjy.com:81/service.asmx/GetXMmcStr?token=${this.user_token}&mc=${this.search_text}`
      axios.get(api).then(res => {
        if (res.data !== '') {
          this.searchResults = res.data.split(';')
          console.log(this.searchResults)
        }
      })
    }
  }
}

</script>
