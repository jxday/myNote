## 适当处理null

> that junior to intermediate developers tend to face at some point: they either don't know or don't trust the contracts they are participating in and defensively overcheck for nulls. Additionally, when writing their own code, they tend to rely on returning nulls to indicate something thus requiring the caller to check for nulls.

there are two instances where null checking comes up:

1. Where null is a valid response in terms of the contract;
2. Where it isn't a valid response.

如果null是一个无效返回，则使用`assert`。

如果null是一个有效返回，可以返回空集合或数组，或者使用空对象模式。