---
layout:     post
title:      macOS-automaker-初次尝试
subtitle:   批量ppt转换为pdf
date:       2019-11-28
author:     枸杞蒲蒻
header-img: 
catalog: true
tags:
    - macOS
    - automaker
    
---

## 批量转换ppt为pdf流程

automaker->文件和文件夹->获取制定的访达项目

实用工具->运行applescript

```bash
on run {input, parameters}
	set theOutput to {}
	tell application "Microsoft PowerPoint" -- work on version 15.15 or newer
		launch
		set theDial to start up dialog
		set start up dialog to false
		repeat with i in input
			open i
			set pdfPath to my makeNewPath(i)
			save active presentation in pdfPath as save as PDF -- save in same folder
			close active presentation saving no
			set end of theOutput to pdfPath as alias
		end repeat
		set start up dialog to theDial
	end tell
	return theOutput
end run

on makeNewPath(f)
	set t to f as string
	if t ends with ".pptx" then
		return (text 1 thru -5 of t) & "pdf"
	else
		return t & ".pdf"
	end if
end makeNewPath
```

完美！