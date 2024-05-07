<!doctype html>
<html>

<head>
  <title>客户群聊界面</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: '微软雅黑'
    }

    #container {
      width: 100%;
      height: 1700px;
      background: #eee;
      position: relative;
      box-shadow: 40px 40px 110px #777;
    }

    .header {
      background: #25421ab0;
      height: 80px;
      color: #fff;
      line-height: 68px;
      font-size: 40px;
      padding: 0 20px;
    }


    body {
      width: 100%;
      background: #eee;
      position: relative;
      font: 60px;
    }

    form {
      background: #000;
      position: fixed;
      bottom: 0;
      width: 100%;
    }

    form input {
      border: 0;
      width: 80%;
      font-size: 40px;
    }

    form button {
      width: 20%;
      background: rgb(130, 224, 255);
      border: none;
      font-size: 40px;
    }

    #messages {
      list-style-type: none;
      margin: 0;
      padding: 0;
      font-size: 40px;
    }

    #messages li {
      padding: 5px 10px;
      font-size: 40px;
    }

    #messages li:nth-child(odd) {
      background: #eee;
    }
  </style>
  <script src="http://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js"></script>
</head>

<body>
  <div class="header">
    <span style="float: left;">客户交流平台</span>
    <!--<span style="float: right;">14:21</span>-->
  </div>
  <ul id="messages"></ul>
  <form action="">
    <input id="m" placeholder="说点什么吧..." autocomplete="off" /><button>发送</button>
  </form>
</body>

<script src="/socket.io/socket.io.js"></script>
<script>
  var name = prompt("请输入你的昵称：");
  var socket = io()

  //发送昵称给后端，并更改网页title
  socket.emit("join", name)
  document.title = name + "的群聊"

  socket.on("join", function (user) {[Uploading index.html…]()

    addLine(user + " 加入了群聊")
  })

  //接收到服务器发来的message事件
  socket.on("message", function (msg) {
    addLine(msg)
  })

  //当发送按钮被点击时
  $('form').submit(function () {
    var msg = $("#m").val() //获取用户输入的信息
    socket.emit("message", msg) //将消息发送给服务器
    $("#m").val("") //置空消息框
    return false //阻止form提交
  })

  function addLine(msg) {
    $('#messages').append($('<li>').text(msg));
  }
</script>

</html>
