<template>
    <div style="border: 1px solid #ccc">
        <Toolbar
            style="border-bottom: 1px solid #ccc"
            :editor="editorRef"
            :defaultConfig="toolbarConfig"
            mode="mode"
        />
        <Editor
            style="height: 500px; overflow-y: hidden;"
            v-model="valueHtml"
            :defaultConfig="editorConfig"
            mode="mode"
            @onCreated="handleCreated"
        />
    </div>
</template>

<script setup>

    import '@wangeditor/editor/dist/css/style.css' // 引入 css
    import { onBeforeUnmount, ref, shallowRef, onMounted } from 'vue'
    import { Editor, Toolbar } from '@wangeditor/editor-for-vue'

    // 编辑器实例，必须用 shallowRef
    const editorRef = shallowRef()

    // 内容 HTML
    const valueHtml = ref('<p>hello</p>')

    // 编辑器配置
    const toolbarConfig = {}

    // 菜单配置
    const editorConfig = {
        placeholder: '请输入内容...',
        MENU_CONF:{
            uploadImage:{
                // 文件基础配置
                server: "后端服务地址",
                fieldName: "file",

                // 上传前触发
                onBeforeUpload: function (file) {
                    console.log(file)
                },

                // 上传进度的回调函数
                onProgress(progress) {
                    // progress 是 0-100 的数字
                    console.log('progress', progress)
                },

                // 单个文件上传成功之后
                onSuccess(file, res) {  // TS 语法
                    console.log(`${file.name} 上传成功`, res)
                },

            }
        },
    }

    // 组件销毁时，也及时销毁编辑器
    onBeforeUnmount(() => {
        const editor = editorRef.value
        if (editor == null) return
        editor.destroy()
    })

    const handleCreated = (editor) => {
        editorRef.value = editor;
    }

</script>
