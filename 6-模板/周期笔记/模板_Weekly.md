---
created: <% tp.date.now("YYYY-MM-DD") %>
updated: <% tp.date.now("YYYY-MM-DD") %>
type: Weekly
status: ⌛️ 等待
年度: <% tp.date.now("YYYY") %>
月份: <% tp.date.now("MM") %>
评分:
---
> [!notes]- 注意事项
> 1. 本模板==必须在周一==创建上一周的 Weekly 时模板的时间过滤器才生效
> 2. 浏览 4 个模块，将需要==查缺补漏==的任务添加到每周一
## Habit Tracking
```dataview
table without id 
	file.link as 日期,
	dateformat(created, "ccc") as 星期,评分,
	早起 AS 🌞早起,
	勤奋 AS 💪勤奋,
	健身 AS 🏃‍♂️健身,
	喝水 AS 💧喝水,
	早睡 AS 🌜早睡,
	写作 AS 📝写作,
	体重 AS 🏋🏻‍♂️体重
from "0-周期笔记"
where type = "Daily"
where created.weekyear = this.created.weekyear - 1
sort created asc
```
## Fleeting Notes
```dataview
table without id 
	file.link as 日期,
	dateformat(created, "ccc") as 星期, 
	L.text as 闪念笔记, 
	L.link as 链接
from "0-周期笔记"
where type = "Daily"
where created.weekyear = this.created.weekyear - 1
flatten file.lists as L
where
	!L.parent and
	meta(L.section).subpath = "Fleeting Notes"
```
## 本周完成的任务
```tasks
done
done on or after <% tp.date.now("YYYY-MM-DD",-7) %>
done on or before <% tp.date.now("YYYY-MM-DD",-1) %>
sort by done
hide edit button
group by done
```
## 本周新建的笔记
```dataview
table type,status
where created.weekyear = this.created.weekyear - 1
sort created asc
```