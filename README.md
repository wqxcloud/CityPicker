<p align="center">
<img src="art/header.png">
</p>

# CityPicker

[![Platform](https://img.shields.io/badge/platform-android-green.svg)](http://developer.android.com/index.html) [![API](https://img.shields.io/badge/API-14%2B-yellow.svg?style=flat)](https://android-arsenal.com/api?level=14)

现在使用较多的类似美团、外卖等APP的城市选择界面，**一行代码搞定**，就是这么简单粗暴！！！

#### 主要功能：

-   字母悬浮栏
-   指定热门城市
-   自定义动画效果
-   自定义主题
-   名称或拼音搜索
-   返回城市名、code等数据
-   提供定位接口，解耦定位SDK

# Preview

![image](https://github.com/zaaach/CityPicker/raw/master/art/screen.gif) ![image](https://github.com/zaaach/CityPicker/raw/master/art/screen1.gif)
![image](https://github.com/zaaach/CityPicker/raw/master/art/screen2.gif) ![image](https://github.com/zaaach/CityPicker/raw/master/art/screen3.gif)

# APK

下载[demo.apk](https://github.com/zaaach/CityPicker/raw/master/art/demo.apk)体验.

# Install

Gradle:

```groovy
implementation 'com.zaaach:citypicker:2.0.1'
```

or Maven:

```xml
<dependency>
  <groupId>com.zaaach</groupId>
  <artifactId>citypicker</artifactId>
  <version>2.0.1</version>
  <type>pom</type>
</dependency>
```

or 下载library手动导入.

# Usage

`CityPicker` 基于`DialogFragment` 实现，已提供定位接口，需要APP自身实现定位。

### 基本使用：

#### Step1:

在`manifest.xml`中给使用`CityPicker` 的`activity`添加主题`android:theme="@style/DefaultCityPickerTheme"`

```xml
<activity android:name=".MainActivity" android:theme="@style/DefaultCityPickerTheme">
  ......
</activity>
```

#### Step2:

注意：热门城市使用`HotCity` ，定位城市使用`LocatedCity` 

```java
List<HotCity> hotCities = new ArrayList<>();
hotCities.add(new HotCity("北京", "北京", "101010100"));
hotCities.add(new HotCity("上海", "上海", "101020100"));
hotCities.add(new HotCity("广州", "广东", "101280101"));
hotCities.add(new HotCity("深圳", "广东", "101280601"));
hotCities.add(new HotCity("杭州", "浙江", "101210101"));
......

CityPicker.getInstance()
  .setFragmentManager(getSupportFragmentManager())	//此方法必须调用
  .enableAnimation(true)	//启用动画效果
  .setAnimationStyle(anim)	//自定义动画
  .setLocatedCity(new LocatedCity("杭州", "浙江", "101210101")))  //APP自身已定位的城市，默认为null（定位失败）
  .setHotCities(hotCities)	//指定热门城市
  .setOnPickListener(new OnPickListener() {
    @Override
    public void onPick(int position, City data) {
      Toast.makeText(getApplicationContext(), data.getName(), Toast.LENGTH_SHORT).show();
    }
      
    @Override
    public void onLocate() {
      //开始定位，这里模拟一下定位
      new Handler().postDelayed(new Runnable() {
        @Override
        public void run() {
          //定位完成之后更新数据
          CityPicker.getInstance()
            .locateComplete(new LocatedCity("深圳", "广东", "101280601"), LocateState.SUCCESS);
        }
      }, 2000);
    }
  })
  .show();
```

### 关于自定义主题：

在`style.xml` 中自定义主题并且继承`DefaultCityPickerTheme` ，别忘了在`manifest.xml` 设置给`activity`。

```xml
<style name="CustomTheme" parent="DefaultCityPickerTheme">
        <item name="cpCancelTextColor">@color/color_green</item>
        <item name="cpSearchCursorDrawable">@color/color_green</item>
        <item name="cpIndexBarNormalTextColor">@color/color_green</item>
        <item name="cpIndexBarSelectedTextColor">@color/color_green</item>
        <item name="cpSectionHeight">@dimen/custom_section_height</item>
        <item name="cpOverlayBackground">@color/color_green</item>
  		......
</style>
```

`CityPicker` 中自定义的所有属性如下，有些属性值必须是引用类型`refrence`，使用时注意。

```xml
<resources>
    <attr name="cpCancelTextSize" format="dimension|reference" />
    <attr name="cpCancelTextColor" format="color|reference" />

    <attr name="cpClearTextIcon" format="reference" />
    <attr name="cpSearchTextSize" format="dimension|reference" />
    <attr name="cpSearchTextColor" format="color|reference" />
    <attr name="cpSearchHintText" format="string|reference" />
    <attr name="cpSearchHintTextColor" format="color|reference" />
    <attr name="cpSearchCursorDrawable" format="reference" />

    <attr name="cpListItemTextSize" format="dimension|reference" />
    <attr name="cpListItemTextColor" format="color|reference" />
    <attr name="cpListItemHeight" format="dimension|reference"/>

    <attr name="cpEmptyIcon" format="reference"/>
    <attr name="cpEmptyIconWidth" format="dimension|reference"/>
    <attr name="cpEmptyIconHeight" format="dimension|reference"/>
    <attr name="cpEmptyText" format="string|reference"/>
    <attr name="cpEmptyTextSize" format="dimension|reference"/>
    <attr name="cpEmptyTextColor" format="color|reference"/>

    <attr name="cpGridItemBackground" format="color|reference"/>
    <attr name="cpGridItemSpace" format="reference"/>
	<!--悬浮栏-->
    <attr name="cpSectionHeight" format="reference"/>
    <attr name="cpSectionTextSize" format="reference" />
    <attr name="cpSectionTextColor" format="reference" />
    <attr name="cpSectionBackground" format="reference" />

    <attr name="cpIndexBarTextSize" format="reference" />
    <attr name="cpIndexBarNormalTextColor" format="reference" />
    <attr name="cpIndexBarSelectedTextColor" format="reference" />
	<!--特写布局-->
    <attr name="cpOverlayWidth" format="dimension|reference"/>
    <attr name="cpOverlayHeight" format="dimension|reference"/>
    <attr name="cpOverlayTextSize" format="dimension|reference"/>
    <attr name="cpOverlayTextColor" format="color|reference"/>
    <attr name="cpOverlayBackground" format="color|reference"/>
</resources>
```

OK，enjoy it~

# Changelog 

#### v2.0.1

-   新增定位接口
-   修改返回类型为`City` ，可获取城市名、code等数据

#### v2.0.0

-   项目重构优化，结构更清晰
-   使用RecyclerView

# 赞赏

如果你喜欢 **CityPicker** 的设计，觉得帮助到了你，可以点右上角 "Star" 支持一下，谢谢！(〃'▽'〃)

你也还可以扫描下面的二维码，请作者喝一杯咖啡~

![image](https://github.com/zaaach/CityPicker/raw/master/art/alipay.png) ![image](https://github.com/zaaach/CityPicker/raw/master/art/wxpay.png)

如果在捐赠留言中备注名称，将会被记录到列表中~ 捐赠时可以留下你的github项目地址或个人主页地址，链接将添加到列表中起到互相推广的作用。

#### [捐赠列表]( https://github.com/zaaach/CityPicker/blob/master/art/donationlist.md)