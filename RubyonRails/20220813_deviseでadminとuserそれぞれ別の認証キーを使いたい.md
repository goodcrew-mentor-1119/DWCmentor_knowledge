#それぞれのモデルに devise の設定を記述できます。

##user.rb

```ruby
class User < ApplicationRecord
  devise :database_authenticatable, authentication_keys: [:name]
```

##admin.rb
```ruby
class Team < ApplicationRecord
  devise :database_authenticatable, authentication_keys: [:email]
  ...
```
