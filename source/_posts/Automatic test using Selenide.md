---

title: Automatic test using Selenide

date: 2019-06-12 10:19:32

tags: [Selenide, Automatic Test]

---

  
  

## What Selenide is?

  

Selenide is a framework for test automation powered by **Selenium WebDriver** that brings the following advantages:

  

Concise fluent API for tests. Powerful selectors. Simple configuration.

  

You don't need to think how to shutdown browser, handle timeouts and StaleElement Exceptions or search for relevant log lines, debugging your tests.

Just focus on your business logic and let Selenide do the rest!

  

## Seleium code

  

``` Java

public  class  GoogleTest {

  
	chromeDriver webdriver;

  

	@Before
	public  void  setUp() {
		System.setProperty("webdriver.chrome.driver","drivers/chromedriver.exe");

		WebDriverManager.chromedriver().setup();

		webdriver = new  ChromeDriver();

	}
																									

	@Test
	public  void  searchGoogle() {
		webdriver.get("[https://www.google.com/ncr](https://www.google.com/ncr)");
		WebElement  q = webdriver.findElementByName("q");
		q.sendKeys("test");
		q.submit();
		wait.until(ExpectedConditions.visibilityOfElementLocated(By.id("resultStats")));

	}
				
  

	@After
	public  void  tearDown() {
		if (webdriver != null) {
		webdriver.quit();
	}

}

  

}

  

```

  

  

Selenide Code

``` java

  
@Listeners(BrowserPerClass.class)
public class GoogleTest {


	@Before
	public void setUp() {
		System.setProperty("webdriver.chrome.driver", "drivers/chromedriver.exe");
		Configuration.browser = WebDriverRunner.CHROME;
		Configuration.baseUrl = "[http://google.com](http://google.com/)";
	}

	  

	@Test
	public void searchGoogle() {
		open("/");
		$(By.name("q")).sendKeys("test");
		$(By.name("q")).submit();
		$(By.id("resultStats")).waitUntil(exist, 10000);

	}

  

}

```

  

  

## get element using Xpath, cssSelector

  

some element have no id or name, is not easy to locate it . there is two ways to locate it: **Xpath**, **cssSelector**. but how to know what the element's xpath/cssSelector is? Chrome!!

  

open the page in Chrome and press F12 to open the **Developer Tool** Panel. Select the element and right click to copy the xpath/cssSelector. something like:

**//*[@id="tsf"]/div[2]/div/div[3]/center/input[1]** 