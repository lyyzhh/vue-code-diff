<template>
  <div style="width: 80%;margin: 0 auto;">
    <a-form>
      <a-row :gutter="10">
        <a-col :span="12">
          <a-form-item label="旧数据：">
            <a-textarea v-model="oldStr" :autosize="{minRows: 3, maxRows: 3}" placeholder="请输入旧数据"></a-textarea>
          </a-form-item>
        </a-col>
        <a-col :span="12">
          <a-form-item label="新数据：">
            <a-textarea v-model="newStr" :autosize="{minRows: 3, maxRows: 3}" placeholder="请输入新数据"></a-textarea>
          </a-form-item>
        </a-col>
        <a-col :span="8">
          <a-form-item label="展示效果：">
            <a-switch v-model="fotmat" checked-children="line-by-line" un-checked-children="side-by-side"></a-switch>
          </a-form-item>
        </a-col>
        <a-col :span="8">
          <a-form-item label="差异化范围：">
            <a-input-number v-model="context" placeholder=""></a-input-number>
          </a-form-item>
        </a-col>
        <a-col :span="8">
          <a-form-item label="清除缓存：">
            <a-button type="danger" @click="handleClearLocalStorage">清除</a-button>
          </a-form-item>
        </a-col>
        <a-col :span="8">
          <a-form-item label="展示对比文件列表">
            <a-switch checked-children="展示" un-checked-children="不展示" v-model="drawFileList"></a-switch>
          </a-form-item>
        </a-col>
        <a-col :span="8">
          <a-form-item label="noDiff不渲染">
            <a-switch checked-children="不渲染" un-checked-children="渲染" v-model="renderNothingWhenEmpty"></a-switch>
          </a-form-item>
        </a-col>
        <a-col :span="8">
          <a-form-item label="noDiff显示源代码">
            <a-switch checked-children="显示" un-checked-children="不显示" v-model="isShowNoChange"></a-switch>
          </a-form-item>
        </a-col>
        <a-col :span="8">
          <a-form-item label="差异级别">
            <a-select v-model="diffStyle" style="width:100px;">
              <a-select-option value="char">char</a-select-option>
              <a-select-option value="word">word</a-select-option>
            </a-select>
          </a-form-item>
        </a-col>
        <a-col :span="8">
          <a-form-item label="文件名">
            <a-input v-model="fileName"></a-input>
          </a-form-item>
        </a-col>
      </a-row>
    </a-form>
    <code-diff :old-string="oldStr" :new-string="newStr" :context="context" :output-format="outputFormat"
      :drawFileList="drawFileList" :renderNothingWhenEmpty="renderNothingWhenEmpty" :diffStyle="diffStyle"
      :fileName="fileName" :isShowNoChange="isShowNoChange" />
  </div>
</template>

<script>
  import codeDiff from './lib/index.js'
  export default {
    name: 'App',
    components: {
      codeDiff
    },
    data() {
      return {
        oldStr: '',
        newStr: '',
        fotmat: false,
        context: 10,
        diffStyle: 'word',
        fileName: '',
        isShowNoChange: true,
        drawFileList: true,
        renderNothingWhenEmpty: false
      }
    },
    computed: {
      outputFormat() {
        return this.fotmat ? 'line-by-line' : 'side-by-side'
      }
    },
    watch: {
      oldStr(v) {
        localStorage.setItem('oldStr', v)
      },
      newStr(v) {
        localStorage.setItem('newStr', v)
      }
    },
    created() {
      this.oldStr = localStorage.getItem('oldStr') || ''
      this.newStr = localStorage.getItem('newStr') || ''
    },
    methods: {
      handleClearLocalStorage() {
        this.newStr = ''
        this.oldStr = ''
        localStorage.setItem('newStr', '')
        localStorage.setItem('oldStr', '')
      }
    }
  }

</script>
