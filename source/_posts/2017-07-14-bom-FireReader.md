---
title: FileReader()
date: 2017-07-14 13:49:21
updated: 2017-07-14 13:49:21
tags: Bom
categories: JavaScript
---
## 简介
FileReader 对象允许Web应用程序异步读取存储在用户计算机上的文件（或原始数据缓冲区）的内容，使用 File 或 Blob 对象指定要读取的文件或数据。

其中File对象可以是来自用户在一个`<input>`元素上选择文件后返回的FileList对象,也可以来自拖放操作生成的 DataTransfer对象,还可以是来自在一个HTMLCanvasElement上执行mozGetAsFile()方法后返回结果.

## demo
```html
<!doctype html>
<html>

<head>
    <meta content="text/html; charset=UTF-8" http-equiv="Content-Type" />
    <title>Image preview example</title>
    <script type="text/javascript">

    if ( typeof(FileReader) === 'undefined' ){
        result.innerHTML = "抱歉，你的浏览器不支持 FileReader，请使用现代浏览器操作！"; 
        // input.setAttribute('disabled','disabled'); 
        return ;
    }

    var oFReader = new FileReader();
    var rFilter = /^(?:image\/bmp|image\/cis\-cod|image\/gif|image\/ief|image\/jpeg|image\/jpeg|image\/jpeg|image\/pipeg|image\/png|image\/svg\+xml|image\/tiff|image\/x\-cmu\-raster|image\/x\-cmx|image\/x\-icon|image\/x\-portable\-anymap|image\/x\-portable\-bitmap|image\/x\-portable\-graymap|image\/x\-portable\-pixmap|image\/x\-rgb|image\/x\-xbitmap|image\/x\-xpixmap|image\/x\-xwindowdump)$/i;

    oFReader.onload = function(oFREvent) {
        document.getElementById("uploadPreview").src = oFREvent.target.result;
        document.getElementById("im-base64g").innerHTML = oFREvent.target.result;
    };

    function loadImageFile() {
        if (document.getElementById("uploadImage").files.length === 0) {
            return;
        }
        var oFile = document.getElementById("uploadImage").files[0];
        if (!rFilter.test(oFile.type)) {
            alert("You must select a valid image file!");
            return;
        }
        oFReader.readAsDataURL(oFile);
    }
    </script>
</head>

<body onload="loadImageFile();">
    <form name="uploadForm">
        <table>
            <tbody>
                <tr>
                    <td><img id="uploadPreview" style="width: 100px; height: 100px;" src="data:image/svg+xml,%3C%3Fxml%20version%3D%221.0%22%3F%3E%0A%3Csvg%20width%3D%22153%22%20height%3D%22153%22%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%3E%0A%20%3Cg%3E%0A%20%20%3Ctitle%3ENo%20image%3C/title%3E%0A%20%20%3Crect%20id%3D%22externRect%22%20height%3D%22150%22%20width%3D%22150%22%20y%3D%221.5%22%20x%3D%221.500024%22%20stroke-width%3D%223%22%20stroke%3D%22%23666666%22%20fill%3D%22%23e1e1e1%22/%3E%0A%20%20%3Ctext%20transform%3D%22matrix%286.66667%2C%200%2C%200%2C%206.66667%2C%20-960.5%2C%20-1099.33%29%22%20xml%3Aspace%3D%22preserve%22%20text-anchor%3D%22middle%22%20font-family%3D%22Fantasy%22%20font-size%3D%2214%22%20id%3D%22questionMark%22%20y%3D%22181.249569%22%20x%3D%22155.549819%22%20stroke-width%3D%220%22%20stroke%3D%22%23666666%22%20fill%3D%22%23000000%22%3E%3F%3C/text%3E%0A%20%3C/g%3E%0A%3C/svg%3E" alt="Image preview" /></td>
                    <td>
                        <input id="uploadImage" type="file" name="myPhoto" onchange="loadImageFile();" /> <br>
                        <textarea id="im-base64g" id="result" rows=30 cols=100></textarea> 
                    </td>
                </tr>
            </tbody>
        </table>
        <div style="text-align: center">
          <input type="submit" value="Send" />
        </div>
    </form>
</body>

</html>

```

## 参考：
- [FileReader()](https://developer.mozilla.org/zh-CN/docs/Web/API/FileReader)