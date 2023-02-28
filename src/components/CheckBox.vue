<template>
  <div class="chbox py-2 px-4" :class="{active: item.checked,disable: disable}" @click="goChecked">
    <div class="box" :class="{active: item.checked,disable: disable}">
        <b-icon icon="check" class="icon" v-if="item.checked"></b-icon>
    </div>
    <div class="content" :class="{active: item.checked,disable: disable}">
        <span>{{item.name}}</span>
    </div>
  </div>
</template>

<script>
export default {
    props:['item','disable'],
    data:()=>({
        // checked:false,
    }),
    methods:{
        goChecked(){
            if(!this.disable){
                this.$emit('goChecked',this.item)
            }
        }
    }
}
</script>

<style lang="scss" scoped>
.chbox{
    // background-color: #6590FF;
    border-radius: 30px;
    cursor: pointer;
    display: flex;

    &.active {
      background-color: #6590FF;
    }
    &.disable{
        cursor: default;
    }
    .box{
        border: 1px solid #CBCBCB;
        height: 20px;
        width:20px;
        border-radius: 3px;
        font-size: 0;

        &.active{
            border: 1px solid #fff;
            background-color: #fff;
        }
        &.disable {
            background-color: #F5F5F5;
        }

        .icon{
            width: 100%;
            height: 100%;
            scale: 1.2;
            color: #6590FF;
        }
    }

    .content{
        color: #333;
        height: 20px;
        width:auto;
        margin-left: 4%;
        font-size: 0;

        &.active{
            color:#fff;
        }
        &.disable{
            color: #CBCBCB;
        }
        
        span{
            font-size: 1rem;
            line-height: 1.3rem;
            letter-spacing: 0.1rem;
            font-weight: 300;
        }
    }
}
</style>
