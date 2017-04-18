---
layout:     post
title:      "Android Code Standard"
subtitle:   "Android 代码规范"
date:       2017-01-19
author:     "StuartHua"
header-img: "img/post-bg-2017.jpg"
catalog: true
tags:
    - Notes
    - Android
    - code standard
---

# Android Code Standard

编码规范在团队协作项目中十分重要，符合规范的代码可以让人简单快速地理解代码的意图。

## 通用规范

首先，界定一些通用规范

* 建议使用Unix系列环境进行开发
* 源文件编码格式为 `UTF-8`
* 把密码和敏感数据放在 `gradle.properties`
* layout中使用tools命名空间，便于布局预览
* 图片的.9处理
* 类注释 `AndroidStudio -> Setting -> Editor -> File and Code Templates -> Includes -> File Header`

```
#set( $USER = "XXXXXX" )
#set( $EMAIL = "XXXXXX@XXX.com" )
/**
 * User $USER
 * Date ${YEAR}-${MONTH}-${DAY}
 * Email $EMAIL
 */
```

* 引用第三方库要慎重，避免应用大容量的第三方库或者过多方法数的库
* strings.xml 中使用 %1$s 实现字符串的通配
* 数据一定要效验，例如字符型转数字型，如果转换失败一定要有缺省值
* 对于未完成的方法，使用 TODO 加以标记
* 若功能已完成，但存在效率等潜在问题时，使用 XXX 加以标记
* 若代码存在严重问题或仅用于调试，使用 FIXME 加以标记
* 如果 if 的条件大于2个，则必须写注释

## 命名规范

### 包/类或接口/方法

#### 包

1. 全部小写. 2个以内单词. 用”.”(点)分开 例如: com.baidu
2. 最好为 1 个单数名词
3. 所有项目的包都要以 `“com.[domain]”` 为父包

#### 类

1. 单个单词，首字母大写，例如: Constants，多个单词，每个单词的首字母大写，例如: WatchVideoActivity
2. 每个类都以功能性名词结尾，如xxxActivity,xxxFragment,xxxView,xxxAdapter

#### 接口

1. 单个单词，首字母大写，例如: `Login`
2. 多个单词，每个单词的首字母大写，例如: `LoginRegisterListener`
3. 如果接口表示一个事件，则应该取名为`OnXxxListener`
4. 如果表示一种能力，应以"able"、"ible"结尾，如`interface Runable`、`interface Accessible`

#### 方法

1. 单个单词, 全部小写, 例如: main() 多个单词, 从第二个单词开始, 每个单词的首字母大写. setPersonalData()
2. 继承的方法可以省略注释，但是被继承方法必须有注释

### 变量/常量

#### 变量

1. 如果是集合或数组，用复数名词，如pets,shops
2. 临时变量以小写字母开头 比如 private String userName;

##### 成员变量

1. 以m开头，后面首字母大写(如Button mBackButton; String mName)
2. 能private就不要default，能default就不要protected，最好不要public

##### 局部变量

1. 按照驼峰命名法, 第一个单词小写, 其他首字母大写
2. 名要尽量短，推荐用缩写 比如 StringBuilder sb

##### 静态成员变量

1. 以s开头 比如 `private static String sUserName;`

#### 常量

1. 使用全大写字母加下划线的方式命名 比如 `public static final String TAG = "tag";`

### 注释

1. 注释必须和代码保持同步
2. 示例代码以 `<PRE></PRE>` 包裹
3. 注释需要换行，用 `<BR>` 标签
4. 注释中有多个章节，用 `<H2>` 标签声明每个章节的标题

### Android 组件命名

对于 `Activity/Fragment/View/数据库/组件`，参照：

1. Activity: 逻辑名称 + Activity, 例如: LoginActivity
2. Fragment: 逻辑名称 + Fragment, 例如: MyPageFragment
3. View: 逻辑名称 + View, 例如: ToggleButton
4. Database: 逻辑名称 + DB, 例如: LabelSearchDB
5. BroadcastReceiver: 逻辑名称 + Receiver, 例如: LoginSuccessExistReceiver

### Layout 命名

#### .xml 文件命名

尽量以结构开头，业务结尾，中间用下划线区分

1. activity layout: activity_shop
2. fragment layout: fragment_shop
3. dialog layout: dialog_shop
4. 列表项 layout: list_item_shop
5. 包含项 layout: include_shop
6. adapter的子布局: item_shop
7. widget layout: widget_shop

#### 控件 id 命名

View的缩写 + “” + (模块名称 + “” +) View的逻辑名称, (全部小写)

1. ImageView iv_title_back;
2. TextView tv_edit_proson_info

View 的缩写：

1. LinearLayout: ll
2. RelativeLayout: rl
3. FramenLayout: fl
4. TextView: tv
5. ImageView: iv
6. ImageButton: ib
7. Button: btn

### Drawable 命名

全部小写，采用下划线命名法，加前缀区分，如

1. 按钮	 btn_main_home.png
2. 分割线 div_maket_white.png
3. 图标	 ic_edit.png
4. 背景 bg_main.png
5. 默认	 def_user_head_portrait.png
6. 颜色选择器 selector_menu_search.xml
7. Shape图形 shape_setting_item_norml.xml

如果有多种形态，如按钮等：

1. btn_xx 按钮图片使用 btn_整体效果
2. btn_xx_normal 按钮图片使用 btn_正常情况效果
3. btn_xx_pressed 按钮图片使用 btn_点击时候效果
4. btn_xx_disabled state_enabled (false) 不可用效果
5. btn_xx_checked state_checked 选中效果
6. btn_xx_selected state_selected 选中效果

### Colors 的命名

前缀{控件}{范围}{_后缀}，控件、范围、后缀可选，但控件和范围至少要有一个：

1. 背景颜色，添加 bg 前缀
2. 文本颜色，添加 text 前缀
3. 分割线颜色，添加 div 前缀
4. 区分状态时，默认状态的颜色，添加 normal 后缀
5. 区分状态时，按下时的颜色，添加 pressed 后缀
6. 区分状态时，选中时的颜色，添加 selected 后缀
7. 区分状态时，不可用时的颜色，添加 disable 后缀

### Strings 命名

几种常用的命名：

1. 页面标题，命名格式为：title_页面
2. 按钮文字，命名格式为：btn_按钮事件
3. 标签文字，命名格式为：label_标签文字
4. 选项卡文字，命名格式为：tab_选项卡文字
5. 消息框文字，命名格式为：toast_消息
6. 编辑框的提示文字，命名格式为：hint_提示信息
7. 图片的描述文字，命名格式为：desc_图片文字
8. 对话框的文字，命名格式为：dialog_文字
9. menu的item文字，命名格式为：action_文字

### Animation 命名

1. 全部小写，采用下划线命名法，加前缀区分

如： fade_in、fade_out、push_down_in、push_down_out

## Reference

* [Android 开发规范](http://www.jianshu.com/p/59c0b0e38231)
* [Android 代码规范参考文档](https://www.gitbook.com/book/shibin/android-codeformat/details)
* [Android代码规范-命名规范](https://segmentfault.com/a/1190000005893399)


