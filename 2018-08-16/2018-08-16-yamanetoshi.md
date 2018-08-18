### [やまねとしあき](https://twitter.com/yamanetoshi)

- [今日やること](https://github.com/JavaKueche/great-okinawa/issues/26)

### 今回

[Go 言語でつくるインタプリタ](https://www.oreilly.co.jp/books/9784873118222/index.html)、のパーサーを真似て scheme のパーサー作ってみようと思ってます。

### とりかかる

とりあえず、トークンを定義して解析できるようになると嬉しい。

### 要件

以下がパースできたら良いのかどうか。

```
5
'xxx
(define x 5)
(define x (lambda (x) (+ 1 x)))
(define add4 (let ((x 4)) (lambda (y) (+ x y))))
'''

### トークン

以下を定義なのかな。

- ILLEGAL
- EOF
- IDENT
- SYMBOL
- INT
- PLUS
- MINUS
- BANG
- ASTERISK
- SLASH
- LT
- GT
- LPAREN
- RPAREN
- DEFINE
- LET
- LAMBDA
- QUOTE

- あと、keywords も定義

### 字句解析

TestNextToken の定義が以下?

```
        input := `5
'xxx
(define x 5)
(define x (lambda (x) (+ 1 x)))
(define add4 (let ((x 4)) (lambda (y) (+ x y))))
`

        tests := []struct {
	        expectedType    token.TokenType
		expectedLiteral string
	}{
	        {token.INT, "5"},
		{token.SYMBOL, "xxx"},
		{token.LPAREN, "("},
		{token.DEFINE, "DEFINE"},
		{token.IDENT, "x"},
		{token INT, "5"},
		{token.RPAREN, ")"},
		{token.LPAREN, "("},
		{token.DEFINE, "DEFINE"},
		{token.IDENT, "x"},
		{token.LPAREN, "("},
		{token.LAMBDA, "LAMBDA"},
		{token.LPAREN, "("},
		{token.IDENT, "x"},
		{token.RPAREN, ")"},
		{token.LPAREN, "("},
		{token.IDENT, "+"},
		{token.INT, "1"},
		{token.IDENT, "x"},
		{token.RPAREN, ")"},
		{token.RPAREN, ")"},
		{token.RPAREN, ")"},
		{token.LPAREN, "("},
		{token.DEFINE, "DEFINE"},
		{token.IDENT, "add4"},
		{token.LPAREN, "("},
		{token.LET, "let"},
		{token.LPAREN, "("},
		{token.LPAREN, "("},
		{token.IDENT, "x"},
		{token.INT "4"},
		{token.RPAREN, ")"},
		{token.RPAREN, ")"},
		{token.LPAREN, "("},
		{token.LAMBDA, "LAMBDA"},
		{token.LPAREN, "("},
		{token.IDENT, "y"},
		{token.RPAREN, ")"},
		{token.LPAREN, "("},
		{token.IDENT, "+"},
		{token.IDENT, "x"},
		{token.IDENT, "y"},
		{token.RPAREN, ")"},
		{token.RPAREN, ")"},
		{token.RPAREN, ")"},
		{token.RPAREN, ")"},
	}

	l := New(input)
```

以下略。

### 字句解析の実装

- 元の lexer.go を踏襲
- Lexer はそのまま
- New もそのまま
- NextToken で云々するのは以下?
 - +
 - -
 - !
 - *
 - /
 - <
 - >
 - (
 - )
 - define
 - let
 - lambda
 - '

あと、パクる手続きが以下かな。

- skipWhitespace
- readChar
- newToken
- isLetter
- isDigit

あとは以下を検討なのかどうか

- readIdentifier
- LookupIdent
- isDigit
- readNumber

### てことで

最初の状態を作って commit を作ります。

### 字句解析

までの試験がようやくパスしたので一度この状態で commit を作ります。引き続きパーサの実装とテストを書きます。

### リポジトリ

以下です。

- https://github.com/yamanetoshi/scheme_interpreter