# 1.常用汇编指令
## 1.通用数据传送指令 MOV 传送指令 
```
指令格式：MOV DST，SRC  ；(DST)←(SRC) DST表示目的操作数, SRC表示源操作数
```
说明：
1.DST为除CS外的各寄存器寻址方式或任意存储器寻址方式。SRC为任意数据寻址方式。

2.DST、SRC不能同时为存储器寻址方式，也不能同时为段寄存器寻址方式，而且在DST为段寄存器时，SRC不能为立即数。

3.MOV指令不影响标志位。
## 2.地址传送指令 
1.LEA——有效地址(EA)送寄存器指令 
```
指令格式：LEA  REG，SRC  ；(REG)←SRC 
```
说明：
1.指令把源操作数(只能是存储器寻址方式)指定的有效地址送到指令指定的16位或32位寄存器(REG)中(但不能是段寄存器)。

2.LEA指令不影响标志位。

## 3.加法指令 
1.ADD——加法指令 
```
指令格式：ADD  DST，SRC  ；(DST)←(DST)+(SRC)
```
## 4.减法指令 
#### 1.SUB——减法指令
```
指令格式：SUB  DST，SRC  ；(DST)←(DST)-(SRC)
```
#### 2.SBB——带借位减法指令
```
指令格式：SBB  DST，SRC   ；(DST)←(DST)-(SRC)-CF
```
## 5.除法指令
#### 1.DIV——无符号数除法指令 
```
指令格式：DIV SRC
```
字节操作：
```
(AL)←(AX)/(SRC)，(AH)←(AX)%(SRC)
```
字 操 作：
```
(AX)←(DX,AX)/(SRC)，(DX)←(DX,AX)%(SRC)
```
## 6.逻辑运算指令：可以对双字、字或字节执行按位的逻辑运算。
#### 1.AND——逻辑与指令
指令格式：AND  DST，SRC		；(DST)←(DST)∧(SRC)
#### 2.OR——逻辑或指令
指令格式：OR  DST，SRC		；(DST)←(DST)∨(SRC)
#### 3. XOR——逻辑异或指令
指令格式：XOR  DST，SRC		；(DST)←(DST)⊕(SRC)
#### 4.PUSH——进栈指令
指令格式：PUSH  SRC；16位指令：(SP)←(SP) –2    ((SP)+1,(SP))←(SRC)

说    明：
①.堆栈：计算机开辟的以“后进先出”方式工作的存储区。它必须存在于堆栈段中，只有一个出入口，所以只有一个堆栈指针SP或ESP。SP或ESP的内容在任何时候都指向当前的栈顶。
②.8086中的SRC不能为立即数寻址方式。286及其后继机型可用立即数寻址方式。
③.PUSH指令不影响标志位。
5.POPF/POPFD——标志出栈指令
```
指令格式：
POPF 				；(FLAGS)←((SP)+1,(SP))，(SP)←(SP)+2
POPFD				；(EFLAGS)←((ESP)+3, (ESP)+2, (ESP)+1, (ESP))，
(ESP)←(ESP) -4
```
说明：这组指令中LAHF、PUSHF/PUSHFD不影响标志位。但POPFD指令不影响VM，RF，IOPL，VIF和VIP的值。
## 7. 移位指令
#### 1.移位指令
1). SHL——逻辑左移指令
```
指令格式：SHL  OPR，CNT		；
```
2). SAL——算术左移指令
```
指令格式：SAL  OPR，CNT		；同上
3). SHR——逻辑右移指令
```
指令格式：SHR  OPR，CNT 		；
```
4). SAR——算术右移指令
```
指令格式：SAR  OPR，CNT		；
```
#### 2.循环移位指令
1). ROR——循环右移指令
```
指令格式：ROR  OPR，CNT		；
```
2). RCR——带进位位循环右移指令
```
指令格式：RCR  OPR，CNT		；
```
说明：
①.OPR为除立即数以外的任意寻址方式。移位次数由CL提供

②.CF位已在指令中给出其影响情况




