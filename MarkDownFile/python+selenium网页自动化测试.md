# python+selenium网页自动化测试

### 元素等待：

	#### 显示等待

```
WebDriverWait(driver, 10).until(
        EC.presence_of_element_located((By.ID, "myDynamicElement"))
    )
```

#### 隐式等待

```
driver.implicitly_wait(10) # seconds
```

### 查找元素方式：

- ind_element_by_id

- find_element_by_name

- find_element_by_xpath

- find_element_by_link_text

- find_element_by_partial_link_text

- find_element_by_tag_name

- find_element_by_class_name

- find_element_by_css_selector

  ​

直接看BasePage的实现：

```python
# coding=UTF-8
import time

from selenium.webdriver import ActionChains
from selenium.webdriver.support import expected_conditions as ec
from selenium.webdriver.support.wait import WebDriverWait

from client.WebClient import WebClient
from util.ConfigUtil import ConfigUtil


class BasePage(object):
    def __init__(self):
        self.driver = WebClient().get_driver()
        self.config_util = ConfigUtil()
        self.wait = WebDriverWait(self.driver, 10)

    def find_element(self, *loc):
        # 需要进行转义，没有转义不可以进行
        try:
            by = loc[0][0]
            locator = loc[0][1]
            loc = (by, locator)
            WebDriverWait(self.driver, 50).until(ec.visibility_of_element_located(loc))
        except Exception:
            print ("No such element found " + str(locator))
        else:
            return self.driver.find_element(*loc)

    def find_elements(self, *loc):
        try:
            by = loc[0][0]
            locator = loc[0][1]
            loc = (by, locator)
            print loc
            for i in loc:
                print i
            WebDriverWait(self.driver, 50).until(ec.visibility_of_element_located(loc))  # 需要找到元素，并且该元素也可见
        except Exception:
            raise ValueError("No such element found " + str(locator))
        return self.driver.find_elements(*loc)

    def is_element_exists(self, *loc):
        try:
            by = loc[0][0]
            locator = loc[0][1]
            loc = (by, locator)
            WebDriverWait(self.driver, 50).until(ec.presence_of_element_located(loc))  # 只关心元素是否存在在页面中
            return self.driver.find_element(loc).text()
        except:
            return False

    # 执行操作js代码
    def execute_js(self, script):
        self.driver.execute_script(script)
        time.sleep(2)

    # 鼠标悬停
    def execute_mouse_hover(self, *loc):
        mouse = self.find_element(*loc)
        ActionChains(self.driver).move_to_element(mouse).perform()

    # 用于等待元素可用（元素肯定会出现，只是时间问题）
    def wait_element_is_enabled(self, timeout, *loc):
        self.driver.implicitly_wait(timeout)
        try:
            self.driver.find_element(*loc).is_enabled()
        except:
            pass
        self.driver.implicitly_wait(8)

```

