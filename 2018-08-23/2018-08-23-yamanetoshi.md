### [やまねとしあき](https://twitter.com/yamanetoshi)

- [今日やること](https://github.com/JavaKueche/great-okinawa/issues/34)

### 今回

[Go 言語でつくるインタプリタ](https://www.oreilly.co.jp/books/9784873118222/index.html)、のパーサーを真似て scheme のパーサー作ってみようと思ってます、の続きをやります。リポジトリは以下です。

- [scheme_interpreter](https://github.com/yamanetoshi/scheme_interpreter)

### 備忘

テストの実行が以下。

```
$ cd src/scheme_interpreter
$ go test ./parser
```

テストのデバッグが以下。

```
dlv test --  -test.run TestCreateTempFile
```

テストケースが記述されてるディレクトリに移動しておくこと。

### げげ

テスツに通っていない事が判明orz

と思ったら S 式評価の試験だけを追加している状態でした。

### デバッガで確認

とりあえず試験としては `(1 5)` という式を、という形。Parser オブジェクトは以下の状態になっています。

```
*scheme_interpreter/parser.Parser {
        l: *scheme_interpreter/lexer.Lexer {input: "(1 5)", position: 2, readPosition: 3, ch: 32},
        errors: []string len: 0, cap: 0, [],
        curToken: scheme_interpreter/token.Token {Type: "(", Literal: "("},
        peekToken: scheme_interpreter/token.Token {Type: "INT", Literal: "1"},}
```

parseProgram してみます。

```
*scheme_interpreter/ast.Program {
        Statements: []scheme_interpreter/ast.Statement len: 4, cap: 4, [
                ...,
```

要素をそれぞれ確認。

```
(dlv) p program.Statements[0]
scheme_interpreter/ast.Statement(*scheme_interpreter/ast.ExpressionStatement) *{
        Token: scheme_interpreter/token.Token {Type: "(", Literal: "("},
        Expression: scheme_interpreter/ast.Expression nil,}
(dlv) p program.Statements[1]
scheme_interpreter/ast.Statement(*scheme_interpreter/ast.ExpressionStatement) *{
        Token: scheme_interpreter/token.Token {Type: "INT", Literal: "1"},
        Expression: scheme_interpreter/ast.Expression(*scheme_interpreter/ast.IntegerLiteral) *{
                Token: (*scheme_interpreter/token.Token)(0xc42000e420),
                Value: 1,},}
(dlv) p program.Statements[2]
scheme_interpreter/ast.Statement(*scheme_interpreter/ast.ExpressionStatement) *{
        Token: scheme_interpreter/token.Token {Type: "INT", Literal: "5"},
        Expression: scheme_interpreter/ast.Expression(*scheme_interpreter/ast.IntegerLiteral) *{
                Token: (*scheme_interpreter/token.Token)(0xc42000e480),
                Value: 5,},}
(dlv) p program.Statements[3]
scheme_interpreter/ast.Statement(*scheme_interpreter/ast.ExpressionStatement) *{
        Token: scheme_interpreter/token.Token {Type: ")", Literal: ")"},
        Expression: scheme_interpreter/ast.Expression nil,}
```

これ、本当は `(1 . (5 . ()))` ってなっていないと、なのでしたっけ。つうか *ast とかも用意しないと、なのかな。あと試験をどう書かないといかんのかがアレ。

### quote な式

- program.Statements[1].(*ast.ExpressionStatement) な Token で判断してますね

quote 以外? の通常の S 式だと car を見て式を判断するのかな。とりあえず先頭要素で判断するのは良いのですが S 式、ってどうすりゃ良いのやら。

### 現状を再度確認

- program.Statements[0] の Expression は nil ですね
- なんか色々ヤバい香りがしますね

てことでこれは quote の試験もダウトだな。ひとまず S 式なデータ構造をでっちあげないと。

### ast の復習必要orz

ast.go が徹底的に駄目な気がしてきたのでそのあたり含めてリファクタします。

- Expression は S 式、ということで良いはず
- S 式の場合、parseExpression の戻りが Expression 属性に設定
- 現時点で nil 設定、String は "" を戻す

ふつうに考えるに Expression 属性、に S 式なオブジェクトが設定されてるべき、なのかなぁ。ええと S 式ですが

- car と cdr がある (ある、というかそれしかない)
- expressionNode というメソッドがあるな

これ、今って quote だとこうなってますが

```
(dlv) p program.Statements[0]
scheme_interpreter/ast.Statement(*scheme_interpreter/ast.ExpressionStatement) *{
        Token: scheme_interpreter/token.Token {Type: "(", Literal: "("},
        Expression: scheme_interpreter/ast.Expression nil,}
(dlv) p program.Statements[1]
scheme_interpreter/ast.Statement(*scheme_interpreter/ast.ExpressionStatement) *{
        Token: scheme_interpreter/token.Token {Type: "QUOTE", Literal: "5"},
        Expression: scheme_interpreter/ast.Expression nil,}
```

- 0 番目の Expression は nil では駄目ですね
- ここに S 式、なオブジェクトが格納されているべき
- quote の場合は quote な S 式?
- それ以外は car と cdr という属性があるべき?

以下なカンジなのかなぁ。

```
type SExpressionStatement struct {
	Expression car
	Expression cdr
}

func (ses *SExpressionStatement) statementNode()       {}
func (ses *SExpressionStatement) TokenLiteral() string { return "" }
func (ses *SExpressionStatement) String() string {
	if ses.Expression != nil {
		return "(" + ses.car.String() + "." + ses.cdr.String() + ")"
	}
	return "()"
}
```

statementNode ってメソドは一体何なのかと。

### TODO

- statementNode という手続きについて確認

あと、現時点の実装では

- ExpressionStatement は Expression を持っている
- Expression は識別子、定数、S 式 (になってないけど)
- 識別子の場合、Expression 属性が *ast.Identifier (属性が若干微妙?
- 定数の場合、Expression 属性が *ast.IntegerLiteral (これも酷いし属性も微妙

とりあえず

- 元の実装の ast な諸々を確認
- 今の Expression の考え方は良いはず

というところで反省会、な 1.5h でしたorz
