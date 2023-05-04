# 保存前のファイルをチェックしたい

## コントローラー側
``` rb
  def create
    @list = List.new(list_params)
    tags = Vision.get_image_data(list_params[:image]) # or params[:list][:image]

    if @list.save
      tags.each do |tag|
        @list.tags.create(name: tag)
      end
      redirect_to todolist_path(@list.id)
    else
      rebder :new
    end    
  end
  private
  def list_params
    params.require(:list).permit(:title, :body, :image)
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
      # 保存後のファイル
      base64_image = Base64.encode64(image_file.tempfile.read) # 保存前のファイル
    # APIリクエスト用のJSONパラメータ
      params = {
        requests: [{
          image: {
            content: base64_image
          },
          features: [
            {
              type: 'LABEL_DETECTION'
            }
          ]
        }]
      }.to_json

      # Google Cloud Vision APIにリクエスト
      uri = URI.parse(api_url)
      https = Net::HTTP.new(uri.host, uri.port)
      https.use_ssl = true
      request = Net::HTTP::Post.new(uri.request_uri)
      request['Content-Type'] = 'application/json'
      response = https.request(request, params)
      response_body = JSON.parse(response.body)
      # APIレスポンス出力
      if (error = response_body['responses'][0]['error']).present?
        raise error['message']
      else
        response_body['responses'][0]['labelAnnotations'].pluck('description').take(3)
      end
    end
  end
end