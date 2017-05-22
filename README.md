# python-headless-chromedriver

This is a Docker container with python and selenium that you can use for headless web scraping. 
It uses Chrome as driver and xvfbwrapper to manage headless displays with Xvfb.

See [Dockerfile](https://github.com/rsanchezavalos/python-headless-chromedriver/blob/master/Dockerfile).

### Dependencies

You can change the requirements, it currently installs:

xvfbwrapper \\
selenium\\
requests==2.13.0\\
beautifulsoup4 \\
numpy==1.11.2\\
boto==2.45.0\\
boto3==1.4.3\\
smart_open

#### Web Driver setup example

```
# Start headless display
display = xvfbwrapper.Xvfb()
display.start()

# Chromedriver location
chromedriver = "/usr/lib/chromium-browser/chromium-browser"
os.environ["webdriver.chrome.driver"] = chromedriver

# Driver extra prefs
chromeOptions = webdriver.ChromeOptions()
mime_types = "application/pdf,application/vnd.adobe.xfdf,application/vnd.fdf,application/vnd.adobe.xdp+xml"
prefs = {"browser.download.folderList": 2, "browser.download.dir": u'/home/ubuntu',
         "browser.download.manager.showWhenStarting": False, "browser.helperApps.neverAsk.saveToDisk": mime_types,
         "pdfjs.disabled": "true", "plugins.plugins_list": [{"enabled": False, "name": "Chrome PDF Viewer"}],
         "plugin.disable_full_page_plugin_for_types": mime_types}
chromeOptions.add_argument('--no-sandbox')
chromeOptions.add_experimental_option("prefs", prefs)

driver = webdriver.Chrome(
    "/usr/lib/chromium-browser/chromedriver", chrome_options=chromeOptions)

# Get initial_url
driver.get(initial_url)

# Close
driver.quit()
display.stop()
```