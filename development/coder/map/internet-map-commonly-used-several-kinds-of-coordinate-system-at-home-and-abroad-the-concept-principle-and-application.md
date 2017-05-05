# 国内外互联网地图常用的几种坐标系统：概念，原理和应用

国内外有几种不同的主流地图平台，Google地图，高德地图和百度地图等。这些地图平台都各自定义了自家地图使用的坐标系统。例如，Google Earth和非中国范围内的Google Map使用WGS84坐标系统，AMap使用GCJ-02坐标系统，百度使用自家的BD09坐标系统。在进行讲述各种坐标系统之间的转换之前，先讲解一下几个有可能被互联网地图工作者误解的重要概念。

（1）坐标系统：用于定位的系统，就跟二维笛卡尔坐标系统一样，一个点使用(x,y)，就能确定该点在笛卡尔坐标系统中的唯一位置。这里讲的坐标系统，相对于笛卡尔坐标系统，要复杂许多，但作用却都是一样，主要用于定位，也就是精确地定位地表上的一点。由于地球表面非常复杂，因此在对其定位时，首先要对其简化，第一步简化就是为地球建立其近似的几何形状（地理坐标系统），第二步就是如何将这个几何形状投影到一个平面上，生成平面地图，方便地供人们查看，这步就叫投影过程，其生成的坐标系统叫做投影坐标系统。简单地来说，地理坐标系统是投影坐标系统的第一步，尽管这样说也不一定准确。如果你对这个过程理解有困难，你可以想象，人头就是地球，而篮球就是对人头的简化，此时篮球的球形坐标相当于人头的地理坐标系统；在篮球下面放一张很大的纸，试图将篮球表面每点对应到纸张的一点，这个过程就是投影过程。你可能会发现很难或者不可能完美地将球面投影到纸张上，因为篮球是圆的，而纸张是平面的，无论采用哪种投影方式，只有球面的一部分能够完全很好地对应到纸张上去。这个恰好也就是为什么世界上存在那么多投影坐标系统的原因，因为每个国家或者地球都选择那种能将自己那块区域与纸张对应最完美的方式进行投影，也就是使用最适合于他们的投影坐标系统。

（2）地理坐标系统：WGS84就是一种地理坐标系统。地理坐标坐标是对地球进行简单几何建模，比如将地球看成一个球体或者类球体，然后再将地表上点投影到该球面上形成的坐标就是地理坐标系统。WGS84就是定义了如何将地球抽象成球体或者类球体的规则。或者简单地来说，WGS84就是一堆参数，用于建立球体或者类球体，来近似地球。

（3）投影坐标系统：就是如何将地理坐标投影到平面后的坐标系统。地理坐标系统一般使用经纬来表示，投影坐标系统有时候使用经纬度，有时候使用距离单位，例如米或者千米之类。投影坐标系统使用经纬度跟地理坐标系统的经纬度意义是一样的，就是球面经纬度在纸面上位置的映射。另外一方面，投影坐标系统使用距离单位时，是因为该投影坐标系统在前面讲的基础上，又选择了一个基准点。举例来说，假设A点投影坐标系统为（1,1）（北纬1度和东经1度），而B点投影坐标为（2,2）（北纬2度和东经2度），此时，我们可以将B点看成在A点的北方100千米和东边100千米处（地表1度大约100千米左右），此时B点的相对坐标是（100,100）。此时，B点使用的就是投影坐标系统中的平面坐标系统。你这样简单来理解投影坐标系统就可以了，否则太复杂了。因为地图投影这个东西太复杂了，一堆老毛子和西方人花了几百年时间才搞出来的东西。

（4）那为什么有地理坐标系统，还需要投影坐标系统呢？那是因为在早期时，人们都需要纸质地图，习惯看平面的东西。当然如果直接将地理坐标系统直接对应到纸张也可以，但是这样对应的坏处就是映射过来的形状都特别奇怪，以至于在现实世界中的一块圆形的农田，映射后变成平面地图上的一个奇怪的椭圆形。人们一看地图上的形状，反而跟真实世界的东西对应不上，此时地图不但没有帮助人们理解空间，反而起了误导作用。因此，人们就需要发明一套投影系统，让真实世界中圆形的农田，投影到地图上依然是圆形，让人们很容易将地图和真实世界联系起来。

（5）国内外主流地图平台的坐标系统：

WGS84 ：

地理坐标系统，Google Earth和中国外的Google Map使用，另外，目前基本上所有定位空间位置的设备都使用这种坐标系统，例如手机的GPS系统。

GCJ-02：

投影坐标系统，Google Map中国、高德和腾讯好像使用，这个是中国自己搞的，目的就是迷惑敌人，也就是说即使你能弄到中国地图，你也无法知道其对应                             的地理坐标，因此也无法对应到真实世界中的位置。当然这个有点自欺欺人，现在天上的卫星那么多，而且技术也那么发达。Google地图中国估计也是没有                              方法，才屈服的。

BD09：

投影坐标系统，百度地图使用，这个就不详说了，跟GCJ-02一样，就是参数不相同而已。

至于GCJ-02和BD09到底是地理坐标系统还是投影坐标系统?我猜是投影坐标系统，因为WGS84投影的地图看起来非常奇怪，而百度地图和高德地图看起来是非常舒                 服。

（6）坐标系统之间的转换。我从网络copy下来的(http://www.zgxue.com/162/1623461.html)，自己测试了一下觉得没有什么问题。

代码下载路径：http://pan.baidu.com/s/1eQwhH4m

    ProjectionUtil.Java  //管理转换函数类

    package MapProjection;

    /**
     * WGS84: Google Earth采用，Google Map中国范围外使用
     * GCJ02: 火星坐标系，中国国家测绘局制定的坐标系统，由WGS84机密后的坐标。Google Map中国和搜搜地图使用，高德
     * BD09:百度坐标，GCJ02机密后的坐标系
     * 搜狗坐标系，图吧坐标等，估计也是在GCJ02基础上加密而成的
     * @author luofx
     *
     */
    public class ProjectionUtils {

    public static final String BAIDU_LBS_TYPE = "bd09ll";
    public static double pi = 3.1415926535897932384626;
    public static double a = 6378245.0;
    public static double ee = 0.00669342162296594323;
     
         /**
          * 84 to 火星坐标系 (GCJ-02) World Geodetic System ==> Mars Geodetic System
           * 
           * @param lat
           * @param lon
          */
          public static Gps gps84_To_Gcj02(double lat, double lon) {
              if (outOfChina(lat, lon)) {
                 return null;
             }
    double dLat = transformLat(lon - 105.0, lat - 35.0);
    double dLon = transformLon(lon - 105.0, lat - 35.0);
    double radLat = lat / 180.0 * pi;
    double magic = Math.sin(radLat);
    magic = 1 - ee * magic * magic;
    double sqrtMagic = Math.sqrt(magic);
    dLat = (dLat * 180.0) / ((a * (1 - ee)) / (magic * sqrtMagic) * pi);
    dLon = (dLon * 180.0) / (a / sqrtMagic * Math.cos(radLat) * pi);
    double mgLat = lat + dLat;
    double mgLon = lon + dLon;
    return new Gps(mgLat, mgLon);
          }
     
         /**
          * * 火星坐标系 (GCJ-02) to 84 * * @param lon * @param lat * @return
          * */
         public static Gps gcj_To_Gps84(double lat, double lon) {
             Gps gps = transform(lat, lon);
             double lontitude = lon * 2 - gps.getLon();
             double latitude = lat * 2 - gps.getLat();
            return new Gps(latitude, lontitude);
         }
     
         /**
         * 火星坐标系 (GCJ-02) 与百度坐标系 (BD-09) 的转换算法 将 GCJ-02 坐标转换成 BD-09 坐标
          * 
          * @param gg_lat
          * @param gg_lon
         */
         public static Gps gcj02_To_Bd09(double gg_lat, double gg_lon) {
            double x = gg_lon, y = gg_lat;
             double z = Math.sqrt(x * x + y * y) + 0.00002 * Math.sin(y * pi);
             double theta = Math.atan2(y, x) + 0.000003 * Math.cos(x * pi);
             double bd_lon = z * Math.cos(theta) + 0.0065;
             double bd_lat = z * Math.sin(theta) + 0.006;
            return new Gps(bd_lat, bd_lon);
         }
     
         /**
          * * 火星坐标系 (GCJ-02) 与百度坐标系 (BD-09) 的转换算法 * * 将 BD-09 坐标转换成GCJ-02 坐标 * * @param
          * bd_lat * @param bd_lon * @return
          */
         public static Gps bd09_To_Gcj02(double bd_lat, double bd_lon) {
             double x = bd_lon - 0.0065, y = bd_lat - 0.006;
             double z = Math.sqrt(x * x + y * y) - 0.00002 * Math.sin(y * pi);
             double theta = Math.atan2(y, x) - 0.000003 * Math.cos(x * pi);
             double gg_lon = z * Math.cos(theta);
            double gg_lat = z * Math.sin(theta);
             return new Gps(gg_lat, gg_lon);
         }
     
         /**
          * (BD-09)-->84
         * @param bd_lat
          * @param bd_lon
          * @return
          */
         public static Gps bd09_To_Gps84(double bd_lat, double bd_lon) {
      
            Gps gcj02 = bd09_To_Gcj02(bd_lat, bd_lon);
             Gps map84 = gcj_To_Gps84(gcj02.getLat(),
                     gcj02.getLon());
             return map84;
     
         }
     
         public static boolean outOfChina(double lat, double lon) {
              if (lon < 72.004 || lon > 137.8347)
                  return true;
              if (lat < 0.8293 || lat > 55.8271)
                return true;
            return false;
         }
     
         public static Gps transform(double lat, double lon) {
             if (outOfChina(lat, lon)) {
                 return new Gps(lat, lon);
            }
            double dLat = transformLat(lon - 105.0, lat - 35.0);
             double dLon = transformLon(lon - 105.0, lat - 35.0);
             double radLat = lat / 180.0 * pi;
             double magic = Math.sin(radLat);
             magic = 1 - ee * magic * magic;
             double sqrtMagic = Math.sqrt(magic);
             dLat = (dLat * 180.0) / ((a * (1 - ee)) / (magic * sqrtMagic) * pi);
             dLon = (dLon * 180.0) / (a / sqrtMagic * Math.cos(radLat) * pi);
             double mgLat = lat + dLat;
             double mgLon = lon + dLon;
            return new Gps(mgLat, mgLon);
         }
     
         public static double transformLat(double x, double y) {
             double ret = -100.0 + 2.0 * x + 3.0 * y + 0.2 * y * y + 0.1 * x * y
                     + 0.2 * Math.sqrt(Math.abs(x));
            ret += (20.0 * Math.sin(6.0 * x * pi) + 20.0 * Math.sin(2.0 * x * pi)) * 2.0 / 3.0;
             ret += (20.0 * Math.sin(y * pi) + 40.0 * Math.sin(y / 3.0 * pi)) * 2.0 / 3.0;
           ret += (160.0 * Math.sin(y / 12.0 * pi) + 320 * Math.sin(y * pi / 30.0)) * 2.0 / 3.0;
             return ret;
         }


         public static double transformLon(double x, double y) {
             double ret = 300.0 + x + 2.0 * y + 0.1 * x * x + 0.1 * x * y + 0.1
                     * Math.sqrt(Math.abs(x));
             ret += (20.0 * Math.sin(6.0 * x * pi) + 20.0 * Math.sin(2.0 * x * pi)) * 2.0 / 3.0;
             ret += (20.0 * Math.sin(x * pi) + 40.0 * Math.sin(x / 3.0 * pi)) * 2.0 / 3.0;
             ret += (150.0 * Math.sin(x / 12.0 * pi) + 300.0 * Math.sin(x / 30.0
                   * pi)) * 2.0 / 3.0;
            return ret;
         }
     
         public static void main(String[] args) {
     
             // 北斗芯片获取的经纬度为WGS84地理坐标 31.426896,119.496145
             Gps gps = new Gps(31.426896, 119.496145);
             System.out.println("gps :" + gps);
            Gps gcj = gps84_To_Gcj02(gps.getLat(), gps.getLon());
            System.out.println("gcj :" + gcj);
             Gps star = gcj_To_Gps84(gcj.getLat(), gcj.getLon());
             System.out.println("star:" + star);
            Gps bd = gcj02_To_Bd09(gcj.getLat(), gcj.getLon());
             System.out.println("bd  :" + bd);
             Gps gcj2 = bd09_To_Gcj02(bd.getLat(), bd.getLon());
             System.out.println("gcj :" + gcj2);
         }
    }


    Gps.java //坐标对象，由经纬度构成

    package MapProjection;


    public class Gps {
    private double lat;
    private double lon;

    public Gps(double lat, double lon) {
    this.lat = lat;
    this.lon = lon;
    }

    public double getLat() {
    return lat;
    }
    public void setLat(double lat) {
    this.lat = lat;
    }
    public double getLon() {
    return lon;
    }
    public void setLon(double lon) {
    this.lon = lon;
    }
    public String toString() {
    return "lat:" + lat + "," + "lon:" + lon;
    }

    }
