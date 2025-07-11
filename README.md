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
    npm create vite@latest
  -ディレクトリに移動
    react-tsx-app
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
    function Header(){
      return <h1>hello world</h1>;
    };
    export default;
    これを使う側では
    <MyConpornent/>
  -TSXでの記述になると
    const Header = () => {
      return <h1>hello world</h1>;
    };
  -App.tsxでは
    import Header from './compornents/Header';
    function App(){
      return (
        <div>
          <Header />
          <p>こんちわ！</p>
        </div>
      );
    }
  -仕組み
   Headerというコンポーネントでは<h1>タグでHello Worldと表示
   それをApp.tsxに読み込んで、return()の()に<Header />として配置する
   そしたら、表示される。これは他のページでも使える→これが再利用！

##2-3-1.Propsとは？
  -親から子へ渡す情報のことで「データを下のコンポーネントに渡す道具」
  -JSXでの基本例
    -Header.jsx側《子》
      function Header(props) {
        return <h1>{props.title}</h1>
      }
      export default Header;
    -App.jsx側《親》
      function App() {
        return <Header title="こんちわ！" />
      }
  -流れ
   App.tsxがHeader.tsxにHeader title=で「こんちわ！」とpropsを渡している
   Header.tsxでは親で設定したtitleをpropsを引数に入れ、情報を受け取って{props.title}で展開してる
  -親で設定するより子で設定した方がスッキリしそう？
   結論、どちらでもOK！
　　親側(App.tsxやHome.tsx)で設定して、子側(button.tsx)はpropsで値を受け取るの流れのメリットは
　　複数のページで使うと、その都度表示したい内容など出てくる時に、button.tsxは編集せずに
　　親側で指定が出来るため、使いまわせる！スタイルの適用も1回で済む！
　　逆に1回しか使用されないものは子側(button.tsx)で値を指定した方が
　　親側は<Button />だけで済むメリットもある
　　<子コンポーネントを汎用化し、親コンポーネントで具体的に役割を付与>

##2-3-2.propsを受け取る構文(型なし)
  -JSX(型なし)での受け取り例
    -Header.jsx
    function Header(props){
      return <h1>{props.title}</h1>;
    }
    -App.jsx
    function App(){
      return <Header title="Hello World" />
    ;
  -Header.jsxで作ったh1タグの中身を、App.jsx(親コンポーネント)側でtitle="Hello World"と指定
  -{props.title}でもいいが、分割代入も覚えた方がいい
  -分割代入が推奨(後述)
  -JSXだけでprops管理をするとエラーに気づかず、補完機能もないため、TSXがいい

##2-3-3.typeを使ったpropsの型定義
  -構文(Header.tsx)
    type Props = {
      title: string
    }
    const Header = ({title}:props){
      return <h1>{title}</h1>
    }
  -type Props = {...}
    {}に囲われた中で、受け取る物(今回ならtitle)の型(:string,number,booleanなど)を指定
  -({title}: Props)
    ()の中に{}を入れ{...}の中に欲しいオブジェクトを入れる
    それを:Propsとすることで、titleの受け取る値をtype Propsで指定した物だけにする
    これのおかげで型のチェックがされる
  -型の設定ファイルを別で作って共通化するのもOK
    import {HeaderProps} from '../types/Header'みたいにしてもいい

##2-3-4.interfaceを使った型定義
  -interfaceはtype型定義と同様に「Propsの構造」を定義できる
  -継承,拡張性に優れてる(特に大規模プロジェクトなどで使われる)
  -保守・再利用・変更が関わってくるのであれば、使うべき
  -typeの書き方
    type Header Props = {
      title: string
    }
  -interfaceの書き方
    interface HeaderProps {
      title: string
    }
  -その後の使い方は変わらない
    const Header = ({title}: Props){
      return <h1>{title}</h1>
    }

  -拡張(extends)
    -利点
      共通の表示ロジックを共通型に集約
      他のコンポーネントでも同じ型定義を使いたくなる
      onClick,color,disabledなど機能と見た目の両方を管理したいなど
      同じプロパティが様々なコンポーネントで必要となる時に
      新しいProps extends 共通PropsでPropsの再利用が可能
    -コード
    //共通の項目
    interface BaseCardProps {
      id: string;
      createAt: Date;
    }
    //ユーザー用
    interface UserCardProps extends BaseCardProps {
      name: string
      avatarUrl: string
    }
    //商品用
    interface ProductCardProps extends BaseCardProps {
      productName: string
      price: number
    }

  -合成(&)
    -利点
      ロジック(機能)とデザイン(論理)を別で定義して合成できる
      既存の型と新しいものを組み合わせて1つのモジュール化しやすい
      分離することで意味が異なるということを明確にしやすい
    -コード
      type ActionProps {
        onClick: () => void;
      }
      type StyleProps {
        color: 'blue' | 'red'
        size: 'small' | 'large'
      }
      type ButtonProps = ActionProps & StyleProps

  -分岐(|union型)
    -利点
      -statusに応じてpropsの構造が変わるのに対応できる
      -statusの条件分岐で型が自動的に絞り込まれる
    (UserStatus.tsx)
      type LoadingState = {status: 'loading'}; //status = ローディング
      type ErrorState = {status: 'error',message: string}; //status = エラー
      type SuccessState = {status: 'success',user: { name: string; age: number }; //status = 成功
      type UserProfileProps = LoadingState | ErrorState | SuccessState;
      //条件分岐
      const UserProfile = (props:UserProfileProps) => {
        if (props.status === 'loading') return <p>読み込み中</p>
        if (props.status === 'error') return <p>エラー</p>
        return <p>ユーザー: {props.user.name}</p>
      }
    (App.tsx)
      import UserProfile from './UserStatus'
      function App() {
        const loading = false
        const error = false
        const props = loading
          ? {status: 'loading'}: error
          ? {status: 'error', message: 'エラー'} : {status: success, user: {name: '直人', age: 26}}
        return <UserProfile {...props}/>
      }

##2-3-5.Propsの分割代入と型注釈
-分割代入とは
  const MyCompornent = ({ title }: props) => { とすることで、returnの中のhtmlタグの中身で{title}とすぐ使える
  分割代入していないと...
  const MyCompornent = (props: Props) => { 
    return <h1>{props.title}</h1>
  のようになり毎回「props.」と付けなくてはならなくて、面倒臭い(
    














































   