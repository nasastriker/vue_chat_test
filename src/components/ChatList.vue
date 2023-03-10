<template>
    <div>
        <section class="chatlist" :class="showSelBox>0?'chatlist-bottom-collapse':'chatlist-bottom'">
            <mt-loadmore :top-method="loadTop" top-pull-text="Битрикс 24>" top-drop-text="Тестовый чат" @top-status-change="handleTopChange" ref="loadmore">
                <ul>
                    <template v-for="item in records">
                        <li class="chat-mine" v-if="item.type==1">
                            <div class="message-wrapper">
                              <div class="chat-user"><img src="../assets/ui-user.svg"></div>
                              <div class="user-name">
                                {{item.name}}
                              </div>
                              <div class="time">{{item.time}}</div>
                              <div class="chat-text" v-html="replaceFace(item.content)"></div>
                            </div>
                        </li>
                        <li v-if="item.type==2">
                          <div class="message-wrapper">
                            <div class="chat-user"><img src="../assets/ui-user.svg"></div>
                            <div class="user-name">
                              {{item.name}}
                            </div>
                            <div class="time">{{item.time}}</div>
                            <div class="chat-text" v-html="replaceFace(item.content)"></div>
                          </div>
                        </li>
                    </template>
                </ul>
            </mt-loadmore>
        </section>

        <section class="foot">
            <mt-field id="txtContent" placeholder="Написать сообщение..." class="con" v-model="content"></mt-field>
            <span class="btn btn-send" v-on:click="sendMsg">Отправить</span>
        </section>

    </div>
</template>

<script>
import util from '../common/util';
import { Toast } from 'mint-ui';

export default {
    name: 'chatlist',
    data() {
        return {
            showSelBox: 0,
            selFace: 'Lorem',
            selOther: 'Ipsum',
            content:'',
            topStatus: '',
            records: [{
                type: 1,
                time: util.formatDate.format(new Date(),'yyyy-MM-dd hh:mm:ss'),
                name: 'User Name',
                content: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit.'
            }, {
                type: 2,
                time: util.formatDate.format(new Date(),'yyyy-MM-dd hh:mm:ss'),
                name: 'Битрикс 24',
                content: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit.'
            }],
            EXPS: [
                { file: '100.gif', code: '/::)', reg:/\/::\)/g },
                { file: '101.gif', code: '/::~', reg:/\/::~/g },
                { file: '102.gif', code: '/::B', reg:/\/::B/g },
                { file: '103.gif', code: '/::|', reg:/\/::\|/g },
                { file: '104.gif', code: '/:8-)', reg:/\/:8-\)/g },
                { file: '105.gif', code: '/::<', reg:/\/::</g },
            ]
        }
    },
    methods: {
        getEXP: function (pageNow,pageSize) {
            return this.EXPS.slice((pageNow - 1) * pageSize, pageSize * pageNow)
        },
        sendMsg: function(){
            var _this=this;

            if(this.content==''){
                Toast('Введите текст сообщения');
                return;
            }

            this.records.push({
                type: 1,
                time: util.formatDate.format(new Date(),'yyyy-MM-dd hh:mm:ss'),
                name: 'User Name',
                content: this.content
            });

            setTimeout(function(){
                _this.records.push({
                    type: 2,
                    time: util.formatDate.format(new Date(),'yyyy-MM-dd hh:mm:ss'),
                    name: 'Чат-бот',
                    content: 'Привет！'
                });
            },100);

            this.content='';

            this.scrollToBottom();
        },
        focusTxtContent:function(){
            document.querySelector("#txtContent input").focus();
        },
        scrollToBottom:function(){
            setTimeout(function(){
                var chatlist = document.getElementsByClassName('chatlist')[0];
                chatlist.scrollTop=chatlist.scrollHeight;
            },100);
        },
        replaceFace:function(con){
            var _this=this;
            var exps=this.EXPS;
            for(var i=0;i<exps.length;i++){
                con = con.replace(exps[i].reg, '<img src="static/emotion/' + exps[i].file +'"  alt="" />');
            }
            return con;
        },
        handleTopChange(status) {
            this.topStatus = status;
        },
        loadTop(id) {
            var _this=this;
            setTimeout(() => {
                var chatlist = document.getElementsByClassName('chatlist')[0];
                var oldHeight=chatlist.scrollHeight;

                _this.records.unshift({
                    type: 1,
                    time: util.formatDate.format(new Date(),'yyyy-MM-dd hh:mm:ss'),
                    name: 'name',
                    content: 'Приветственное сообщение'
                }, {
                    type: 2,
                    time: util.formatDate.format(new Date(),'yyyy-MM-dd hh:mm:ss'),
                    name: 'name',
                    content: 'Lorem ipsum dolor sir amet'
                });

                setTimeout(function(){
                    var newHeight=chatlist.scrollHeight;
                    chatlist.scrollTop=newHeight-oldHeight;
                },100);

                this.$refs.loadmore.onTopLoaded(id);
            }, 1500);
        }
    },
    mounted:function(){
        this.scrollToBottom();
        this.focusTxtContent();
    }
}
</script>

<style>
    .message-wrapper {
        display: inline-block;
        background: #fff;
        padding: 20px;
        border-radius: 15px;
    }
    .chatlist {
        position: absolute;
        top: 60px;
        bottom: 48px;
        left: 0px;
        right: 0px;
        overflow-x: hidden;
        padding: 10px;
    }
    .user-name {
        color: #2067b0;
        font-size: 16px;
        font-weight: 600;
        margin-bottom: 5px;
    }
    .chatlist-bottom {
        bottom: 48px;
    }

    .chatlist-bottom-collapse {
        bottom: 198px;
    }

    .chatlist ul {
        min-height: 300px;
    }

    .chatlist ul .chat-mine {
        text-align: right;
        padding-left: 0;
        padding-right: 60px;
    }

    .chatlist ul li {
        position: relative;
        margin-bottom: 10px;
        padding-left: 60px;
        min-height: 68px;
    }

    .chat-mine .chat-user {
        left: auto;
        right: 3px;
    }

    .chat-user {
        position: absolute;
        left: 3px;
    }

    .chat-text,
    .chat-user {
        display: inline-block;
        vertical-align: top;
    }

    .chat-user img {
        width: 40px;
        height: 40px;
        border-radius: 100%;
    }

    .time {
        font-size: 13px;
        background: 0;
        padding-left: 0;
        color: #828b95 !important;
    }

    cite {
        left: auto;
        right: 60px;
        text-align: right;
        font-style: normal;
        line-height: 24px;
        font-size: 12px;
        white-space: nowrap;
        color: #fff;
        text-align: left;
    }

    cite i {
        font-style: normal;
        padding-left: 5px;
        padding-right: 5px;
        font-size: 12px;
    }

    .chat-mine .chat-text {
        margin-left: 0;
        text-align: left;
        background-color: #33DF83;
        color: #fff;
    }

    .chat-text {
        position: relative;
        line-height: 22px;
        padding: 10px 15px;
        border-radius: 23px;
        background-color: #edf1f3;
        color: #333;
        word-break: break-all;
        max-width: 460x;
        margin-top: 10px;
    }

    .chat-text,
    .chat-user {
        display: inline-block;
        vertical-align: top;
        font-size: 14px;
    }

    .chat-text img {
        max-width: 100%;
        vertical-align: middle;
    }

    .chat-user {
        position: absolute;
        left: 3px;
        background: #7b8691;
        border-radius: 50%;
        height: 40px;
    }

    .foot {
        width: 100%;
        min-height: 48px;
        position: fixed;
        bottom: 0px;
        left: 0px;
        background-color: #F8F8F8;
        display: flex;
        padding: 10px;
        justify-content: space-between;
        box-sizing: border-box;
        align-items: center;
    }

    .foot .con {
      width: 100%;
    }


    .foot .selbox {
        height: 150px;
        margin-top: 48px;
        border-top: 1px solid #d9d9d9;
    }

    .show {
        display: block;
    }

    .hide {
        display: none;
    }

    .face-box {
        padding: 15px 15px 0px 15px;
        overflow: hidden;
        width: 290px;
        margin: 0px auto;
        height: 135px;
    }

    .face-box li {
        width: 30px;
        float: left;
        padding: 6px 10px 0px 8px;
    }

    .btn {
        display: inline-block;
        vertical-align: top;
        font-size: 14px;
        line-height: 32px;
        margin-left: 5px;
        padding: 5px 20px;
        background-color: #33DF83;
        color: #fff;
        border-radius: 10px;
    }

    .btn-send,
    .mint-field-clear{
        cursor: pointer;
    }
</style>
