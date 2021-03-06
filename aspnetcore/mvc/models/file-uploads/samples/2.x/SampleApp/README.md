# <a name="upload-files-sample-app"></a>ファイルのアップロードのサンプル アプリ

このサンプル アプリでは、「[ASP.NET Core でのファイルのアップロード](https://docs.microsoft.com/aspnet/core/mvc/models/file-uploads)」トピックで説明されている概念を示します。

## <a name="security-considerations"></a>セキュリティに関する考慮事項

サーバーにファイルをアップロードする機能をユーザーに提供するときは、十分に注意してください。 攻撃者が、[サービス拒否](/windows-hardware/drivers/ifs/denial-of-service)攻撃を実行したり、ウイルスやマルウェアのアップロードを試みたり、他の方法でネットワークやサーバーを侵害しようとしたりする可能性があります。

攻撃の成功の可能性を少なくするセキュリティ手順は、次のとおりです。

* システムの専用のファイル アップロード領域 (できれば、システム ドライブ以外) にファイルをアップロードします。 専用の場所を使用すると、アップロードされるファイルにセキュリティ対策を適用しやすくなります。 ファイルのアップロード場所に対する実行アクセス許可を無効にします。&dagger;
* アプリと同じディレクトリ ツリーに、アップロードしたファイルを保持しないでください。&dagger;
* アプリによって決められた安全なファイル名を使用します。 ユーザー入力によって指定されたファイル名や、アップロードされるファイルの信頼されていないファイル名は、使用しないでください。&dagger;
* 承認されている特定のファイル拡張子のみを許可します。&dagger;
* ユーザーが偽装ファイルをアップロードできないように (拡張子を *.txt* に変更して *.exe* ファイルをアップロードするなど)、ファイル形式のシグネチャを確認します。&dagger;
* クライアント側のチェックがサーバーでも実行されることを確認します。 クライアント側のチェックは簡単に回避できます。&dagger;
* アップロードのサイズを確認し、アップロードのサイズが予想より大きくならないようにします。&dagger;
* 同じ名前でアップロードされたファイルによってファイルが上書きされないようにする必要があるときは、ファイルをアップロードする前に、データベースまたは物理ストレージに対してファイル名を確認します。
* **ファイルを格納する前に、アップロードされる内容に対してウイルス/マルウェア スキャナーを実行します。**

&dagger;サンプル アプリで、条件を満たす方法が示されています。

> [!WARNING]
> システムへの悪意のあるコードのアップロードは、頻繁に次のような内容のコードの実行するための足がかりとなります。
>
> * システムを完全に乗っ取る。
> * システムがクラッシュする結果で、システムを過負荷状態にする。
> * ユーザーまたはシステムのデータを破壊する。
> * パブリック UI に落書きする。
>
> ユーザーからファイルを受け入れる際の外部アクセスによる攻撃を減らす方法については、次の資料を参照してください。
>
> * [Unrestricted File Upload (ファイルの無制限のアップロード)](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [Azure セキュリティ: ユーザーから受け入れるファイルに適切な管理制御を整備する](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

詳細については、「[ASP.NET Core でのファイルのアップロード](https://docs.microsoft.com/aspnet/core/mvc/models/file-uploads)」を参照してください。

## <a name="how-to-use-the-sample"></a>このサンプルを使用する方法

*appsettings.json* ファイルで、次のようにします。

1. 格納されているファイルのパスを設定します (`StoredFilesPath`)。

   * サンプル アプリでは、値は `c:\\files` に設定されます。システムの C: ドライブのルートに、*files* という名前のフォルダーが存在することが想定されています。
   * パスが存在している必要があります。 システムの C: ドライブに *files* フォルダーを作成するか、パスを適切な場所に設定します。
   * アプリのプロセスには、そのパスに対する読み取り/書き込みアクセス許可が必要です。
   * **重要:** そのパスですべてのユーザーに対して実行アクセス許可を無効にします。

1. ファイル サイズの上限 (`FileSizeLimit`) をバイト単位で設定します。 サンプル アプリの既定値 `2097152` (2,097,152 バイト) では、最大 2 MB のファイルをアップロードできます。
