## Math对象

###  **new** *Date*()

 var d = **new** *Date*();

####  getFullYear() 获取4位数的年份

####  getMonth() 获取月份 返回值 0~11 0表示1月 11表示12月

#### getDate() 返回一个月中的某一天 返回值：1~31

#### getHours() 小时 返回值0~23

#### getMinutes() 获取分钟 返回值：0~59

#### getSeconds() 获取秒数 返回值：0~59

#### getMilliseconds() 获取毫秒 返回值：0~999

####  getDay() 获取一周中的某一天 就是星期几 返回值：0~6 0代表星期天 1代表星期一

####  getTime() 获取时间戳 返回从1970年1月1日00时00分00秒到现在的毫秒数！

####  toLocaleString() 根据本地时间把 Date 对象转换为字符串，并返回结果

```
        console.log( d.toLocaleString() );
        console.log( d.toLocaleDateString() );
        console.log( d.toLocaleTimeString() );
```

![](C:\Users\86189\Desktop\笔记\js基础\对象\imgs\toLocalString.png)

### 练习

当前时间写成这个格式   2021年08月16日 14:06:08 星期

```
  // 零填充函数
        function zeroFill( num ){
            return num < 10 ? "0" + num : num;
        }
        
        // 创建日期对象
        var d = new Date();
        var year = d.getFullYear();
        var month = zeroFill( d.getMonth() + 1 );// 返回的结果是从0开始的,0代表1月
        var day = zeroFill( d.getDate() );
        var h = zeroFill( d.getHours() );
        var m = zeroFill( d.getMinutes() );
        var s = zeroFill( d.getSeconds() );

        var arrWeek = "天一二三四五六".split("");
        var week = d.getDay();// 返回的结果0到6 0代表星期天 1代表星期一
        console.log(year + "年" + month + "月" + day + "日"+" " + h + ":" + m + ":" + s + " 星期" + arrWeek[week]);
```

