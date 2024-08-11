# アパレルサイトにおけるレンダリング方法の使用場面

SWR については [こちら](swr_usage_updated.md)

## サーバーサイドレンダリング (SSR)

- **使用場面**：初回ページロードのパフォーマンスを向上させたい場合や、SEO を重視する場合。
- **具体例**：製品詳細ページ。初回ロード時に製品の全情報をサーバー側でレンダリングし、ユーザーに完全なページを提供します。これにより、SEO の効果も期待できます。

## 静的サイト生成 (SSG)

- **使用場面**：頻繁に更新されないページや、事前にビルドしてキャッシュできるページ。
- **具体例**：ホームページ、ブログ、ニュースページ。これらのページは頻繁に更新されないため、ビルド時に生成してキャッシュすることで、高速なページロードを実現できます。

## インクリメンタル・スタティック・リジェネレーション (ISR)

- **使用場面**：静的サイト生成の利点を活かしつつ、定期的にコンテンツが更新されるページ。
- **具体例**：特定のキャンペーンページやセール情報のページ。これらのページは定期的に更新される必要がありますが、ISR を使うことで、事前に生成した静的ページを使用しつつ、一定の間隔で再生成します。

## クライアントサイドレンダリング (CSR)

- **使用場面**：ユーザーがページ間を素早く移動できるようにしたい場合や、インタラクティブなコンポーネントが多い場合。
- **具体例**：製品検索結果ページやカテゴリーページ。ユーザーがフィルターを適用したり、ソート順を変更したりする際に、瞬時に結果を表示できます。

## ハイブリッドレンダリング

- **使用場面**：各ページの特性に合わせて、最適なレンダリング方法を組み合わせて使用したい場合。
- **具体例**：アパレルサイト全体。例えば、製品詳細ページは SSR でレンダリングし、ユーザーアカウントページは CSR でインタラクティブにし、ホームページやブログは SSG で高速化する、といった具合にページごとに適したレンダリング方法を組み合わせます。

## 具体的な実装例

### 製品詳細ページ (SSR)

```jsx
// pages/product/[id].js
import { getProductById } from "../lib/api";

export async function getServerSideProps({ params }) {
  const product = await getProductById(params.id);
  return { props: { product } };
}

export default function ProductPage({ product }) {
  return (
    <div>
      <h1>{product.name}</h1>
      <p>{product.description}</p>
      <p>${product.price}</p>
    </div>
  );
}
```

### ホームページ (SSG)

```jsx
// pages/index.js
import { getFeaturedProducts } from "../lib/api";

export async function getStaticProps() {
  const featuredProducts = await getFeaturedProducts();
  return { props: { featuredProducts } };
}

export default function HomePage({ featuredProducts }) {
  return (
    <div>
      <h1>Welcome to Our Store</h1>
      <ul>
        {featuredProducts.map((product) => (
          <li key={product.id}>{product.name}</li>
        ))}
      </ul>
    </div>
  );
}
```

### 製品リストページ (CSR)

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
