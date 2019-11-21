<template>
    <div class="org-tree-node">
        <div class="all-checked fl">
            <Checkbox
                    :indeterminate="data.indeterminate"
                    :value="data.checked"
                    @click.prevent.native="handleCheckAll(data)">{{data.name}}</Checkbox>
            <a href="javacript:;" class="icon" v-if="data.children" @click="hideChildren">+</a>

        </div>
        <div class="org-tree-node-children list" v-if="data.children">
            <org-tree-node v-for="node in data.children" :data="node" :key="node.id" v-on:indeterminate="indeterminate(data)"></org-tree-node>
        </div>
    </div>
</template>

<script>
export default {
  name: 'OrgTreeNode',
  props: {
    data: Object
  },
  data () {
    return {
      checkAllGroup: [],
      isShow: true
    }
  },
  methods: {
    handleCheckAll (data) {
      data.checked = !data.checked
      data.indeterminate = false
      console.log('--',this.checkAllGroup)

      if (data.checked) {
        this.checkAllGroup.push(data.id)
        console.log('add',this.checkAllGroup)
        if (data.children && data.children.length > 0) this.tree(this.checkAllGroup, data.children)
      } else {
        this.$emit('indeterminate', true)
        console.log('--',this.checkAllGroup)
        if (data.children && data.children.length > 0) this.tree(this.checkAllGroup, data.children, 'remove')
      }
    },
    indeterminate (data) {
      console.log('2',data)
      data.indeterminate = true
    },
    tree (arr, data, str) {
      if (data) {
        data.map(res => {
          res.indeterminate = false
          if (str === 'remove') {
            res.checked = false
          } else {
            res.checked = true
            this.checkAllGroup.push(res.id)
          }
          if (res.children && res.children.length > 0) {
            this.tree(arr, res.children, str)
          }
        })
      }
    },
    hideChildren () {
      this.isShow = false
    }
  }
}
</script>

<style>
    .fl{
        float: left;
        position: relative;
    }
    .list{
        display: inline-block;
    }
    .icon{
        width: 20px;
        height: 20px;
        line-height: 20px;
        text-align: center;
        background: #ccc;
        border-radius: 50%;
        display: inline-block;
        color: #fff;
        font-size: 14px;
    }
</style>