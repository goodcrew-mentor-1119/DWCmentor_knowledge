# 保存前のファイルをチェックしたい

## コントローラー側
``` rb
  def create
    @post = Post.new(post_params)
    if Vision.get_image_data(post_params[:image]) # or params[:post][:image]
      @post.save
      redirect_to item_path(@post.id)
    else
      rebder :new
    end
  end

  private
  def post_params
    params.require(:item).permit(:title, :description, :image) # has_one_attached :image の場合
  end
```
## vidion.rb
``` rb
module Vision
  class << self
    def get_image_data(image_file)
      # APIのURL作成
      api_url = "https://vision.googleapis.com/v1/images:annotate?key=#{ENV['GOOGLE_API_KEY']}"

      # 画像をbase64にエンコード
      #dir_tree =  image_file.key.scan(/.{1,#{2}}/)
      #base64_image = Base64.encode64(open("#{Rails.root}/public/uploads/#{dir_tree[0]}/#{dir_tree[1]}/#{image_file.key}").read) # 保存後のファイル
      base64_image = Base64.encode64(image_file.tempfile.read) # 保存前のファイル
