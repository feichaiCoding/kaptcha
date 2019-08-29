# kaptcha - A kaptcha generation engine.

This repo is the copy of http://code.google.com/p/kaptcha/ and published to maven central
```
<dependency>
  <groupId>com.github.penggle</groupId>
  <artifactId>kaptcha</artifactId>
  <version>2.3.2</version>
</dependency>
```
Please see the website for more information about this project.

http://code.google.com/p/kaptcha/

thanks!

web.xml中的配置
```
<servlet>
  	<servlet-name>Kaptcha</servlet-name>
  	<servlet-class>com.google.code.kaptcha.servlet.KaptchaServlet</servlet-class>
  	<!-- 是否有边框 -->
  	<init-param>
  		<param-name>kaptcha.border</param-name>
  		<param-value>no</param-value>
  	</init-param>
  	<!-- 字体颜色 -->
  	<init-param>
  		<param-name>kaptcha.textproducer.font.color</param-name>
  		<param-value>red</param-value>
  	</init-param>
  	<!-- 字体大小 -->
  	<init-param>
  		<param-name>kaptcha.textproducer.font.size</param-name>
  		<param-value>43</param-value>
  	</init-param>
  	<!-- 字体 -->
  	<init-param>
  		<param-name>kaptcha.textproducer.font.names</param-name>
  		<param-value>Arial</param-value>
  	</init-param>
  	<!-- 图片宽度 -->
  	<init-param>
  		<param-name>kaptcha.image.width</param-name>
  		<param-value>135</param-value>
  	</init-param>
  	<!-- 图片高度 -->
  	<init-param>
  		<param-name>kaptcha.image.height</param-name>
  		<param-value>50</param-value>
  	</init-param>
  	<!-- 使用那些字符生成验证码 -->
  	<init-param>
  		<param-name>kaptcha.textproducer.char.string</param-name>
  		<param-value>ABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890</param-value>
  	</init-param>
  	<!-- 干扰线的颜色 -->
  	<init-param>
  		<param-name>kaptcha.noise.color</param-name>
  		<param-value>black</param-value>
  	</init-param>
  	<!-- 字符个数 -->
  	<init-param>
  		<param-name>kaptcha.textproducer.char.length</param-name>
  		<param-value>4</param-value>
  	</init-param>
  </servlet>
  <servlet-mapping>
  	<servlet-name>Kaptcha</servlet-name>
  	<url-pattern>/Kaptcha</url-pattern>
  </servlet-mapping>
  
```
前端关于验证码的相关HTML代码
```
<!-- 验证码 kaptcha -->
<li>
  <div class="item-content">
    <div class="item-media"></div>
    <div class="item-inner">
      <div class="item-title label">验证码</div>
      <input type="text" id="j_kaptcha" placeholder="please input verifyCode">
      <div class="item-input">
        <img id="kaptcha_img" alt="click change" title="click change" onclick="changeVerifyCode(this)" src="../Kaptcha">
      </div>
    </div>
  </div>
</li>


<!-- 说明：这里的src="../Kaptcha"是跳转到相应的servlet中进行加载验证码。因为web.xml在WEB-INF目录下，我们无法通过地址栏直接访问到Kaptcha，需要我们退一级目录，即../Kaptcha，这样可以访问到了 -->

```
前端关于验证码的相关js代码：点击刷新验证码
```
//点击切换验证码
function changeVerifyCode(img){
	img.src = "../Kaptcha?" + Math.floor(Math.random() * 100);//?后面接的是一些标识参数 
}
```

java代码编写：完成验证码匹配
```
/**
 * 这是kaptcha验证码工具类
 * @author SHsama
 *
 */
public class CodeUtil {
	public static boolean checkVerifyCode(HttpServletRequest request) {
		String verifyCodeExcepted = (String) request.getSession().getAttribute(com.google.code.kaptcha.Constants.KAPTCHA_SESSION_KEY);
		String verifyCodeActual = HttpServletRequestUtil.getString(request, "verifyCodeActual");//忽略大小写：kachepa.equalsIgnoreCase(kaptchaExpected)
		if(verifyCodeActual == null || !verifyCodeActual.equals(verifyCodeExcepted)) {
			return false;
		}
		return true;
	}

}

```
