■Gemfileの中身
---------------------------
group :test do
  gem ‘capybara’
  gem ‘rspec-rails’
  gem 'factory_bot_rails'
  gem ‘faker’
  gem ‘selenium-webdriver’
  gem ‘webdrivers’
end
---------------------------
■chrome binaryインストール方法(結構時間かかります。)
---------------------------
$ curl https://intoli.com/install-google-chrome.sh | bash
---------------------------
■chrome version確認
---------------------------
$ google-chrome-stable --version
---------------------------
■chromedriverのインストール(上記確認にて、google-chrome-stableのバージョンが87.0.4280.88の場合)
※chromeバージョンによってchromedriverの取得元URLは異なる。
    https://chromedriver.storage.googleapis.com/87.0.4280.88/chromedriver_linux64.zip
---------------------------
$ sudo yum install unzip
$ curl -O -L https://chromedriver.storage.googleapis.com/87.0.4280.88/chromedriver_linux64.zip
$ unzip chromedriver_linux64.zip
$ rm chromedriver_linux64.zip
$ sudo mv chromedriver /usr/local/bin
---------------------------
■spec/rails_helper.rb ファイルに以下追加
---------------------------
RSpec.configure do |config|
 # ...
config.before(:each) do |example|
    if example.metadata[:type] == :system
      if example.metadata[:js]
        driven_by :selenium_chrome_headless, screen_size: [1400, 1400]
      else
        driven_by :rack_test
      end
    end
  end
end
-------------------------------