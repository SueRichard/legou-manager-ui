<template>
<!--  @input输入事件，multiple多选（html5属性）flat浮动 options数据来源 disable-branch-nodes禁止选择非叶子结点 show-count显示当前节点下节点个数-->
    <treeselect :value="value" @input="handleInput" :multiple="true" :flat="true" :options="categoryList" :disable-branch-nodes="true" :show-count="true" />
</template>

<script>
    import instance from '@/libs/api/index'
    // 导入数组转换成树状数据结构js文件，注意这里没有写后缀
    //貌似js文件不用写后缀，或者是导入有别名的关系？？？
    import { listToTree } from '@/libs/util'
    // import the component
    import Treeselect from '@riophae/vue-treeselect'
    // import the styles
    import '@riophae/vue-treeselect/dist/vue-treeselect.css'

    export default {
      //声明
        components: { Treeselect },
      //给组件起名字
        name: 'selectCategorys',
        data() {
            return {
                categoryList: []
            }
        },
        props: {value: Array}, // 接收一个 value prop
        methods: {
            handleInput(value) {
              //参考1-品牌 P59
              //父子组件传值的基本方式（子组件里面触发父组件的方法），传给父组件。使得父组件能获得子组件更改的新值
                this.$emit('input', value) // 触发 input 事件，并传入新值,v-model：使用:value读，使用@input写
            }
        },
        created() {
            instance.post(`/item/category/list`).then(response => {
              // 数组转树状数据结构 es6的前端方法
                this.categoryList = listToTree(response.data)
            }).catch(error => {
                console.log(error)
            })
        }
    }
</script>
