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
                <>
                    <#Header />
                    <#p>こんちわ！</p>
                </>
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
        type HeaderProps = {
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
        type Props = {
            title: string
        }
        const MyCompornent = ({ title }: props) => { とすることで、returnの中のhtmlタグの中身で{title}とすぐ使える
        分割代入していないと...
        const MyCompornent = (props: Props) => { 
        return <h1>{props.title}</h1>
        のようになり毎回「props.」と付けなくてはならなくて、面倒臭いから先に({ title }: props)としたほうがいい
    -型注釈について
        ({ title }: ...)の分割代入した後に「:」でその型を決める
        これにより、typeで指定した型をTypeScriptが読み取る
        つまり、先に...のtype指定もしていないとだめ
        type Props = { items: TodoItem[] , onToggle: (id: number) => void }などの「Props」を
        ...の箇所に書いて、型注釈を定義する
    -使い分け
        propsが多いとき(Form全体など) : 通常の props.title
        propsが少ないとき(buttonやcardなど) : 分割代入が推奨
    
##2-4.stateの型定義
    -useStateでは、型を正しく定義し状態管理を安定させること
    -TypeScriptと併用することで、型の自動補完・エラー防止・安全な状態操作が可能
    -明示的に型を指定 or 推論に任せるの使い分けが必要
    -useStateとは？
        Reactで「状態」を扱うためのHook
    -書き方
        const [count, setCount] = useState<number>(0);
        JSと違うのは= useState(初期値)の()前に <型> を入力すること
        無い場合は、TypeScriptが勝手に推論してくれる
    -型の指定一覧
        数値 : useState<number>(0) : カウンターなど
        文字列 : useState<string>("") : 入力フォームなど
        真偽値 : useState<boolean>(true/false) : チェック状態など
        配列 : useState<string[]>(["実際のリスト"]) : リスト表示
        オブジェクト : useState<user | null>(null) : ユーザー情報など
        関数 : useState<(() => void) | null>(null) : イベントハンドラーなど
    -型推論との使い分け
        型省略OK ver
            const [name, setName] = useState("naoto")
        型省略NG ver
            const [user, setUser] = useState<user | null>(null)
            //初期値がnullだと、型がnullとして推論されるため明示が必要
    -オブジェクトの型を扱う時の注意点
    (User.tsx)
        type User = {
            name: string
            age: number
        }
        const [user, setUser] = useState<User | null>(null)
        //更新時は全体を渡す事(部分更新ではない)
        setUser({ name: "直人", age: 26});
    -useStateで関数を扱うとき
        ユーザーがある操作をしたときに関数を呼び出し、後から登録・切り替えなどをするケース
        const [handler, setHandler] = useState<(() => void) | null>(null);
        () => voidは「引数・戻り値なしの関数」 
    -関数型一覧
        引数(-),戻り値(-) : () => void
        引数(+),戻り値(-) : (arg: string) => void
        引数(+),戻り値(+) : (id: number) => string
        //void = 空という意味
    -使用例
        const [onSend, setOnSend] = useState<(() => void) | null>(null)
        const sendMessage = () => {
            console.log("メッセージ")
        }
        useEffect(() => {
            setOnSend(() => sendMessage)
        },[])  
    -使用例2
    (ConfirmModal.tsx)
        type ConfirmModalProps = {
            onConfirm: () => void;
         };
    (App.tsx)
        const [confirmAction, setConfirmAction] = useState<(() => void) | null>(null);
        const handleDelete = (id: number) => {
            setConfirmAction(() => () => {  //これは高階関数で1つ目の () => は関数を返す関数
                deleteItem(id); // 削除処理
            });
            openModal(); // モーダルを開く
        };
        <ConfirmModal onConfirm={() => confirmAction?.()} />
         //省略記法でないver
        const [confirmAction, setConfirmAction] = useState<(() => void) | null>(null)
        const handleDelete = (id: number) => {
            const action = () => {
                deleteItem(id)
            }
            setConfirmAction(() => action);
        }
    -注意点
        setX(() => value は「valueをそのまま入れたいとき」に使う
        setX(() => () => {...} は「関数を状態として保存したいとき」に使う
        () => () => {...} は関数を返す高階関数である
        即時実行したくないときは、ネスト構造にすることでエラーを防ぐ

##2-5.イベントの型定義
    -イベントとは？
        ユーザーのアクションにより発火する処理(クリック,入力など)
        (例文)
        <button onClick={handleClick}>送信</button>
        <input onChange={handleInput} />
    -イベント型一覧
        onClick : React.MouseEvent<HTMLButtonElement> : ボタンなど
        onChange : React.ChangeEvent<HTMLInputElement> : テキスト入力欄
        onSubmit : React.FormEvent<HTMLFormElement> : フォーム送信
        onKeyDown : React.KeyboardEvent<HTMLInputElement> : キーボード操作
        onFocus : React.FocusEvent<HTMLInputElement> : フォーカス取得
        onBlur : React.FocusEvent<HTMLInputElement> : フォーカス喪失
    -ボタン使用例( Button.tsx , Form.tsx , etc... )
        const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => {
            e.preventDefault() //デフォルト動作防止用
            alert("クリックされました")
        }
    -入力欄使用例
        const handleChange = (e: React.ChangeEvent<HTMLInputElement> => {
            const value = e.target.value
            console.log(value)
        }
    -フォーム使用例
        const handleSubmit = (e: React.FormEvent<HTMLInputElement> => {
            e.preventDefault(
        )
    -TSX内での構成の考え方
        -e: React.MouseEvent<HTMLButtonElement> => void は
         Button.tsxのtype指定のところで、onClickという型の定義で書く
         外部にイベントをエクスポートするのであれば、そのイベントの型はButton.tsxで絶対に指定する
        -App.tsx側では書くのか？
            より安全にするのであれば、handleClick関数を作る時に(引数)のeに:型をつけたほうが安全
        -コード
            <Button text="..." onClick={handleClick} />
##2-6.条件分岐レンダリング
    -条件分岐の方法はif文か三項演算子(... ? 〜〜〜 : ¥¥¥)
    -if文
    (App.tsx)
        let content
        if (isLoggedIn) {
            content = <p>ログイン済み</p>
        } else {
            content = <p>ゲスト</p>
        }
        return <div>{content}</div>
        -ポイント
            -複雑な処理の時に分かりやすく推奨される
    -三項演算子
    (App.tsx)
        return (
            <>
                {isLoggedIn ? <p>ログイン済み</p> : <p>ゲスト</p>
            </>
        )
        -ポイント
            -1行でかけるのでスッキリ
            -ネストすると可読性が悪くなる
    -条件が1つだけでtrue or falseのみの場合
    (App.tsx)
        {isAdmin && <p>管理者</p>}
        -trueなら表示、falseなら非表示と条件が一分岐しかない時はこれがいい
    -使い分け
        単純 : 三項演算子
        中程度 : if文
        複雑 : swichや関数で分岐処理を定義
    -サンプルコード(ユーザーロールに応じた表示切替)
    (RoleBadge.tsx)
        type Props = {
            role: 'admin' | 'user' | 'guest' //typeはunionで分岐
        }
        const RoleBadge = ({ role }: Props) => {
            if (role === 'admin') return <p>管理者</p>
            if (role === 'user') return <p>一般ユーザー</p>
            return <p>ゲスト</p>
        }
    (App.tsx)
        <RoleBadge role="admin" /> //便宜上adminを入れてる

##2-7.リストレンダリングとkey
    -配列を使った複数JSXの描画
    -基本コード
        const fruits = ['apple','orange','grape']
        return (
            {fruits.map((fruits, index) => ) //.mapでfruitsを展開
                <#li key={index}>{fruits}</li> //keyはindexとなってる
            ))}
        )
    -keyは再レンダリング時の変化を明確にするために必要
    -keyがないと再描画・再利用のバグが起こる可能性あり
    -keyの型の付け方
        index : 配列の順番で使う : 並び替えや削除があるとバグに繋がる
        idなどのユニーク値 : DBのIDなどを使う : 推奨,安定性高い
        nameなどstring : 重複がないならOK : ユニークではないためkeyに不向き
    -推奨コード
        const user = [
            {id: 1, name:"直人"}
            {id: 2, name:"眞子"}
        ]
        return (
            {users.map((user) => (
            <#li key={user.id}>{user.name}</li>
        )
    -型定義付きの推奨コード
    (User.tsx)
        type User = { //各ユーザーの型(オブジェクト構造)を定義
            id: number,
            name: string
        ｝
        type Props = { //User.tsxに渡すPropsの型定義(User型の配列)
            users: user[]
        }
        const user = ({ users }: Props) => { //分割代入によるprops受け取りの型定義
        return (
            <#ul>
            {users.map((user) => ( //受け取った配列を.mapでループし<li>を出力
                <#li key={user.id}>{user.name}</li>
            ))}
            <#/ul>
        }
    (App.tsx)
    function App() {
        const data = [ //user型の配列
            {id: 26,name: "直人"}
            {id: 27,name: "眞子"}
        ]
        return(
            <#div>
                <h2>ユーザ一覧</h2>
                <User users={data} />
            <#/div>
        )
    }
    -まとめ
        -.mapを使い配列の要素をJSXとして描画できる
        -各要素にkeyを絶対につける
        -リストの定義はtype Props = { users: user[] }のように配列で定義

##Todoリスト作成
    -目的
        -typescriptの型定義について
            Type TodoItemやtype Propsによる厳格なデータ構造の定義
        -propsの受け渡し
            App → TodoListにデータ(配列)と関数(onToggle)を渡す設定
        -関数をpropsとして渡す
            onClick={() => onToggle(item:id)}で関数を動的に呼び出す仕組み
        -useStateの利用
            useState<TodoItem[]>で状態を型付きで管理。配列の更新もmapで処理
        -コンポーネントの再利用
            TodoListはpropsで動作が変わる汎用性の高い部品になっている
        -クリックでの状態変更
            completedを!completedに反転 → UIに反映
    -実際のコード
----(App.tsx)--------------------------------------
        import { useState } from "react";
        import TodoList from "./components/TodoList";
        import "./App.css";
    // 型定義 //
        type TodoItem = {
            id: number;
            title: string;
            completed: boolean;
        };
    // 初期情報とToggleイベント //
        function App() {
            const [todos, setTodos] = useState<TodoItem[]>([
                { id: 1, title: "洗濯", completed: false },
                { id: 2, title: "Reactの勉強", completed: false },
                { id: 3, title: "ゴミ出し", completed: false },
            ]);
            const toggleTodo = (id: number) => {
                setTodos((prevTodos) =>
                    prevTodos.map((todo) =>
                        todo.id === id ? { ...todo, completed: !todo.completed } : todo
                    )
                );
            };
    // 実際に表示させるUI部分 //
            return (
                <#div>
                    <h2>Todoリスト</h2>
                    <TodoList items={todos} onToggle={toggleTodo} />
                </#div>
            );
        }
        export default App;
----解説-------------------------------------------
        -Todoの状態をuseStateで管理し、その状態をTodoListコンポーネントに渡して表示、クリックで状態を切り替えるという処理
            1.TodoItem型の定義    
                type TodoItem = {
                    id: number;
                    title: string;
                    completed: boolean;
                }
            2.useStateでtodosを定義
                const [todos, setTodos] =useState<TodoItem[]>([
                    {id: 1,title: "洗濯",completed: false},
                    {id: 2,title: "勉強",completed: false},
                    {id: 3,title: "ゴミ出し",completed: false}
                ])
                -初期状態はTodoItemの配列で、useState<TodoItem[]>で配列の型定義
                -setTododsで状態を更新して、todosがUIの表示に使われる
            3.toggleTodo関数
                const toggleTodo = (id: number) => {
                    setTodos((prevTodos) =>
                        prevTodos.map((todo) =>
                            todo.id === id ? {...todo, completed: !todo.completed } : todo
                        )
                    )
                }
                -関数定義後の = () => { の ()内引数にid:numberを指定して型を意識
                -prevTodosは何かの処理の直前までの状態を示しており、これを参照することで今の状態を把握
                -...todoのスプレッド構文で非破壊更新をしつつ、completedを反転
                -setTodos((prevTodos) => { で新しい配列に更新して再描画させている
----(TodoList.tsx)---------------------------------
    // TodoItemの型定義 //
        type TodoItem = {
            id: number;
            title: string;
            completed: boolean;
        };
    // Propsの型定義 //
        type Props = {
            items: TodoItem[];
            onToggle: (id: number) => void;
        };
        const TodoList = ({items,onToggle}:Props)=>{
    // mapを使ってitems内の各要素であるitemを取り出し、liに表示 //
            return(
                <#ul>
                    {items.map((item)=>(
                        <#li
                            key={item.id}
                            onClick={() => onToggle(item.id)}
                            style={{cursor:"pointer",textDecoration: item.completed ? 'line-through' : 'none'}}>
                            {item.title}
                        </#li>
                    ))}
                </#ul>
            )
        }
        export default TodoList;
----解説-------------------------------------------
        -親コンポーネントから受け取ったTodoリストの配列と、クリック時の切り替え関数で、liに展開し、クリックでの状態管理
            1.TodoItem型の定義
                type TodoItem = {
                    id: number,
                    title: string,
                    completed: boolean
                };
                -親と同じ定義をここでも定義(定義用コンポーネントで共通化もOK)
            2.Props型の定義
                type Props = {
                    items: TodoItem[];
                    onToggle: (id:number) => void;
                };
                -itemsはTodoListの配列
                -onToggleはクリックさせたTodoのidを受け取って親側で状態を反転させる関数
            3.コンポーネントの定義と分割代入
                const TodoList = ({ items, onToggle ): Props) => {
                -Propsを直接分割代入で受け取ることでコードが簡略化できる
                -Propsという型注釈を作るために上で型を定義した
            4.JSXの返却(ulとmapによる展開)
                return (
                    <#ul>
                        {items.map((item) => (
                            <#li
                                key={item.id}
                                onClick={() => onToggle(item.id)}
                                style={{
                                    cursor: "pointer",
                                    textDecoration: item.completed ? "line-through" : "none",
                                }}
                            #>{item.title}</li>
                        ))}
                    <#/ul>
                );
                -mapは配列ループ用
                -key={item.id}は各要素を一意に識別。
                -onClick={() => onToggle(item.id)}はクリックされた要素のidを親に渡す
                -style={...}で要素の状態にの見た目を変えている

##3-1-1.useStateとは？
    -Reactの関数コンポーネント内での「状態」を定義・操作するためのHook
    -動的なUI変化を伴うページには必須
    -なぜ「状態」が必要なのか    
        -Reactアプリの画面上の見た目は全て「状態」に依存しているから
        -カウント=5 : 5と表示
        -入力値が"田中" : フォームに田中と表示
        -ログイン済み : マイページへのボタンなど
    -useStateの役割とは？
        -宣言 : 状態の名前と初期値を定義する
        -更新 : 状態を更新するための関数を取得する
        -再レンダリング : 状態が変わったら、自動で画面が再描写される
    -useStateの構造
        const [state, setState] = useState(初期値);
        -state : 現在の状態の値
        -setState : 状態を更新する関数
        -初期値 : 最初の状態の値
    -状態変更ができない理由
        let [count, setCount] = useState(0);
        count = 5;
        -このコードだと再レンダリングが走らないから更新されない
        setCount(5);
        -これだとコードが再レンダリング可能で、画面反映がされる
    -例コード(カウンター)
        function Counter(){
            const [count, setCount] = useState(0);
            return(
                <>
                    <#p>現在のカウント:{count}</p>
                    <button onClick={() => setCount(count + 1)}> +1 </button>
                </>
            )
        };
    -状態と再レンダリングの関係
        ボタンクリック
            ↓
        setState関数で状態更新
            ↓
        Reactが差分を検出
            ↓
        該当コンポーネントを再描画
            ↓
        画面が最新の状態に更新される
    -実務での使用例
        入力フォームの入力内容 : const [name, setName] = useState("");
        チェックボックスのON/OFF : const [isChecked, setIsChecked] = useState(false);
        商品一覧データ(配列) : const [item, setItem] = useState<Product[]>([]);

##3-1-2.useStateの基本構文
    -setStateとprev
        setState(prev => 新しい値);
        -前回の状態を基に新しい状態を決めるための関数型の書き方
        -複数更新が重なっても正確に処理するために絶対必要
    -基本構文と使い方
        const [count, setCount] = useState<number>(0);
        setCount((prevCount) => prevCount + 1)
    -なぜ必要か
        -Reactは状態更新(setState)は非同期的に実行されるため
        setCount(count + 1); // 0 + 1 = 1
        setCount(count + 1); // 0 + 1 = 1
        上記のように最終的にcountを2にしたいのに、count(初期値:0)に+1を繰り返してるだけで
        カウントアップされていかない。
        -正しい書き方
        setCount(prev => prev + 1); // 0(prev) + 1 = 1
        setCount(prev => prev + 1); // 1(prev) + 1 = 2
        この書き方であれば、最新の状態をprevで参照して、正確な処理ができる
    -実務例
        カウンターの増加 : setCount(prev => prev + 1);
        配列に要素を追加 : setItem(prev => [...prev, newItem]); //配列はprev => [...スプレット構文,追加要素]
        オブジェクトを部分的に更新 : setUser(prev => ({...prev, name: "直人"})); //objは(prev => ({...prev,type: "value"}))
        トグルの切り替え : setOpen : (prev => !prev); //!prevは反転を意味

#3-1-3.プリミティブ型の状態管理
    -プリミティブ型とは
        -string : フォーム入力,名前,コメント
        -number : カウント,価格,年齢
        -boolean : チェック状態,ログイン判定,表示制御
        これらのように単独で意味を持つ型
    -値を直接変更できないイミュータブル
    -useStateの型の書き方
        const [count, setCount] = useState<number>(0)
        const [isLoggedIn, setIsLoggedIn] = useState<boolean>(false)
        const [現在値, set関数] = useState<型>(初期値)
    -各プリミティブ型のコード例
        -string型
            const [inputText, setInputText] = useState<string>("");
            const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
                setInputText(e.target.value)
            }
        -number型
            const [quantity, setQuantity] = useState<number>(0)
            const increase = () => setQuantity(prev => prev + 1)
        -boolean型
            const [isOpen, setIsOpen] = useState<boolean>(false)
            const toggleOpen = () => setIsOpen(prev => !prev)





























   
