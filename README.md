须知：本文档用于说明如何在已有项目中加入图聚蓝牙定位功能，如需其他功能请参考https://www.ipalmap.com/docs/

一、开发环境配置

1.创建待添加定位功能的Android工程

2.将图聚sdk中Nagrand-ble.jar拷贝到项目的libs
  将resources/lua文件夹下的所有内容拷贝到项目的SD卡目录下/Nagrand/
  将resources/media文件夹下的所有内容拷贝到项目的assets

3.添加AndroidManifest.xml添加权限等：
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_ALL_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-feature android:glEsVersion="0x00020000" android:required="true" />


4.初始化蓝牙定位

BeaconPositioningManager pm = new BeaconPositioningManager( // 蓝牙定位管理对象
            this,
            mapId, //输入mapId，用于下载特征数据库
            "AppKey", //输入AppKey
    );
注意：蓝牙定位还需要在AndroidManifest.xml添加一个service

<service android:name="com.palmaplus.nagrand.position.ble.BeaconService"></service>

5.设置定位回调
在完成BeaconPositioningManager的创建后我们需要监听一个位置改变的接口，以用来获取新的位置。
pm.setOnLocationChangeListener(
        new PositioningManager.OnLocationChangeListener<BleLocation>() {
          @Override
          public void onLocationChange(PositioningManager.LocationStatus status, BleLocation oldLocation, BleLocation newLocation) {

              	//每次获得新的位置会回调
	      switch (status) {
              case START:
                break;
              case MOVE:
                double x = newLocation==null ? 0 : newLocation.getPoint().getCoordinate().getX();
                double y = newLocation==null ? 0 : newLocation.getPoint().getCoordinate().getY();

                //获取到定位坐标，在此处调用第三方如（智慧图）的定位点显示方法
                //........
                //........

              default：
                break;
              }
    
            });
pm.start();

