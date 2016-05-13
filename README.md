# ubuntu-winfonts

关于ubuntu中node-canvas不能显示中文的解决方法: 在Ubuntu上安装Windows字体


## Step1 拷贝字体 
在 /usr/share/fonts/ 中创建一个新的文件夹 winfonts
```
cp -rf winfonts /usr/share/fonts/
```


## Step2 修改权限
执行下面的命令 
sudo mkdir /usr/share/fonts/winfonts 
同时修改权限 
sudo chmod 777 /usr/share/fonts/winfonts  


## Step3 生成核心字体信息 
分别执行如下三个命令 
```
sudo mkfontscale 
sudo mkfontdir 
sudo fc-cache -f -v 
```


## Step4 查找字体名 
虽然我知道他叫微软雅黑，但是使用的时候要使用他的英文名，中文名经过测试是不行的。 

执行fc-list，寻找含有“微软雅黑”的信息，也就是刚才你新安装的字体。 


像下面这一行： 
/usr/share/fonts/cnfont/msyhbd.ttf: 微软雅黑,Microsoft YaHei:style=Bold,...... 


可以看到英文名叫Microsoft YaHei 


##  Test

```
npm install canvas
```

Script

```
var Canvas = require('canvas')
  , Image = Canvas.Image
  , canvas = new Canvas(200, 200)
  , ctx = canvas.getContext('2d');

ctx.font = '30px Impact';
ctx.rotate(.1);
ctx.fillText("蛇!", 50, 100);

var te = ctx.measureText('蛇!');
ctx.strokeStyle = 'rgba(0,0,0,0.5)';
ctx.beginPath();
ctx.lineTo(50, 102);
ctx.lineTo(50 + te.width, 102);
ctx.stroke();

// console.log('<img src="' + canvas.toDataURL() + '" />');
var img = canvas.toDataURL();
var fs = require("fs");

var data = img.replace(/^data:image\/\w+;base64,/, "");
var buf = new Buffer(data, 'base64');
fs.writeFile('image.png', buf);
```