# laravel-docker-dev-environment

## 立ち上げたあとにやること
新規の場合は、アプリケーションサーバーに入って、プロジェクトをつくる。既存の場合はボリューム内にもってくる。
```bash
composer create-project --prefer-dist laravel/laravel blog
```
`.env`ファイルを編集。主にDB関連。マイグレーションすれば、使用できるはず

## Duskの環境構築
そのままだとchromeが異常終了するエラーが出るので、`test/DuskTestCase`の`driver()`を編集する。
あと、[ドキュメント](https://readouble.com/laravel/7.x/ja/dusk.html)に書かれているとおりにデフォルトの設定を無効して、URLを編集する

```php
public static function prepare()
{
    // static::startChromeDriver();
}


protected function driver()
    {
        $options = (new ChromeOptions)->addArguments([
            '--disable-gpu',
            '--headless',
            '--window-size=1920,1080',
            '--no-sandbox', // <-追加する
        ]);

        return RemoteWebDriver::create(
            // URL
            'http://chrome:4444', DesiredCapabilities::chrome()->setCapability(
                ChromeOptions::CAPABILITY, $options
            )
        );
    }
```

あとは、`.env`でウェブサーバーのURLを`APP_URL=http://web`とする。
