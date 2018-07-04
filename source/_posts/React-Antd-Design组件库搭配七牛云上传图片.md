---
title: React Antd Design组件库搭配七牛云上传图片
abbrlink: 19118
date: 2018-04-17 15:50:58
tags:
---
### 注册七牛云
1. 常规地去注册七牛云，新建一个存储空间需要身份证认证。
2. 新建一个存储空间为：images。
3. 获取对应的上传凭证。[可以在这生成](http://jsfiddle.net/b0zt725o/3/)。
4. ak和sk在个人中心的密钥管理中找到。
5. bucketName就是存储空间名字。

<!--more-->

### Antd Design中的Upload组件
```javascript
class UploadDemo extends Components {
    this.state = {
        loading: false,
        imageUrl: '',
    }

    beforeUpload = (file) => {
        const isJPG = file.type === 'image/jpeg';
        if (!isJPG) {
            message.error('You can only upload JPG file!');
        }
        const isLt2M = file.size / 1024 / 1024 < 2;
        if (!isLt2M) {
            message.error('Image must smaller than 2MB!');
        }
        return isJPG && isLt2M;
    }

    handleAvatarChange = (info) => {
        if (info.file.status === 'uploading') {
            this.setState({ loading: true });
            return;
        }
        if (info.file.status === 'done') {
            this.getBase64(info.file.originFileObj, imageUrl => this.setState({
                imageUrl: `http://p7b9iw239.bkt.clouddn.com/${info.file.response.hash}`,
                loading: false,
            }));
        }
    }

    render() {
        const uploadData = {
            'token': '******'
        }
        return(
            <Upload
                name="file"
                className="avatar-uploader"
                showUploadList={false}
                action="https://upload-z2.qiniup.com"
                beforeUpload={this.beforeUpload}
                data={uploadData}
                onChange={this.handleAvatarChange}
            >
        )
    }
}
export default UploadDemo;
```
**说明：**  
以上是蚂蚁金服组件的基本使用。  
1. `beforeUpload` 可以对上传图片做一些限制，限制格式、大小等都在这个函数做处理。
2. `handleAvatarChange` 上传图片回调，在这里处理返回的数据。
3. Upload组件的name必须是file，否则服务器将200错误。
4. action为文件上传的api，对应要看自己建立的存储空间地区。具体看：[地区api](https://developer.qiniu.com/kodo/manual/1671/region-endpoint)。
5. 直白的在前端暴露上传凭证（`uploadData`）是不行的，时间不多，仅作为例子使用。正确应当在服务端处理。
6. handleAvatarChange 回调当 `info.file.status === 'done'` 就可以了拿到七牛云返回的数据，数据包含一个hash和一个key。  
7. 将hash和外链默认域名（七牛云-存储空间列表-内容管理）拼接就可以使用上传的图片外链了。