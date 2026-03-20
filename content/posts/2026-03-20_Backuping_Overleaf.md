---
title: "Backup Overleaf"
date: 2026-03-20
tags: [backup, git]
---

To get the `git clone`-commands for all of your Overleaf projects, you can execute the following snippet in the Javascript console on the Overleaf web page. **Note,** you should press the button *Show me all projects*.

```javascript 
let script = ""
for ( a of  document.querySelectorAll("td.dash-cell-name > a"))  {
    let name = a.textContent
    let id = a.href.replace("https://www.overleaf.com/project/", "")
    script = script + "\n"+ ("git clone https://git@git.overleaf.com/"+id+" \""+name+"\"" )
}
```

The result is written into the log. 

