# BaseTabStrip
![ICON](https://github.com/AlexMofer/ProjectX/blob/master/basetabstrip/icon.png)

继承自View，实现ViewPager.Decor接口（因其为包内私有接口，无法外部继承，故BaseTabStrip类必须存放在android.support.v4.view包中），版本兼容到API4(Android 1.6)，
其实可以去掉ViewPager.Decor接口，其目的是作为子View放入ViewPager，并能够达到自动捆绑ViewPager与显示于其内部的上方或者下方，BaseTabStrip为基础类，仅实现了一些通用基础逻辑。具体的实现效果需要实现与重写部分方法。
## 要求
- minSdkVersion 4
- com.android.support:support-v4:23.0.0(<24.0.0)

## 警告
不支持com.android.support:support-v4:24.0.0及以上版本，需要支持24.0.0，请使用最新版
## 引用
```java
dependencies {
    ⋯
    compile 'am.widget:basetabstrip:2.1.0'
    ⋯
}
```
## 使用
必须实现的方法（实现子类独有的效果）：
```java
    /**
     * 直接跳转到
     *
     * @param current 位置
     */
    protected abstract void jumpTo(int current);

    /**
     * 滑向左边
     *
     * @param current 当前页
     * @param next    目标页
     * @param offset  偏移
     */
    protected abstract void gotoLeft(int current, int next, float offset);

    /**
     * 滑向右边
     *
     * @param current 当前页
     * @param next    目标页
     * @param offset  偏移
     */
    protected abstract void gotoRight(int current, int next, float offset);
```
可重写的方法：
完成PagerAdapter绑定，但并未刷新界面及布局：
```java
    /**
     * 捆绑PagerAdapter
     */
    protected void onBindPagerAdapter() {

    }
```
若子类要实现点击事件，需要实现将点击的点转化为子项Position：
```java
    /**
     * 由触摸点转为Position
     *
     * @param x X坐标
     * @param y Y坐标
     * @return 坐标对应位置
     */
    protected int pointToPosition(float x, float y) {
        return 0;
    }
```
若设置了子项的Background，需要设置Background的hotspot，则需要重写：
```java
    /**
     * set hotspot's x location
     *
     * @param background 背景图
     * @param position   图片Position
     * @param motionX    点击位置X
     * @param motionY    点击位置Y
     * @return x location
     */
    protected float getHotspotX(Drawable background, int position, float motionX, float motionY) {
        return background.getIntrinsicWidth() * 0.5f;
    }

    /**
     * set hotspot's y location
     *
     * @param background 背景图
     * @param position   图片Position
     * @param motionX    点击位置X
     * @param motionY    点击位置Y
     * @return y location
     */
    protected float getHotspotY(Drawable background, int position, float motionX, float motionY) {
        return background.getIntrinsicHeight() * 0.5f;
    }
```
## 注意
- 如果子类实现子项点击事件，需要在构造方法中setItemClickable(ture)，且同时可设置点击时是否平滑滚动setClickSmoothScroll(boolean)；
- 可通过setItemBackground()来给子项设置背景，但背景并未在基类中绘画出来，因此需要在子类中自行将其绘制出来，如果不需要则可以忽略
- OnItemClickListener中实现了：单击、点击已选中子项、双击三种点击事件
- ItemTabAdapter为角标基本容器，需要实现角标，可以基于该接口
- 不支持com.android.support:support-v4:24.0.0及以上版本