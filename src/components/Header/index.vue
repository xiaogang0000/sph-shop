<template>
  <!-- 头部 -->
        <header class="header">
            <!-- 头部的第一行 -->
            <div class="top">
                <div class="container">
                    <div class="loginList">
                        <p>尚品汇欢迎您！</p>
                        <p v-show="!userInfo.id">
                            <span>请</span>
                            <router-link to="/login">登录</router-link>
                            <router-link to="/register" class="register">免费注册</router-link>
                        </p>
                         <p v-show="userInfo.id">
                            <span>欢迎，{{userInfo.name}}</span>
                            <a class="register" @click="handleLogout">退出登录</a>
                        </p>
                    </div>
                    <div class="typeList">
                           <router-link to="/test" class="register">测试</router-link>
                        <a href="###">我的订单</a>
                        <router-link to="/cart"  >我的购物车</router-link>
                        <a href="###">我的尚品汇</a>
                        <a href="###">尚品汇会员</a>
                        <a href="###">企业采购</a>
                        <a href="###">关注尚品汇</a>
                        <a href="###">合作招商</a>
                        <a href="###">商家后台</a>
                    </div>
                </div>
            </div>
            <!--头部第二行 搜索区域-->
            <div class="bottom">
                <h1 class="logoArea">
                    <router-link @click.native="keyword= ''" class="logo" title="尚品汇" to="/home" >
                        <img src="./images/logo.png" alt="">
                    </router-link>
                </h1>
                <div class="searchArea">
                    <form  @submit.prevent="toSearch" class="searchForm">
                        <input 
                         type="text"
                          id="autocomplete"
                           class="input-error input-xxlarge" 
                           v-model="keyword"
                         
                           />

                        <button @click="toSearch" class="sui-btn btn-xlarge btn-danger" type="button">搜索</button>
                    </form>
                </div>
            </div>
        </header>
</template>

<script>
import { mapState } from 'vuex'
export default {
   name:'Header',
   data(){
     return{
        keyword:''
     } //搜索的关键字
   },
   computed:{
...mapState({
    userInfo:state => state.user.userInfo
})
   },
   methods:{
    toSearch(){
        const {query} = this.$route
        this.$router.push({
            path:'/search',
            query:{
                ...query,
                //undefined 路由默认忽视
                keyword:this.keyword || undefined
            }
        })
    },
    //退出登录按钮的回调
    handleLogout(){
        if(confirm('确定退出嘛？')){
this.$store.dispatch('logout','退出登录成功！')
        }
    }
  
   },
   mounted(){
    //给总线绑定'clear-keyword'事件，用于清楚搜索词
   this.$bus.$on('clear-keyword',()=>{
   this.keyword = ''
   })
   },
   beforeDestroy(){
    this.$bus.$off('clear-keyword')
   }
}
</script>

<style lang="less" scoped>
  .header {
        &>.top {
            background-color: #eaeaea;
            height: 30px;
            line-height: 30px;

            .container {
                width: 1200px;
                margin: 0 auto;
                overflow: hidden;

                .loginList {
                    float: left;

                    p {
                        float: left;
                        margin-right: 10px;

                        .register {
                            border-left: 1px solid #b3aeae;
                            padding: 0 5px;
                            margin-left: 5px;
                        }
                    }
                }

                .typeList {
                    float: right;

                    a {
                        padding: 0 10px;

                        &+a {
                            border-left: 1px solid #b3aeae;
                        }
                    }

                }

            }
        }

        &>.bottom {
            width: 1200px;
            margin: 0 auto;
            overflow: hidden;

            .logoArea {
                float: left;

                .logo {
                    img {
                        width: 175px;
                        margin: 25px 45px;
                    }
                }
            }

            .searchArea {
                float: right;
                margin-top: 35px;

                .searchForm {
                    overflow: hidden;

                    input {
                        box-sizing: border-box;
                        width: 490px;
                        height: 32px;
                        padding: 0px 4px;
                        border: 2px solid #ea4a36;
                        float: left;

                        &:focus {
                            outline: none;
                        }
                    }

                    button {
                        height: 32px;
                        width: 68px;
                        background-color: #ea4a36;
                        border: none;
                        color: #fff;
                        float: left;
                        cursor: pointer;

                        &:focus {
                            outline: none;
                        }
                    }
                }
            }
        }
    }

</style>
