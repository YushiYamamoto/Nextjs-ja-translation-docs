---
description: 'クライアント側でのデータ取得に関する知識を深め、キャッシュの管理、データの再バリデーション、フォーカス追跡、一定間隔でのデータの再取得などを行うReactフックライブラリであるSWRの使い方について解説します。'
---

# クライアント側のデータ取得

クライアント側のデータ取得は、ページが SEO インデックスを必要としない場合、データを事前にレンダリングする必要がない場合、またはページのコンテンツを頻繁に更新する必要がある場合に便利です。サーバー側のレンダリング API とは異なり、クライアント側のデータ取得をコンポーネントレベルで使用できます。

ページレベルで行った場合、データはランタイムで取得され、データの変更に合わせてページの内容が更新されます。コンポーネントレベルで使用した場合、データはコンポーネントのマウント時に取得され、データの変更に合わせてコンポーネントの内容が更新されます。

クライアント側のデータ取得を使用すると、アプリケーションのパフォーマンスやページの読み込み速度に影響を与える可能性があることに注意が必要です。これは、データの取得がコンポーネントまたはページのマウント時に行われ、データがキャッシュされないためです。

## useEffectを使ったクライアント側のデータ取得

以下の例は、useEffect フックを使ってクライアント側でデータを取得する方法を示しています。

```jsx
function Profile() {
  const [data, setData] = useState(null)
  const [isLoading, setLoading] = useState(false)

  useEffect(() => {
    setLoading(true)
    fetch('api/profile-data')
      .then((res) => res.json())
      .then((data) => {
        setData(data)
        setLoading(false)
      })
  }, [])

  if (isLoading) return <p>読み込み中。</p>
  if (!data) return <p>プロファイルデータがありません。</p>

  return (
    <div>
      <h1>{data.name}</h1>
      <p>{data.bio}</p>
    </div>
  )
}
```

## SWR を使ったクライアント側のデータ取得

Next.js の開発チームは、[**SWR**](https://swr.vercel.app/)というデータ取得用の React フックライブラリを作成しました。クライアント側でデータを取得する際には SWR を使用することを**強くお勧めします。**SWR はキャッシュの管理、再検証、フォーカス追跡、一定間隔での再取得などを扱います。

上記の例と同じように、今度は SWR を使ってプロファイルデータを取得します。SWR は自動的にデータをキャッシュし、データが古くなったら再検証します。

 SWR の使用方法についての詳細は、[SWRのドキュメント](https://swr.vercel.app/docs/getting-started)をご覧ください。

```jsx
import useSWR from 'swr'

const fetcher = (...args) => fetch(...args).then((res) => res.json())

function Profile() {
  const { data, error } = useSWR('/api/profile-data', fetcher)

  if (error) return <div>読み込みに失敗しました。</div>
  if (!data) return <div>読み込み中。</div>

  return (
    <div>
      <h1>{data.name}</h1>
      <p>{data.bio}</p>
    </div>
  )
}
```

## 関連事項情報

次のステップや更に詳しい情報を知りたい場合は、以下に示すセクションを参照することをお勧めします。

<div class="card">
  <a href="/docs/routing/introduction.md">
    <b>ルーティング:</b>
    <small>Next.jsでのルーティングについては、こちらをご覧ください。</small>
  </a>
</div>