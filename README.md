# React-TSX-Practice
##1-1.Reactとは
  -UIを構築するライブラリ
  -UIを状態によって管理する
  -コンポーネントで部品分けして再利用する
  -仮想DOMを使って差分を検出して再描画
  -propsは親から子コンポーネントへ情報を渡す
  -UIは状態とpropsで描かれる

##1-2.TypeScriptとは
  -JavaScriptの上位互換
  -型を事前に指定するためコードが安定する
  -型にはnumber,string,array,object,関数などがある

##1-3.TSXとは
  -TSXはTypeScriptでJSX構文を扱う拡張構文
  -JSXがHTMLライクに書けるものに、型を付けたTSX進化版
    type Props = {
      name: string;
    }
    function Hello({name}:props){
      return <p>Hello,{name}</p>
    }
    このようにfunctionの前にtypeを別でしっかり指定する
  -TSXの強みはこのtype指定がされるため、numberが入るとエラーになる
  -propsだと
    type props = {text: string}
  -useStateだと
    useState<number>(初期値)
  -イベントだと
    onclick:(e:React.MouseEvent<>) => void
  -注意点
    -型が多くて難しくコードも長くなる
    -コンポーネントに正確な型を付けないと再利用不可のため、propsとstateを明確に設計する
    -タグの閉じ忘れや式の書き方に注意

##1-4.React + TypeScript開発環境構築(vite)
  -Node.jsを使用する(v18以上)
  -ターミナルを開いて以下を実行
    npm create vite@latest react-tsx-app(実際のプロジェクト名) -- --template react-ts
  -ディレクトリに移動
    cd react-tsx-app
  -必要なパッケージをインストール
    npm install
  -開発サーバーを起動
    npm run dev
  -フォルダ構成の概要
  -react-tsx-app
    |―public/          #静的ファイル
    |―src/
    | |―compornents/   #コンポーネントフォルダ
    | | |―header.tsx
    | | |―button.tsx
    | |―pages/         #画面単位※SPAの場合
    | |―App.tsx        #ルートコンポーネント
    | |―main.tsx       #エントリーポイント(ReactDOM.createRoot)
    | |―vite-env.d.ts  #型定義
    |―index.html       #HTMLテンプレート
    |―package.json     #TypeScript設定
    |―tsconfig.json    #npm設定

##2-1.TSXとJSXの違い
  -HTMLライクに書けるJSXに型を加えた拡張版

##2-2.コンポーネントの作り方(関数)
  -Reactでは関数を使ってUIの部品を作る
   これを「関数型コンポーネント」という
  -基本構文
    function MyConpornent(){
      return <h1>hello world</h1>;
    };
    export default;
    これを使う側では
    <MyConpornent/>
  -TSXでの記述になると
    const MyConpornent = (): JSX.Element => {
      return <h1>hello world</h1>;
    };
  -(): JSX.Elementは「JSXを返す関数」の型注釈
