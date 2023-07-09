# Typora-upload-script
开通了阿里的OSS，想要写一个脚本，实现能够根据我当前图片路径上传到OSS的对应路径，便于管理  如：本路路径为D:/A/B,则在OSS的路径也是D/A/B  shell调用Python,Python调用阿里的SDK,将上传后的url存储在剪贴板  不在shell里面echo出URL路径的原因是这样后Typora无法识别bash里的中文，会乱码，所以最后的URL只放在剪贴板手动粘贴
