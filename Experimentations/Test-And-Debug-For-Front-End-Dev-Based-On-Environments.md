## 前端测试与异常处理

前端测试是一个较为繁重但又不可或缺的任务，涉及的版本较多同时受各种环境的影响较大，为了能为不同的用户提供类似的使用体验，在当前未全面引入**自动化测试**之前，引入一些基本的测试规范，进而未将来展开全面的前端测试做基础。

### 测试流程

* 测试标准：通过列表的形式列出需要进行测试的页面、效果和行为
* 参考标准：通过设计图/标准页面提供预期的页面效果与行为
* 测试进行：在列出的浏览器中与**测试标准**进行对比并**记录异常**
* 测试反馈：在测试标准约束下将**异常结果**反馈在**反馈结果**中

### 异常处理

* 异常处理：对**异常结果**进行Debug并记录原因，收录进**浏览器差异数据库**
* 异常解决：更新Debug后的版本到工作分支，通知分支负责人->通知主分支负责人

#### 浏览器版本

<table class="environments">
<tbody>
    <tr>
        <th align="left">Internet Explorer</th>
        <td align="center">8.0</td>
        <td align="center">9.0</td>
        <td align="center">10.0</td>
        <td colspan="2" align="center">11.0</td>
    </tr>
    <tr>
        <th align="left">Chrome †</th>
        <td colspan="6" align="center">Latest stable</td>
    </tr>
    <tr>
        <th align="left">Firefox †</th>
        <td colspan="6" align="center">Latest stable</td>
    </tr>
    <tr>
        <th align="left">Safari</th>
        <td align="center">iOS 5.†</td>
        <td align="center">iOS 6.†</td>
        <td align="center">iOS 7.†</td>
        <td align="center">Safari 6.1 <br> (OS X 10.8)</td>
        <td colspan="2" align="center">Safari 7.† <br> (OS X 10.9)</td>
    </tr>
    <tr>
        <th align="left">Android</th>
        <td align="center">2.3.†</td>
        <td colspan="5" align="center">4.†</td>
    </tr>
    <tr>
        <th align="left">Node.js*</th>
        <td align="center">0.8.†</td>
        <td colspan="5" align="center">0.10.†</td>
    </tr>
    <tr>
        <th align="left">Windows (Native)</th>
        <td colspan="6" align="center">Windows 8 Apps (WinJS &gt;= 2.†)</td>
    </tr>
</tbody>
</table>

#### 系统版本

<table class="environments">
<tbody>
    <tr>
        <th align="left">winddows</th>
        <td align="center">7</td>
        <td align="center">8</td>
        <td colspan="4" align="center">8.1</td>
    </tr>
    <tr>
        <th align="left">osx</th>
        <td colspan="6" align="center">Latest stable</td>
    </tr>
    <tr>
        <th align="left">linux</th>
        <td colspan="6" align="center">Latest stable</td>
    </tr>
        <tr>
        <th align="left">android</th>
        <td colspan="6" align="center">Latest stable</td>
    </tr>
</tbody>
</table>