<template>
  <div class="questions-wraps">
    <div class="header">
      <!-- 查看所有试题 /exam/questions/new -->
      <h3>试题详情</h3>
    </div>
    <div class="section">
      <div class="content">
        <div class="left">
          <p class="emitor">发布者:{{ detailobj.user_name }}</p>
          <p>题目信息</p>
          <el-button
            style="color: #1890ff;background: #e6f7ff;border-color: #91d5ff;"
          >{{ detailobj.questions_type_text }}</el-button>
          <el-button
            style="color: #2f54eb;background: #f0f5ff; border-color: #adc6ff; padding 6px 10px"
          >{{ detailobj.subject_text }}</el-button>
          <el-button style="color: #fa8c16;background: #fff7e6;border-color: #ffd591;">{{ detailobj.exam_name }}</el-button>
          <p>{{ detailobj.title }}</p>

          <markdown-editor v-model="content" height="400px" :options="{hideModeSwitch:false,previewStyle:false,toolbarItems:[]}" />
        </div>
        <div class="right">
          <p class="emitor" style="width:100% height:50px lineHeight:50px">答案信息</p>
          <markdown-editor v-model="contentanswer" :options="{hideModeSwitch:false,previewStyle:false,toolbarItems:[]}" height="400px" />

        </div>

      </div>
    </div>
  </div>
</template>
<script>
import MarkdownEditor from '@/components/MarkdownEditor'
import { mapActions, mapGetters } from 'vuex'
export default {
  components: {
    MarkdownEditor
  },
  props: {},
  data() {
    return {
      content: '',
      contentanswer: '',
      detailobj: {}
    }
  },

  computed: {
    ...mapGetters(['checkitemlist'])
  },
  mounted() {
    this.checkitems()
    const id = this.$route.query.id
    this.detailobj = this.checkitemlist.find((item, ind) => {
      return item.questions_id === id
    })
    if (this.detailobj) { // 一进页面 试题详情这一条没有 所以要做下判断  不然会刷新会报错
      this.content = this.detailobj.questions_stem
      this.contentanswer = this.detailobj.questions_answer
    } else {
      this.detailobj = {}
    }
  },
  methods: {
    ...mapActions({
      checkitems: 'examType/checkitems'
    }),
    haha(value) {
      console.log('value')
    }
  }
}
</script>
<style scoped lang="scss">
* {
  padding: 0;
  margin: 0;
  list-style: none;
  font-size: 14px;
}
@mixin num($w, $h) {
  width: $w;
  height: $h;
}
@mixin flex($j: space-between) {
  display: flex;
  justify-content: $j;
  align-items: center;
  display: -webkit-flex;
  -webkit-justify-content: $j;
  -webkit-align-items: center;
}

.questions-wraps {
  @include num(100%, 637px);
  background: #f0f2f5;
  overflow-y: auto;
}
.questions-wraps {
  .header {
    @include num(100%, 100px);

    h3 {
      @include num(100%, 100px);
      line-height: 100px;
      font-weight: 400;
      font-size: 24px;
      text-indent: 2em;
    }
  }
}
.section {
    @include num(100%, 100%);
    .content {
      padding-left: 24px;
      width: 95%;
      height: 100%;
      margin: 0 auto;
      @include flex(space-between);
      .left{
        width:60%;
        height:100%;
        background: #fff;
        padding:0 15px;
         border-radius: 10px;
          .leftcode{
          padding:12px 12px 12px 0;
          code{
            border-radius:10px;
          }
        }
         p{
           width:100%;
           height:40px;
           line-height:40px;
           margin:5px 0px;
         }
         .el-button--medium {
        padding: 4px 10px;
      }

      }
      .right{
        width:30%;
        background: #fff;
        height:100%;
        border-radius: 10px;
        margin:0 10px;
         p{
           width:100%;
           height:40px;
           line-height:40px;
           margin:5px 0px;
         }
        .rightcode{
          padding:12px;
          code{
            border-radius:10px;
          }
        }
      }
    }
  }
  .mark /deep/.te-toolbar-section{ //前面要改的原组件的大盒子类名  后面是 在这边的类名
    border:none;
  }

</style>
