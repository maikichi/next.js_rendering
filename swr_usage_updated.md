# アパレルサイトにおける SWR の使用場面

## 使用場面

1. **製品リストの表示**

   - **使用場面**：カテゴリーページや検索結果ページで、製品のリストを表示する際に SWR を使用します。最初にキャッシュされたデータを表示し、その後最新のデータをバックグラウンドでフェッチすることで、ユーザーにスムーズな体験を提供します。
   - **具体例**：カテゴリーページ、検索結果ページ。ユーザーがフィルターを適用したり、ソート順を変更したりする際に、瞬時に結果を表示できます。

2. **製品詳細ページ**

   - **使用場面**：特定の製品の詳細ページで、製品情報や在庫状況、レビューなどのデータをフェッチするために SWR を使用します。これにより、ページロード時に最新の製品情報を取得しつつ、ユーザーには素早くデータを表示することができます。
   - **具体例**：製品詳細ページ。製品の詳細情報やレビューをリアルタイムで取得し、表示します。

3. **ユーザーのカート情報**

   - **使用場面**：ユーザーのカートに入っているアイテムの情報を SWR で管理し、常に最新のカート状態を表示します。例えば、他のページでカートにアイテムを追加した場合でも、カートページに戻ると自動的に更新された情報が表示されます。
   - **具体例**：カートページ。カートに追加されたアイテムや総計をリアルタイムで表示します。

4. **在庫状況のリアルタイム更新**

   - **使用場面**：在庫が頻繁に変動するアパレル商品では、在庫状況を定期的に更新することが重要です。SWR を使用して在庫情報をリアルタイムでフェッチし、ユーザーに正確な在庫状況を提供できます。
   - **具体例**：製品詳細ページ。商品の在庫状況をリアルタイムで表示します。

5. **ユーザープロフィールと注文履歴**

   - **使用場面**：ログインユーザーのプロフィール情報や過去の注文履歴を表示するページでも SWR を利用できます。ユーザーがプロフィール情報を更新した場合や、新しい注文が追加された場合にも、SWR がバックグラウンドで最新データを取得して更新します。
   - **具体例**：プロフィールページ、注文履歴ページ。ユーザーのプロフィール情報や注文履歴をリアルタイムで表示します。

6. **フィルターとソート機能**
   - **使用場面**：商品リストのフィルターやソート機能を使用する際に、ユーザーの選択に応じてデータをフェッチし、即座に更新された結果を表示するために SWR を使用します。
   - **具体例**：カテゴリーページや検索結果ページ。フィルターやソートの結果を即座に表示します。

## 具体的な実装例

### 製品リストページ (SWR)

```jsx
// components/ProductList.js
import useSWR from "swr";

const fetcher = (url) => fetch(url).then((res) => res.json());

export default function ProductList() {
  const { data, error } = useSWR("/api/products", fetcher);

  if (error) return <div>Failed to load products</div>;
  if (!data) return <div>Loading...</div>;

  return (
    <ul>
      {data.map((product) => (
        <li key={product.id}>{product.name}</li>
      ))}
    </ul>
  );
}
```

### 製品詳細ページ (SWR)

```jsx
// components/ProductDetail.js
import useSWR from "swr";

const fetcher = (url) => fetch(url).then((res) => res.json());

export default function ProductDetail({ productId }) {
  const { data, error } = useSWR(`/api/products/${productId}`, fetcher);

  if (error) return <div>Failed to load product details</div>;
  if (!data) return <div>Loading...</div>;

  return (
    <div>
      <h1>{data.name}</h1>
      <p>{data.description}</p>
      <p>${data.price}</p>
    </div>
  );
}
```

### カート情報の管理 (SWR)

```jsx
// components/Cart.js
import useSWR from "swr";

const fetcher = (url) => fetch(url).then((res) => res.json());

export default function Cart() {
  const { data, error } = useSWR("/api/cart", fetcher);

  if (error) return <div>Failed to load cart items</div>;
  if (!data) return <div>Loading...</div>;

  return (
    <div>
      <h2>Your Cart</h2>
      <ul>
        {data.items.map((item) => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
      <p>Total: ${data.total}</p>
    </div>
  );
}
```

### 在庫状況のリアルタイム更新 (SWR)

```jsx
// components/StockStatus.js
import useSWR from "swr";

const fetcher = (url) => fetch(url).then((res) => res.json());

export default function StockStatus({ productId }) {
  const { data, error } = useSWR(`/api/stock/${productId}`, fetcher);

  if (error) return <div>Failed to load stock status</div>;
  if (!data) return <div>Loading...</div>;

  return (
    <div>
      <p>Stock Status: {data.inStock ? "In Stock" : "Out of Stock"}</p>
    </div>
  );
}
```
