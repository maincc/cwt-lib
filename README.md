# cwt-lib (chain-web-token)

用于区块链身份认证的js库；  
js library for blockchain identity authentication;  

使用区块链的私钥对数据结构（{header: ... , payload: ...}）进行签名，生成cwt;  
Using the private key data structure of the blockchain ({header:... , payload: ... }) sign and generate cwt;  

使用区块链的公钥对cwt进行验证，证明其身份；  
Verify the cwt using the blockchain's public key to prove its identity;  

- [Synopsis](#Synopsis)
- [Installtion && import](#Installtion && import)
- [Quick sign](#Quick sign)
- [Decode](#Decode)
- [Class: ChainToken](#Class: ChainToken)
- [Class: EthWallet](#Class: EthWallet)
- [Class: RippleWallet](#Class: RippleWallet)

Synopsis
=========
`chain web token （缩写cwt)` 是有三部分组成 ` header.payload.Signature `,其中的 `Signature` 是对 ` header.payload `进行签名。  
`chain web token (abbreviated cwt)` is composed of three parts` header.payload.Signature `, where ` Signature ` is the signature of `header.payload`.

` cwt-lib ` 是基于 `jsontokens、crypto` 库进行扩展。  
` cwt-lib ` is based on the ` jsontokens, crypto ` library extension.

目前支持`ethereum`、`ripple`。  
`ethereum` and `ripple` are currently supported.

Installation && Import
======================
**Installation**

```shell
npm install @maincc/cwt-lib
```

**Import**

```js
import ChainToken from "@maincc/cwt-lib"
```

Quick Sign
==========
` quickSign(key: string, usr: string, chain: string, alg: string) `  

**syntax:** *const cwt = ChainToken.quickSign(key, usr, chain, alg)*  

**快速签名**; 通过私钥或者密钥、用户名、链名和算法快速生成cwt。  
** Quick Signature **; quickly generate cwt by privateKey or secret, usr, chain and alg.

Decode
======
` decode(token: string) `

**syntax:** *const decode = ChainToken.decode(cwt)*  

**解码**; 将生成的 cwt 进行解码。  
**Decode**; Decode the generated cwt.

Class: ChainToken
=================

- static **quickSign(key: string, usr: string, chain: string, alg: string)** => *用于快速签名生成cwt。*

`key:` 用于签名的私钥和密钥；`usr:` 用于验证身份的用户名；  

`chain:` cwt-lib支持的链名； `alg:` chain生成key的算法；

```js
const cwt = ChainToken.quickSign('sh3BFjgUiiN3MQvj9toyTZty3RepE', 'jc_ripple', 'ripple', 'secp256k1');

// eyJ4NWMiOlsiLS0tLS1CRUdJTiBQVUJMSUMgS0VZLS0tLS1cbk1GWXdFQVlIS29aSXpqMENBUVlGSzRFRUFBb0RRZ0FFWGZCeTNSaHhkNWliUjlXbDhhOVVYUDQ1SDM1M2NONkdcbkpSckt1VTNrbE5rWFNiNFJiNFZUMk0rNU9Za3dMV0tMWXE2MEJyZkFzL3d3YkQzTmlGbFNlZz09XG4tLS0tLUVORCBQVUJMSUMgS0VZLS0tLS0iXSwidHlwZSI6IkNXVCIsImNoYWluIjoicmlwcGxlIn0.eyJ1c3IiOiJqY19yaXBwbGUiLCJ0aW1lIjoxNzE4MTYzNTU0fQ.MEUCIFbRfg9tDyLMXY_qTFgpEvP3U6VZrYdR2p34Y3Hc_3WPAiEAuaPlEHEzBd-37wXFbH7wEyHm1R2tA-u64hRLdYV_UJY
```
- static **decode(token: string)** => *解码生成的cwt。*

`token:` 生成的cwt；

```js
const decoded = ChainToken.decode(cwt);

// {
//     header: {
//       x5c: [
//         '-----BEGIN PUBLIC KEY-----\n' +
//           'MFYwEAYHKoZIzj0CAQYFK4EEAAoDQgAEXfBy3Rhxd5ibR9Wl8a9UXP45H353cN6G\n' +
//           'JRrKuU3klNkXSb4Rb4VT2M+5OYkwLWKLYq60BrfAs/wwbD3NiFlSeg==\n' +
//           '-----END PUBLIC KEY-----'
//       ],
//       type: 'CWT',
//       chain: 'ripple'
//     },
//     payload: { usr: 'jc_ripple', time: 1718163554 },
//     signature: 'MEUCIFbRfg9tDyLMXY_qTFgpEvP3U6VZrYdR2p34Y3Hc_3WPAiEAuaPlEHEzBd-37wXFbH7wEyHm1R2tA-u64hRLdYV_UJY'
//   }
```

- **constructor (chain: string, alg: string, key: string, keyType: string)** => *构造函数。*  
 
`chain:` cwt-lib支持的链名； `alg:` chain生成key的算法； 

`key:` chain所需要的公钥、私钥或者密钥； `keyType:` key的种类**（private、public、secret）**；

- **sign(data: {header: any, payload: any}, format: string = "der")** => *进行签名。*  

`data:` 需要签名的数据结构； `format:` 指定cwt里的signature部分的格式；可以是`jose`和`der`,默认是`der`；

- **verify(token: string, format: string = "der")** => *对生成cwt验证。*

`token:` 生成的cwt； `format:` 指定cwt里的signature部分的格式；可以是`jose`和`der`,默认是`der`；

#### CODE:
```js
import { ChainToken } from "@maincc/cwt-lib";

const ripple = {
    secret: 'sh3BFjgUiiN3MQvj9toyTZty3RepE',
    address: 'rhhH4HZc3h5XtK3i1SbDtHzdUr9jseNPgd',
    privateKey: 'c541eaf57d65d5c842ef777cbab1fb8249fce14d5e4ab56e439ec78547a040e7',
    publicKey: '045df072dd187177989b47d5a5f1af545cfe391f7e7770de86251acab94de494d91749be116f8553d8cfb93989302d628b62aeb406b7c0b3fc306c3dcd8859527a'
  };
const data = {
  header: {
    x5c: [
      '-----BEGIN PUBLIC KEY-----\n' +
        'MFYwEAYHKoZIzj0CAQYFK4EEAAoDQgAEXfBy3Rhxd5ibR9Wl8a9UXP45H353cN6G\n' +
        'JRrKuU3klNkXSb4Rb4VT2M+5OYkwLWKLYq60BrfAs/wwbD3NiFlSeg==\n' +
        '-----END PUBLIC KEY-----'
    ],
    type: 'CWT',
    chain: 'ripple'
  },
  payload: { usr: 'jc_ripple', time: 1718163554 },
}
const chainToken = new ChainToken('ripple', 'secp256k1', ripple.secret, 'secret')
const cwt = chainToken.sign(data);
console.log(cwt);
console.log(ChainToken.decode(cwt));
console.log(chainToken.verify(cwt));

// eyJ4NWMiOlsiLS0tLS1CRUdJTiBQVUJMSUMgS0VZLS0tLS1cbk1GWXdFQVlIS29aSXpqMENBUVlGSzRFRUFBb0RRZ0FFWGZCeTNSaHhkNWliUjlXbDhhOVVYUDQ1SDM1M2NONkdcbkpSckt1VTNrbE5rWFNiNFJiNFZUMk0rNU9Za3dMV0tMWXE2MEJyZkFzL3d3YkQzTmlGbFNlZz09XG4tLS0tLUVORCBQVUJMSUMgS0VZLS0tLS0iXSwidHlwZSI6IkNXVCIsImNoYWluIjoicmlwcGxlIn0.eyJ1c3IiOiJqY19yaXBwbGUiLCJ0aW1lIjoxNzE4MTYzNTU0fQ.MEUCIFbRfg9tDyLMXY_qTFgpEvP3U6VZrYdR2p34Y3Hc_3WPAiEAuaPlEHEzBd-37wXFbH7wEyHm1R2tA-u64hRLdYV_UJY
// {
//   header: {
//     x5c: [
//       '-----BEGIN PUBLIC KEY-----\n' +
//         'MFYwEAYHKoZIzj0CAQYFK4EEAAoDQgAEXfBy3Rhxd5ibR9Wl8a9UXP45H353cN6G\n' +
//         'JRrKuU3klNkXSb4Rb4VT2M+5OYkwLWKLYq60BrfAs/wwbD3NiFlSeg==\n' +
//         '-----END PUBLIC KEY-----'
//     ],
//     type: 'CWT',
//     chain: 'ripple'
//   },
//   payload: { usr: 'jc_ripple', time: 1718163554 },
//   signature: 'MEUCIFbRfg9tDyLMXY_qTFgpEvP3U6VZrYdR2p34Y3Hc_3WPAiEAuaPlEHEzBd-37wXFbH7wEyHm1R2tA-u64hRLdYV_UJY'
// }
// true

```

Class: EthWallet
=================
继承@ethereumjs/wallet库，用于生成以太坊相关密钥对。  
Inherit the @ethereumjs/wallet library for generating Ethereum-related key pairs.

Class: RippleWallet
====================
对@swtc/wallet库进行扩展，用于生成ripple相关密钥对。  
Extension of the @swtc/wallet library for generating Ripple-related key pairs.


<br>