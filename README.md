# Quicky小A 程序功能详细说明文档

> 程序版本：20260515-2157  
> 生成日期：2026-05-16  
> 程序用途：批量测试和分析大模型Prompt效果的质量分析工具

---

## 一、系统登录与密码管理

<table>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold; width:150px;">功能名称</td>
<td><strong>登录与密码管理</strong></td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">功能用途</td>
<td>
1. 保护系统安全，防止未授权访问<br>
2. 支持首次设置密码、修改密码、密码验证<br>
3. 具备防暴力破解机制（失败锁定）
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">使用方法</td>
<td>
1. 首次访问时，系统会引导你设置密码（至少6位）<br>
2. 设置完成后，每次访问都需要输入密码登录<br>
3. 在设置页面可以修改密码<br>
4. 本地访问（127.0.0.1）自动信任，不记录失败次数
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">常见问题</td>
<td>
<strong>Q: 密码忘记了怎么办？</strong><br>
A: 删除数据目录下的 password/password_config.json 文件，重启程序后可重新设置。<br><br>
<strong>Q: 输入密码多次错误被锁定了？</strong><br>
A: 等待锁定时间结束自动解锁，或通过测试接口 /api/password/reset-lock 重置锁定状态。<br><br>
<strong>Q: 为什么本地访问不需要验证失败次数？</strong><br>
A: 本地访问（127.0.0.1/localhost）被系统信任，不会触发锁定机制，方便本地调试使用。
</td>
</tr>
</table>

---

## 二、文件上传与解析

<table>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold; width:150px;">功能名称</td>
<td><strong>文件上传与解析</strong></td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">功能用途</td>
<td>
1. 上传需要批量处理的数据文件（Excel或CSV）<br>
2. 解析文件列名，用于后续字段映射<br>
3. 智能检测文件每行平均字符数，辅助分批设置
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">使用方法</td>
<td>
1. 点击上传按钮，选择 .xlsx、.xls 或 .csv 文件<br>
2. 上传完成后，系统会自动解析文件列名<br>
3. 可以使用"采样字符"功能，自动计算每条数据的平均字符数<br>
4. 上传的文件会生成 file_token，供后续创建任务使用
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">常见问题</td>
<td>
<strong>Q: 支持哪些文件格式？</strong><br>
A: 支持 Excel（.xlsx、.xls）和 CSV（.csv）格式。<br><br>
<strong>Q: 文件大小有限制吗？</strong><br>
A: 系统采用分块上传（1MB/块），理论上支持大文件，但建议单文件不超过100MB以保证性能。<br><br>
<strong>Q: 上传失败怎么办？</strong><br>
A: 检查文件格式是否正确，确保文件未被其他程序占用，然后重试。
</td>
</tr>
</table>

---

## 三、任务创建与管理

<table>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold; width:150px;">功能名称</td>
<td><strong>任务创建与管理</strong></td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">功能用途</td>
<td>
1. 创建批量处理任务，将数据发送给大模型处理<br>
2. 支持多种API提供商（OpenAI、Bella Agent等）<br>
3. 支持普通模式和MapReduce模式<br>
4. 可暂停、恢复、停止正在运行的任务
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">使用方法</td>
<td>
1. 上传数据文件后，填写Prompt模板<br>
2. 选择API模板（包含API Key、模型等配置）<br>
3. 选择运行模式：<br>
   - 普通模式：逐条处理<br>
   - MapReduce模式：分批处理，适合大数据量<br>
4. 点击"开始任务"启动处理<br>
5. 在任务列表中可以暂停/恢复/停止任务
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">常见问题</td>
<td>
<strong>Q: 任务卡住了怎么办？</strong><br>
A: 可以点击"停止"按钮终止任务，然后重新创建。如果是网络问题，系统会自动暂停任务。<br><br>
<strong>Q: MapReduce模式是什么？</strong><br>
A: 将数据分成多批处理，每批包含多条记录，适合大数据量场景，可以提高处理效率。<br><br>
<strong>Q: 如何选择合适的批大小？</strong><br>
A: 系统提供智能分批功能，会根据每条数据的字符数自动计算合适的批大小。也可以手动设置。
</td>
</tr>
</table>

---

## 四、任务状态控制

<table>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold; width:150px;">功能名称</td>
<td><strong>任务状态控制</strong></td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">功能用途</td>
<td>
1. 暂停正在运行的任务<br>
2. 恢复已暂停的任务<br>
3. 停止任务并终止处理
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">使用方法</td>
<td>
1. 在任务列表中找到目标任务<br>
2. 点击对应的操作按钮：<br>
   - 暂停：临时停止处理，保留进度<br>
   - 恢复：继续处理未完成的数据<br>
   - 停止：永久终止任务
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">常见问题</td>
<td>
<strong>Q: 暂停后数据会丢失吗？</strong><br>
A: 不会，暂停只是停止处理，已完成的数据会保留，可以随时恢复继续。<br><br>
<strong>Q: 停止后还能恢复吗？</strong><br>
A: 不能，停止是永久性操作，未处理的数据将不会被处理。如需继续，需要重新创建任务。
</td>
</tr>
</table>

---

## 五、结果查看与下载

<table>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold; width:150px;">功能名称</td>
<td><strong>结果查看与下载</strong></td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">功能用途</td>
<td>
1. 查看任务处理结果<br>
2. 下载处理结果文件（Excel格式）<br>
3. 查看任务日志和统计信息
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">使用方法</td>
<td>
1. 在任务列表中点击"查看结果"<br>
2. 可以在线浏览处理结果<br>
3. 点击"下载"按钮导出Excel文件<br>
4. 可以查看详细的处理日志
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">常见问题</td>
<td>
<strong>Q: 结果文件在哪里？</strong><br>
A: 结果文件保存在数据目录的 results 文件夹中，也可以通过界面直接下载。<br><br>
<strong>Q: 如何查看某条数据的处理详情？</strong><br>
A: 在结果列表中点击具体记录，可以查看大模型的原始返回内容。
</td>
</tr>
</table>

---

## 六、API模板管理

<table>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold; width:150px;">功能名称</td>
<td><strong>API模板管理</strong></td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">功能用途</td>
<td>
1. 保存常用的API配置（API Key、模型、Base URL等）<br>
2. 支持多种API提供商（OpenAI、Bella Agent、Bella工作流等）<br>
3. 快速切换不同的API配置
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">使用方法</td>
<td>
1. 点击"API模板"进入管理页面<br>
2. 点击"新建模板"填写配置信息<br>
3. 选择提供商类型（OpenAI/Bella Agent/Bella工作流）<br>
4. 填写对应的认证信息和参数<br>
5. 保存后可在创建任务时选择使用
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">常见问题</td>
<td>
<strong>Q: 支持哪些API提供商？</strong><br>
A: 支持 OpenAI 兼容接口、Bella Agent、Bella 工作流三种类型。<br><br>
<strong>Q: API Key安全吗？</strong><br>
A: API Key保存在本地数据目录中，不会上传到任何服务器。建议定期更换Key以保证安全。<br><br>
<strong>Q: 如何测试API配置是否正确？</strong><br>
A: 可以创建一个测试任务，发送少量数据验证API连通性。
</td>
</tr>
</table>

---

## 七、Prompt模板管理

<table>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold; width:150px;">功能名称</td>
<td><strong>Prompt模板管理</strong></td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">功能用途</td>
<td>
1. 保存常用的Prompt模板<br>
2. 支持变量替换（如 {{字段名}}）<br>
3. 快速复用已有的Prompt配置
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">使用方法</td>
<td>
1. 点击"Prompt模板"进入管理页面<br>
2. 点击"新建模板"编写Prompt<br>
3. 使用 {{字段名}} 语法引用数据列<br>
4. 保存后可在创建任务时选择使用
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">常见问题</td>
<td>
<strong>Q: 如何在Prompt中引用数据字段？</strong><br>
A: 使用双花括号语法，如 {{姓名}}、{{问题描述}}，系统会自动替换为对应的数据值。<br><br>
<strong>Q: Prompt有长度限制吗？</strong><br>
A: Prompt本身没有硬性限制，但需要考虑大模型的上下文窗口限制，建议控制在合理范围内。
</td>
</tr>
</table>

---

## 八、数据分析与统计

<table>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold; width:150px;">功能名称</td>
<td><strong>数据分析与统计</strong></td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">功能用途</td>
<td>
1. 对处理结果进行统计分析<br>
2. 按维度（城市、类别、状态等）分组统计<br>
3. 生成可视化分析报告<br>
4. 支持二次深度分析
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">使用方法</td>
<td>
1. 任务完成后，点击"统计分析"<br>
2. 选择统计维度（如按城市、按状态等）<br>
3. 系统自动计算各分组的数量和占比<br>
4. 可触发"深度分析"让大模型生成分析报告
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">常见问题</td>
<td>
<strong>Q: 统计分析支持哪些维度？</strong><br>
A: 支持数据中的任意枚举型字段作为统计维度，如城市、状态、类别等。<br><br>
<strong>Q: 深度分析是什么？</strong><br>
A: 深度分析会调用大模型，根据统计数据生成专业的分析报告，包含趋势分析、异常识别、优化建议等。
</td>
</tr>
</table>

---

## 九、双模型任务测试

<table>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold; width:150px;">功能名称</td>
<td><strong>双模型任务测试</strong></td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">功能用途</td>
<td>
1. 使用相同的配置对同一批数据执行两次任务<br>
2. 对比两次任务的处理结果<br>
3. 用于验证任务结果的一致性和稳定性
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">使用方法</td>
<td>
1. 创建任务时开启"双模型测试"选项<br>
2. 配置API和Prompt（两次任务使用相同的配置）<br>
3. 系统会执行两次相同的任务<br>
4. 结果会分别保存，方便对比查看
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">常见问题</td>
<td>
<strong>Q: 双模型测试会消耗更多Token吗？</strong><br>
A: 是的，每条数据会执行两次请求，Token消耗约为单次任务的2倍。<br><br>
<strong>Q: 两次任务的配置可以不同吗？</strong><br>
A: 目前两次任务使用相同的API和Prompt配置。
</td>
</tr>
</table>

---

## 十、时间计算与二次跑批

<table>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold; width:150px;">功能名称</td>
<td><strong>时间计算与二次跑批</strong></td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">功能用途</td>
<td>
1. 计算两个时间字段之间的时间差<br>
2. 基于计算结果进行二次处理<br>
3. 用于需要时间维度分析的场景
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">使用方法</td>
<td>
1. 在任务配置中开启"时间计算"功能<br>
2. 选择原始时间字段和被减时间字段<br>
3. 设置结果列名（默认"时间差(分钟)"）<br>
4. 配置二次跑批的Prompt和模型<br>
5. 第一次跑批完成后自动触发时间计算和二次处理
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">常见问题</td>
<td>
<strong>Q: 支持哪些时间格式？</strong><br>
A: 支持常见的时间格式，如 "2024-01-01 12:00:00"、"2024/1/1 12:00" 等。<br><br>
<strong>Q: 时间差的单位是什么？</strong><br>
A: 默认单位是分钟，可以在配置中修改结果列名来表明单位。
</td>
</tr>
</table>

---

## 十一、Webhook通知

<table>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold; width:150px;">功能名称</td>
<td><strong>Webhook通知</strong></td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">功能用途</td>
<td>
1. 任务完成时自动发送通知<br>
2. 支持企业微信群机器人<br>
3. 可配置多个Webhook地址
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">使用方法</td>
<td>
1. 在"Webhook管理"中添加企业微信机器人地址<br>
2. 创建任务时选择要通知的Webhook<br>
3. 任务完成后自动发送包含统计信息的通知
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">常见问题</td>
<td>
<strong>Q: 如何获取企业微信Webhook地址？</strong><br>
A: 在企业微信群中添加机器人，复制机器人的Webhook地址即可。<br><br>
<strong>Q: 通知包含哪些信息？</strong><br>
A: 通知包含任务名称、处理数量、成功率、耗时等关键统计信息。
</td>
</tr>
</table>

---

## 十二、定时任务

<table>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold; width:150px;">功能名称</td>
<td><strong>定时任务</strong></td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">功能用途</td>
<td>
1. 定时自动触发任务执行<br>
2. 支持周期性调度（每天、每周等）<br>
3. 无人值守自动化处理
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">使用方法</td>
<td>
1. 创建定时任务配置<br>
2. 设置触发时间（Cron表达式或简单周期）<br>
3. 关联要执行的任务模板<br>
4. 启用定时任务后自动按计划执行
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">常见问题</td>
<td>
<strong>Q: 程序关闭后定时任务还会执行吗？</strong><br>
A: 不会，定时任务依赖程序运行，程序关闭后定时任务会暂停。<br><br>
<strong>Q: 如何查看定时任务的执行记录？</strong><br>
A: 在定时任务管理页面可以查看每次执行的状态和结果。
</td>
</tr>
</table>

---

## 十三、日志管理

<table>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold; width:150px;">功能名称</td>
<td><strong>日志管理</strong></td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">功能用途</td>
<td>
1. 记录系统运行日志<br>
2. 便于问题排查和审计<br>
3. 支持日志查看、下载、清空
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">使用方法</td>
<td>
1. 在"系统日志"页面查看最近的日志<br>
2. 可以下载完整的日志文件<br>
3. 日志文件按大小自动轮转（单文件50MB，保留10个备份）
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">常见问题</td>
<td>
<strong>Q: 日志保存在哪里？</strong><br>
A: 日志保存在数据目录的 logs 文件夹中，主日志文件名为 app.log。<br><br>
<strong>Q: 日志会占用很多空间吗？</strong><br>
A: 系统会自动轮转日志，最大占用约500MB（50MB × 10个备份）。
</td>
</tr>
</table>

---

## 十四、模板导入导出

<table>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold; width:150px;">功能名称</td>
<td><strong>模板导入导出</strong></td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">功能用途</td>
<td>
1. 导出所有配置模板为JSON文件<br>
2. 从JSON文件导入模板配置<br>
3. 便于在不同环境间迁移配置
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">使用方法</td>
<td>
1. 点击"导出模板"下载JSON配置文件<br>
2. 点击"导入模板"选择JSON文件上传<br>
3. 系统会自动合并导入的模板（不覆盖已有的）
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">常见问题</td>
<td>
<strong>Q: 导入会覆盖现有模板吗？</strong><br>
A: 不会，导入是增量操作，只添加不存在的模板。<br><br>
<strong>Q: 可以只导出部分模板吗？</strong><br>
A: 目前导出是全量导出，如需部分导出，可以手动编辑JSON文件。
</td>
</tr>
</table>

---

## 十五、系统重置

<table>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold; width:150px;">功能名称</td>
<td><strong>系统重置</strong></td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">功能用途</td>
<td>
1. 清空所有历史任务数据<br>
2. 重置密码配置<br>
3. 保留API模板和Prompt模板配置<br>
4. 用于系统初始化或空间清理
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">使用方法</td>
<td>
1. 在系统设置中点击"重置系统"<br>
2. 确认操作后，所有任务数据和密码配置将被删除<br>
3. API模板和Prompt模板会保留<br>
4. 仅允许本地访问（127.0.0.1）执行此操作
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">常见问题</td>
<td>
<strong>Q: 重置后数据还能恢复吗？</strong><br>
A: 不能，重置是不可逆操作，请谨慎使用。<br><br>
<strong>Q: 重置会删除什么？</strong><br>
A: 会删除所有任务数据、结果文件、上传文件、日志文件（当前正在写的日志除外）以及密码配置。<br><br>
<strong>Q: 重置会保留什么？</strong><br>
A: 会保留API模板和Prompt模板配置。<br><br>
<strong>Q: 远程访问能执行重置吗？</strong><br>
A: 不能，系统重置仅允许本地访问（127.0.0.1/localhost）执行，这是安全限制。
</td>
</tr>
</table>

---

## 十六、网络监控与防睡眠

<table>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold; width:150px;">功能名称</td>
<td><strong>网络监控与防睡眠</strong></td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">功能用途</td>
<td>
1. 实时监控网络连通性<br>
2. 网络断开时自动暂停任务<br>
3. 防止系统进入睡眠状态影响任务执行
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">使用方法</td>
<td>
1. 系统自动启用网络监控（每5秒检测一次）<br>
2. 检测到网络断开会自动暂停正在运行的任务<br>
3. 网络恢复后可以手动恢复任务<br>
4. 程序运行时自动阻止系统睡眠
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">常见问题</td>
<td>
<strong>Q: 网络断开后任务数据会丢失吗？</strong><br>
A: 不会，系统会自动暂停任务，已处理的数据会保留。<br><br>
<strong>Q: 如何关闭防睡眠功能？</strong><br>
A: 防睡眠功能在程序退出时自动关闭，不影响系统正常睡眠设置。
</td>
</tr>
</table>


---

## 十七、存储信息查看

<table>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold; width:150px;">功能名称</td>
<td><strong>存储信息查看</strong></td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">功能用途</td>
<td>
1. 查看数据存储目录位置<br>
2. 了解各类型文件的存储路径<br>
3. 便于文件管理和备份
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">使用方法</td>
<td>
1. 在系统设置中点击"存储信息"<br>
2. 查看各类数据的存储路径：<br>
   - 任务目录：存储任务配置和结果<br>
   - 日志目录：存储运行日志<br>
   - 模板目录：存储API和Prompt模板
</td>
</tr>
<tr>
<td style="background-color:#f0f0f0; font-weight:bold;">常见问题</td>
<td>
<strong>Q: 可以修改存储目录吗？</strong><br>
A: 可以通过设置环境变量 ANALYZER_DATA_DIR 来修改存储目录。<br><br>
<strong>Q: 默认存储在哪里？</strong><br>
A: 默认存储在 C:\QualityAnalyzerData 目录下。
</td>
</tr>
</table>

---

## 附录：环境变量说明

| 环境变量 | 说明 | 默认值 |
|---------|------|--------|
| ANALYZER_DATA_DIR | 数据存储目录 | C:\QualityAnalyzerData |

---

## 附录：默认端口

程序默认运行端口：**5000**（如果5000被占用，会自动尝试5001-5029）

访问地址：http://localhost:5000

---

## 附录：支持的API提供商

| 提供商类型 | 说明 | 相关参数 |
|-----------|------|---------|
| OpenAI | OpenAI兼容接口 | API Key、Model、Base URL |
| Bella Agent | Bella智能体 | Token、Base URL、Application ID、UCID、Thread ID |
| Bella 工作流 | Bella工作流 | Token、Base URL、Workflow ID、Tenant ID、User ID |

---

*文档生成完成，如有疑问请联系系统管理员*
