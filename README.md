# BUAA Complier #

## 北航计算机学院编译技术课程设计 ##

简单C0文法的编译器

使用语言：C++

GramOrign/ 目录下为未优化的版本

根目录下为优化后的版本

C0文法及其语义说明：

```c
＜加法运算符＞ ::= +｜-
＜乘法运算符＞  ::= *｜/
＜关系运算符＞  ::=  <｜<=｜>｜>=｜!=｜==
＜字母＞   ::= ＿｜a｜．．．｜z｜A｜．．．｜Z
＜数字＞   ::= ０｜＜非零数字＞
＜非零数字＞  ::= １｜．．．｜９
＜字符＞    ::=  '＜加法运算符＞'｜'＜乘法运算符＞'｜'＜字母＞'｜'＜数字＞'
＜字符串＞   ::=  "｛十进制编码为32,33,35-126的ASCII字符｝"
＜程序＞    ::= ［＜常量说明＞］［＜变量说明＞］{＜有返回值函数定义＞|＜无返回值函数定义＞}＜主函数＞
＜常量说明＞ ::=  const＜常量定义＞;{ const＜常量定义＞;}
＜常量定义＞   ::=   int＜标识符＞＝＜整数＞{,＜标识符＞＝＜整数＞}
                  | char＜标识符＞＝＜字符＞{,＜标识符＞＝＜字符＞}
＜无符号整数＞  ::= ＜非零数字＞｛＜数字＞｝| 0
＜整数＞        ::= ［＋｜－］＜无符号整数＞
＜标识符＞    ::=  ＜字母＞｛＜字母＞｜＜数字＞｝  //标识符和保留字都区分大小写
＜声明头部＞   ::=  int＜标识符＞ |char＜标识符＞
＜变量说明＞  ::= ＜变量定义＞;{＜变量定义＞;}
＜变量定义＞  ::= ＜类型标识符＞(＜标识符＞|＜标识符＞'['＜无符号整数＞']'){,(＜标识符＞|＜标识符＞'['＜无符号整数＞']' )} 
                 //＜无符号整数＞表示数组元素的个数，其值需大于0
                 //变量没有初始化的情况下没有初值  
＜类型标识符＞      ::=  int | char
＜有返回值函数定义＞  ::=  ＜声明头部＞'('＜参数表＞')' '{'＜复合语句＞'}'
＜无返回值函数定义＞  ::= void＜标识符＞'('＜参数表＞')''{'＜复合语句＞'}'
＜复合语句＞   ::=  ［＜常量说明＞］［＜变量说明＞］＜语句列＞
＜参数表＞    ::=  ＜类型标识符＞＜标识符＞{,＜类型标识符＞＜标识符＞}| ＜空＞
＜主函数＞    ::= void main‘(’‘)’ ‘{’＜复合语句＞‘}’
＜表达式＞    ::= ［＋｜－］＜项＞{＜加法运算符＞＜项＞}   //[+|-]只作用于第一个<项>
＜项＞     ::= ＜因子＞{＜乘法运算符＞＜因子＞}
＜因子＞    ::= ＜标识符＞｜＜标识符＞'['＜表达式＞']'|'('＜表达式＞')'｜＜整数＞|＜字符＞｜＜有返回值函数调用语句＞         //char 类型的变量或常量，用字符的ASCII 码对应的整数参加运算 
     //＜标识符＞'['＜表达式＞']'中的＜表达式＞只能是整型，下标从0开始
    //单个＜标识符＞不包括数组名，即数组不能整体参加运算，数组元素可以参加运算
＜语句＞    ::= ＜条件语句＞｜＜循环语句＞| '{'＜语句列＞'}'| ＜有返回值函数调用语句＞; 
                           |＜无返回值函数调用语句＞;｜＜赋值语句＞;｜＜读语句＞;｜＜写语句＞;｜＜空＞;|＜返回语句＞;
＜赋值语句＞   ::=  ＜标识符＞＝＜表达式＞|＜标识符＞'['＜表达式＞']'=＜表达式＞  
             //＜标识符＞＝＜表达式＞中的<标识符>不能为常量名和数组名
＜条件语句＞  ::= if '('＜条件＞')'＜语句＞［else＜语句＞］
＜条件＞    ::=  ＜表达式＞＜关系运算符＞＜表达式＞｜＜表达式＞ //表达式需均为整数类型才能进行比较，第二个侯选式中表达式为0条件为假，否则为真
＜循环语句＞   ::=  while '('＜条件＞')'＜语句＞| do＜语句＞while '('＜条件＞')' |for'('＜标识符＞＝＜表达式＞;＜条件＞;＜标识符＞＝＜标识符＞(+|-)＜步长＞')'＜语句＞     //for语句先进行条件判断，符合条件再进入循环体
＜步长＞::= ＜无符号整数＞  
＜有返回值函数调用语句＞ ::= ＜标识符＞'('＜值参数表＞')'
＜无返回值函数调用语句＞ ::= ＜标识符＞'('＜值参数表＞')'
＜值参数表＞   ::= ＜表达式＞{,＜表达式＞}｜＜空＞   
               //实参的表达式不能是数组名，可以是数组元素
              //实参的计算顺序,要求生成的目标码运行结果与Clang8.0.0 编译器运行的结果一致
＜语句列＞   ::= ｛＜语句＞｝
＜读语句＞    ::=  scanf '('＜标识符＞{,＜标识符＞}')'  
               //从标准输入获取<标识符>的值，该标识符不能是常量名和数组名
               //生成PCODE代码的情况：需要处理为一个scanf语句中，若有多个<标识符>，无论标识符的类型是char还是int，每输入一项均需回车
              //生成MIPS汇编的情况：按照syscall指令的用法使用即可
＜写语句＞    ::= printf '(' ＜字符串＞,＜表达式＞ ')'| printf '('＜字符串＞ ')'| printf '('＜表达式＞')'  
			//printf '(' ＜字符串＞,＜表达式＞ ')'
              //printf '(' ＜字符串＞,＜表达式＞ ')'输出时，先输出字符串的内容，再输出表达式的值，两者之间无空格
              //表达式为字符型时，输出字符；为整型时输出整数
              //＜字符串＞原样输出（不存在转义）
              //每个printf语句的内容输出到一行，按结尾有换行符\n处理
＜返回语句＞   ::=  return['('＜表达式＞')']    
		//无返回值的函数中可以没有return语句，也可以有形如return;的语句
         //有返回值的函数只要出现一条带返回值的return语句即可，不用检查每个分支是否有带返回值的return语句
                                        
另：关于类型和类型转换的约定：
1. 表达式类型为char型有以下三种情况：
 1）表达式由<标识符>或＜标识符＞'['＜表达式＞']构成，且<标识符>的类型为char，即char类型的常量和变量、char类型的数组元素。
 2）表达式仅由一个<字符>构成，即字符字面量。
 3）表达式仅由一个有返回值的函数调用构成，且该被调用的函数返回值为char型
  除此之外的所有情况，<表达式>的类型都是int
2. 只在表达式计算中有类型转换，字符型一旦参与运算则转换成整型，包括小括号括起来的字符型，也算参与了运算，例如(‘c’)的结果是整型。
3. 其他情况，例如赋值、函数传参、if/while条件语句中关系比较要求类型完全匹配，并且＜条件＞中的关系比较只能是整型之间比，不能是字符型，if ‘(’＜条件＞‘)’和while ‘(’＜条件＞‘)’里边，如果<条件>是单个表达式，则必须是整型。
```

