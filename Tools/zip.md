# Kali

### · binwalk分析文件

利用binwalk可分析文件成分，分析其中组成

### · foremost分离文件

利用Linux下的foremost工具， foremost 图片名 如下，foremost默认的输出文件夹为output，在这个文件夹中可以找到分离出的zip（推荐使用这种方法，因为foremost还能分离出其他隐藏的文件）

```
root@kali:~# Downloads/meizi.bmp -o Downloads/meizi_output
Processing: Downloads/meizi.bmp
|foundat=tips.txt���ߤh� ��UљfƇI�}�d@s(b�Y����7޿�侧Z
                                                   ߀�PK?
*|
```

### · fcrackzip破解

针对之前解出的文件中的zip文件，可以使用fcrackzip破解密码，其中-b表示暴力破解，-c指定字符集，1表示纯数字，-l表示长度，-u显示出破解出的密码。

```
fcrackzip -b -c 1 -l 4 -u ~/Downloads/meizi_output/zip/00003375.zip
```

### · zip2john转换 + john破解

```
root@kali:~# zip2john Downloads/meizi_output/zip/00003375.zip > passhash
ver 2.0 00003375.zip/tips.txt PKZIP Encr: cmplen=52, decmplen=40, crc=C67CDA5F
```

```
root@kali:~# cat passhash 
00003375.zip/tips.txt:$pkzip2$1*1*2*0*34*28*c67cda5f*0*26*0*34*c67c*ae45*80e613b5dfa4688520fdb155d19966c6874910a47dbb64407328629059bb07e419b2ae020218133703debfaae4bea75a0cdf80f2*$/pkzip2$:tips.txt:00003375.zip::Downloads/meizi_output/zip/00003375.zip
```

```
root@kali:~# john passhash 
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 4 OpenMP threads
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, almost any other key for status
Warning: Only 4 candidates buffered for the current salt, minimum 8 needed for performance.
Warning: Only 6 candidates buffered for the current salt, minimum 8 needed for performance.
Almost done: Processing the remaining buffered candidate passwords, if any.
Warning: Only 3 candidates buffered for the current salt, minimum 8 needed for performance.
Proceeding with wordlist:/usr/share/john/password.lst, rules:Wordlist
Proceeding with incremental:ASCII
6666             (00003375.zip/tips.txt)
1g 0:00:00:23 DONE 3/3 (2021-07-22 04:38) 0.04336g/s 1830Kp/s 1830Kc/s 1830KC/s 054619944..6ox7
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

