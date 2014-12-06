> * title: DateTimePicker 修改版
> * date: 2014-09-24 15:32:25

DateTimePicker 是一个优秀的可用于时间和日期选择的Js插件，但是他本身不具有选择秒的功能，为了项目需要，对源代码进行了修改，使其能够选择时间。

#### About

DateTimePicker 

Responsive flat design jQuery DateTime Picker plugin for Web & Mobile .

* Demo：[http://curioussolutions.github.io/DateTimePicker/](http://curioussolutions.github.io/DateTimePicker/ "DateTimePicker")
* Source：[https://github.com/CuriousSolutions/DateTimePicker](https://github.com/CuriousSolutions/DateTimePicker "DateTimePicker")

#### Usage

    # Include style, jquery and javascript file .

    <link rel="stylesheet" type="text/css" href="../src/DateTimePicker.css" />
    <script type="text/javascript" src="jquery-1.11.0.min.js"></script>
    <script type="text/javascript" src="../src/DateTimePicker-with-Seconds.js"></script>

    # add html tags
    <input id="dts" type="text" data-field="datetime">
    <div id="dtBox"></div>

    # add javascript
    <script type="text/javascript">		
    $(document).ready(function(){
        $("#dtBox").DateTimePicker({
            dateTimeFormat: "dd-MM-yyyy hh:mm:ss"
        });
    });
    </script>

#### More

更多的说明文档可以在以下地址查看：

[http://curioussolutions.github.io/DateTimePicker/](http://curioussolutions.github.io/DateTimePicker/ "DateTimePicker")

本人修改的可以选择秒的版本可以在我的github仓库下载：

[https://github.com/thonatos/DateTimePicker](https://github.com/thonatos/DateTimePicker "DateTimePicker")
