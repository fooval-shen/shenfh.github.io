---
layout: post
title: Vim 常用命令
author: shefh
catalog:  true
tags:
    - linux
---


## 光标移动
<table width="100%">
	<body >
		<tr>
			<th width="20%">命令</th>
			<th width="80%">作用（解释）</th>
		</tr>
		<tr class="tableRow">
			<td><code >h,j,k,l</code></td>
			<td><code >h</code>表示往左，<code >j</code>表示往下，<code >k</code>表示往右，<code >l</code>表示往上</td>
		</tr>
		<tr class="tableRow">
			<td><code >Ctrl</code>+<code >f</code></td>
			<td>上一页</td>
		</tr>
		<tr class="tableRow">
			<td><code >Ctrl</code>+<code >b</code></td>
			<td>下一页</td>
		</tr>
		<tr class="tableRow">
			<td><code >w</code>, <code >e</code>, <code >W</code>, <code >E</code></td>
			<td>跳到单词的后面，小写包括标点</td>
		</tr>
		<tr class="tableRow">
			<td><code >b</code>, <code >B</code></td>
			<td>以单词为单位往前跳动光标，小写包含标点</td>
		</tr>
		<tr class="tableRow">
			<td><code >O</code></td>
			<td>开启新的一行</td>
		</tr>
		<tr class="tableRow">
			<td><code >^</code></td>
			<td>一行的开始</td>
		</tr>
		<tr class="tableRow">
			<td><code >$</code></td>
			<td>一行的结尾</td>
		</tr>
		<tr class="tableRow">
			<td><code >gg</code></td>
			<td>文档的第一行</td>
		</tr>
		<tr class="tableRow">
			<td><code >[N]G</code></td>
			<td>文档的第N行或者最后一行</td>
		</tr>
	</body>
</table>

## 插入模式
<table width="100%">
	<tbody>
		<tr>
			<th width="20%">命令</th>
			<th width="80%">作用（解释)</th>
		</tr>
		<tr class="tableRow">
			<td>
				<code >i</code>
			</td>
			<td>插入到光标前面</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >I</code>
			</td>
			<td>插入到行的开始位置</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >a</code>
			</td>
			<td>插入到光标的后面</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >A</code>
			</td>
			<td>插入到行的最后位置</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >o</code>, <code >O</code>
			</td>
			<td>新开一行</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >Esc</code>
			</td>
			<td>关闭插入模式</td>
		</tr>
	</tbody>
</table>

## 编辑
<table width="100%">
	<tbody>
		<tr>
			<th width="20%">命令</th>
			<th width="80%">作用（解释）</th>
		</tr>
		<tr class="tableRow">
			<td>
				<code >r</code>
			</td>
			<td>在插入模式替换光标所在的一个字符</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >J</code>
			</td>
			<td>合并下一行到上一行</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >s</code>
			</td>
			<td>删除光标所在的一个字符, 光标还在当行</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >S</code>
			</td>
			<td>删除光标所在的一行，光标还在当行，不同于<code >dd</code></td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >u</code>
			</td>
			<td>撤销上一步操作</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >U</code>
			</td>
			<td>撤销对整行的操作</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >ctrl</code>+<code >r</code>
			</td>
			<td>恢复上一步操作</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >.</code>
			</td>
			<td>重复最后一个命令</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >~</code>
			</td>
			<td>变换为大写</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >[N]>></code>
			</td>
			<td>一行或N行往右移动一个tab</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >[N]<<</code>
			</td>
			<td>一行或N行往左移动一个tab</td>
		</tr>
	</tbody>
</table>

## 关闭
<table width="100%">
	<tbody>
		<tr>
			<th width="20%">命令</th>
			<th width="80%">作用（解释)</th>
		</tr>
		<tr class="tableRow">
			<td>
				<code >:w</code>
			</td>
			<td>保存</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >:wq</code>,
				<code >:x</code>
			</td>
			<td>保存并关闭</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >:q</code>
			</td>
			<td>关闭（已保存）</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >:q!</code>
			</td>
			<td>强制关闭</td>
		</tr>
	</tbody>
</table>

## 搜索
<table width="100%">
	<tbody>
		<tr>
			<th width="20%">命令</th>
			<th width="80%">作用（解释)</th>
		</tr>
		<tr class="tableRow">
			<td>
				<code >/pattern</code>
			</td>
			<td>搜索（非插入模式)</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >?pattern</code>
			</td>
			<td>往后搜索</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >n</code>
			</td>
			<td>光标到达搜索结果的前一个目标</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >N</code>
			</td>
			<td>光标到达搜索结果的后一个目标</td>
		</tr>
	</tbody>
</table>

## 视觉模式
<table width="100%">
	<tbody>
		<tr>
			<th width="20%">命令</th>
			<th width="80%">作用（解释)</th>
		</tr>
		<tr class="tableRow">
			<td>
				<code >v</code>
			</td>
			<td>选中一个或多个字符</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >V</code>
			</td>
			<td>选中一行</td>
		</tr>
	</tbody>
</table>

## 剪切和复制
<table width="100%">
	<tbody>
		<tr>	
			<th width="20%">命令</th>
			<th width="80%">作用（解释)</th>
		</tr>
		<tr class="tableRow">
			<td>
				<code >dd</code>
			</td>
			<td>删除一行</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >dj</code>
			</td>
			<td>删除上一行</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >dk</code>
			</td>
			<td>删除下一行</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >10d</code>
			</td>
			<td>删除当前行开始的10行</td>
		</tr>	 

		<tr class="tableRow">
			<td>
				<code >dw</code>
			</td>
			<td>删除一个单词</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >x</code>
			</td>
			<td>删除后一个字符</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >3x</code>
			</td>
			<td>删除当前光标开始向后三个字符</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >X</code>
			</td>
			<td>删除前一个字符</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >D</code>
			</td>
			<td>删除一行最后一个字符</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >[N]yy</code>
			</td>
			<td>复制一行或者N行</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >yw</code>
			</td>
			<td>复制一个单词</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >p</code>
			</td>
			<td>粘贴</td>
		</tr>
	</tbody>
</table>

## 窗口操作
<table width="100%">
	<tbody>
		<tr>
			<th width="20%">命令</th>
			<th width="80%">作用（解释)</th>
		</tr>
		<tr class="tableRow">
			<td>
				<code >:split</code>
			</td>
			<td>水平方向分割出一个窗口</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >:vsplit</code>
			</td>
			<td>垂直方向分割出一个窗口</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >:close</code>
			</td>
			<td>关闭窗口</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >Ctrl</code>+<code >W</code>
			</td>
			<td>切换窗口, <code >h</code>到左边窗口，<code >j</code>到下方窗口，<code >k</code>到上方窗口，<code >l</code>到右边窗口</td>
		</tr>
	</tbody>
</table>

## 执行shell命令

<table width="100%">
	<tbody>
		<tr>
			<th width="20%">命令</th>
			<th width="80%">作用（解释)</th>
		</tr>
		<tr class="tableRow">
			<td>
				<code >:!command</code>
			</td>
			<td><code>command</code> 命令</td>
		</tr>
		<tr class="tableRow">
			<td>
				<code >:!ls</code>
			</td>
			<td>列出当前目录下文件</td>
		</tr>
		
		
	</tbody>
</table>


