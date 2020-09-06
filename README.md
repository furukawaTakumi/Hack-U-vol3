# Hack-U-vol3

## Build Setup

```bash
# install dependencies
$ npm install

# serve with hot reload at localhost:3000
$ npm run dev

# build for production and launch server
$ npm run build
$ npm run start

# generate static project
$ npm run generate
```

For detailed explanation on how things work, check out [Nuxt.js docs](https://nuxtjs.org).

## Google Mapの利用可能にする
Google Mapを利用するためにはAPI KEYを環境変数に設定する必要があります。
以下のコマンドをこのREADME.mdと同じ階層で実行してください。
※API_KEYの部分は各自Google Cloud Platformで取得したapi keyと置き換えてください。

```.sh
touch .env 
echo "NUXT_ENV_GMAP_API_KEY=API_KEY" >> .env
```

## prettierを実行する手順
### 準備
`npm install`を実行後、以下を実行し、プロジェクトでprettierへのパスを通して実行可能にします。

```
export PATH=$PATH:./node_modules/.bin
```

### 使い方
```
prettier '適用ディレクトリまたはファイル名' --write
```

`--write`は上書きの設定

### 注意点
eslintとprettierでフォーマットの形式が違う場合があり、必ずしもエラーが出ない状態になるとは限りません。設定が必要な場合はpackage.jsonの"prettier"に設定を追加してください。