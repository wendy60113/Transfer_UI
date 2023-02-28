<template>
  <div class="bg">
    <div class="card_custom p-3">
        <div class="title">
            <b-form-input v-model="nameA" class="input">{{nameA}}</b-form-input>
        </div>
        <div class="card_line px-3">
            <div class="checkBox_Box">
                <div v-for="(item,indx) in list" :key="'Y'+indx" class="my-3">
                    <CheckBox
                        :item="item"
                        :disable="showType==''?false:showType=='list'?false:true"
                        @goChecked="goChecked"
                    />
                </div>
            </div>
            <div class="add_box pt-3">
                <b-input-group>
                    <b-form-input v-model="tag"></b-form-input>
                    <b-input-group-append class="add_box_back" >
                        <b-button @click="addTag(tag)">
                            <b-icon icon="plus"></b-icon>
                        </b-button>
                    </b-input-group-append>
                </b-input-group>
            </div>
        </div>
    </div>

    <div class="swap_btn" @click="transfer">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
            <title>swap-horizontal</title>
            <path d="M21,9L17,5V8H10V10H17V13M7,11L3,15L7,19V16H14V14H7V11Z" :style="{fill:showType!=''?'#6590FF':'#E8E8E8'}"/>
        </svg>
    </div>

    <div class="card_custom p-3">
        <div class="title">
            <b-form-input v-model="nameB" class="input">{{nameB}}</b-form-input>
        </div>
        <div class="card_line px-3 pb-3">
            <div class="checkBox_Box" style="height: 100%;">
                <div v-for="(item,indx) in area" :key="'Y'+indx" class="my-3">
                    <CheckBox
                        :item="item"
                        :disable="showType==''?false:showType=='area'?false:true"
                        @goChecked="goChecked"
                    />
                </div>
            </div>
        </div>
    </div>
  </div>
</template>

<script>
import CheckBox from '@/components/CheckBox.vue'
export default {
    data:()=>({
        list:[
            // {'name':'List1','type':'list','idx':0,'checked':false}
        ],
        tag:'',
        area:[
            // {'name':'area1','type':'area','idx':0,'checked':false}
        ],
        checkList:[],
        listToArea:false,
        areaToList:false,
        showType:'',
        nameA:'Tag List',
        nameB:'Area List'
    }),
    components:{
        CheckBox,
    },
    methods:{
        addTag(tag){
            this.list.push({'name':tag,'type':'list','idx':this.list.length,'checked':false})
            this.tag=''
        },
        goChecked(item){
            item.checked = !item.checked
            //Add/Slice to CheckList
            if(item.checked){
                this.checkList.push(item)
            }else{
                let index = this.checkList.findIndex(e=>e.idx==item.idx)
                this.checkList.splice(index,1)
            }
            //Check Type
            if(this.checkList.length==0){
                this.showType=''
            }else{
                this.showType=item.type
            }
        },
        transfer(){
            //Record Change Index
            let sliceArray=[]
            this.checkList.forEach(item=>{
                //type change => list <-> area
                if(item.type=='list'){
                    sliceArray.push(this.list.findIndex(e=>e.idx==item.idx))
                    item.type = 'area'
                }else{
                    sliceArray.push(this.area.findIndex(e=>e.idx==item.idx))
                    item.type = 'list'
                }
                
            })

            //List Change And Update
            sliceArray.forEach(e=>{
                if(this.showType=='list'){
                    this.area.push(this.checkList[this.checkList.findIndex(k=>k.idx==e)])
                    this.list.splice(this.list.findIndex(k=>k.idx==e),1)
                }else{
                    this.list.push(this.checkList[this.checkList.findIndex(k=>k.idx==e)])
                    this.area.splice(this.area.findIndex(k=>k.idx==e),1)
                }
                
                this.checkList.splice(this.checkList.findIndex(k=>k.idx==e),1)
            })
            this.$nextTick(()=>{
                this.clear()
            })

        },
        clear(){
            this.list.forEach((item,idx)=>{
                item.idx = idx
                item.checked = false
            })
            this.area.forEach((item,idx)=>{
                item.idx = idx
                item.checked = false
            })

            this.showType=''
        }
    }
}
</script>

<style lang="scss" scoped>
.bg{
    display: flex;
    flex-direction:row;
    justify-content:center;
    align-items: center;
    height: 100%;
    text-align: center;

}
.card_custom{
    width: 20%;
    height: 400px;
    background-color: #fff;
    box-shadow: 4px 4px 20px 8px rgba(213, 213, 213, 0.1);
    border-radius: 2%;

    .title{
        .input{
            border: 0;
            text-align: center;

            height: 15%;
            font-weight: 300;
            font-size: 1.5rem;
            color: #333;
            display: flex;
            align-items: center; 
            justify-content: center;
        }
        .form-control:focus{
            box-shadow: 0 0 0 0rem ;
        }
    }
    .card_line{
        border: 1px solid #E8E8E8;
        border-radius: 2%;
        height: 85%;
        min-height: 200px;
        padding-bottom: 10%;

        .checkBox_Box{
            overflow:scroll;
            height: 82%;
        }

        .add_box{
            height: 18%;
            // border: 1px solid #E8E8E8;

            .form-control{
                border-radius: 4px;
                border-color: #E8E8E8;
                border-right: 0;
            }

            .btn{
                background-color: #6590FF;
                border: 0;
                padding: 0;
                border-radius: 4px;
                width: 30px;
                height: 30px;
                top: 50%;
                transform: translateY(-50%);
            }

            .form-control {
                color: #333;
                letter-spacing: 0.1rem;
                font-weight: 300;
            }

            .add_box_back{
                border: 1px solid #E8E8E8;
                border-radius: 4px;
                border-left: 0;
                padding: 5px;
            }
        }
    }
}

.swap_btn{
    cursor: pointer;
    width:50px;
    height: 50px;
    margin: 0 10px;    
    .icon{
        position: relative;
        width: 100%;
        height: 100%;
        top: 50%;
        transform: translateY(-50%);
    }
}
@media(max-width: 768px){
    .bg{
        flex-direction:column;
    }
    .card_custom{
        width: 60%;
        .card_line{
            padding-bottom: 20%;
        }
    }
    
}
</style>