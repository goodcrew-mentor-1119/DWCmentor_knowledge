# 複数のviewから呼び出されるた際のバリデーションメッセージを表示するrender先を判断する方法
request.refererを利用してrender先を制御する
Rails.application.routes.recognize_path(request.referer)
```ruby
def create
  if @hoge.save
	redirect_to hoge_path  
  else
	path = Rails.application.routes.recognize_path(request.referer)
	render "#{paths[:controller]}/#{path[:action]}"
  end
end
```