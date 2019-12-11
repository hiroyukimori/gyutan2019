# Demo- Blazor

## Blazor

* BlazorとはC#でインタラクティブなWebユーザーインターフェースを記述するためのフレームワーク
* .NETの技術を利用して処理を記述することができるため、サーバーとクライアントでロジックの共有が可能
* WebAssemblyによってCoreCLRを実行する仕組みを実現しているため、モバイルブラウザーを含めた広範囲のブラウザーで動作する
* [Blazor](//blazor.net)
* [Blazorの概要](https://docs.microsoft.com/ja-jp/aspnet/core/blazor/?view=aspnetcore-3.0?WT.mc_id=DT-MVP-4000668)

### 準備

* .NET Core 3.1 SDK をインストール
* Visual Studio をインストール
* Visual Studio Codeをインストール

### demo

Blazorのテンプレートを作成、実行

#### Blazor Serverアプリを作成する場合

``` commandline
dotnet new blazorserver
dotnet run
```



VSCodeを起動

``` commandline
code .
```

1. PagesフォルダーのIndex.razorを開く
2. テーブルを追加

``` html
<table>
    <tr>
        <th>Datetime</th>
        <th>Memo</th>
    </tr>
    <tr>
        <td></td>
        <td></td>
    </tr>
</table>
```

1. コードを追加
2. 値を保持するための変数宣言を行う

``` razor
@code{
    Dictionary<string, string> memoItems = new Dictionary<string, string>();
    string memo = string.Empty;
}
```

1. コードの上にメモの入力用フォームを追加

``` html
<form>
    <input />
    <button>Add</button>
</form>
```

1. フォームのメモを追加するハンドラを追加


``` csharp
    void AddMemo()
    {
        memoItems.Add(DateTime.Now.ToLongTimeString(), memo);
        memo = string.Empty;
    }
```

1. inputにbindを追加
2. formにsubmit時の関数を指定

``` html
<form @onsubmit="AddMemo">
    <input @bind="memo"/>
    <button>Add</button>
</form>
```

1. tableに明細を表示するようコードを追加

``` razor
    @foreach (var item in memoItems)
    {
        <tr>
            <td>@item.Key</td>
            <td>@item.Value</td>
        </tr>
    }
```

### 完成

``` razor
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<table>
    <tr>
        <th>Datetime</th>
        <th>Memo</th>
    </tr>
    @foreach (var item in memoItems)
    {
        <tr>
            <td>@item.Key</td>
            <td>@item.Value</td>
        </tr>
    }
</table>

<form @onsubmit="AddMemo">
    <input @bind="memo"/>
    <button>Add</button>
</form>

@code{
    Dictionary<string, string> memoItems = new Dictionary<string, string>();
    string memo = string.Empty;

    void AddMemo()
    {
        memoItems.Add(DateTime.Now.ToLongTimeString(), memo);
        memo = string.Empty;
    }
}
```

#### Blazor WebAssembly アプリを作成したい場合

Preview版のテンプレートをインストールする

``` commandline
dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
```

Visual Studio またはdotnet CLIで以下のように指定してプロジェクトを作成

``` commandline
dotnet new blazorwasem
```

ただし、Blazor v3.1.0-preview4.19579.2は、現在プロジェクトがビルドできなくなるというIssueが立っています。

Blazor WebAssembly のPreview4.19579.2がビルドできない問題に対するIssue(英語)
https://github.com/aspnet/AspNetCore/issues/17619

執筆時点の回避策としては、 .NET Standard 2.0およびBlazor v3.1.0-preview3.19555.2 に戻すか、 BlazorLinkOnBuildオプションを無効(false)にすることで動作させることができるようになります。

BlazorLinkOnBuildオプションはVSなどでデバッグするときに使用するデバッグシンボルを埋め込み、中間言語(IL)とのリンクを行うオプションなので、動作させるだけならオフにしても問題ありません。
