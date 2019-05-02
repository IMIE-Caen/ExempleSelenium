```js

var webdriver = require('selenium-webdriver')
var assert = require('assert');
let By=webdriver.By;
let Key=webdriver.Key;
let Builder=webdriver.Builder;
var driver;
describe('First result on Google', function() {
    before(function(){
        driver = new Builder().forBrowser('firefox').build();
    })
    after(function(){
        driver.quit();
    });
    it("should be first result", async function(){
        this.timeout(60000);
        await driver.get("http://www.google.com") ;
        let el = await driver.findElement(By.name("q"));
        await el.sendKeys("Imie caen");
        await el.sendKeys(Key.ENTER);
        await driver.sleep(1000)
        first = await driver.findElement(By.css("#search a"))
        await first.click();
        let title = await driver.getTitle()
        assert(title == "Campus de Caen - IMIE", "Should be imie caen")
        await driver.sleep(8000);
        el = await driver.findElement(By.css("#quadmenu_0 > ul:nth-child(1) > li:nth-child(2)"));
        await driver.actions({bridge: true}).move({x: 0, y: 0, origin: el}).perform();
        await driver.sleep(3000);

        await driver.takeScreenshot().then(
            function(image, err) {
                require('fs').writeFile('out.png', image, 'base64', function(err) {
                    console.log(err);
                });
            }
        );
    });
});

```
