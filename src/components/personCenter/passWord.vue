<template lang="pug">
    div.password
        Header(title="修改密码")
        div.content
           div.arow(v-for="items in arr")
                ul 
                    li {{items.name}} 
                        span *
                    li
                        el-input(type="password" v-model="items.val" name="password" :placeholder="items.placeholder")              
        el-col.btnParent(:span="24") 
            div.btn(:span="24" @click="sumbit") 提交
        .success-dialog
            <el-dialog :visible.sync="dialogTableVisible" center>
                .content
                    img(src="static/img/success.png")
                    .name 报名
                    .time(v-if="sendMsgDisabled") {{time+'s回到详情页'}}
            </el-dialog>     
</template>
<script>
export default {
	name: 'password',
	data() {
		return {
            dialogTableVisible:false,
            time: 3, // 发送验证码倒计时
            sendMsgDisabled: false,
            arr:[
              {
              	name:'旧密码',
              	placeholder:'旧密码',
              	val:''
              },
              {
              	name:'新密码',
              	placeholder:'至少6位新密码',
              	val:''
              },
              {
              	name:'验证新密码',
              	placeholder:'验证新密码',
              	val:''
              }
            ],
            // pShow:false,
            num:3 
		}
	},
	methods: {
        Goback(){
            this.$router.push({name:'PersonalCenter'});
        },
        errorTip(content){
            this.$alert(content, '提示', {
                confirmButtonText: '知道了',
                callback: action => {
                }
            });
        },
        sumbit(){
            let this_=this;
            if(!this_.arr[0].val){
                this_.errorTip('请填写旧密码。');
                return false
            }
            if(!this_.arr[1].val){
                this_.errorTip('请填写新密码。');
                return false
            }
            if(!this_.arr[2].val){
                this_.errorTip('请填写验证新密码。');
                return false
            }
            if(this_.arr[2].val.split('').length<6){
                this_.errorTip('请填写至少六位的新密码');
                return false
            }
            if(this_.arr[1].val!=this_.arr[2].val){
                this_.errorTip('验证新密码与新密码不一致。');
                return false
            }
            this_.dialogTableVisible=true;
            setInterval(function(){
                if(this_.num===1){
                    this_.$router.push({name:'Login'});
                }
                this_.num--;
            },1000)
        }
    },
    watch:{
        dialogTableVisible(val){
            if(val){
                let me = this;
                me.sendMsgDisabled = true;
                let interval = window.setInterval(function() {
                    if ((me.time--) <= 1) {
                        me.time = 3;
                        me.sendMsgDisabled = false;
                        window.clearInterval(interval); //停止
                        // me.$router.push({
                        //     path:'/Login'
                        // })
                    }
                }, 1000);
            }
        }
    }
}
</script>
<style lang="stylus">
.password 
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    padding-bottom: 1rem;
    background: #fff;
    overflow: hidden;
    box-sizing: border-box;
	.line 
		width: 100%;
		background: #138be7;
		position: relative;
		.navigation 
            overflow: hidden;
            padding: 0.3rem;
            font-size: 0.38rem;
            text-align: center;
            color: #fff;

			.imgFl 
                float: left;
                width: 2%;
                cursor:pointer;
			
			.imgFr 
				float: right;
				width: 3.0%;
				cursor:pointer;
			
		.trigger
            background #fff
            width 240px
            position absolute
            right 0.1rem
            top 88px
            z-index 999
            border 1px solid #e3e3e3
            span
                padding 35px 50px
                font-size 30px
                display block
			
            span:nth-child(1)
              border-bottom 1px solid #e3e3e3
			
            span:after
                content ''
                position absolute
                width 0
                height 0
                top -15px
                right 35px
                border-bottom 20px solid #fff
                border-left 20px solid transparent
                border-right 20px solid transparent
		
		.imgCity 
            width: 100%
            padding-top:1rem
		
	.content
		width:100%
		.arow
			width:100%
			font-size:.38rem
			box-sizing:border-box
			ul
                padding: 1.0rem 1.5rem 0rem 1.5rem
                overflow:hidden
                li
                    &:nth-child(1) 
                        float: left
                        width: 30%
                        height: 1.0rem
                        line-height: 1.0rem
                        span 
                            color:red
                            margin-left: 10px
                    &:nth-child(2)
                        float:left
                        width:70%
                        input
                            width: 90%
                            height: 1.0rem
                            box-sizing:border-box
                            padding:10px 20px
                            font-size:30px
				
	
	.btnParent
        position:absolute
        bottom:0
        left:0
        width:100%
        background:#fff
        padding:20px
        border-top: 1px solid #e5e5e5
		.btn
            padding:30px
            text-align:center
            color:#fff
            background:#138be7
            font-size: .38rem
            cursor: pointer
	.success-dialog
        .content
            text-align center
            img
                width 100px
                height 100px
            .name
                color #000
                font-size 36px
                font-weight bold
                margin 20px 0
            .time
                color #909399
                margin 50px 0
                font-size 28px
            .el-button
                padding 30px 60px

</style>
