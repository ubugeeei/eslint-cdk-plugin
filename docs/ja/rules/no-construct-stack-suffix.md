---
title: eslint-cdk-plugin - no-construct-stack-suffix
titleTemplate: ":title"
---

# no-construct-stack-suffix

<div class="info-item">
  ✅ <a href="/ja/rules/#recommended-rules">recommended</a>
  を使用した場合、このルールが有効になります。
</div>

このルールは、コンストラクト ID およびスタック ID で `Construct` または `Stack` サフィックスの使用を禁止するものです。  
(このルールは `Construct` または `Stack` を継承したクラスにのみ適用されます)

コンストラクト ID に `Construct` が含まれていると、CDK の世界で止めるべき問題が CloudFormation テンプレートおよび AWS の世界に漏れてしまうため、好ましくありません。(スタック ID についても同様です)

## オプション

このルールには以下のプロパティを持つオプションがあります：

- `disallowedSuffixes` (デフォルト: `["Construct", "Stack"]`) - 禁止するサフィックスの配列。"Construct"、"Stack"、または両方を含めることができます。

---

#### 🔧 使用方法

```ts
// eslint.config.mjs
export default [
  {
    // ... some configs
    rules: {
      // デフォルト: "Construct" と "Stack" の両方のサフィックスを禁止
      "cdk/no-construct-stack-suffix": "error",

      // "Construct" サフィックスのみを禁止
      "cdk/no-construct-stack-suffix": [
        "error",
        { disallowedSuffixes: ["Construct"] },
      ],

      // "Stack" サフィックスのみを禁止
      "cdk/no-construct-stack-suffix": [
        "error",
        { disallowedSuffixes: ["Stack"] },
      ],
    },
  },
];
```

#### ✅ 正しい例

```ts
import { Construct } from "constructs";
import { Bucket } from "aws-cdk-lib/aws-s3";

export class MyConstruct extends Construct {
  constructor(scope: Construct, id: string) {
    super(scope, id);

    // ✅ "Construct" サフィックスが追加されていない場合は許可されます
    const bucket = new Bucket(this, "MyBucket");
  }
}
```

#### ❌ 不正な例

```ts
import { Construct } from "constructs";
import { Bucket } from "aws-cdk-lib/aws-s3";

export class MyConstruct extends Construct {
  constructor(scope: Construct, id: string) {
    super(scope, id);

    // ❌ "Construct" サフィックスを使用すべきではありません
    const bucket = new Bucket(this, "BucketConstruct");
  }
}
```
