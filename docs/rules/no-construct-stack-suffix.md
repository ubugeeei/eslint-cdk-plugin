---
title: eslint-cdk-plugin - no-construct-stack-suffix
titleTemplate: ":title"
---

# no-construct-stack-suffix

<div class="info-item">
  ✅ Using
  <a href="/rules/#recommended-rules">recommended</a>
  in an ESLint configuration enables this rule.
</div>

This rule is to disallow using the `Construct` or `Stack` suffix in construct IDs and stack IDs.  
(This rule applies only to classes that extends from `Construct` or `Stack`.)

If the Construct ID includes "Construct," the issues that should be stopped in the CDK world will leak into the CloudFormation template and the AWS world, so not good.(the same for Stack ID )

## Options

This rule has an options with the following properties:

- `disallowedSuffixes` (default: `["Construct", "Stack"]`) - An array of suffixes to disallow. Can include "Construct", "Stack", or both.

---

#### 🔧 How to use

```ts
// eslint.config.mjs
export default [
  {
    // ... some configs
    rules: {
      // Default: disallow both "Construct" and "Stack" suffixes
      "cdk/no-construct-stack-suffix": "error",

      // Disallow only "Construct" suffix
      "cdk/no-construct-stack-suffix": [
        "error",
        { disallowedSuffixes: ["Construct"] },
      ],

      // Disallow only "Stack" suffix
      "cdk/no-construct-stack-suffix": [
        "error",
        { disallowedSuffixes: ["Stack"] },
      ],
    },
  },
];
```

#### ✅ Correct Example

```ts
import { Construct } from "constructs";
import { Bucket } from "aws-cdk-lib/aws-s3";

export class MyConstruct extends Construct {
  constructor(scope: Construct, id: string) {
    super(scope, id);

    // ✅ When the suffix "Construct" is not added, it is permitted.
    const bucket = new Bucket(this, "MyBucket");
  }
}
```

#### ❌ Incorrect Example

```ts
import { Construct } from "constructs";
import { Bucket } from "aws-cdk-lib/aws-s3";

export class MyConstruct extends Construct {
  constructor(scope: Construct, id: string) {
    super(scope, id);

    // ❌ Shouldn't use the suffix "Construct"
    const bucket = new Bucket(this, "BucketConstruct");
  }
}
```
