---
title: EF6 の Load メソッド
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
caps.latest.revision: 3
ms.openlocfilehash: 83af79220b52de6e3063868fd9bdac56867d49cb
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122315"
---
# <a name="the-load-method"></a>Load メソッド
それらのエンティティにすぐに何もせず、データベースからのコンテキストにエンティティを読み込む必要のあるいくつかのシナリオがあります。 これの良い例」の説明に従ってデータ バインディングのエンティティの読み込みが[ローカル データ](~/ef6/querying/local-data.md)します。 これを行う 1 つの一般的な方法は、LINQ クエリを記述し、それを作成するリストを直ちに破棄するだけで ToList を呼び出すです。 Load 拡張メソッドは、完全リストの作成を回避する点を除いて、ToList と同じように動作します。  

このトピックで紹介するテクニックは、Code First および EF Designer で作成されたモデルに等しく使用できます。  

負荷を使用して 2 つの例を示します。 最初は負荷が使用されている Windows フォーム データ バインド アプリケーションから取得」の説明に従って、ローカル コレクションにバインドする前にエンティティを照会する[ローカル データ](~/ef6/querying/local-data.md):  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

」の説明に従って、関連エンティティのフィルター選択されたコレクションを読み込むロードを使用して、2 番目の例に示す[関連エンティティの読み込み](~/ef6/querying/related-data.md):  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();
}
```  
