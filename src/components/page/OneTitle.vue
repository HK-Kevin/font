<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-date"></i>Add Title</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="form-box">
            <el-form ref="form" :model="form" label-width="100px">
                <el-form-item label="Title">
                    <el-input v-model="form.title"></el-input>
                </el-form-item>
                <el-form-item label="选择开关">
                    <el-switch on-text="" off-text="" v-model="form.delivery"></el-switch>
                </el-form-item>
                <el-form-item label="Company">
                    <el-checkbox-group v-model="form.company">
                        <el-checkbox v-for="(item,index) in companyCont" :key="index" :label="item"
                                     name="type"></el-checkbox>
                    </el-checkbox-group>
                </el-form-item>
                <el-form-item label="Type">
                    <el-checkbox-group v-model="form.type">
                        <el-checkbox v-for="(item,index) in typeCont" :key="index" :label="item"
                                     name="type"></el-checkbox>
                    </el-checkbox-group>
                </el-form-item>
                <el-form-item label="Hard">
                    <el-radio-group v-model="form.hard">
                        <el-radio v-for="(item,index) in hardCont" :key="index" :label="item"></el-radio>
                    </el-radio-group>
                </el-form-item>
                <el-form-item label="shortAnswer">
                    <el-input type="textarea" v-model="form.shortAnswer"></el-input>
                </el-form-item>
                <el-form-item label="answer">
                    <div id="editorElemOne" style="text-align:left;width: 200%"></div>
                </el-form-item>

            </el-form>
        </div>
  <pre>
      <code>
          editor.customConfig.uploadImgServer = 'http://47.94.94.52:3000/upload' ; // 上传图片到服务器
            editor.customConfig.uploadFileName = 'avator';
            editor.create();
            this.editor = editor;

      </code>
  </pre>
        <el-button class="editor-btn" type="primary" @click="update">确认更改</el-button>
    </div>
</template>
<script>
    import hljs from 'highlight.js'
    import E from 'wangeditor'
    export default {
        data: function () {
            return {
                editorContent: '',
                hardCont: [1, 2, 3, 4, 5],
                companyCont: ['阿里', '腾讯', '百度', "360", "小米"],
                typeCont: ['CSS', '原生JS', 'jQuery', 'HTTP', 'Ajax',],
                form: {
                    title: '',
                    type: [],
                    hard: 1,
                    company: [],
                    shortAnswer: '',
                    answer: '',
                    titleClickTimes: 0,
                    date:'',
                    _id:''
                },
                editor:{},
            }
        },
        mounted() {
            hljs.initHighlighting();
            let editor = new E('#editorElemOne');
            editor.customConfig.onchange = (html) => {
                this.form.answer = html;
            };
            editor.customConfig.uploadImgServer = 'http://47.94.94.52:3000/upload' ; // 上传图片到服务器
            editor.customConfig.uploadFileName = 'avator';
            editor.create();
            this.editor = editor;
            },
        created(){
            let idObj = this.$route.params;
            this.$axios.get(`/oneTitle?id=${idObj.id}`).then(res => {
                this.form = res.data;
                this.editor.txt.html(res.data.answer)
            })
        },
        methods: {
            onSubmit() {
                this.$message.success('提交成功！');
            },
            update(){
                this.$axios.put(`/updateTitle`,this.form).then(res => {
                    this.form = res.data;
                    this.$message.success('更新成功！');
                })
            }
        },
    }
</script>

<style scoped>
    .editor-btn {
        margin-top: 20px;
    }
</style>
