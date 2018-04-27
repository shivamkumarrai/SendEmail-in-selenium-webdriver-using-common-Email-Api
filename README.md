sendEmail.java


package Demo1;

import org.testng.annotations.Test;
import org.testng.annotations.BeforeClass;
import org.testng.annotations.DataProvider;

import java.util.concurrent.TimeUnit;

import org.apache.commons.mail.EmailException;
import org.openqa.selenium.By;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.Assert;
import org.testng.ITestResult;
import org.testng.annotations.AfterClass;
import org.testng.annotations.AfterMethod;

public class MyTest1 {
	ChromeDriver driver;
	@Test(dataProvider="Yummz")
     public void startApp(String email , String password) throws InterruptedException
     {
		System.setProperty("webdriver.chrome.driver","K:\\chromedriver.exe\\");
		driver = new ChromeDriver();
		driver.get("https://yummzapp.com:3020/app/#!/Login");
		driver.manage().timeouts().implicitlyWait(30, TimeUnit.SECONDS);
		driver.findElement(By.id("email")).sendKeys(email);
		driver.findElement(By.id("password")).sendKeys(password);
		driver.findElement(By.xpath("/html/body/div[1]/div/div/form/div[2]/div[3]/button")).click();
		Thread.sleep(5000);
	    Assert.assertTrue(driver.getCurrentUrl().contains("https://yummzapp.com:3020/app/#!/dashboard"),("User is not able to login- Invalid credentials"));
	    System.out.println("PageTitle- User login successfully");
	   }
	@AfterMethod
		public void tearDown()
		{
		driver.quit();
	   }
	@DataProvider(name="Yummz")
	public  Object[][] passData()	
	{
	Object[][] data = new Object[4][2];
	 data [0][0]="carlie@gmail.com";
	 data [0][1]="Apple@123";
	 
	 data [1][0]="car@gmail.com";
	 data [1][1]="Apple@123";
	 
	 data [2][0]="ce@gmail.com";
	 data [2][1]="Apple@123";
		 
	 data [3][0]="suteki@yummzapp.com";
	 data [3][1]="Apple@123";
	 
	  return data;
	}


  @AfterMethod
  public void afterClass(ITestResult result) throws EmailException 
  {
	  if(result.getStatus()==ITestResult.FAILURE)
	  {
		  SendEmailJava.sendEmail();
		  
		  System.out.println("Test Failed and Email Send");
	  }
	 
  }

}





emailSend.java


package Demo1;

import org.apache.commons.mail.DefaultAuthenticator;
import org.apache.commons.mail.Email;
import org.apache.commons.mail.EmailException;
import org.apache.commons.mail.SimpleEmail;

public class SendEmailJava {
	
	public static void sendEmail() throws EmailException {
		
		Email email = new SimpleEmail();
		email.setHostName("smtp.googlemail.com");
		email.setSmtpPort(465);
		email.setAuthenticator(new DefaultAuthenticator("rock.gsit@gmail.com", "nsha@2114shmma"));
		email.setSSLOnConnect(true);
		email.setFrom("shivam.phys@gmail.com");
		email.setSubject("Selenium Test Mail");
		email.setMsg("User is not able to login- Invalid credentials");
		email.addTo("shivam.rai@globalsysinfo.com");
		
		email.send();
	}

}





porm.xml



<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>EmailSent</groupId>
  <artifactId>EmailSent</artifactId>
  <version>0.0.1-SNAPSHOT</version>

<dependencies>


<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-email</artifactId>
    <version>1.3.1</version>
</dependency>


<dependency>
    <groupId>org.testng</groupId>
    <artifactId>testng</artifactId>
    <version>6.9.10</version>
    <scope>test</scope>
</dependency>



</dependencies>

</project>
