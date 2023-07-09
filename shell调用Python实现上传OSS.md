## 脚本编写背景

开通了阿里的OSS，想要写一个脚本，实现能够根据我当前图片路径上传到OSS的对应路径，便于管理

如：本路路径为D:/A/B,则在OSS的路径也是D/A/B

shell调用Python,Python调用阿里的SDK,将上传后的url存储在剪贴板

不在shell里面echo出URL路径的原因是这样后Typora无法识别bash里的中文，会乱码，所以最后的URL只放在剪贴板手动粘贴

## shell 实现上传图片的脚本

```shell
sh "D:\MyFiles\Some program\Typora\upload_img.sh"
```

## upload_img.sh 文件内容如下：

```shell
#!/bin/bash
file_path='D:\PyShell\SDKts.py' arr=()
echo Upload Success:
for a in "$@"
do
python $file_path $a
done 
##exec /bin/bash
```

## SDKts.py 内容如下：

```python
# -*- coding: utf-8 -*-
import oss2
import  re

import pyperclip as cb



import os
import  sys
# 阿里云账号AccessKey拥有所有API的访问权限，风险很高。强烈建议您创建并使用RAM用户进行API访问或日常运维，请登录RAM控制台创建RAM用户。
auth = oss2.Auth('KEY_ID', 'KEY_Secret')
# yourEndpoint填写Bucket所在地域对应的Endpoint。以华东1（杭州）为例，Endpoint填写为https://oss-cn-hangzhou.aliyuncs.com。
# 填写Bucket名称。
bucket = oss2.Bucket(auth, 'https://oss-cn-shanghai.aliyuncs.com', '存储桶')




# # 上传文件。
# # 如果需要在上传文件时设置文件存储类型（x-oss-storage-class）和访问权限（x-oss-object-acl），请在put_object中设置相关Header。
# # headers = dict()
# # headers["x-oss-storage-class"] = "Standard"
# # headers["x-oss-object-acl"] = oss2.OBJECT_ACL_PRIVATE
# # 填写Object完整路径和字符串。Object完整路径中不能包含Bucket名称。
# # result = bucket.put_object('exampleobject.txt', 'Hello OSS', headers=headers)
# result = bucket.put_object('exampleobject.txt', 'Hello OSS')
#
# # HTTP返回码。
# print('http status: {0}'.format(result.status))
# # 请求ID。请求ID是本次请求的唯一标识，强烈建议在程序日志中添加此参数。
# print('request_id: {0}'.format(result.request_id))
# # ETag是put_object方法返回值特有的属性，用于标识一个Object的内容。
# print('ETag: {0}'.format(result.etag))
# # HTTP响应头部。
# print('date: {0}'.format(result.headers['date']))
# para ="D:\\collection\\1.png"
def main(para):
    # with open(para, 'rb') as fileobj:
    #     # Seek方法用于指定从第1000个字节位置开始读写。上传时会从您指定的第1000个字节位置开始上传，直到文件结束。
    #     fileobj.seek(1000, os.SEEK_SET)
    #     # Tell方法用于返回当前位置。
    #     current = fileobj.tell()
    #     # 填写Object完整路径。Object完整路径中不能包含Bucket名称。
    #     # bucket.put_object('img//img.png', fileobj)
    regex = re.compile(r"\\")
    name = para
    name = regex.sub('/', name)
    regex = re.compile(r"\:")
    name = regex.sub('', name)
    bucket.put_object_from_file('Typora_img/'+name , para)
    url = "http://存储桶.oss-cn-shanghai.aliyuncs.com/Typora_img/" + name
    # 这里的存储桶需要替换成自己的
    # print("Upload Success:\n")
    # print(url)
    # x="备份图片:   "+para+"\n"+"![ "+name+"]("+url+")"
    x = "![ " + name + "](" + url + ")"
    cb.copy(x)  # 复制到剪切板
    # print(cb.paste())  # 从剪切板粘贴(获取内容),并打印
    ##return url

    # 通过sys.argv获得入参
#
main (sys.argv[1])
```

