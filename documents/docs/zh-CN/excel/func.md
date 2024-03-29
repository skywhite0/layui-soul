## 调用方法导出

### 效果
除了可以通过框架导出之外，你也可以通过调用 `export` 方法进行自定义导出。
```js
var myTable = table.render({}); // 渲染表格

// 导出 excel
soulTable.export(myTable)

// 或者你也可以自定义文件名及样式
soulTable.export(myTable, {
   filename: '诗词.xlsx', // 文件名
   head:{ // 表头样式
       family: 'Calibri', // 字体
       size: 12, // 字号
       color: '000000', // 字体颜色
       bgColor: 'C7C7C7' // 背景颜色
   },
   font: { // 正文样式
       family: 'Calibri', // 字体
       size: 12, // 字号
       color: '000000', // 字体颜色
       bgColor: 'FFFFFF' //背景颜色
   } 
});
```
:::demo 通过列的 `excel` 进行设置
```html
<button class="layui-btn" id="export"><i class="layui-icon layui-icon-download-circle"></i>导出</button>
<table id="myTable" ></table>
<script>
layui.use(['form', 'table','soulTable'], function () {
    var table = layui.table,
        soulTable = layui.soulTable;

    var myTable = table.render({
        elem: '#myTable'
        ,url: 'data-1.json'
        ,height: 500
        ,page: false
        ,cols: [[
            {type: 'checkbox', fixed: 'left'},
            {field: 'title', title: '诗词', width: 200, sort: true, filter: true, 
              excel: {color:'FFFFFF',bgColor:'5FB878'} // 导出excel显示样式
            },
            {field: 'dynasty', title: '朝代', width: 100, sort: true, filter: true, excel: function(row) {
               // row 为当前行数据
               return {
                   color: row.dynasty === '唐代' ? 'FFFFFF' : '000000', // 唐代显示为黄色
                   bgColor: row.dynasty === '唐代' ? '01AAED' : 'FFFFFF', // 唐代显示为浅蓝色底色
               }
            }},
            {field: 'author', title: '作者', width: 165 , filter: true},
            {field: 'content', title: '内容', width: 123, filter: true},
            {field: 'type', title: '类型', width: 112,  filter: {split:','}, sort:true},
            {field: 'heat', title: '点赞数', width: 112,  filter: true, fixed: 'right', sort:true, excel:{cellType: 'n'}},
            {field: 'createTime', title: '录入时间', width: 165, fixed: 'right',excel: {cellType: 'd'}},
        ]]
        ,done: function () {
            soulTable.render(this)
        }
    });
    
    layui.$('#export').on('click', function() {
      soulTable.export(myTable, {
          filename: '手动导出.xlsx' 
      });
    })
})
</script>
```
:::

### 仅导出勾选数据
:::demo 
```html
<button class="layui-btn" id="export2"><i class="layui-icon layui-icon-download-circle"></i>导出</button>
<table id="myTable2" ></table>
<script>
layui.use(['form', 'table','soulTable'], function () {
    var table = layui.table,
        soulTable = layui.soulTable;

    var myTable2 = table.render({
        id: 'myTable2'
        ,elem: '#myTable2'
        ,url: 'data-1.json'
        ,height: 500
        ,page: false
        ,cols: [[
            {type: 'checkbox', fixed: 'left'},
            {field: 'title', title: '诗词', width: 200, sort: true, filter: true, 
              excel: {color:'FFFFFF',bgColor:'5FB878'} // 导出excel显示样式
            },
            {field: 'dynasty', title: '朝代', width: 100, sort: true, filter: true, excel: function(row) {
               // row 为当前行数据
               return {
                   color: row.dynasty === '唐代' ? 'FFFFFF' : '000000', // 唐代显示为黄色
                   bgColor: row.dynasty === '唐代' ? '01AAED' : 'FFFFFF', // 唐代显示为浅蓝色底色
               }
            }},
            {field: 'author', title: '作者', width: 165 , filter: true},
            {field: 'content', title: '内容', width: 123, filter: true},
            {field: 'type', title: '类型', width: 112,  filter: {split:','}, sort:true},
            {field: 'heat', title: '点赞数', width: 112,  filter: true, fixed: 'right', sort:true, excel:{cellType: 'n'}},
            {field: 'createTime', title: '录入时间', width: 165, fixed: 'right',excel: {cellType: 'd'}},
        ]]
        ,done: function () {
            soulTable.render(this)
        }
    });
    
    layui.$('#export2').on('click', function() {
      if (table.checkStatus('myTable2').data.length > 0) {
        soulTable.export(myTable2, {
            filename: '手动导出.xlsx',
            checked: true // 只导出勾选数据
        });
      } else {
          layer.msg('勾选数据不能为空！');
      }
      
    })
})
</script>
```
:::
