# Paperclip upload to S3

### Requirements
- aws-sdk
- paperclip

### gem install

``Gemfile``

```sh
gem 'aws-sdk'
gem 'paperclip'

```

and run

``bundle install``

### s3 생성

http://www.pyrasis.com/book/TheArtOfAmazonWebServices/Chapter11 여길 참조

### set s3 information

https://console.aws.amazon.com/iam/home?#security_credential 에서 access key와 secret key를 발급 받아서 잘 적어둔다.

그리고 나서 ``config/aws.yml`` 파일을 만든다

```sh
development: &default
  access_key_id: 'YOUR_ACCESS_KEY'
  secret_access_key: 'YOUR_SECRET_ACCESS_KEY'
  region: 'YOUR_BUCKET_REGION'
  
production:
  <<: *default
  
test:
  <<: *default
```

파일을 저장한후에 envrionment에 s3 정보를 추가한다

``config/environments/production.rb`` 또는 ``config/environments/development.rb``등등.

```sh
config.paperclip_defaults = {
  :storage => :s3,
  :s3_credentials => "#{Rails.root}/config/aws.yml",
  :s3_host_alias => 'YOUR_CLOUDFRONT_URL',
  :s3_host_name => "s3-ap-northeast-1.amazonaws.com",
  :s3_protocol => "",
  :url => ":s3_alias_url",
  :bucket => 'YOUR_BUCKET_NAME',
  :path => ":attachment/:id/:style/:filename"
}
```

만약 아직 CloudFront와 연동을 하지 않았다면 s3_host_alias와 url 부분은 지워도 된다.

path도 원하는 형식으로 수정해서 사용해도 되고, s3_host_name은 본인의 s3 region에 따라 바꿔주면 된다.


### cloudfront 배포

http://www.pyrasis.com/book/TheArtOfAmazonWebServices/Chapter12 여길 참조.


