<template>
    <a-card title="svg-数据流">
        <svg-icon name="aboutSU" />
        <!-- template里面可以直接拿到store数据 -->
        <p class="myFont theme">自定义字体和全局颜色</p>
        <div class="text-3xl text-orange-500">{{ $store.state.count }}</div>
        <button @click="handleGetters">getters</button>
        <button @click="handleMutations">mutations</button>
        <button @click="handleActions">actions</button>

        <div>{{ $store.state.userModule.userCount }}</div>
        <button @click="handleUserGetters">userGetters</button>
        <button @click="handleUserMutations">userMutations</button>
        <button @click="handleUserActions">userActions</button>
    </a-card>

    <a-card title="路由 请求">
        <button @click="goPage2">goPage2</button>
        <button @click="fetch">fetch</button>
    </a-card>

    <a-card title="国际化">
        <p>{{ $t("message.name") }}</p>
        <p>{{ $t("message.text", { msg: '自定义值' }) }}</p>
        <button @click="changeLang('zh')">中文</button>
        <button @click="changeLang('en')">英文</button>
    </a-card>
    <div style="height: 500px"></div>

</template>

<script>
import { useStore } from 'vuex'
import { useRoute, useRouter } from "vue-router"
import { useI18n } from 'vue-i18n'
import { getData } from '@utils/services/test'

export default {
    setup() {
        // setup里面要拿到vuex数据要使用钩子
        const store = useStore()

        // 路由信息
        const router = useRoute()
        // 路由操作
        const routers = useRouter()

        const { locale } = useI18n()

        const changeLang = (lang) => {
            locale.value = lang
        }

        const goPage2 = () => {
            routers.push('/page2')
        }

        const fetch = async () => {
            getData().then(res => {
                console.log('res', res)
            }).catch(err => {
                console.log('err', err)
            })
        }

        const handleGetters = () => {
            console.log('getters', store.getters.getCount)
        }

        const handleMutations = () => {
            store.commit('increment', {
                count: 10
            })
        }

        const handleActions = () => {
            store.dispatch('increment', { count: 20 })
        }

        const handleUserGetters = () => {
            console.log('userGetters', store.getters['userModule/getUserCount'])
        }

        const handleUserMutations = () => {
            store.commit('userModule/userIncrement', {
                userCount: 20
            })
        }

        const handleUserActions = () => {
            store.dispatch('userModule/userIncrement', { userCount: 30 })
        }

        return {
            handleGetters,
            handleMutations,
            handleActions,
            handleUserGetters,
            handleUserMutations,
            handleUserActions,
            goPage2,
            fetch,
            changeLang,
        }
    }
}

</script>

<style lang="less" scoped>
.myFont {
    font-family: "WORKSANS-BOLD";
}

.theme {
    color: @base_color;
}
</style>