# BEの環境構築
## 　エディタの設定
### IntelliJ IDEA Community　のダウンロード
Communityの方をダウンロードすること（無料版）
https://www.jetbrains.com/ja-jp/idea/download/?section=mac

### Spring　Bootの設定
以下からSpring Initializrの設定を手動で作成
https://start.spring.io/
必要な設定↓
- Project	Gradle
  - Kotlin
- Language
  - Kotlin
- Dependencies (依存関係)
  - Spring Web, Spring Data JPA, MySQL Driver

設定をダウンロードするとzipファイルができるのでBEのリポジトリ配下に移動させる
### Spring　Bootの設定
IntelliJを起動し、開くでBEのリポジトリを開き、Gradleが同期されるので待つ
```
Cannot find a Java installation on your machine (Mac OS X 14.4 aarch64) matching: {languageVersion=17,
```
上記が出たら、ダウンロードした設定は「Java 17」だが、IntelliJ IDEAが使用しようとしている「Java 21」の設定が合致していないので
`build.gradle.kts`Gradleの設定ファイルを修正
```
java {
    toolchain {
        languageVersion.set(JavaLanguageVersion.of(17)) // ここを２１に変更
    }
}
```
###　Git使えるようにする
```
git init
git add .
git commit -m "なんかメッセージを入れる"
git remote add origin git@github.com:persica-persicum/momo-backend.git
git push -u origin main
```

## 　DBと接続
`application.properties`にDBの設定を記載
```
# ==================================
# MySQL接続設定
# ==================================
spring.datasource.url=jdbc:mysql://localhost:3306/momo_db?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=Asia/Tokyo&autoReconnect=true
spring.datasource.username=root
spring.datasource.password= # mysql_secure_installationで設定したパスワードを入れる

# ==================================
# JPA/Hibernate設定
# ==================================
spring.jpa.hibernate.ddl-auto=update
spring.jpa.database-platform=org.hibernate.dialect.MySQLDialect
```
`BackendApplicationKt`で`fun main(args: Array<String>)`の横の矢印を実行

`application.properties`の`datasource.url`設定間違えると多分出るエラー
```
Caused by: java.sql.SQLNonTransientConnectionException: Could not create connection to database server. Attempted reconnect 3 times. Giving up.
Caused by: java.time.zone.ZoneRulesException: Unknown time-zone ID: JST
```
