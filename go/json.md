# jsonのエンコーディングでーコーディング
## decode/encodeとmarshal/unmarshalの使い分けについて
* io.Readerのストリームからデータを取得する場合(http.Requestから取得する場合)  
decode/encodeを使用する
* 文字列データや、メモリ内のデータ
marshal/unmarshalを使用する