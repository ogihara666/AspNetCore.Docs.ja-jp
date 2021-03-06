---
title: ASP.NET Core Razor コンポーネント クラス ライブラリ
author: guardrex
description: 外部コンポーネント ライブラリから、コンポーネントを Blazor アプリに含める方法について説明します。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/27/2020
no-loc:
- ASP.NET Core Identity
- cookie
- Cookie
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/components/class-libraries
ms.openlocfilehash: afd1bfffae11520a5d9abccc1d2ee4cf3a46a4bf
ms.sourcegitcommit: 24106b7ffffc9fff410a679863e28aeb2bbe5b7e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/17/2020
ms.locfileid: "90722463"
---
# <a name="aspnet-core-no-locrazor-components-class-libraries"></a>ASP.NET Core Razor コンポーネント クラス ライブラリ

作成者: [Simon Timms](https://github.com/stimms)

コンポーネントは、プロジェクト間で [Razor クラス ライブラリ (RCL)](xref:razor-pages/ui-class) で共有できます。 *Razor コンポーネント クラス ライブラリ*は、次から含めることができます。

* ソリューションの別のプロジェクト。
* NuGet パッケージ。
* 参照されている .NET ライブラリ。

コンポーネントが通常の .NET 型であるのと同様に、RCL によって提供されるコンポーネントは通常の .NET アセンブリです。

## <a name="create-an-rcl"></a>RCL を作成する

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 新しいプロジェクトを作成します。
1. **[Razor クラス ライブラリ]** を選択します。 **[次へ]** を選択します。
1. **[新しい Razor クラス ライブラリを作成します]** ダイアログで **[作成]** を選択します。
1. **[プロジェクト名]** フィールドにプロジェクト名を入力するか、既定のプロジェクト名をそのまま使用します。 このトピックの例では、プロジェクト名 `ComponentLibrary` を使用します。 **[作成]** を選択します。
1. RCL をソリューションに追加します。
   1. ソリューションを右クリックします。 **[追加]**  >  **[既存のプロジェクト]** を選択します。
   1. RCL のプロジェクト ファイルに移動します。
   1. RCL のプロジェクト ファイル (`.csproj`) を選択します。
1. アプリから RCL の参照を追加します。
   1. アプリ プロジェクトを右クリックします。 **[追加]**  >  **[参照]** の順に選択します。
   1. RCL プロジェクトを選択します。 **[OK]** を選択します。

> [!NOTE]
> テンプレートから RCL を生成するときに **[ページとビューのサポート]** チェック ボックスがオンになっている場合は、生成したプロジェクトのルートに、次の内容で `_Imports.razor` ファイルも追加して、Razor コンポーネントを作成できるようにします。
>
> ```razor
> @using Microsoft.AspNetCore.Components.Web
> ```
>
> 生成されたプロジェクトのルートにファイルを手動で追加します。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

1. コマンド シェルで [`dotnet new`](/dotnet/core/tools/dotnet-new) コマンドを使用して、 **Razor クラス ライブラリ** テンプレート (`razorclasslib`) を使用します。 次の例では、`ComponentLibrary` という名前の RCL が作成されます。 コマンドの実行時に、`ComponentLibrary` を保持するフォルダーが自動的に作成されます。

   ```dotnetcli
   dotnet new razorclasslib -o ComponentLibrary
   ```

   > [!NOTE]
   > テンプレートから RCL を生成するときに、`-s|--support-pages-and-views` スイッチが使用されている場合、生成したプロジェクトのルートに、次の内容で `_Imports.razor` ファイルも追加して、Razor コンポーネントを作成できるようにします。
   >
   > ```razor
   > @using Microsoft.AspNetCore.Components.Web
   > ```
   >
   > 生成されたプロジェクトのルートにファイルを手動で追加します。

1. 既存のプロジェクトにライブラリを追加するには、コマンド シェルで [`dotnet add reference`](/dotnet/core/tools/dotnet-add-reference) コマンドを使用します。 次の例では、RCL がアプリに追加されています。 ライブラリへのパスを使用して、アプリのプロジェクト フォルダーから次のコマンドを実行します。

   ```dotnetcli
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="consume-a-library-component"></a>ライブラリ コンポーネントの使用

別のプロジェクトのライブラリに定義されているコンポーネントを使用するには、次のいずれかの方法を使用します。

* 名前空間と完全な型名を使用します。
* Razor の [`@using`](xref:mvc/views/razor#using) ディレクティブを使用します。 個々のコンポーネントを名前で追加することができます。

次の例で、`ComponentLibrary` は `Component1` コンポーネント (`Component1.razor`) を含むコンポーネント ライブラリです。 `Component1` コンポーネントは、ライブラリの作成時に RCL プロジェクト テンプレートによって自動的に追加されるサンプルのコンポーネントです。

`Component1` コンポーネントをその名前空間を使用して参照します。

```razor
<h1>Hello, world!</h1>

Welcome to your new app.

<ComponentLibrary.Component1 />
```

または、[`@using`](xref:mvc/views/razor#using) ディレクティブを使用してライブラリをスコープ内に取り込み、名前空間なしでコンポーネントを使用します。

```razor
@using ComponentLibrary

<h1>Hello, world!</h1>

Welcome to your new app.

<Component1 />
```

必要に応じて、最上位の `_Import.razor` ファイルに `@using ComponentLibrary` ディレクティブを含めて、プロジェクト全体でライブラリのコンポーネントを使用できるようにします。 ディレクティブを任意のレベルの `_Import.razor` ファイルに追加して、名前空間をフォルダー内の 1 つのコンポーネントまたは複数のコンポーネントに適用します。

::: moniker range=">= aspnetcore-5.0"

`Component1` の `my-component`CSS クラスをコンポーネントに提供するには、フレームワークの `Component1.razor` 内の [`Link` コンポーネント](xref:blazor/fundamentals/additional-scenarios#influence-html-head-tag-elements)を使用して、ライブラリのスタイルシートにリンクします。

```razor
<div class="my-component">
    <Link href="_content/ComponentLibrary/styles.css" rel="stylesheet" />

    <p>
        This Blazor component is defined in the <strong>ComponentLibrary</strong> package.
    </p>
</div>
```

アプリ全体にスタイルシートを提供するには、アプリの `wwwroot/index.html` ファイル (Blazor WebAssembly) または `Pages/_Host.cshtml` ファイル (Blazor Server) 内でライブラリのスタイルシートにリンクすることもできます。

```html
<head>
    ...
    <link href="_content/ComponentLibrary/styles.css" rel="stylesheet" />
</head>
```

`Link` コンポーネントが子コンポーネントで使用されている場合、`Link` コンポーネントを持つ子がレンダリングされていれば、親コンポーネントのその他の子コンポーネントでもリンクされたアセットを使用できます。 子コンポーネントで `Link`コンポーネントを使用することと、`wwwroot/index.html` または `Pages/_Host.cshtml` に `<link>` HTML タグを配置することの違いは、フレームワーク コンポーネントのレンダリングされた HTML タグが次のようになることです。

* アプリケーションの状態によって変更できます。 ハードコーディングされた `<link>` HTML タグは、アプリケーションの状態によって変更することはできません。
* 親コンポーネントがレンダリングされなくなると、HTML `<head>` から削除されます。

::: moniker-end

::: moniker range="< aspnetcore-5.0"

`Component1` の `my-component`CSS クラスを提供するには、アプリの `wwwroot/index.html` ファイル (Blazor WebAssembly) または `Pages/_Host.cshtml` ファイル (Blazor Server) 内でライブラリのスタイルシートにリンクします。

```html
<head>
    ...
    <link href="_content/ComponentLibrary/styles.css" rel="stylesheet" />
</head>
```

::: moniker-end

## <a name="create-a-no-locrazor-components-class-library-with-static-assets"></a>静的アセットを含む Razor コンポーネント クラス ライブラリを作成する

RCL には、静的アセットを含めることができます。 静的アセットは、ライブラリを使用するすべてのアプリで使用できます。 詳細については、「<xref:razor-pages/ui-class#create-an-rcl-with-static-assets>」を参照してください。

## <a name="supply-components-and-static-assets-to-multiple-hosted-no-locblazor-apps"></a>複数のホスト型 Blazor アプリにコンポーネントと静的アセットを提供する

詳細については、「<xref:blazor/host-and-deploy/webassembly#static-assets-and-class-libraries>」を参照してください。

::: moniker range=">= aspnetcore-5.0"

## <a name="browser-compatibility-analyzer-for-no-locblazor-webassembly"></a>Blazor WebAssembly のブラウザー互換性アナライザー

Blazor WebAssembly アプリは完全な .NET API 領域を対象としていますが、ブラウザー サンドボックスの制約により、すべての .NET API が WebAssembly でサポートされているわけではありません。 サポートされていない API は、WebAssembly で実行すると <xref:System.PlatformNotSupportedException> がスローされます。 開発者が、アプリのターゲット プラットフォームでサポートされていない API をアプリで使用すると、プラットフォーム互換性アナライザーから警告を受け取ります。 Blazor WebAssembly アプリの場合、API がブラウザーでサポートされているかどうかが確認されるということです。 互換性アナライザーの .NET フレームワーク API に注釈を付けることは、進行中のプロセスであるため、現在、すべての .NET フレームワーク API に注釈が付けられるわけではありません。

Blazor WebAssembly および Razor クラス ライブラリ プロジェクトでは、`SupportedPlatform` MSBuild 項目でサポートされているプラットフォームとして `browser` を追加することで、ブラウザーの互換性チェックを "*自動的*" に有効にします。 ライブラリ開発者は、`SupportedPlatform` 項目をライブラリのプロジェクト ファイルに手動で追加して、この機能を有効にすることができます。

```xml
<ItemGroup>
  <SupportedPlatform Include="browser" />
</ItemGroup>
```

ライブラリを作成する場合は、<xref:System.Runtime.Versioning.UnsupportedOSPlatformAttribute> に `browser` を指定して、ブラウザーで特定の API がサポートされていないことを示します。

```csharp
[UnsupportedOSPlatform("browser")]
private static string GetLoggingDirectory()
{
    ...
}
```

詳細については、「[特定のプラットフォームでサポートされていない API に注釈を付ける (dotnet/designs GitHub リポジトリ)」](https://github.com/dotnet/designs/blob/main/accepted/2020/platform-exclusion/platform-exclusion.md#build-configuration-for-platforms)」参照してください。

## <a name="no-locblazor-javascript-isolation-and-object-references"></a>Blazor JavaScript の分離とオブジェクト参照

Blazor により、標準 [JavaScript モジュール](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Modules)で JavaScript の分離が有効にされます。 JavaScript の分離には、次のような利点があります。

* インポートされる JavaScript によって、グローバル名前空間が汚染されなくなります。
* ライブラリおよびコンポーネントのコンシューマーは、関連する JavaScript を手動でインポートする必要がありません。

詳細については、「<xref:blazor/call-javascript-from-dotnet#blazor-javascript-isolation-and-object-references>」を参照してください。

::: moniker-end

## <a name="build-pack-and-ship-to-nuget"></a>ビルド、パック、NuGet への配布

コンポーネント ライブラリは標準 .NET ライブラリであるため、それらをパッケージ化して NuGet に配布することは、ライブラリをパッケージ化して NuGet に配布する場合と変わりはありません。 パッケージ化は、コマンド シェルで [`dotnet pack`](/dotnet/core/tools/dotnet-pack) コマンドを使用して実行します。

```dotnetcli
dotnet pack
```

コマンド シェルで [`dotnet nuget push`](/dotnet/core/tools/dotnet-nuget-push) コマンドを使用して、パッケージを NuGet にアップロードします。

## <a name="additional-resources"></a>その他の技術情報

::: moniker range=">= aspnetcore-5.0"

* <xref:razor-pages/ui-class>
* [XML 中間言語 (IL) トリマーの構成ファイルをライブラリに追加する](xref:blazor/host-and-deploy/configure-trimmer)

::: moniker-end

::: moniker range="< aspnetcore-5.0"

* <xref:razor-pages/ui-class>
* [XML 中間言語 (IL) リンカーの構成ファイルをライブラリに追加する](xref:blazor/host-and-deploy/configure-linker#add-an-xml-linker-configuration-file-to-a-library)

::: moniker-end
