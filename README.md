# AWS CloudFormation Sample Web App Configuration

AWS CloudFormationのテンプレート。基本的なWebアプリケーション構成の参考テンプレート。

「Template」内にSampleのテンプレート(CreateSampleWebAppConfiguration.json)を用意しました。
ご参考になれば幸いです。

## CreateSampleWebAppConfiguration.jsonの構成イメージ

疎通する上でのネットワーク構成

![ネットワーク構成](https://github.com/tanukinokegawa/AWSCloudFormationSampleWebAppConfiguration/blob/master/img/20190807_AWS_CloudFormation_02.PNG)

非機能部分の構成

![非機能部分の構成](https://github.com/tanukinokegawa/AWSCloudFormationSampleWebAppConfiguration/blob/master/img/20190807_AWS_CloudFormation_03.PNG)

##  事前準備

* どのAMIでEC2を起動するか決めておく。AMI IDをメモしておく。
* EC2用のキーペアを用意。キーペア名をメモしておく。
* CertificateManagerに証明書を用意。証明書の識別番号をメモしておく。
（※HTTPS接続するため。特にいらないようであればテンプレート(CreateSampleWebAppConfiguration.json)から「CertificateNum」「ALBHttpsListener」のブロックを消す。）
* 自分の接続元を決めておく。インターネット接続するのであれば、自分の利用しているGlobal IPを確認する。自分の利用しているGlobal IPを確認するには[こちら](https://www.cman.jp/network/support/go_access.cgi)。後でVPN接続するなら、Private IPを確認する。
（※各EC2にSSH接続する際のインバウンドとして登録されます）
* アラート送付先のメールアドレスを決めておく。
（※今回のアラートはメール送付にしました。Slackとか利用するなら少し書き換えが必要ですね）

## 実行

CloudFormationにてスタックを作成し、テンプレート(CreateSampleWebAppConfiguration.json)を設定する。

![スタック作成中](https://github.com/tanukinokegawa/AWSCloudFormationSampleWebAppConfiguration/blob/master/img/20190807_AWS_CloudFormation_01.PNG)

* AmiId: 事前準備でメモしたAMI IDを記す。
* KeyName: 事前準備でメモしたキーペア名を記す。
* MyLocation: 自分の接続元のCIDRを記す。
* CertificateNum: 事前準備でメモしたCertificateManagerの証明書の識別番号を記す。
* AlartMailAddress: アラート送付先のメールアドレスを記す。

※もちろん、S3にテンプレートを置いて実行でもOKです！

## 注意

* この構成は東京リージョンで作成することを想定しています。そのため、サブネットのAvailabilityZoneをap-northeast-1に直書きされていたり、ログ用のS3バケットのBucketPolicyのPrincipalに582318560864(東京)と直書きされていたりします。他のリージョンで使用したい場合にはご修正後利用してください。
* いつでも作成した構成を消せるように「DisableApiTermination」をfalseにしていますが、消してはいけない構成にしたい場合、trueに変更してください。
* スタックを削除しても、ログ用のS3バケットが消えません。そのため、スタック削除後に手動でログ用のS3バケットを消す必要があります。
* お試し用なので、ログは作成されてから4日でGLACIERへ。作成されてから5日で削除するようにしています。
* 本稿はサンプルとしてご参考になればと記したものです。ご自分の環境にそぐわない場合もございますことご承知おきください。