# 邮件发送与接收

> form.php（提交地址，用于接收数据及邮件发送）

> class.smtp.php

> class.phpmailer.php

## 小程序提交方式

```javascript
<form bindsubmit="indexForm">
  <input name="title" value="{{titleValue}}" placeholder="请输入邮件标题" maxlength="50" type="text" />
  <textarea name="content" value="{{contentValue}}" placeholder="请输入发送邮件的内容" />
  <button form-type="submit">立即提交</button>
</form>
```

```javascript
indexForm: function (e) {
        var that = this;
        wx.request({
            url: 'https://www.******.com/form.php',
            method: 'POST',
            dataType: 'json',
            'data': {
                'title': e.detail.value.title,
                'content': e.detail.value.content,
            },
            header: {
                'content-type': 'application/x-www-form-urlencoded'
            },
            success: function (res) {
                if (res.data == "200") {
                    wx.showToast({
                        title: '提交成功',
                        duration: 2000,
                        success(res) {
                            that.setData({
                                'titleValue': '',
                                'contentValue': ''
                            });
                            that.setData({
                                formstate: true
                            });
                        },
                        fail(res) {
                            console.log(`showToast调用失败`);
                        }
                    });
                } else if (res.data == "401") {
                    wx.showModal({
                        title: '提示',
                        content: '请输入标题',
                        showCancel: false,
                        confirmText: '确定',
                        success(res) {
                            that.setData({

                            });
                            that.setData({
                                formstate: true
                            });
                        }
                    })
                } else if (res.data == "402") {
                    wx.showModal({
                        title: '提示',
                        content: '请输入要反馈的内容',
                        showCancel: false,
                        confirmText: '确定',
                        success(res) {
                            that.setData({

                            });
                            that.setData({
                                formstate: true
                            });
                        }
                    })
                }
            },
            fail: function (res) {
                console.log(res);
            }
        });

    }
```

## Ajax

```javascript
<form>
    <input type="text" class="title" placeholder="请输入邮件标题">
    <input type="text" class="content" placeholder="请输入发送邮件的内容" autocomplete="off" />
    <div class="submit"></div>
</form>
```

```javascript
$(document).ready(function () {
  function ajaxFn(title, content) {
    $.ajax({
      url: "https://www.xxxxxx.com/form.php",
      type: "POST",
      async: false,
      data: {
        title: title,
        content: content,
      },
      success: function (data) {
        if (data === "200") {
          $(".title").val("");
          $(".content").val("");
          alert("邮件发送成功");
        } else if (data === "401") {
          alert("请输入邮件标题");
        } else if (data === "402") {
          alert("请输入邮件内容");
        }
      },
    });
  }

  $(".submit").off("click").on("click", function () {
      var title = $(this).siblings(".title").val();
      var content = $(this).siblings(".content").val();
      ajaxFn(title, content);
    });
});
```

```javascript
$(document).ready(function () {
  $(".submit")
    .off("click")
    .on("click", function () {
      var title = $(this).siblings(".title").val();
      var content = $(this).siblings(".content").val();
      $.ajax({
        url: "https://www.xxxxxx.com/form.php",
        type: "POST",
        async: false,
        data: {
          title: title,
          content: content,
        },
        success: function (data) {
          if (data === "200") {
            $(".title").val("");
            $(".content").val("");
            alert("邮件发送成功");
          } else if (data === "401") {
            alert("请输入邮件标题");
          } else if (data === "402") {
            alert("请输入邮件内容");
          }
        },
      });
    });
});
```
