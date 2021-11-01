# citygml-tools
citygml-toolsは、CityGMLファイルを処理するためのいくつかの操作をバンドルするコマンドラインユーティリティです.

## ライセンス
citygml-toolsは、[Apache License、Version 2.0]（http://www.apache.org/licenses/LICENSE-2.0）の下でライセンスされています.
詳細については、 `LICENSE`ファイルを参照してください。

## 最新のリリース
citygml-toolsの最新の安定したリリースは1.4.3です.

citygml-tools1.4.3 リリースバイナリをダウンロードします
[ここ](https://github.com/citygml4j/citygml-tools/releases/download/v1.4.3/citygml-tools-1.4.3.zip). 以前のバージョンは[リリースセクション](https://github.com/citygml4j/citygml-tools/releases)から入手できます.

## 貢献するには
*　ソフトウェアで見つかったバグを報告するには、GitHubのissueを作成してください.
*　提出された問題を修正するためのコードを提供するには、問題IDを使用してプルリクエストを作成してください.
*　新しい機能を提案するには、GitHubのissueを作成し、ディスカッションを開始してください.

## citygml-tools の使用法
最新リリースをダウンロードして解凍するかソースからビルドします[ビルド](https://github.com/citygml4j/citygml-tools#building). その後、シェル環境を開き、プログラムフォルダから `citygml-tools` スクリプトを実行してプログラムを起動します.

ヘルプメッセージとcitygml-toolsで使用可能なすべてのコマンドを表示するには、次のように入力します:

    > citygml-tools --help

すると、次の使用情報が出力されます:

```
Usage: citygml-tools [-hV] [--log-file=<file>] [--log-level=<level>]
                     [@<filename>...] COMMAND
Collection of tools for processing CityGML files.
      [@<filename>...]      One or more argument files containing options.
  -h, --help                Show this help message and exit.
      --log-file=<file>     Write log messages to the specified file.
      --log-level=<level>   Log level: error, warn, info, debug (default: info).
  -V, --version             Print version information and exit.
Commands:
  help              Displays help information about the specified command
  validate          Validates CityGML files according to the given subcommand.
  change-height     Changes the height values of city objects by a given offset.
  remove-apps       Removes appearances from city objects.
  move-global-apps  Converts global appearances to local ones.
  clip-textures     Clips texture images to the extent of the target surface.
  filter-lods       Filters the LoD representations of city objects.
  reproject         Reprojects city objects to a new spatial reference system.
  from-cityjson     Converts CityJSON files into CityGML.
  to-cityjson       Converts CityGML files into CityJSON.
```

citygml-toolsの特定のコマンドに関するヘルプを取得するには、次のように入力し、 `COMMAND`　を詳細を知りたいコマンドの名前に置き換えます:

    > citygml-tools help COMMAND

次の例は、 `to-cityjson`コマンドを使用してCityGMLファイルをCityJSONに変換する方法を示しています:

    > citygml-tools to-cityjson /path/to/your/CityGML.gml

## システム要件
* Java JRE or JDK >= 1.8
  
citygml-toolsは、適切なJavaサポートを提供する任意のプラットフォームで実行できます. 

## Docker イメージ


citygml-toolsはDockerイメージとしても利用できます。提供されている `Dockerfile`を使用して自分でイメージをビルドするか、DockerHubからビルド済みのイメージを利用できます: https://hub.docker.com/r/citygml4j/citygml-tools.

イメージをビルドするには、リポジトリをローカルマシンに複製し、リポジトリのルートから次のコマンドを実行します:

    > docker build -t citygml-tools .

### イメージの使い方
    
dockerを介してcitygml-toolsを使用するのは簡単です:
 
     > docker run --rm citygml-tools
     
 すると、citygml-toolsのヘルプメッセージと使用可能なすべてのコマンドが表示されます.
 
 次のコマンドは、ボリュームをマウントし、マウントされたボリューム内のすべてのCityGMLファイルに対してcitygml-toolsの `to-cityjson`コマンドを実行します.

    > docker run --rm -u 1000 -v /path/to/your/data:/data citygml-tools to-cityjson /data

`-u`パラメータを使用して、現在のホストのユーザーのユーザー名またはUIDを渡し、マウントされたディレクトリに生成されたファイルに正しいファイル権限を設定します.

### 詳細な技術情報

citygml-toolsイメージは[OpenJDK](https://hub.docker.com/_/openjdk) Alpine Linux を作成するイメージのサイズを小さくするために利用しています. さらに、マルチステージイメージとして記述され、「JDKイメージ」はビルドにのみ使用するため、最終的なアプリケーションはより小さな「JREイメージ」にラップされます.

デフォルトでは、コンテナプロセスはroot以外のユーザーとして実行されます。含まれているエントリーポイントスクリプトを使用すると、コンテナーの開始時に任意のユーザーが作成される可能性のあるOpenShift環境でもイメージを使用できます.

コンテナ内のデフォルトの作業ディレクトリは `/data` です.

## citygml-tools をライブラリとして利用

citygml-toolsは単なるCLIプログラムではありません。ほとんどのコマンドは、個別のJARライブラリとしても利用できます。クラスパスの `lib`フォルダーからライブラリファイルを配置するだけで、citygml4jプロジェクトで操作を使用できます. `citygml-tools-common- <version> .jar`ライブラリは、すべてのコマンドに必須の依存関係をレンダリングします.

ライブラリは、[Maven Central Repository]（https://search.maven.org/search?q=org.citygml4j.tools）から[Maven]（http://maven.apache.org/）アーティファクトとしても入手できます.　たとえば、Mavenを使用してプロジェクトにグローバルな外観を削除するための `global-app-mover`ライブラリを追加するには、次のコードを` pom.xml`に追加します. `global-app-mover`のバージョン番号を調整する必要があるかもしれません.

```xml
<dependency>
  <groupId>org.citygml4j.tools</groupId>
  <artifactId>global-app-mover</artifactId>
  <version>1.4.3</version>
</dependency>
```

Gradleプロジェクトで `global-app-mover`を使用する方法は次のとおりです:

```gradle
repositories {
  mavenCentral()
}

dependencies {
  compile 'org.citygml4j.tools:global-app-mover:1.4.3'
}
```

個別のJARライブラリとして使用できないコマンドは、citygml4jを使用して数行のコードをだけで実装できます。ソースコードをチェックして、それらがどのように実装されているかを確認してください.

## 建物
citygml-toolsはビルドシステムとして[Gradle]（https://gradle.org/）を使用します。ソースからプログラムをビルドするには、リポジトリをローカルマシンに複製し、リポジトリのルートから次のコマンドを実行します.

    > gradlew installDist
    
スクリプトは、citygml-toolsの構築と実行に必要なすべての依存関係を自動的にダウンロードします。したがって、インターネットに接続していることを確認してください.ビルドプロセスはすべての主要なオペレーティングシステムで実行され、実行にはJava 8JDK以上が必要です.

ビルドが成功した場合は、「citygml-tools / build / install」の下にcitygml-toolsパッケージがあるはずです.