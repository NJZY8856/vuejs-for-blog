<template>
  <!--S=联系我-->
  <div class="col-md-12">
    <div class="archive-comment" >
      <div class="archive-comment-part1" id="commentField">
        <div class="col-md-4">
          <input type="text" class="form-control" name="name" placeholder=" 你的昵称 «-必填" v-model="nameModel">
          <div class="text-center input-error">{{nameErrors.errorText}}&nbsp;</div>
        </div>
        <div class="col-md-4">
          <input type="text" class="form-control" name="email" id="email" v-model="emailModel" placeholder=" 电子邮件«-必填，不公开">
          <div class="text-center input-error">{{emailErrors.errorText}}&nbsp;</div>
        </div>
        <div class="col-md-4">
          <input type="text" class="form-control" name="url" placeholder=" 个人网址«-选填" v-model="urlModel"></div>
        <div class="text-center input-error">{{urlErrors.errorText}}&nbsp;</div>
        <div class="col-md-12" style="padding: 0">
          <textarea name="content" class="form-control"  id="content" rows="5"
                    placeholder="请填写您的联系原因"></textarea>
        </div>
      </div>
      <div class="col-md-12" style="padding: 0">
        <button class="btn btn-primary form-control" type="button" @click="submitComment">&nbsp;提交&nbsp;</button>
      </div>
      <div class="parting-line">&nbsp;</div>
    </div>
  </div>
  <!--E=联系我-->
</template>

<script>
  export default {
    created: function () {
    },
    mounted: function () {
      $(".dialog-wrap").hide()
    },
    data () {
      return {
        content: '',
        nameModel: '',
        emailModel: '',
        urlModel: ''
      }
    },
    computed: {
      emailErrors () {
        let errorText, status
        if (!/^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/g.test(this.emailModel)) {
          status = false
          errorText = '邮箱格式不合法'
        }
        else {
          status = true
          errorText = ''
        }
        if (!this.emailFlag) {
          errorText = ''
          this.emailFlag = true
        }
        return {
          status,
          errorText
        }
      },
      nameErrors () {
        let errorText, status
        if (!/^\w{1,50}$/g.test(this.nameModel)) {
          status = false
          errorText = '昵称长度不合法'
        }
        else {
          status = true
          errorText = ''
        }
        if (!this.userFlag) {
          errorText = ''
          this.userFlag = true
        }
        return {
          status,
          errorText
        }
      },
      urlErrors () {
        let errorText, status
        if (!/(([a-zA-z]+:\/\/)?[^\s]*\.[^\s]+)/g.test(this.urlModel) && (this.urlModel).trim() != "") {
          status = false
          errorText = '网址格式不合法'
        }
        else {
          status = true
          errorText = ''
        }
        if (!this.urlFlag) {
          errorText = ''
          this.urlFlag = true
        }
        return {
          status,
          errorText
        }
      }
    },
    methods: {
      updateData () {
        //然后是获取数据
        this.content = document.getElementsByClassName("content")[0].innerHTML;
      },
      submitComment() {
        if(!this.nameErrors.status || !this.emailErrors.status || !this.urlErrors.status) {
          if(!/[^\s]{1,}/g.test(this.content))
            alert('请填写评论内容');
          else
            alert('部分选项未按要求填写');
        }else{
          var name = document.getElementsByName("name")[0].value;
          var email = document.getElementsByName("email")[0].value;
          var url = document.getElementsByName("url")[0].value;
          var options = {
            name: name,
            email: email,
            url: url,
            content: this.content
          };
          //提交评论
          this.$http.post('/someUrl', options).then(response => {
              alert("提交成功");
           }, response => {
              alert("提交失败");
          })
        }
      }
    }
  }

</script>

<style scoped>
  .index-left-block h2 {
    background: #4fc08d;
    color: #fff;
    padding: 10px 15px;
    font-size: 15px;
    margin-top: 0;
    margin-bottom: 0;
  }

  .index-left-block h3 {
    padding: 0 15px 5px 15px;
    font-weight: bold;
    color: #222;
  }

  .index-left-block ul {

    padding: 2px 15px;
  }

  .index-left-block li {
    padding: 2px;
  }


  .index-board-item h2 {
    font-size: 18px;
    font-weight: bold;
    color: #000;
    margin-bottom: 15px;
  }

  .parting-line {
    border: none;border-bottom: 1px solid #eeeeee;height: 1px;
    clear: both;
  }


  .archive-comment {
    clear:both;padding-top: 20px;padding-bottom: 10px
  }
  .archive-comment-part2-list-op span{
    color:#999999;
    cursor: pointer;
    letter-spacing: normal;
  }
  .archive-comment-part2-list-author  span{
    color:#999999;
    letter-spacing: normal;
  }



  .archive-comment-part1 .col-md-4,.col-md-4 input{
    margin-left: 0;
    padding-left: 0;
    margin-right: 0;
    padding-right: 0;
  }


  .archive-comment-part1 .reply-someone span:hover{
    color: #4398ed;
    cursor: pointer;
  }

  .archive-comment-part2-title span {
    height: 20px;line-height: 50px
  }
  .input-error {
    color: #999999;
  }

</style>
