# ApiGateway

- カスタムドメイン
- WebSocket URL :  wss://(ApiGatewayのAPI-ID).execute-api.(リージョン).amazonaws.com/(ステージ)/
- 接続URl        ： https://(ApiGatewayのAPI-ID).execute-api.(リージョン).amazonaws.com/(ステージ)/@connections

- websocket のAPI id = AAABB
- 

## 手順
- WebSocket API Gateway の前にCloudFrontを置く <br>
https://dev.classmethod.jp/articles/cloudfront-in-front-on-websocket-api-gateway/

1.ACM証明書, Aレコード<br>
「kazue.xxxxx.xxxx」ホストゾーンをRoute 53で管理<br>
<br>
2.「cfront.kazue.xxxxx.xxxx」でAlternate Domain Nameを設定<br>
<br>
 クライアントは、CloudFrontに「https://cfront.kazue.xxxxx.xxxx」でアクセスします<br>
 <br>
3.東京リージョンにAPI Gatewayを作成<br>
- API Gatewayのカスタムドメインを使い、WebSocket API GatewayのProdステージとマッピング<br>
<br>
https://xxx.execute-api.リージョン.amazonaws.com/dev/test のようなアクセスが、
https://ドメイン/test でアクセスできるようになるのがカスタムドメイン ※ステージ名が隠れる

ドメイン名は、cloudfrontのAlternate Domain Nameと同じとする<br>
<br>
4.CloudFrontディストリビューションにオリジン apigateway用のオリジンを追加し、その<br>
Path Patternを「websocket*」とします<br>
※上記はビヘイビアのPath Pattern。オリジンパスとは別<br>
<br>
「wss://cfront.kazue.xxxxx.xxxx/websocket」とするとWebSocket通信を開始できます。

5.CloudFrontのOrigin Domain NameはAPI Gatewayのオリジナルのドメイン名で設定する
「xxxxxxx.execute-api.ap-northeast-1.amazonaws.com」



### cloudfront参考
https://github.com/endw0901/aws/blob/main/cloudfront.md

## カスタムドメイン
AWS API Gatewayのカスタムドメインを実装する方法<br>

https://confrage.jp/aws-api-gateway%E3%81%AE%E3%82%AB%E3%82%B9%E3%82%BF%E3%83%A0%E3%83%89%E3%83%A1%E3%82%A4%E3%83%B3%E3%82%92%E5%AE%9F%E8%A3%85%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95/

ApiGatewayのカスタムドメイン名と
CloudFrontの代替ドメイン名は同じにする

<br>
WebSocket APIのカスタムドメイン名の設定(AWS公式doc) <br> 
わかりづらい <br>
https://docs.aws.amazon.com/ja_jp/apigateway/latest/developerguide/websocket-api-custom-domain-names.html?icmpid=apigateway_console_help


### カスタムドメインを使わない場合

カスタムドメインを使わない場合、<br>
WebSocket API GatewayをオリジンとするBehaviorのPath Patternは、<br>
WebSocket API Gatewayのステージ名と同じにする必要があります。<br>
<br>
例えば今回の場合ステージ名がProdなので、Path Patternの値は「Prod*」にする必要があります。<br>
<br>

xxxxxxx.cloudfront.net/Prod へのアクセス → <br>
xxxxxxx.execute-api.ap-northeast-1.amazonaws.com/Prod に転送される<br>
<br>
Origin Pathに/hogeを設定していた場合<br>
xxxxxxx.cloudfront.net/Prod へのアクセス →<br>
xxxxxxx.execute-api.ap-northeast-1.amazonaws.com/hoge/Prod に転送される<br>


