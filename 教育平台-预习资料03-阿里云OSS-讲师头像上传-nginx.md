# 一、阿里云OSS

## 1.1 对象存储OSS

为了解决海量数据存储与弹性扩容，项目中我们采用云存储的解决方案- 阿里云OSS。 

### 1.1.1 开通"对象存储OSS"服务

#### （1）申请阿里云账号

  网址：https://www.aliyun.com/

  注册，建议使用支付宝注册，方便实名认证

  注册后用自己注册的账号登录。

  登录后如下图

![image-20210528155210445](C:\Users\qiaoxin\Desktop\temp\教育平台-图\登录2.png)

点击红框--后

![image-20210528155323452](C:\Users\qiaoxin\Desktop\temp\教育平台-图\登录3png)

账号中要充值 【不需要多充，2毛就够了】



#### （2）实名认证

![image-20210528154104503](C:\Users\qiaoxin\Desktop\temp\教育平台-图\认证1.png)

![image-20210528154016261](C:\Users\qiaoxin\Desktop\temp\教育平台-图\认证2.png)

![image-20210528154337878](C:\Users\qiaoxin\Desktop\temp\教育平台-图\认证3.png)

#### （3）开通“对象存储OSS”服务

![image-20210528155914309](C:\Users\qiaoxin\Desktop\temp\教育平台-图\OSS1.png)

**点击进入 对象存储OSS**

![image-20210528160050212](C:\Users\qiaoxin\Desktop\temp\教育平台-图\OSS2.png)

**点击立即开通**

![image-20210528160425350](C:\Users\qiaoxin\Desktop\temp\教育平台-图\OSS4.png)



#### （4）进入管理控制台

### 1.1.2 创建Bucket

相当于创建文件夹

![image-20210528162124853](C:\Users\qiaoxin\Desktop\temp\教育平台-图\bucket1.png)

点击 【创建Bucket】

![image-20210528162342750](C:\Users\qiaoxin\Desktop\temp\教育平台-图\bucket2.png)

按上图选择 Bucket项

![image-20210528162739506](C:\Users\qiaoxin\Desktop\temp\教育平台-图\bucket3.png)



可以上传图片

![image-20210528163639585](C:\Users\qiaoxin\Desktop\temp\教育平台-图\bucket4.png)



实际公司中，普通员工没有这个操作阿里后台的权限。

## 1.2 阿里云开发准备

### 准备工作

需要阿里颁发的**id和密钥**

![image-20210528194743478](C:\Users\qiaoxin\Desktop\temp\教育平台-图\常用入口.png)

点击上图中 【Access Key】

弹出下图，选择【继续使用AccessKey】

![image-20210528194902254](C:\Users\qiaoxin\Desktop\temp\教育平台-图\提示1.png)



![image-20210528195011243](C:\Users\qiaoxin\Desktop\temp\教育平台-图\选择1.png)

点击【创建AccessKey】

![image-20210528195143930](C:\Users\qiaoxin\Desktop\temp\教育平台-图\验证1.png)



点击获取 输入手机收到的校验码后 确定

![image-20210528195322005](C:\Users\qiaoxin\Desktop\temp\教育平台-图\确定1.png)

![image-20210528195452538](C:\Users\qiaoxin\Desktop\temp\教育平台-图\AccessKey1.png)

### 文档位置

https://oss.console.aliyun.com/overview

![image-20210528195928187](C:\Users\qiaoxin\Desktop\temp\教育平台-图\学习路径.png)

**点击 OSS学习路径 **

![image-20210528200225597](C:\Users\qiaoxin\Desktop\temp\教育平台-图\SDK.png)

**点击Java SDK ** 可以进入文档

![image-20210528201241608](C:\Users\qiaoxin\Desktop\temp\教育平台-图\学习参考.png)



如何查看Bucket地域地址？

![image-20210528201506504](C:\Users\qiaoxin\Desktop\temp\教育平台-图\地址1.png)

![image-20210528201817682](C:\Users\qiaoxin\Desktop\temp\教育平台-图\地域.png)

## 1.3 讲师管理-上传讲师头像-后端环境搭建

### 1.3.1 新建模块并创建启动类

模块 service_oss

![image-20210529082556356](C:\Users\qiaoxin\Desktop\temp\教育平台-图\service_oss.png)

启动类

```java
package com.wxj.oss;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.ComponentScan;

@SpringBootApplication
@ComponentScan(basePackages = "com.wxj")
public class OssApplication {
    public static void main(String[] args) {
        SpringApplication.run(OssApplication.class,args);
    }
}
```

### 1.3.2 配置pom文件

```xml
<dependencies>
        <!-- 阿里云oss依赖 -->
        <dependency>
            <groupId>com.aliyun.oss</groupId>
            <artifactId>aliyun-sdk-oss</artifactId>
        </dependency>

        <!-- 日期工具栏依赖 -->
        <dependency>
            <groupId>joda-time</groupId>
            <artifactId>joda-time</artifactId>
        </dependency>
    </dependencies>
```

### 1.3.3 创建application.properties文件并配置

**注意其中的 阿里云OSS的配置需要改为自己在阿里云颁发的地址，ID，密钥，bucket名称**

```properties
#服务端口
server.port=8002
#服务名
spring.application.name=service-oss

#环境设置：dev、test、prod
spring.profiles.active=dev

#阿里云 OSS
#不同的服务器，地址不同
aliyun.oss.file.endpoint=oss-cn-beijing.aliyuncs.com
aliyun.oss.file.keyid=LTAI5tG7VNg5YRwj9vbRTW6t
aliyun.oss.file.keysecret=LOSeFvUkUOMgDz0ntAtoDnrJDzve8y
#bucket可以在控制台创建，也可以使用java代码创建
aliyun.oss.file.bucketname=yk1001
```

### 1.3.4 运行启动类

可能会报错

![image-20210529085027169](C:\Users\qiaoxin\Desktop\temp\教育平台-图\报错1.png)

原因：启动时默认找数据库信息，但现在的模块不需要连库

解决方式：

方式1.添加数据库连库信息

方式2.在启动类上加属性，默认不去加载数据库配置-建议使用

![image-20210529085340392](C:\Users\qiaoxin\Desktop\temp\教育平台-图\解决1.png)

## 1.4 上传讲师头像-常量类

创建常量类，读取配置文件内容 ConstantPropertiesUtils

```java
package com.wxj.oss.uitls;

import org.springframework.beans.factory.InitializingBean;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

//当项目启动，spring接口，spring加载后，执行接口中方法
@Component
public class ConstantPropertiesUtils implements InitializingBean {

    //读取配置文件内容
    @Value("${aliyun.oss.file.endpoint}")
   private String endpoint;
    @Value("${aliyun.oss.file.keyid}")
   private String keyId;
    @Value("${aliyun.oss.file.keysecret}")
   private String keySecret;
    @Value("${aliyun.oss.file.bucketname}")
   private String bucketName;

    //定义公开静态常量
    public static String END_POINT;
    public static String ACCESS_KEY_ID;
    public static String ACCESS_KEY_SECRET;
    public static String BUCKET_NAME;
    @Override
    public void afterPropertiesSet() throws Exception {
        END_POINT=endpoint;
        ACCESS_KEY_ID=keyId;
        ACCESS_KEY_SECRET=keySecret;
        BUCKET_NAME=bucketName;
    }
}

```

## 1.5 讲师头像-创建后端接口

**Controller**

```java
package com.wxj.oss.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/eduoss/fileoss")
@CrossOrigin
public class OssController {
}

```

**Service接口及实现类**

```java
package com.wxj.oss.service;

public interface OssService {
}

```

```java
package com.wxj.oss.service.impl;

import com.wxj.oss.service.OssService;
import org.springframework.stereotype.Service;

@Service
public class OssServiceImpl implements OssService {
}

```

## 1.6 讲师头像-后端接口实现

**Service接口**

```java
package com.wxj.oss.service;

import org.springframework.web.multipart.MultipartFile;

public interface OssService {
    //上传头像到oss
    String uploadFileAvater(MultipartFile file);
}
```

**控制器**

```java
package com.wxj.oss.controller;

import com.wxj.commonutils.R;
import com.wxj.oss.service.OssService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.multipart.MultipartFile;

@RestController
@RequestMapping("/eduoss/fileoss")
@CrossOrigin
public class OssController {

    @Autowired
    private OssService ossService;
    //上传头像
    @PostMapping
    public R uploadOssFile(MultipartFile file){
        //返回上传到oss的路径
        String url=ossService.uploadFileAvater(file);
        return R.ok().data("url",url);
    }
}
```

**Service接口实现类**

```java
package com.wxj.oss.service.impl;


import com.aliyun.oss.OSS;
import com.aliyun.oss.OSSClientBuilder;
import com.wxj.oss.service.OssService;
import com.wxj.oss.uitls.ConstantPropertiesUtils;
import org.joda.time.DateTime;
import org.springframework.stereotype.Service;
import org.springframework.web.multipart.MultipartFile;

import java.io.InputStream;
import java.util.UUID;

@Service
public class OssServiceImpl implements OssService {

    // 上传头像到oss
    @Override
    public String uploadFileAvater(MultipartFile file) {

        // 工具类获取值
        String endpoint = ConstantPropertiesUtils.END_POINT;
        String accessKeyId = ConstantPropertiesUtils.ACCESS_KEY_ID;
        String accessKeySecret = ConstantPropertiesUtils.ACCESS_KEY_SECRET;
        String bucketName = ConstantPropertiesUtils.BUCKET_NAME;

        try {
            // 创建OSS实例。
            OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

            //获取上传文件输入流
            InputStream inputStream = file.getInputStream();
            //获取文件名称
            String fileName = file.getOriginalFilename();

            //1 在文件名称里面添加随机唯一的值
            String uuid = UUID.randomUUID().toString().replaceAll("-","");
            // yuy76t5rew01.jpg
            fileName = uuid+fileName;

            //2 把文件按照日期进行分类
            //获取当前日期
            //   2019/11/12
            String datePath = new DateTime().toString("yyyy/MM/dd");
            //拼接
            //  20201/11/12/ewtqr313401.jpg
            fileName = datePath+"/"+fileName;

            //调用oss方法实现上传
            //第一个参数  Bucket名称
            //第二个参数  上传到oss文件路径和文件名称   aa/bb/1.jpg
            //第三个参数  上传文件输入流
            ossClient.putObject(bucketName,fileName , inputStream);

            // 关闭OSSClient。
            ossClient.shutdown();

            //把上传之后文件路径返回
            //需要把上传到阿里云oss路径手动拼接出来
            //  https://yk1001.oss-cn-beijing.aliyuncs.com/01.jpg
            String url = "https://"+bucketName+"."+endpoint+"/"+fileName;
            return url;
        }catch(Exception e) {
            e.printStackTrace();
            return null;
        }
    }
}

```

## 1.7swagger测试

http://localhost:8002/swagger-ui.html

测试之后，阿里云存储OSS上查看上传的图片

# 二、nginx

## 2.1内容回顾

反向代理服务器

请求转发，负载均衡，动静分离

1、启动：

C:\server\nginx-1.0.2>start nginx

或

C:\server\nginx-1.0.2>nginx.exe

注：建议使用第一种，第二种会使你的cmd窗口一直处于执行中，不能进行其他命令操作。

如果需要特殊设置nginx的配置文件路径，可以这样执行start nginx -c conf/nginx.conf

2、停止：

C:\server\nginx-1.0.2>nginx.exe -s stop

或

C:\server\nginx-1.0.2>nginx.exe -s quit

注：stop是快速停止nginx，可能并不保存相关信息；quit是完整有序的停止nginx，并保存相关信息。

3、杀死nginx命令

taskkill /f /im nginx.exe

## 2.2 配置

### 2.2.1 修改默认端口为81

![image-20210529104232567](C:\Users\qiaoxin\Desktop\temp\教育平台-图\81.png)

### 2.2.2 配置转发规则

![image-20210529105143784](C:\Users\qiaoxin\Desktop\temp\教育平台-图\转发规则.png)

```properties
server {
        listen       9001;
        server_name  localhost;
		
		location ~ /eduservice/	{
			proxy_pass http://localhost:8001;
		}
		location ~ /eduoss/	{
			proxy_pass http://localhost:8002;
		}
        
    }
```

注意配置在 conf-->nginx.conf文件的http内部

nginx配置修改完后，要先停止，再启动

## 2.3 修改前端地址

修改前端请求地址为nginx地址

![image-20210529105612083](C:\Users\qiaoxin\Desktop\temp\教育平台-图\前端地址修改.png)

# 2.4 测试

分别启动 后端8001，8002两个服务，前端也启动

前端能正常访问说明配置正确。

# 三、讲师管理-上传讲师头像-前端实现

## 3.1 复制上传组件

![image-20210529112612218](C:\Users\qiaoxin\Desktop\temp\教育平台-图\上传组件.png)

把这两个组件复制到前端src-->components文件夹下

## 3.2 页面中增加组件

### 3.2.1 save页面中增加上传组件

![image-20210529113050612](C:\Users\qiaoxin\Desktop\temp\教育平台-图\组件.png)

```vue
<!-- 讲师头像 -->
      <el-form-item label="讲师头像">

          <!-- 头衔缩略图 -->
          <pan-thumb :image="teacher.avatar"/>
          <!-- 文件上传按钮 -->
          <el-button type="primary" icon="el-icon-upload" @click="imagecropperShow=true">更换头像
          </el-button>

          <!--
      v-show：是否显示上传组件
      :key：类似于id，如果一个页面多个图片上传控件，可以做区分
      :url：后台上传的url地址
      @close：关闭上传组件
      @crop-upload-success：上传成功后的回调 
        <input type="file" name="file"/>
      -->
          <image-cropper
                        v-show="imagecropperShow"
                        :width="300"
                        :height="300"
                        :key="imagecropperKey"
                        :url="BASE_API+'/eduoss/fileoss'"
                        field="file"
                        @close="close"
                        @crop-upload-success="cropSuccess"/>
      </el-form-item>
```



### 3.2.2 save页面中 data中增加上传组件配置

```vue
 //上传弹框组件是否显示
      imagecropperShow:false,
      imagecropperKey:0,//上传组件key值
      BASE_API:process.env.BASE_API, //获取dev.env.js里面地址
```

![image-20210529114421305](C:\Users\qiaoxin\Desktop\temp\教育平台-图\配置1.png)



### 3.2.3 methods中增加方法

```javascript
 close() { //关闭上传弹框的方法
        this.imagecropperShow=false
        //上传组件初始化
        this.imagecropperKey = this.imagecropperKey+1
    },
    //上传成功方法
    cropSuccess(data) {
      this.imagecropperShow=false
      //上传之后接口返回图片地址
      this.teacher.avatar = data.url
      this.imagecropperKey = this.imagecropperKey+1
    },
```

如下图：

![image-20210529114617198](C:\Users\qiaoxin\Desktop\temp\教育平台-图\method方法增加.png)

### 3.2.4 引入组件

```javascript
import ImageCropper from '@/components/ImageCropper'
import PanThumb from '@/components/PanThumb'
```

### 3.2.5 增加组件声明

```javascript
components: { ImageCropper, PanThumb },
```

![image-20210529115120472](C:\Users\qiaoxin\Desktop\temp\教育平台-图\组件声明.png)

### 3.2.6 修改上传组件url

![image-20210529122719237](C:\Users\qiaoxin\Desktop\temp\教育平台-图\上传地址.png)

其中url 单引号中的地址为 控制器OssController的请求路径

BASE_API为前端修改好的请求地址--访问nginx的地址

### 3.2.7 测试

分别启动后端两服务、前端、nginx 进行测，可以直接测试增加讲师方法

测试结束，可以查看数据库中头像的路径





