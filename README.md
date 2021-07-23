# Study Rust Lambda

Rust による AWS Lambda の勉強用リポジトリです。

## 開発

### ビルド

[lambda-rust](https://github.com/softprops/lambda-rust) を使用してビルドを行う。ただ、現在 [Docker Hub](https://hub.docker.com/r/softprops/lambda-rust/tags?page=1&ordering=last_updated) に上がっている最新バージョンのものは古く、[aws-lambda-rust-runtime](https://github.com/awslabs/aws-lambda-rust-runtime) の v0.3 系を使用している場合にうまく実行ができない。
なので、master ブランチの Dockerfile をビルドしたイメージを使用する。

```sh
docker build -t softprops/lambda-rust:1.51 https://github.com/softprops/lambda-rust.git#e6137ddbac36d104236407eb537c6c03a16a30fa
```

本リポジトリのビルドは下記コマンドより実行する。

```sh
docker container run --rm -u $(id -u):$(id -g) -v ${PWD}:/code -v ${HOME}/.cargo/registry:/cargo/registry -v ${HOME}/.cargo/git:/cargo/git softprops/lambda-rust:1.51
```

### デプロイ

ビルド成果物が `./target/lambda/release/study-rust-lambda.zip` に出力されているため、これをマネジメントコンソールもしくは AWS CLI 経由でアップロードする。

```sh
aws lambda create-function --function-name rustTest --handler doesnt.matter --zip-file fileb://./target/lambda/release/study-rust-lambda.zip --runtime provided.al2 --role arn:aws:iam::412545772278:role/toggle-reporter-ToggleReporterFunctionRole-5GFRRELF0Q2Z --environment Variables={RUST_BACKTRACE=1} --tracing-config Mode=Active
```
