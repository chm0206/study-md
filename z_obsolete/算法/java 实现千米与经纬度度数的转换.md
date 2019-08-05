## java实现千米与经纬度度数的转换
```java
public class Tools {
	
	/**
     * 将公里数转换为度数
     * @param l
     * @return
     */
    public static Double km2Degree(Double l){
        //公式:l(弧长)=degree（圆心角）× π（圆周率）× r（半径）/180
        //转换后的公式：degree（圆心角）=l(弧长) × 180/(π（圆周率）× r（半径）)
        Double earthRadius = 6371.393;//地球半径:km
        Double degree = (180/earthRadius/Math.PI)*l;
        return degree;
    }
    /**
     * 将度数转换为公里数
     * @param degree
     * @return
     */
    public static Double degree2Km(Double degree){
        //公式:l(弧长)=degree（圆心角）× π（圆周率）× r（半径）/180
        Double earthRadius = 6371.393;//地球半径:km
        Double l = earthRadius/180*Math.PI*degree;//将180放在前面，降低数值
        return l;
    }
    public static void main(String[] args){//测试
        System.out.println("1公里="+km2Degree(1.0)+"度");//单位是公里
        System.out.println("1度="+degree2Km(1.0)+"公里");//单位是度
    }
}
```

结果
```
1公里=0.008992661340005603度
1度=111.20178578851908公里
```