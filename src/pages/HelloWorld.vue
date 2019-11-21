<template>
  <div>
      <org-tree-node v-for="node in rows" :data="node" :key="node.id" v-on:indeterminate="indeterminate(rows)"></org-tree-node>

      <!--<tree-line :detail="rows"></tree-line>-->
  </div>
</template>

<script>
    import OrgTreeNode from '@/components/OrgTreeNode'

    import TreeLine from '@/components/TreeLine'
export default {
  name: 'HelloWorld',
  data () {
    return {
      checkAllGroup:[],
        rows: [
            {
                id: 0, name: '项目管理', indeterminate: false, type: 1, checked: false,
                children: [
                    { id:10,name: '训练营课程组合', indeterminate: false, type: 2,checked: false,
                        children: [
                            { id: 20, name: '课程组合模板', indeterminate: false,type: 3, checked: false,
                                children: [
                                    {id: 30, name: '查看列表', indeterminate: false, type: 4, checked: false},
                                    {id: 31, name: '查询', indeterminate: false, type: 4, checked: false},
                                    {id: 32, name: '查询详情', indeterminate: false, type: 4, checked: false},
                                    {id: 33, name: '创建', indeterminate: false, type: 4, checked: false}]},
                            {id: 21, name: '团队课程组合', indeterminate: false, type: 4, checked: false}
                        ]
                    },
                    {id: 1, name: '训练营课程库', indeterminate: false, type: 2, checked: false}
                ]
            }
        ]
    }
  },
  components: {
    OrgTreeNode,
    TreeLine
  },
  methods: {
    indeterminate (data) {
      console.log('1',data)
      data.indeterminate = true
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
