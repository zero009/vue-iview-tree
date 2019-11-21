<template>
  <div class="wrap">
      <horizontal-tree v-for="node,index in rows" :data="node" :count="index" :key="node.id" class="node_con"></horizontal-tree>
			<button @click="submit">保存</button>	
      <!--<tree-line :detail="rows"></tree-line>-->
  </div>
</template>

<script>
    import HorizontalTree from '@/components/HorizontalTree'

    import TreeLine from '@/components/TreeLine'
export default {
  name: 'HelloWorld',
  data () {
    return {
      checkAllGroup:[],
      count:0,
        rows: [
            {
                id: 0, name: '海贼王', indeterminate: false, isFold: false, checked: false,
                children: [
                    { id:10,name: '路飞', indeterminate: false, isFold: false,checked: false,
                        children: [
                            { id: 20, name: '娜美', indeterminate: false,isFold: false, checked: false,
                                children: [
                                    {id: 30, name: '小兵1', indeterminate: false, isFold: false, checked: false},
                                    {id: 31, name: '小兵2', indeterminate: false, isFold: false, checked: false},
                                    {id: 32, name: '小兵3', indeterminate: false, isFold: false, checked: false},
                                    {id: 33, name: '小兵4', indeterminate: false, isFold: false, checked: false}]},
                            {id: 21, name: '乌索普', indeterminate: false, isFold: false, checked: false}
                        ]
                    },
                    {id: 11, name: '索隆', indeterminate: false, isFold: false, checked: false,
                    children: [
                                    {id: 100, name: '小虾米1', indeterminate: false, isFold: false, checked: false},
                                    {id: 102, name: '小虾米2', indeterminate: false, isFold: false, checked: false},
                                    {id: 312, name: '小虾米3', indeterminate: false, isFold: false, checked: false},
                                    {id: 133, name: '小虾米4', indeterminate: false, isFold: false, checked: false}]}
                ]
            },
            {
            	id: 2, name: '火影忍者', indeterminate: false, isFold: false, checked: false,
            	children: [
                    { id:20,name: '宇智波', indeterminate: false, isFold: false,checked: false},
                    { id:21,name: '鸣人', indeterminate: false, isFold: false,checked: false}
                    ]
            }
        ]
    }
  },
  components: {
    HorizontalTree,
    TreeLine
  },
  methods: {
  	submit () {
  		this.rows.map(item=>{
  			if(item.checked){
  				this.checkAllGroup.push(item.id)
  			}
  			this.cycleData(item.children)
  		})
  	},
  	cycleData(data){
  		data.map(m => {
  			if(m.checked){
  				console.log(m.id,m)
  				this.checkAllGroup.push(m.id)
  				if(m.children&&m.children.length>0){
		  			this.cycleData(m.children)
		  		}
  			}
  			
  		})
  	}
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	.wrap{
		width: 900px;
		text-align: center;
	}
	button{
		width: 130px;
		height: 30px;
		line-height: 30px;
		font-size: 18px;
		background: #ccc;
		color: #fff;
		outline: none;
		border: 1px solid #ddd;
	}
	.node_con{
		border:none;
	}
	.node_con:before{
		display: none;
	}
</style>
