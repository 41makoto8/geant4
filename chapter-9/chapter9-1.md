# Chapter9. Analysis
## 9.1. Introduction
外部の分析用パッケージとGent4とプログラムをリンクすることなくGeant4から直接使用することのできる「軽量」な分析ツールをて提供する目的で、Geant4 ver9.5においてg4toolsを元にした新しい分析カテゴリーが導入された。このツールはアナライシスマネージャークラスで構成されており、g4toolsを含んでいる。

g4toolsはいくつかの形式 : ROOT, XML AIDA, CSVにおいてヒストグラムやnタプルを読み書きするコードを提供している。これは内部と外部のライブラリの一部であり、フィッテイングやプロッティングのような他の機能を含んでいる。

このアナライシスクラスは一定の規則に従った、g4toolsに対してユーザーに優しいインターフェースを提供しており、ユーザーから選択された出力の違いを隠すようにしている。それらはg4toolsのオブジェクト(ファイル、ヒストグラム、nタプル)を高いレベルで注意深く管理しており、メモリにおける王ジェクトの割当と消去を取り扱っていて索引を通じてそれらにアクセスする方法を提供している。それらは完全にGeant4にグレー無ワークに統合されている。それらはGeant4のコーディングスタイルに従い、ユーザーが使用するアナライシスオブジェクトを定義・設定することで使われる組み込みのGeant4のUIコマンドを実装している。

アナライシスマネージャークラスの使い方の一例が例B4のB4RunActionとB4EventActionで提供されている。