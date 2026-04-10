# About Token
SOL is Solana's native token.
It is used to run programs on the Solana blockchain — for example, to pay transaction fees and account rent.

# SPL (Solana Program Library) とは
Solana公式が提供している汎用プログラムの集まり。Ethereumでいう「ERC標準 + OpenZeppelin」のような位置付け。

代表的なプログラム:
- **SPL Token**: 独自トークンやNFTを発行するための標準プログラム。USDCなどもこれで発行されている。ERC-20 / ERC-721 に相当。
- **SPL Token-2022**: Token の拡張版。手数料徴収、転送制限、メタデータ埋め込みなど。
- **SPL Associated Token Account (ATA)**: ユーザーごとのトークン保管アカウントを決定論的に作る仕組み。
- **SPL Memo**: トランザクションに任意のメモを添付。

Solanaでは「コードと状態が分離」されているので、トークンを作るときに新しいスマートコントラクトをデプロイする必要はない。SPL Tokenプログラム1つが既にチェーン上にあり、誰でもそれを呼び出して新しいトークン (mintアカウント) を作れる。

# Mint / Token Account / ATA の違い

Solanaでトークンを扱うときに出てくる3つの概念。

## 1. Mint アカウント
**「トークンの定義」を持つアカウント**。トークンそのもののマスター情報。

- 持つ情報: 総供給量、小数点の桁数 (decimals)、mint権限を持つアドレス、freeze権限など
- 1つのトークン種別につき **1つだけ** 存在
- 例: USDC の Mint は `EPjFWdd5...` という固有アドレス。世界に1つ。

例えるなら「USドルという通貨そのものの設計図」。

## 2. Token アカウント
**「誰が、どのトークンを、何枚持っているか」を表すアカウント**。残高の入れ物。

- 持つ情報: どのMintに紐づくか、所有者 (owner)、保有数量
- 1人のユーザーが、1つのトークンに対して **複数持てる**
- Mint と Owner の組み合わせで残高が決まる

例えるなら「銀行口座」。1つのMint (通貨) に対して、ユーザーごとに別々の口座がある。

## 3. ATA (Associated Token Account)
**Token アカウントの「お決まりの場所」を決める仕組み**。

ふつうの Token アカウントは好きなアドレスに作れるが、それだと「あの人にUSDCを送りたいけど、どのToken アカウント宛てに送ればいいの？」と毎回聞かないといけない。

ATA は `(Owner address, Mint address)` から **決定論的にアドレスを計算** するルール。なので:

- 「Aさんの USDC を保管するATA」は計算で一意に決まる
- 送金する側は相手のWalletアドレスとMintさえ分かれば、送り先のToken アカウントが自動で割り出せる
- ウォレットアプリも全部これを使う標準

例えるなら「メイン口座の口座番号は、本人のIDから自動計算される」というルール。

## 図解

```
Mint (USDC定義)
   │
   ├── Token Account (Aさんの口座) ← ATA推奨
   │       owner: Aさん
   │       balance: 100
   │
   └── Token Account (Bさんの口座) ← ATA推奨
           owner: Bさん
           balance: 50
```

## CLIの例

```bash
spl-token create-token              # ① Mint を作る
spl-token create-account <MINT>     # ② 自分用の ATA (Token Account) を作る
spl-token mint <MINT> 100           # ③ 自分の ATA に100ミント
spl-token transfer <MINT> 10 <相手のWallet> --fund-recipient
                                    # ④ 相手の ATA に10送る (無ければ自動作成)
```

## まとめ
- **Mint** = トークンの定義 (1つだけ)
- **Token Account** = 残高入れ (人ごと・トークンごと)
- **ATA** = Token Account を「決まったアドレスに作る」ルール
