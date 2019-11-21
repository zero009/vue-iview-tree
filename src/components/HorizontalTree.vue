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
	            <horizontal-tree v-for="node,index in data.children" :count="index" :data="node" :key="node.id"></horizontal-tree>
	        </div>
        </transition>
       
    </div>
</template>

<script>
export default {
  name: 'HorizontalTree',
  props: {
    data: Object,
    count:Number
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
  mounted(){
  },
  methods: {
    handleCheckAll (data) {
      data.checked = !data.checked
      data.indeterminate = false
	  
      if (data.checked) {
        this.checkAllGroup.push(data.id)
        this.cycleParent(this.$parent)
        if (data.children && data.children.length > 0) this.tree(this.checkAllGroup, data.children)
      } else {
      	this.cycleParent(this.$parent,'remove')
        if (data.children && data.children.length > 0) this.tree(this.checkAllGroup, data.children, 'remove')
      }
    },
    // 父影响子
    tree (arr, data, str) {
      if (data) {
        data.map(res => {
         //console.log(res)
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
    // 子影响父
    cycleParent ($p,str) {
    	let arrChecked = []
    	if($p.data) {
    		$p.data.checked = true
    		if ($p.data.children && $p.data.children.length>0){
    			$p.data.children.map(m=>{
    				if(m.checked==true) arrChecked.push(m.checked)
	    		})
    			if (str || this.isIndeter){
    				$p.data.indeterminate = true
    			} else {
    				if ($p.data.children.length == arrChecked.length){ console.log(1)
	    				$p.data.indeterminate = false
	    			} else {
	    				$p.data.indeterminate = true
	    				this.isIndeter = true
	    			}
    			}
    			
    			if (arrChecked.length == 0){
    				$p.data.checked = false
    				$p.data.indeterminate = false
    			}
    		}
    		if ($p.$parent.data){
    			this.cycleParent($p.$parent,str)
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