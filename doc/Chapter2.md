# 1章 名前に情報を詰め込む


| KeyWord |
|:------------|
| 名前に情報を詰め込む |
| 気取った言い回しよりも明確で正確なほうがよい |

変数，関数，クラスでも名前はそれだけでも短いコメント（情報がある）

## 2.1 明確な単語を選ぶ
名前に情報を詰め込むには明確な単語を選ぶ必要がある．

getのようなものは明確ではない

```c
int Getpage()
{
  ...
}
```
Getpageからはなにも情報がない．どこから取ってきたものなのか．DB？キャッシュ？ネット？

ネットからデータを取得するのであれば，Fetchpage()とかDownloadpageとかにしたほうがわかりやすい．

```c
class BinaryTree{
  int size();
}
```
この場合の返り値は何を指しているのか．ツリーの深さ？ノード数？Sizeだけでは何も伝わらない．名前をつけるならNumNodeとか．

他にも何かを停止させる関数であったときに
```c
class Thread{
  void stop();
}
```
これは並列処理を停止させるだけ？killしないのかとかわからない．関数の役割に応じた名前をつけよう

### もっとカラフルな名前をつける
類似辞典を用いて，もっと良い名前がないか調べる．ただ，splitとexplodeは何かを分割させる意味だが，それの区別はつかない．

## 2.2 tmpやretvalなどの汎用的な名前を避ける
汎用的な変数名ではなく，<font color="Red">エンティティの値や目的を表した名前を使用する</font>

```c
int function (std::vector<int> cArray)
{
  int retval = 0;
  for(int i = 0; i < cArray.size(); ++i)
  {
    retval += cArray[i] * cArray[i];
  }
  return retval;
}
```

良い変数名=変数の目的や値を表すもの
だから今回の場合は，retval →　sun_squaresとかにすれば良い

<font color=Red>retvalという変数名には情報が無い．変数の値を表すような名前を使用する</font>

### tmp
汎用的な名前も使用方法をきちんとすれば良い

```c
if(right < left)
{
  tmp   = right;
  right = left;
  left  = tmp;
}
```
上記のような場合，tmpは本当に一時的なものとして保管される役割を持っているだけ．生存期間も短い．

つまり，今回のtmpは渡された値に「操作」をして変更しない点があげられる

次の例ではそれが行われていない．

```c
std::string tmp = user.name;
tmp += " " + user.phone;
tmp += " " + user.email;
std::cout << tmp << std::endl;
```
生存期間はさっきの例と同様に短いが，ここで必要なのは一時的な保存以外の役割を持っていること．tmp→user_infoとかにすれば，わかりやすい

```c
tmp_file = getFileName();
...
saveData(tmp_file);
```
上記のtmp_fileをtmpにすると，ファイル名なのかデータなのか見分けがつかない

<font color = Red>tmpという変数名は生存期間が短く（使用する行が短い），役割として一時的な保管がある場合のみだけ使用する</font>

### ループイテレータ
イテレータ等はiとかjとかkとかiterとかよく使われるものだが，バグの発見するのが困難になるときがある．
そのような場合は使用する配列の変数名の頭文字を使用すると良い

```c
for (int i = 0; i < clubs.size(); ++i)
 for (int j = 0; j < clubs[i].members.size(); ++j)
  for (int k = 0; k < users.size(); ++k)
   if(clubs[i].members[k] == users[j]) #iとjが逆
   std::cout << "user[" << j << "] はclub[" << i << "]" << std::endl;
```

実際にこれを実行するとインデックスが逆になっている．これを目視して確認するのは難しいため，以下のように変更する．

```c
if(clubs[i].members[k] == users[j])
```
→
```c
if(clubs[ci].members[ui] == users[mi])#バグですね
```
とすると一発で，何かがおかしいとわかる．
```c
if(clubs[ci].members[mi] == users[ui])#頭文字が同じ
```

### 汎用的な名前のまとめ
<font color=Red>tmp,it，retvalのような汎用的な名前を使用するときは，理由もきちんと考えよう</font>

日々，変数名をきちんと考える習慣をつけて，命名力を鍛えよう
