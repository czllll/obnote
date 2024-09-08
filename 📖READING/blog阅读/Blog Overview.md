```dataview
table file.ctime as "File Day"
from #blog-reading 
where file.name != "Blog Reading Template"
sort file.mtime desc
```
