Some dockerfiles for running tests with [nightwatch.js](http://nightwatchjs.org/).

* Uses the base 'java' image (OpenJDK 8)
* Assumes you will be starting your own Selenium server

These are available on [Docker Hub](https://hub.docker.com/) as:

* `tildedave/nightwatch-xvfb-firefox`
* `tildedave/nightwatch-xvfb-chrome`

## Tips

To run nightwatch inside of Xvfb, you can use `xvfb-run`.  For example,

```bash
xvfb-run --server-args="-screen 0 1600x1200x24" nightwatch -e chrome -a login
```

### Getting Chrome Working

Selenium ships with Firefox behavior built-in.  To automate against Chrome, you must use Selenium through [Chromedriver](https://code.google.com/p/selenium/wiki/ChromeDriver).

To run Chrome, you must either have `chromedriver` in your `PATH` environment variable, or specified as your `chrome.cli.opts` in your Nightwatch configuration.  Here's a configuration that's worked for me:

```json
  "selenium" : {
    "start_process" : true,
    "server_path" : "node_modules/selenium-server/lib/runner/selenium-server-standalone-2.44.0.jar",
    "log_path" : "",
    "host" : "127.0.0.1",
    "port" : 4444,
    "cli_args" : {
      "webdriver.chrome.driver" : "node_modules/chromedriver/lib/chromedriver/chromedriver",
      "webdriver.ie.driver" : ""
    }
  }
```

As documented [elsewhere](https://github.com/beatfactor/nightwatch/wiki/Chrome-Setup), you must also start Chrome with `--no-sandbox`.

```
"chrome" : {
  "desiredCapabilities": {
    "browserName": "chrome",
    "javascriptEnabled": true,
    "acceptSslCerts": true,
    "chromeOptions": {
      "args": [ "--no-sandbox" ]
    }
  }
}
```