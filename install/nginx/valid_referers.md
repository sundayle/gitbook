
```
valid_referers none blocked *.sundayhk.com *.sundayle.com;
  if ($invalid_referer){   
	return 503;
   } 
```