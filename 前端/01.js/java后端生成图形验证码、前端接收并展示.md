# java后端生成图片验证码，前端接收并展示
## 一、工具类  

```java
import java.awt.Color;
import java.awt.Font;
import java.awt.Graphics;
import java.awt.Graphics2D;
import java.awt.RenderingHints;
import java.awt.geom.AffineTransform;
import java.awt.image.BufferedImage;
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStream;
import java.util.*;
import javax.imageio.ImageIO;
 public class VerifyCodeUtils{
    //使用到Algerian字体，系统里没有的话需要安装字体，字体只显示大写，去掉了1,0,i,o几个容易混淆的字符
    public static final String VERIFY_CODES = "23456789ABCDEFGHJKLMNPQRSTUVWXYZ";
    private static Random random = new Random();
    /**
     * 使用系统默认字符源生成验证码
     * @param verifySize    验证码长度
     * @return
     */
    public static String generateVerifyCode(int verifySize){
        return generateVerifyCode(verifySize, VERIFY_CODES);
    }
    /**
     * 使用指定源生成验证码
     * @param verifySize    验证码长度
     * @param sources   验证码字符源
     * @return
     */
    public static String generateVerifyCode(int verifySize, String sources){
        if(sources == null || sources.length() == 0){
            sources = VERIFY_CODES;
        }
        int codesLen = sources.length();
        Random rand = new Random(System.currentTimeMillis());
        StringBuilder verifyCode = new StringBuilder(verifySize);
        for(int i = 0; i < verifySize; i++){
            verifyCode.append(sources.charAt(rand.nextInt(codesLen-1)));
        }
        return verifyCode.toString();
    }
    /**
     * 生成随机验证码文件,并返回验证码值
     * @param w
     * @param h
     * @param outputFile
     * @param verifySize
     * @return
     * @throws IOException
     */
    public static String outputVerifyImage(int w, int h, File outputFile, int verifySize) throws IOException{
        String verifyCode = generateVerifyCode(verifySize);
        outputImage(w, h, outputFile, verifyCode);
        return verifyCode;
    }
    /**
     * 输出随机验证码图片流,并返回验证码值
     * @param w
     * @param h
     * @param os
     * @param verifySize
     * @return
     * @throws IOException
     */
    public static String outputVerifyImage(int w, int h, OutputStream os, int verifySize) throws IOException{
        String verifyCode = generateVerifyCode(verifySize);
        outputImage(w, h, os, verifyCode);
        return verifyCode;
    }
    /**
     * 生成指定验证码图像文件
     * @param w
     * @param h
     * @param outputFile
     * @param code
     * @throws IOException
     */
    public static void outputImage(int w, int h, File outputFile, String code) throws IOException{
        if(outputFile == null){
            return;
        }
        File dir = outputFile.getParentFile();
        if(!dir.exists()){
            dir.mkdirs();
        }
        try{
            outputFile.createNewFile();
            FileOutputStream fos = new FileOutputStream(outputFile);
            outputImage(w, h, fos, code);
            fos.close();
        } catch(IOException e){
            throw e;
        }
    }
    /**
     * 输出指定验证码图片流
     * @param w
     * @param h
     * @param os
     * @param code
     * @throws IOException
     */
    public static void outputImage(int w, int h, OutputStream os, String code) throws IOException{
        int verifySize = code.length();
        BufferedImage image = new BufferedImage(w, h, BufferedImage.TYPE_INT_RGB);
        Random rand = new Random();
        Graphics2D g2 = image.createGraphics();
        g2.setRenderingHint(RenderingHints.KEY_ANTIALIASING,RenderingHints.VALUE_ANTIALIAS_ON);
        Color[] colors = new Color[5];
        Color[] colorSpaces = new Color[] { Color.WHITE, Color.CYAN,
                Color.GRAY, Color.LIGHT_GRAY, Color.MAGENTA, Color.ORANGE,
                Color.PINK, Color.YELLOW };
        float[] fractions = new float[colors.length];
        for(int i = 0; i < colors.length; i++){
            colors[i] = colorSpaces[rand.nextInt(colorSpaces.length)];
            fractions[i] = rand.nextFloat();
        }
        Arrays.sort(fractions);
        g2.setColor(Color.GRAY);// 设置边框色
        g2.fillRect(0, 0, w, h);
        Color c = getRandColor(200, 250);
        g2.setColor(c);// 设置背景色
        g2.fillRect(0, 2, w, h-4);
        //绘制干扰线
        Random random = new Random();
        g2.setColor(getRandColor(160, 200));// 设置线条的颜色
        for (int i = 0; i < 20; i++) {
            int x = random.nextInt(w - 1);
            int y = random.nextInt(h - 1);
            int xl = random.nextInt(6) + 1;
            int yl = random.nextInt(12) + 1;
            g2.drawLine(x, y, x + xl + 40, y + yl + 20);
        }
        // 添加噪点
        float yawpRate = 0.05f;// 噪声率
        int area = (int) (yawpRate * w * h);
        for (int i = 0; i < area; i++) {
            int x = random.nextInt(w);
            int y = random.nextInt(h);
            int rgb = getRandomIntColor();
            image.setRGB(x, y, rgb);
        }
        shear(g2, w, h, c);// 使图片扭曲
        g2.setColor(getRandColor(100, 160));
        int fontSize = h-4;
        Font font = new Font("Algerian", Font.ITALIC, fontSize);
        g2.setFont(font);
        char[] chars = code.toCharArray();
        for(int i = 0; i < verifySize; i++){
            AffineTransform affine = new AffineTransform();
            affine.setToRotation(Math.PI / 4 * rand.nextDouble() * (rand.nextBoolean() ? 1 : -1), (w / verifySize) * i + fontSize/2, h/2);
            g2.setTransform(affine);
            g2.drawChars(chars, i, 1, ((w-10) / verifySize) * i + 5, h/2 + fontSize/2 - 10);
        }
        g2.dispose();
        ImageIO.write(image, "jpg", os);
    }
    /**
     * 随机生成字符 或 者数字
     * @return
     */
    private static String getRandom(){
        String value = "";
        Random random = new Random();
        int gen = random.nextInt(2);
        String charOrNum = gen % 2 ==0 ? "char":"num";
        if ("char".equals(charOrNum)) {
            //字符
            int temp = random.nextInt(2)%2==0?65:97;
            int ascii = random.nextInt(26);
            value += (char)(ascii + temp);
        }else if ("num".equals(charOrNum)) {
            //是数字
            value += String.valueOf(random.nextInt(10));
        }
        return value;
    }
    /**
     * 随机生成字符串（包含字符和数字）
     * @param length 指定长度
     * @return 返回set
     */
    private static Set<String> getStrAndNum(int length) {
        Set<String> set = new HashSet<>();
        for (int i = 0; i < length; i++) {
            String value = getRandom();
            set.add(value);
        }
        //若生成的字符串没达到指定长度 继续生成
        if (set.size() < length) {
            String value = getRandom();
            set.add(value);
        }
        return set;
    }
    /**
     * 存放在set中的字符组拼接成字符串
     * @param set
     * @return
     */
    private static  String printSet(Set set){ //打印set的方法
        Iterator iterator = set.iterator();
        String value = "";
        while (iterator.hasNext()) {
            //String ele = (String) iterator.next();
            value += (String)iterator.next();
        }
        return value;
    }
    /**
     * 返回生成的随机字符串
     * @param length 指定随机字符串长度
     * @return 指定长度 大于零 返回指定长度随机字符，小于等于零 返回null
     */
    public static String getRandonString(int length){
        String value= "";
        if (length > 0) {
            //如果返回的字符串小于指定长度 重新生成
            if (value.length() < length) {
                Set<String> store = getStrAndNum(length);
                value = printSet(store);
            }
            return value;
        }else{
            return value;
        }
    }
    private static Color getRandColor(int fc, int bc) {
        if (fc > 255)
            fc = 255;
        if (bc > 255)
            bc = 255;
        int r = fc + random.nextInt(bc - fc);
        int g = fc + random.nextInt(bc - fc);
        int b = fc + random.nextInt(bc - fc);
        return new Color(r, g, b);
    }
    private static int getRandomIntColor() {
        int[] rgb = getRandomRgb();
        int color = 0;
        for (int c : rgb) {
            color = color << 8;
            color = color | c;
        }
        return color;
    }
    private static int[] getRandomRgb() {
        int[] rgb = new int[3];
        for (int i = 0; i < 3; i++) {
            rgb[i] = random.nextInt(255);
        }
        return rgb;
    }
    private static void shear(Graphics g, int w1, int h1, Color color) {
        shearX(g, w1, h1, color);
        shearY(g, w1, h1, color);
    }
    private static void shearX(Graphics g, int w1, int h1, Color color) {
        int period = random.nextInt(2);
        boolean borderGap = true;
        int frames = 1;
        int phase = random.nextInt(2);
        for (int i = 0; i < h1; i++) {
            double d = (double) (period >> 1)
                    * Math.sin((double) i / (double) period
                            + (6.2831853071795862D * (double) phase)
                            / (double) frames);
            g.copyArea(0, i, w1, 1, (int) d, 0);
            if (borderGap) {
                g.setColor(color);
                g.drawLine((int) d, i, 0, i);
                g.drawLine((int) d + w1, i, w1, i);
            }
        }
    }
    private static void shearY(Graphics g, int w1, int h1, Color color) {
        int period = random.nextInt(40) + 10; // 50;
        boolean borderGap = true;
        int frames = 20;
        int phase = 7;
        for (int i = 0; i < w1; i++) {
            double d = (double) (period >> 1)
                    * Math.sin((double) i / (double) period
                            + (6.2831853071795862D * (double) phase)
                            / (double) frames);
            g.copyArea(i, 0, 1, h1, 0, (int) d);
            if (borderGap) {
                g.setColor(color);
                g.drawLine(i, (int) d, i, 0);
                g.drawLine(i, (int) d + h1, i, h1);
            }
        }
    }
}

```
## 二、 controller  

```java
	@GetMapping(value="/imgs")
    public void authImage(HttpServletRequest request, HttpServletResponse response) throws IOException {
        response.setHeader("Pragma", "No-cache");
        response.setHeader("Cache-Control", "no-cache");
        response.setDateHeader("Expires", 0);
        response.setContentType("image/jpeg");
        // 生成随机大写字串
        String verifyCode = VerifyCodeUtils.generateVerifyCode(4);
        // 存入会话session
        HttpSession session = request.getSession(true);
        // 删除以前的
        session.removeAttribute("verCode");
        session.removeAttribute("codeTime");
        session.setAttribute("verCode", verifyCode.toLowerCase());
        session.setAttribute("codeTime", new Date());
        // 生成图片
        int w = 100, h = 30;
        OutputStream out = response.getOutputStream();
        VerifyCodeUtils.outputImage(w, h, out, verifyCode);
    }
```
## 三、前端代码  

```html
<%@page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8" %>

<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>
<%
    String path = request.getContextPath();
    String basePath = request.getScheme() + "://" + request.getServerName() + ":" + request.getServerPort()
            + path + "/";
%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
    <base href="<%=basePath%>"><!-- 配置页面基准路径 -->
    <meta charset="UTF-8"/>
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>登陆</title>
</head>
<body class="login-bg">
<div class="tips-toast" ></div>
<!--  container -->
<div class="loginBox">
    <div class="login-box">
        <div class="row">
            <form role="form" action="login.html" method="post" class="login-form" id="defaultForm">

                <div class="form-group">
                    <input type="text" name="username" placeholder="请输入账号" class="form-username form-control" id="form-username">
                </div>
                <div class="form-group">
                    <input type="password" name="password" placeholder="请输入密码" class="form-password form-control" id="form-password" />
                </div>
                <div class="form-group form-block"   >
                    <div class="col-md-6 remove-padding">
                        <input type="text" name="validateCode" placeholder="验证码" class="form-control" id="form-code">
                    </div>
                    <div class="col-md-6 remove-padding border">
                        <span class="code-img"><img id="img" src="static/images/index/login-code.png"/></span>
                    </div>
                </div>
                <div class="form-group form-block"  >
                    <div class="col-md-6 remove-padding remember" >
                        <input type="checkbox" class="remember-check" />   记住我
                    </div>
                    <div class="col-md-6 remove-padding border text-right">
                        <a href="forget_password.html">忘记密码?</a>
                    </div>
                </div>
                <button id="validateBtn" type="button" class="btn btn-primary  btn-form">登录</button>
            </form>
        </div>
    </div>
</div>
<script>
	$(function(){
        setTimeout(getImg(),50);
    })
    $('.code-img').click(function () {
        getImg();
    });
    function getImg(){
        var img = document.getElementById("img");
        img.src = "/code/imgs?date=" + new Date().valueOf();//直接使用new Date()会导致ie内核的浏览器无法加载验证码
    }
</script>
</body>
</html>
```
[更多相关资料](https://blog.csdn.net/qq_25598453/article/details/86605423)  
