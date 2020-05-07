graphql-transform-transformer
========================

A custom transformer of the amplify-cli. It can transform fields.

```
directive @transform(expression: String!) on FIELD_DEFINITION
```

## Usage

### 1. Install package

```
npm install graphql-transform-transformer -D
```

or

```
yarn add graphql-transform-transformer -D
```

### 2. Setup custom transformer

Edit `amplify/backend/api/<YOUR_API>/transform.conf.json` and append `"./graphql-transform-transformer"` to `transformers` field.

```
    "transformers": [
      "graphql-transform-transformer"
    ]
```

### 3. Use @transform directive

Append `@transform` to target fields.

```
type Post @model {
  id: ID!
  title: String! @transform(expression: ".toLowerCase().trim()")
  text: String
}
```

You can refer the field itself by `.`.
This transformer doesn't check type soundness, writing correct expressions are up to you.

#### 3.1 How it works

AppSync uses Apache Velocity for resolver mapping.
It supports basic operators like logical operators.
Besides, It supports Java methods. Thus, you can do whatever you want.
But I'm not sure which Java version is used inside AppSync, you may find missing methods.

| GraphQL Type | Java Class                                                                                |
| ------------ | ----------------------------------------------------------------------------------------- |
| ID           | [java.lang.String](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)       |
| String       | [java.lang.String](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)       |
| Int          | [java.lang.Integer](https://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html)     |
| Float        | [java.lang.Double](https://docs.oracle.com/javase/8/docs/api/java/lang/Double.html)       |
| Boolean      | [java.lang.Boolean](https://docs.oracle.com/javase/8/docs/api/java/lang/Boolean.html)     |
| Array        | [java.util.ArrayList](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html) |
| Enum         | [java.lang.String](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)       |


### 4. Export NODE_PATH

This step isn't necessary once https://github.com/aws-amplify/amplify-cli/pull/3236 merged.

```
export NODE_PATH=./node_modules
```

## License

Fork of https://github.com/amazon-archives/aws-reinvent-2019-mobile-workshops/tree/master/MOB405/01%20-%20Finished

ISC

   Copyright 2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   Copyright 2020 Hiroshi Ioka. All Rights Reserved.
