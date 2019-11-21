<template>
    <div id="tree-wrap">
        <div class="container">
            <CheckboxGroup v-model="checkAllGroup" @on-change="checkAllGroupChange">
            <div v-for="item in detail">
                <div class="all-checked fl">
                    <Checkbox
                        :indeterminate="item.indeterminate"
                        :label="item.id"
                        @click.prevent.native="handleCheckAll(item)">{{item.name}}</Checkbox>
                </div>
                <div class="list">
                    <tree-line :detail="detail.children" v-if="detail.children"></tree-line>
                    <!--<CheckboxGroup v-model="item.checkAllGroup" @on-change="checkAllGroupChange">-->
                        <!--<div v-for="item1 in item.children">-->
                            <!--<div class="all-checked fl">-->
                                <!--<Checkbox-->
                                    <!--:indeterminate="item1.indeterminate"-->
                                    <!--:label="item1.id"-->
                                    <!--@click.prevent.native="handleCheckAll(item1)">{{item1.name}}</Checkbox>-->
                            <!--</div>-->
                            <!--<div class="list">-->
                                <!--<CheckboxGroup v-model="item1.checkAllGroup" @on-change="checkAllGroupChange">-->
                                    <!--<div v-for="item2 in item1.children">-->
                                        <!--<div class="all-checked fl">-->
                                            <!--<Checkbox :label="item2.id">{{item2.name}}</Checkbox>-->
                                        <!--</div>-->
                                        <!--<div class="list">-->
                                            <!--<CheckboxGroup v-model="item2.checkAllGroup" @on-change="checkAllGroupChange">-->
                                                <!--<div v-for="item3 in item2.children">-->
                                                    <!--<div class="all-checked fl">-->
                                                        <!--<Checkbox :label="item3.id">{{item3.name}}</Checkbox>-->
                                                    <!--</div>-->
                                                <!--</div>-->

                                            <!--</CheckboxGroup>-->
                                        <!--</div>-->
                                    <!--</div>-->

                                <!--</CheckboxGroup>-->
                            <!--</div>-->
                        <!--</div>-->
                    <!--</CheckboxGroup>-->
                </div>
            </div>
            </CheckboxGroup>
        </div>
    </div>
</template>
<script>
import TreeLine from '@/components/TreeLine'
export default {
  name: 'TreeLine',
  props: ['detail'],
  data () {
    return {
      checkAllGroup: [],
      childrenData: [],
      indeterminate: false,
      checkAll: false,
      single: true
    }
  },
  components: {
    TreeLine
  },
  methods: {
    tree (arr, data, str) {
      if (data) {
        data.map(res => {
          arr.push(res.id)
          res.checked = !data.checked
          if (str === 'remove') {
            arr = []
          }
          if (res.children && res.children.length > 0) {
            this.tree(res.checkAllGroup, res.children)
          }
        })
      }
    },
    handleCheckAll (data) {
      data.checkAllGroup = []
      data.checked = !data.checked
      // if (data.indeterminate) {
      //     data.checkAll = false;
      // } else {
      //     data.checkAll = !data.checkAll;
      // }
      // data.indeterminate = false;
      // 全选
      if (data.checked) {
        // this.checkAllGroup.push(data.id)
        console.log(this.checkAllGroup)
        if (data.children && data.children.length > 0) this.tree(data.checkAllGroup, data.children)
      } else {
        // console.log(this.checkAllGroup.filter((item) => item.id === data.id))
        if (data.children && data.children.length > 0) this.tree(data.checkAllGroup, data.children, 'remove')
      }
    },
    checkAllGroupChange (data) {
      console.log(data)
    }
  }
}
</script>
<style>
    .ivu-checkbox-indeterminate .ivu-checkbox-inner{
        background-color: #ed174c;
        border-color: #ed174c;
    }
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
        border-radius: 50%;
        font-size: 14px;
        background: #ccc;
    }
    .line{
        width: 30px;
        border-bottom: 1px solid #ccc;
        height: 1px;
        display: inline-block;
    }
    .container{

    }

</style>
