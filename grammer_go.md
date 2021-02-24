# Golang grammer

## print
```
fmt.Println("hello world")
fmt.Print("hello world") //改行なし
x := 1
fmt.Printf("x = %d\n", x) //変数のprint
```

最初の文字が大文字で始まる名前は外部パッケージから参照できる公開された名前  
ex: Pi // = 3.14159...

## function
```
func 関数名(引数) 戻り値 {
    return xxx
}
```
```
func add(x int, y int) int {
	return x + y
}
```
```
// 型が同じなら後ろにまとめて記載できる
func add(x, y int) int {
	return x + y
}
```
```
// 戻り値は複数指定できる
func swap(x, y string) (string, string) {
	return y, x
}
```
```
// 戻り値に名前を書くと returnのみで返せる（変数名書いてもOK）
func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
    // return x, y
}
```

## variables
varで変数宣言
```
// 最後に型名でまとめて型宣言
var c, python, java bool
// 変数毎に初期化可能　型
// 書かなかったら初期化したものの型になる
var i, j int = 1, 2
var c, python, java = true, false, "no!"

// := でvarを使わず暗黙的な型宣言ができる
// 関数の外では利用不可
var i, j int = 1, 2
k := 3
```
constで定数  
文字(character)、文字列(string)、boolean、数値(numeric)のみで使える  
:=での宣言は不可

## 型の種類
```
bool

string

// intは32-bitのシステムでは32 bitで、64-bitのシステムでは64 bit
int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // uint8 の別名

rune // int32 の別名
     // Unicode のコードポイントを表す

float32 float64

// 複素数 ex: g := 0.867 + 0.5i
complex64 complex128
```

変数に値を与えなかったらZero Valueになる
数値 : 0
bool : false
string : ""

変数 v, 型 Tがある場合
T(v)でキャストできる
```
i := 42
f := float64(i)
u := uint(f)
```

## for
```
for i := 0; i < 10; i++ {

}
```
```
// 1番目と3番目の記述は任意
sum := 1
for ; sum < 1000; {
    sum += sum
}
```
範囲for文
```
var x = []int{1, 2, 3, 4, 5}
for i, v := range x {
    fmt.Printf("i = %d v = %d\n", i, v)
}

// index, valueは_で捨てることもできる
for _, v := range x {
    fmt.Printf("v = %d\n", v)
}
for i, _ := range x {
    fmt.Printf("i = %d\n", i)
}
// 引数1つの場合はindexを参照する
for i := range x {
    fmt.Printf("i = %d\n", i)
}
```

## while
while文はforを使って書く
```
sum := 1
for sum < 1000 {
    sum += sum
}
```
無限ループ
```
for {

}
```

## if
if文
```
x := 1
if x > 0 {
    fmt.Println("ok")
}else{
    fmt.Println("ng")
}
```
if文の中で変数宣言もできる  
※if elseのスコープ内でのみ使用可
```
if x := 1; x > 1{
    fmt.Println("ok")
}else{
    fmt.Println("ng")
}
```

## switch
switch文  
※ breakは自動的に提供されるので書かなくていい  
※※ case は定数である必要はなく、 関係する値は整数である必要はない
```
x := 2
switch x {
case 1:
    fmt.Println("x = 1")
case 2:
    fmt.Println("x = 2")
default:
    fmt.Printf("other")
}
```
switchに条件を書かないと  
switch true の意味  
if-then-elseをシンプルに書く際に使用
```
t := time.Now()
switch {
case t.Hour() < 12:
    fmt.Println("Good morning!")
case t.Hour() < 17:
    fmt.Println("Good afternoon.")
default:
    fmt.Println("Good evening.")
}
```

## defer
deferで渡した関数の実行を、呼び出し元の関数の終わり(return時)まで遅延させる  
複数書いた場合LIFOで実行される
```
defer fmt.Println("XXX")
defer fmt.Println("world")
fmt.Println("hello")

// hello \n world \n XXXが出力される
```

## pointers
※ポインタ演算はない
```
x := 10
p = &x
fmt.Println(*p) // 10
*p = 20
fmt.Println(*p) // 20
fmt.Println(i)  // 20
```

## struct
```
type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	v.X = 4
	fmt.Println(v.X)

    // ポインタの時、(*p).Xの代わりにp.Xを使用できる
    p := &v
    p.X = 1e9
    fmt.Println(v)

    // 変数の一部だけ初期化する or 初期化しないこともできる
    v2 := Vertex{X: 1}
    fmt.Println(v2)
    v3 := Vertex{}
    fmt.Println(v3)

    // &を先頭につけると新しく宣言されたstructのポインタを戻す
    p := &Vertex{1, 2}
    fmt.Println(p.X)
}
```

## arrays
宣言とアクセス
※配列は固定長
```
var a [2]string
a[0] = "Hello"
a[1] = "World"
fmt.Println(a[0], a[1])
fmt.Println(a)

primes := [6]int{2, 3, 5, 7, 11, 13}
fmt.Println(primes)
```

## slices
可変長の配列  
宣言
```
x := []int{1,2,3,4}
fmt.Println(x)

//空のスライス宣言
var y []int
fmt.Println(y)
```
配列の一部取り出し
```
primes := [6]int{2, 3, 5, 7, 11, 13}

// a[left : right] : [left, right)の半開区間
var s []int = primes[1:4]
fmt.Println(s)
fmt.Println(primes[1:]) // 1から最後まで
fmt.Println(primes[:4]) // 0から3(4-1)まで
fmt.Println(primes[:])  // 全て
```
スライスの要素を変更すると元の配列も変更される
```
x := [5]int{0, 1, 2, 3, 4}
s := x[1:5]
s[0] = 2
fmt.Println(x)
```

スライスの長さ len(s)  
スライスの容量 cap(s)  
長さ : そのスライスに含まれる要素の数  
容量 : そのスライスの最初の要素から数えて、元となる配列の要素数
```
s := []int{2, 3, 5, 7, 11, 13}

var t = s[:0]
// len = 0, cap = 6
var u = s[2:4]
// len = 2, cap = 4
```
スライスのゼロ値はnil
```
var x = []int
fmt.Println(x) // []が出力される
if x == nil {
    fmt.Println("nil!") // nil!が出力される
}
```

makeを使ったスライスの宣言
```
// 値はゼロ値が入る
a := make([]int, 5)
// 第三引数でcapの指定ができる
b := make([]int, 1, 5)
```

2次元スライス
```
board := [][]string{
    []string{"_", "_", "_"},
    []string{"_", "_", "_"},
    []string{"_", "_", "_"},
}
board[0][0] = "X"
board[2][2] = "O"
```
要素の追加
```
var s []int
s = append(s, 0)
s = append(s, 1, 2) // 一度に複数要素appendできる
//capはかならずしもlenと連動せず、lenより大きくなることもある
```

## maps
mapのゼロ値はnilでキーの追加もできない  
makeで初期化する
```
// make(map[keyの型]valの型)
var mp = make(map[int]int)
mp[2] = 3
fmt.Println(x[2])

//構造体も型として選択できる
type Vertex struct {
	Lat, Long float64
}
var m = make(map[string]Vertex)
m["Bell Labs"] = Vertex{
    40.68433, -74.39967,
}
```
mapの初期代入
```
type Vertex struct {
	Lat, Long float64
}

var m = map[string]Vertex{
	"Bell Labs": Vertex{
		40.68433, -74.39967,
	},
	"Google": Vertex{
		37.42202, -122.08408,
	},
}

//トップレベルの型が単純な場合型名を省略できる
var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967},
	"Google":    {37.42202, -122.08408},
}
```
mapの操作
```
var x = make(map[int]int)
//追加
x[2] = 3
//削除
delete(x, 2) //第二引数はkey
//要素の存在の確認
v, ok := x[2] //okに存在するかどうかbool値が入る vは存在しなかった場合ゼロ値になる
```

## function values
関数自身も関数の引数や戻り値として利用できる
```
func compute(fn func(float64, float64) float64) float64 {
	return fn(3, 4)
}

func main() {
	hypot := func(x, y float64) float64 {
		return math.Sqrt(x*x + y*y)
	}
	fmt.Println(hypot(5, 12))

	fmt.Println(compute(hypot))
	fmt.Println(compute(math.Pow))
}
```

## function closures
変数に関数をバインド  
関数を実行することで変数を更新するような動作をする
```
func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}

func main() {
	pos:= adder()
    fmt.Println(pos(1))
	fmt.Println(pos(2))
}
```