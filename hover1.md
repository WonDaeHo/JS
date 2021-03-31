```
  <!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<link rel="stylesheet" href="color.css">
<link href='https://fonts.googleapis.com/css?family=Bangers' rel='stylesheet' type='text/css'>
<title>Document</title>
</head>
<body>
<div class="container">
    <div class="box1">
        <div class="box2">
            <span>Button</span>
        </div>
    </div>
</div>
</body>
</html>
```

```css
  html,body {
    width:100%;
    height:100%;
    margin:0px;
    padding:0px;
}
.container{
     width:100%;height:100%;background:#000;
}
.box2, .box2 span, .box1{ 
    position:absolute;
    display:block; left:50%;
    transform:translate(-50%,-50%);
} 
.box1{
    top:50%;
    width:300px;
    height:400px;
    cursor:pointer;
} 
.box2{
    top:40%;
    width:150px;
    height:150px;
    border-radius:50%; 
    background:red;
    box-shadow: 0 0 15px red;
}
.box2 span{
    top:50%; 
    font-size:2em;
    font-weight: bold;
    font-family: 'Bangers', cursive;
}
.box1:hover > .box2{
    animation:circlemove 1.5s infinite linear;
    /*1.5초동안 애니메이션이 발동되고 무한루프로 속도는 일정하게 한다.*/
}
@keyframes circlemove{
    0%, 100%{
    transform:translate(-50%,-50%);
}
    50%{ transform:translate(-50%,-60%);
    box-shadow: 0 0 30px red;
}
}
```
