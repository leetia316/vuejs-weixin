<template>
<div class="page consumer">
    <header id="header" class="mui-bar mui-bar-nav haed2 ">
        <h1 class="mui-title">店铺评价</h1>
        <a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
    </header>
    <div class="mui-scroll-wrapper" style="bottom: 0px;">
    <div class="mui-scroll">
    <div id="sliderSegmentedControl" class="mui-slider-indicator mui-segmented-control mui-segmented-control-inverted">
        <a class="mui-control-item mui-active" href="#item1">全部({{info ? info.all : '0'}})</a>
        <a class="mui-control-item" href="#item2">好评({{info ? info.good : '0'}})</a>
        <a class="mui-control-item" href="#item3">中评({{info ? info.middle : '0'}})</a>
        <a class="mui-control-item" href="#item4">差评({{info ? info.bad : '0'}})</a>
    </div>
    
    <div id="item1" class="mui-control-content fade-in-left mui-active">
        <ul class="comment-item">
            <li v-for="item in all.list">
                <div class="item">
                    <img :src="item.wechat_headimgurl" class="head">
                    {{item.wechat_nickname}} <i class="lv lv{{item.wechat_userlevel}}"></i><br>
                    <div class="starts-container"><start :value="parseInt(item.comment_score_store)"><start></div>
                    <h6>
                        {{item.comment_time | format 'yyyy-MM-dd'}}
                    </h6>
                    <div class="tag">
                        <a v-for="tag in item.comment_store_label" v-if="tag!=''">{{$store.state.storeLabel[parseInt(tag)-1]}}</a>
                    </div>
                    <p>{{item.comment}}</p>
                    <small v-for="reply in item.reply">
                        <span>{{reply.comment_userid ? '用户回复' : '店铺回复'}}: </span>
                        {{reply.comment}}
                        <br>
                    </small>
                </div>
            </li>
            <div class="no-data" v-if="all.list.length<=0">
                <h1>没有相关评论</h1>
            </div>
            <a class="btn-more" v-if="all.btn" @click="getAll">查看更多</a>
        </ul>
    </div>

    <div id="item2" class="mui-control-content fade-in-left">
        <ul class="comment-item">
            <li v-for="item in good.list" v-if="item.comment_level == '1'">
                <div class="item">
                    <img :src="item.wechat_headimgurl" class="head">
                    {{item.wechat_nickname}} <i class="lv lv{{item.wechat_userlevel}}"></i><br>
                    <div class="starts-container"><start :value="parseInt(item.comment_score_store)"><start></div>
                    <h6>{{item.comment_time | format 'yyyy-MM-dd'}}</h6>
                    <div class="tag">
                        <a v-for="tag in item.comment_store_label" v-if="tag!=''">{{$store.state.storeLabel[parseInt(tag)-1]}}</a>
                    </div>
                    <p>{{item.comment}}</p>
                    <small v-for="reply in item.reply">
                        <span>{{reply.comment_userid ? '用户回复' : '店铺回复'}}: </span>
                        {{reply.comment}}
                        <br>
                    </small>
                </div>
            </li>
            <div class="no-data" v-if="good.list.length<=0">
                <h1>没有相关评论</h1>
            </div>
            <a class="btn-more" v-if="good.btn" @click="getGood">查看更多</a>
        </ul>
    </div>

    <div id="item3" class="mui-control-content fade-in-left">
        <ul class="comment-item">
            <li v-for="item in midd.list" v-if="item.comment_level == '2'">
                <div class="item">
                    <img :src="item.wechat_headimgurl" class="head">
                    {{item.wechat_nickname}} <i class="lv lv{{item.wechat_userlevel}}"></i><br>
                    <div class="starts-container"><start :value="parseInt(item.comment_score_store)"><start></div>
                    <h6>{{item.comment_time | format 'yyyy-MM-dd'}}</h6>
                    <div class="tag">
                        <a v-for="tag in item.comment_store_label" v-if="tag!=''">{{$store.state.storeLabel[parseInt(tag)-1]}}</a>
                    </div>
                    <p>{{item.comment}}</p>
                    <small v-for="reply in item.reply">
                        <span>{{reply.comment_userid ? '用户回复' : '店铺回复'}}: </span>
                        {{reply.comment}}
                        <br>
                    </small>
                </div>
            </li>
            <div class="no-data" v-if="midd.list.length<=0">
                <h1>没有相关评论</h1>
            </div>
            <a class="btn-more" v-if="midd.btn" @click="getMidd">查看更多</a>
        </ul>
    </div>

    <div id="item4" class="mui-control-content fade-in-left">
        <ul class="comment-item">
            <li v-for="item in bad.list" v-if="item.comment_level == '3'">
                <div class="item">
                    <img :src="item.wechat_headimgurl" class="head">
                    {{item.wechat_nickname}} <i class="lv lv{{item.wechat_userlevel}}"></i><br>
                    <div class="starts-container"><start :value="parseInt(item.comment_score_store)"><start></div>
                    <h6>{{item.comment_time | format 'yyyy-MM-dd'}}</h6>
                    <div class="tag">
                        <a v-for="tag in item.comment_store_label" v-if="tag!=''">{{$store.state.storeLabel[parseInt(tag)-1]}}</a>
                    </div>
                    <p>{{item.comment}}</p>
                    <small v-for="reply in item.reply">
                        <span>{{reply.comment_userid ? '用户回复' : '店铺回复'}}: </span>
                        {{reply.comment}}
                        <br>
                    </small>
                </div>
            </li>
            <div class="no-data" v-if="bad.list.length<=0">
                <h1>没有相关评论</h1>
            </div>
            <a class="btn-more" v-if="bad.btn" @click="getBad">查看更多</a>
        </ul>
    </div>
    
    </div>
    </div>
    
</div>
</template>

<script>
import Start from '../components/start.vue'
export default{
    name: 'consumerShopCom',
    data: function(){
        return{
            info: {},
            all: {
                list: [],
                page: 0,
                btn: false
            },
            bad: {
                list: [],
                page: 0,
                btn: false
            },
            good: {
                list: [],
                page: 0,
                btn: false
            },
            midd: {
                list: [],
                page: 0,
                btn: false
            }
        }
    },
    ready: function(){
        document.title = '店铺评价'
        mui('.mui-scroll-wrapper').scroll({bounce: false})
        this.getAll()
        this.getGood()
        this.getMidd()
        this.getBad()
    },
    methods: {
        local: function(name){
            this.$router.go('/'+ name +'/' + this.$route.params.token)
        },
        getAll(){
            this.all.page++;
            this.$store.state.showTip({type: 'loading', content: '加载中...' });
            this.$http.post(API.getTechnicianEvaluate, {
                store: this.$route.params.storeid,
                page: this.all.page
            }).then( (data) => {
                if(data.data.errorInfo) return
                this.$store.state.hideTip()
                if(!data.data.list){
                    this.all.btn = false;
                    return;
                }else if(data.data.list.length < this.$store.state.pageSize){
                    this.all.btn = false;
                }else{
                    this.all.btn = true;
                };
                this.all.list = this.all.list.concat(data.data.list);
                this.info = data.data.object;
                for(let i=0; i<this.all.list.length; i++){
                    if(!this.all.list[i].wechat_user_viptime){
                        this.all.list[i].wechat_userlevel = 0
                    }else if(this.all.list[i].wechat_user_viptime == '1'){
                        this.all.list[i].wechat_userlevel = 7
                    }else if( this.all.list[i].wechat_user_viptime - Date.parse(new Date())/1000 <= 0 ){
                        this.all.list[i].wechat_userlevel = 0
                    }else{
                        this.all.list[i].wechat_userlevel = 1
                    }
                    if(this.all.list[i].wechat_headimgurl.indexOf('http')<0){
                        this.all.list[i].wechat_headimgurl = this.$store.state.URL + this.all.list[i].wechat_headimgurl
                    }
                }
            })
        },
        getGood(){
            this.good.page++;
            this.$store.state.showTip({type: 'loading', content: '加载中...' });
            this.$http.post(API.getTechnicianEvaluate, {
                store: this.$route.params.storeid,
                page: this.good.page,
                type: 1
            }).then( (data) => {
                if(data.data.errorInfo) return
                this.$store.state.hideTip()
                if(!data.data.list){
                    this.good.btn = false;
                    return;
                }else if(data.data.list.length < this.$store.state.pageSize){
                    this.good.btn = false;
                }else{
                    this.good.btn = true;
                };
                this.good.list = this.good.list.concat(data.data.list);
                this.info = data.data.object;
                for(let i=0; i<this.good.list.length; i++){
                    if(!this.good.list[i].wechat_user_viptime){
                        this.good.list[i].wechat_userlevel = 0
                    }else if(this.good.list[i].wechat_user_viptime == '1'){
                        this.good.list[i].wechat_userlevel = 7
                    }else if( this.good.list[i].wechat_user_viptime - Date.parse(new Date())/1000 <= 0 ){
                        this.good.list[i].wechat_userlevel = 0
                    }else{
                        this.good.list[i].wechat_userlevel = 1
                    }
                    if(this.good.list[i].wechat_headimgurl.indexOf('http')<0){
                        this.good.list[i].wechat_headimgurl = this.$store.state.URL + this.good.list[i].wechat_headimgurl
                    }
                }
            })
        },
        getMidd(){
            this.midd.page++;
            this.$store.state.showTip({type: 'loading', content: '加载中...' });
            this.$http.post(API.getTechnicianEvaluate, {
                store: this.$route.params.storeid,
                page: this.midd.page,
                type: 2
            }).then( (data) => {
                if(data.data.errorInfo) return
                this.$store.state.hideTip()
                if(!data.data.list){
                    this.midd.btn = false;
                    return;
                }else if(data.data.list.length < this.$store.state.pageSize){
                    this.midd.btn = false;
                }else{
                    this.midd.btn = true;
                };
                this.midd.list = this.midd.list.concat(data.data.list);
                this.info = data.data.object;
                for(let i=0; i<this.midd.list.length; i++){
                    if(!this.midd.list[i].wechat_user_viptime){
                        this.midd.list[i].wechat_userlevel = 0
                    }else if(this.midd.list[i].wechat_user_viptime == '1'){
                        this.midd.list[i].wechat_userlevel = 7
                    }else if( this.midd.list[i].wechat_user_viptime - Date.parse(new Date())/1000 <= 0 ){
                        this.midd.list[i].wechat_userlevel = 0
                    }else{
                        this.midd.list[i].wechat_userlevel = 1
                    }
                    if(this.midd.list[i].wechat_headimgurl.indexOf('http')<0){
                        this.midd.list[i].wechat_headimgurl = this.$store.state.URL + this.midd.list[i].wechat_headimgurl
                    }
                }
            })
        },
        getBad(){
            this.bad.page++;
            this.$store.state.showTip({type: 'loading', content: '加载中...' });
            this.$http.post(API.getTechnicianEvaluate, {
                store: this.$route.params.storeid,
                page: this.bad.page,
                type: 3
            }).then( (data) => {
                if(data.data.errorInfo) return
                this.$store.state.hideTip()
                if(!data.data.list){
                    this.bad.btn = false;
                    return;
                }else if(data.data.list.length < this.$store.state.pageSize){
                    this.bad.btn = false;
                }else{
                    this.bad.btn = true;
                };
                this.bad.list = this.bad.list.concat(data.data.list);
                this.info = data.data.object;
                for(let i=0; i<this.bad.list.length; i++){
                    if(!this.bad.list[i].wechat_user_viptime){
                        this.bad.list[i].wechat_userlevel = 0
                    }else if(this.bad.list[i].wechat_user_viptime == '1'){
                        this.bad.list[i].wechat_userlevel = 7
                    }else if( this.bad.list[i].wechat_user_viptime - Date.parse(new Date())/1000 <= 0 ){
                        this.bad.list[i].wechat_userlevel = 0
                    }else{
                        this.bad.list[i].wechat_userlevel = 1
                    }
                    if(this.bad.list[i].wechat_headimgurl.indexOf('http')<0){
                        this.bad.list[i].wechat_headimgurl = this.$store.state.URL + this.bad.list[i].wechat_headimgurl
                    }
                }
            })
        }
    },
    components: {
        Start
    }
}
    
</script>