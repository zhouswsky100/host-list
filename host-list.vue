<template>
  	<div class="list-view" @scroll="handleScroll">
        <div
            class="list-view-phantom"       
            :style="{
                height: contentHeight
            }">
        </div>
        <div ref="content"  class="list-view-content">
            <div 
                class="list-view-item"  
                :style="{
                    height: itemHeight + 'px'
                }" 
                v-for='(item, index) in hospitalList'>
                <div class="item-media">
                    <img  slot="media"  style="width: 73px; height:73px" :src="item.logoUrl" onerror="this.src='../../static/images/dt/hospital.png';this.onerror = null;">
                </div>
                <div class="item-inner" >
                    <div class="item-title-row" style="margin-bottom:5px;">
                        <div class="item-title ">{{item.name}} </div>
                    </div>
                    <span class="item-subtitle hos " v-if="item.otype!=null">{{item.otype | formatotype}}</span>
                    <span class="item-subtitle hos hosle" v-if="item.olevel!=''">{{item.olevel |formatolevel}}</span>
                    <span class="fr" v-if="item.distance!='?km'">{{ item.distance}} </span>
                    <span class="fr" v-else>0.00km</span>
                    <div class="item-address " style="color:#9293A0">{{item.addr}}</div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {
             hospitalList: []
        };
    },
    name:  'HosListView',
    props:{
    data: {
        type: Array,
        required: true
    },
    itemHeight: {
      type: Number,
      default: 125
    }
   },
   computed: {
    contentHeight() {
        return this.data.length * this.itemHeight + 'px';
    }
  },
  methods: {
    updateVisibleData(scrollTop) {
        scrollTop = scrollTop || 0;
        const visibleCount = Math.ceil(this.$el.clientHeight / this.itemHeight);
        const start = Math.floor(scrollTop / this.itemHeight);
        const end = start + visibleCount;
        this.hospitalList = this.data.slice(start, end);
        this.$refs.content.style.webkitTransform = `translate3d(0, ${ start * this.itemHeight }px, 0)`;
    },
    handleScroll() {
      const scrollTop = this.$el.scrollTop;
      this.updateVisibleData(scrollTop);
    }
  },
  mounted() {
      this.updateVisibleData();
  },
  filters: {
        formatotype (val) {
                if(val==3){
                    return "社康"
                }
                var  hospitalType= [
                                    {key:"2",name:"综合医院"},
                                    {key:"6",name:"诊所"},
                                    {key:"9",name:"社康"},
                                    {key:"99",name:"其他"},
                                    {key:"10",name:"特等医院"},
                                ]
                for(var i = 0;i<hospitalType.length;i++){
                    if(hospitalType[i].key==val){
                        return hospitalType[i].name
                    }
                }
                return "其他"
        },
        formatolevel(val){
                    var  hospitalType=	[
                                {key:"20",name:"三级甲等"},
                                {key:"30",name:"三级乙等"},
                                {key:"40",name:"三级丙等"},
                                {key:"50",name:"二级甲等"},
                                {key:"60",name:"二级乙等"},
                                {key:"70",name:"二级丙等"},
                                {key:"80",name:"一级甲等"},
                                {key:"90",name:"一级乙等"},
                                {key:"100",name:"一级丙等"},
                                {key:"999",name:"其他"},	]
                for(var i = 0;i<hospitalType.length;i++){
                    if(hospitalType[i].key==val){
                        return hospitalType[i].name 
                    }
                }
                return "其他"
        }
	}
}
</script>

<style type="text/css" scoped lang="scss">
.list-view {
  position: relative;
  height: 100%;
  overflow: auto;
  background: #fff;
  margin-top: 49px;
}


.list-view-phantom {
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  z-index: -1;
}
.list-view-content {
  left: 0;
  right: 0;
  top: 0;
  position: absolute;
}
.list-view-item {
    padding: 10px;
    color: #666;
    justify-content: space-between;
    box-sizing: border-box;
    display: flex;
    align-items: center;
    border-bottom: 1px solid rgb(233, 233, 233);
}
.item-media{
    padding: 17.5px 0 19px;
}
.item-title{
    font-size:18px;color:#020221;font-weight:400;
}
.item-inner{
    margin-left: 15px;
    margin-right: 10px;
    width: 100%;
    display: block;
    align-self: stretch;
}
.item-inner:after{
    display: none;
}
.fr{
    float: right;
    color: #9293A0;
    font-size: 12px;
    line-height: 25px;
}
.hos{
    color: #0084FD;
    background: #E7F2FF;
    font-size: 10px;
    margin-top: 5px;
    margin-bottom: 5px;
    text-align: center;
    border-radius: 2px;
    height: 20px;
    line-height: 20px;
    padding: 5px;
}
.hosle{
    color: #37D4AB;
    background: #EAFAF6;
    height: 20px;
    line-height: 20px;
}
.item-address{
    color: rgb(146, 147, 160);
    padding-top: 5px;
    display: -webkit-box;
    font-size: 12px;
}
</style>