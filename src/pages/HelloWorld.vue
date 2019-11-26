<template>
  <div class="wrap">
      <horizontal-tree v-for="node in rows" :data="node" :key="node.id" :class="[rows.length==1 ? 'node_con':'node_wrap']"></horizontal-tree>
			<button @click="submit">保存</button>
  </div>
</template>

<script>
    import HorizontalTree from '@/components/HorizontalTree'

export default {
  name: 'HelloWorld',
  data () {
    return {
      checkAllGroup:[],
        rows: [
            {
                id: 0, name: '海贼王', checked: 1,
                children: [
                    { id:10,name: '路飞', checked: 1,
                        children: [
                            { id: 20, name: '娜美', checked: 1,
                                children: [
                                    {id: 30, name: '小兵1', checked: 1},
                                    {id: 31, name: '小兵2', checked: 1},
                                    {id: 32, name: '小兵3', checked: 1},
                                    {id: 33, name: '小兵4', checked: false,
                                    children: [{id: 33, name: '小小兵4', checked: false,
                                     children: [{id: 34, name: '小小兵5', checked: false}]
                                     }]
                                    }
                                    ]},
                            {id: 21, name: '乌索普', checked: false}
                        ]
                    },
                    {id: 11, name: '索隆', checked: false,
                    children: [
                                    {id: 100, name: '小虾米1', checked: false},
                                    {id: 102, name: '小虾米2', checked: false},
                                    {id: 312, name: '小虾米3', checked: false},
                                    {id: 133, name: '小虾米4', checked: false}]}
                ]
            },
            {
            	id: 2, name: '火影忍者', checked: false,
            	children: [
                    { id:20,name: '宇智波', checked: false,children:[
                        { id: 21, name: '佐助', checked: false},
                        { id: 24, name: '鸣人', checked: false},
                        { id: 22, name: '小樱', checked: false},
                        { id: 23, name: '左良娜', checked: false}
                      ]},

                    ]
            }
        ]
    }
  },
  components: {
    HorizontalTree
  },
  created() {
    this.initData(this.rows)
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
  	},
    // 初始化数据转成需要的格式
    initData (data) {
      if (data.length > 0) {
        data.map(item => {
          let arr = []
          this.$set(item, 'isFold', false)
          if(item.checked) arr.push(item.checked)
          if (item.children&&item.children.length>0){
            if (item.children.length == arr.length){
              item.checked = true
              this.$set(item,'indeterminate', false)
            } else if(item.children.length != arr.length && arr.length>0){
              item.checked = true
              this.$set(item,'indeterminate', true)
            } else{
              this.$set(item,'indeterminate', false)
            }
            this.initData(item.children)
          } else {
            if(item.checked) {
              item.checked= true
            }
            this.$set(item,'indeterminate', false)
          }
        })
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped type="text/css" lang="less">
	.wrap{
		width: 900px;
		text-align: center;
    padding: 40px;
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
    .node_wrap:nth-last-of-type(1){
      border: none;
    }
	}
</style>
