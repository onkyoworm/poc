##Poc of Directory Traversal in LaobanCMS V2.0
###Precondition
Do not need to login the cms, make sure the file `xxx.com/install/mysql_hy.php` is exists (http code is 200) then the Vulnerability can be use.</br>
###Details
In `install/mysql_hy.php`, the developer didn't filter the parameter `$riqi` and `$i` before using.</br>
See the pic below</br>
![](https://i.imgur.com/yN8eML5.png)</br>
In line 6, we can see the parameter `$riqi` is join with `../data/` as the result of parameter `$sql_path`.</br>
In line 7, the `$sql_path` use as the parameter of PHP function `scandir()`</br>
In php, `scandir()` is used to list files and directories inside the specified path, and return an array of files and directories from the directory.</br>
In line 9, the `$i` in `$sqls[$i]` is defined in line 5, and the attacker can control it. And the result of `$sqls[$i]` is given to `$table`.</br>
In line 11, you can see, the parameter `$table` is echo directly.
###construction of poc
the poc use two parameter in the url requesting arguments towards `install/mysql_hy.php`</br>
**riqi** --> **the directory you want to attack**</br>
**i** --> **the index use in php function scandir()**</br>
In the picture below, i use `../../../../../../` as `riqi` and `3 - 8` as the index `i`. And my test server is ubuntu, so, you can see the folder name of the parent path.(like boot, boot, dev, etc)</br> 
![](https://i.imgur.com/Td0KCig.png)
Below is test in windows</br>
![](https://i.imgur.com/aBgO5Sb.png)