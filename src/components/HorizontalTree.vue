<template>
    <div class="tree-node cf">
        <div class="all-checked fl">
            <Checkbox
                    :indeterminate="data.indeterminate"
                    :value="data.checked"
                    @click.prevent.native="handleCheckAll(data)">{{data.name}}</Checkbox>
            <a href="javacript:;" class="icon" v-if="data.children" @click="hideChildren(data)">{{data.isFold?'+':'-'}}</a>

        </div>
        <transition name="fade">
            <div class="tree-children list" v-if="data.children" v-show="!data.isFold">
              <horizontal-tree v-for="node in data.children" :data="node" :key="node.id"></horizontal-tree>
            </div>
        </transition>

    </div>
</template>

<script>
export default {
  name: 'HorizontalTree',
  props: {
    data: Object
  },
  data () {
    return {
      checkAllGroup: [],
      isIndeter: false,
      isShow: true
    }
  },
  computed: {
  },
  mounted () {
  },
  methods: {
    handleCheckAll (data) {
      data.checked = !data.checked
      data.indeterminate = false
      if (data.checked) {
        this.cycleParent(this.$parent)
        if (data.children && data.children.length > 0) this.tree(data.children)
      } else {
        this.cycleParent(this.$parent, 'remove')
        if (data.children && data.children.length > 0) this.tree(data.children, 'remove')
      }
    },
    // 父影响子
    tree (data, str) {
      if (data) {
        data.map(res => {
          res.indeterminate = false
          res.checked = str === 'remove' ? false:true
          if (res.children && res.children.length > 0) {
            this.tree(res.children, str)
          }
        })
      }
    },
    // 子影响父
    cycleParent ($p, str) {
      let arrChecked = [], id = null
      if($p.data) {
        $p.data.checked = true
        if ($p.data.children && $p.data.children.length > 0) {
          $p.data.children.map(m => {
            if (m.checked) {
              arrChecked.push(m)
              if (m.name === "佐助" && !str) {
                $p.data.children.map(item => {
                  if (item.name === "小樱"||item.name === "鸣人"||item.name === "左良娜") item.checked = true
                })
              }
            } else {
              if (m.name === "佐助") {
                $p.data.children.map(item => {
                  if (item.name === "小樱" || item.name === "鸣人" || item.name === "左良娜") item.checked = false
                })
              }
            }
            if (m.indeterminate === true) id = $p.data.id
          })
          if (str && this.isIndeter) {
            $p.data.indeterminate = true
          } else {
            if ($p.data.children.length == arrChecked.length) {
              $p.data.indeterminate = false
              if(id === $p.data.id) {
                $p.data.indeterminate = true
              }
            } else {
              $p.data.indeterminate = true
              // this.isIndeter = true
            }
          }
          if (arrChecked.length == 0) {
            $p.data.checked = false
            $p.data.indeterminate = false
          }
        }
        if ($p.$parent.data) {
          this.cycleParent($p.$parent, str)
        }
      }
    },
    // 收起展开
    hideChildren (data) {
      data.isFold = !data.isFold
    }
  }
}
</script>

<style>
	.cf:after{
		content:" ";
		display:block;
		height:0;
		visibility:hidden;
		clear:both;
	}
    .fl{
        float: left;
        margin-bottom: 10px;
    }
    .list{
        display: inline-block;
        margin-bottom: 20px;
        float: left;
    }
    a:hover{
    	color: #fff;
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
        font-size: 16px;
        font-weight: bold;
    }
    .all-checked{
    	margin-right: 10px;
    	max-width: 200px;
    	text-align: left;
    	padding-left: 30px;
    	position: relative;
    	top: -10px;
    }
    .ivu-checkbox-wrapper{
    	font-size: 14px;
    }
    .fade-enter-active, .fade-leave-active {
	  transition: opacity .2s;
	}
	.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
	  opacity: 0;
	}
	.tree-children .tree-node:nth-last-child(1){
		border: none;
	}
	.tree-node{
		border-left: 1px solid #ccc;
		position: relative;
	}
	.tree-node:before{
		position: absolute;
		border-top: 1px solid #ccc;
		width: 26px;
		height: 1px;
		top:0px;
		left: 0;
		content: " ";
	}
</style>
