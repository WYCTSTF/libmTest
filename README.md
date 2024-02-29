# 以下开发与测试流程为remainder，其他函数的开发测试工作可参考此流程

## 开发使用流程
1. `src/remainder.c、remainderl.c、remainderf.c`为实现文件，分别对应`double、long double、float`类型的`remainder`函数， `src/include/remainder.h`为头文件。
2. `src/fprem1.c`为`fprem1`指令实现文件，`src/include/fprem1.h`为头文件
3. 在`test`目录下执行以下命令，生成可执行文件
```shell
cd test && make REMAINDERF=TRUE && make REMAINDER=TRUE && make REMAINDERL=TRUE
```
1. 如make成功将会在`test/output`中得到对应的可执行文件，用于测试
## 测试使用流程
### 生成测试数据
1. 在`x86`服务器使用`data/gen_two_double_input_hex.py`文件，生成测试数据文件
2. 使用以下命令生成测试数据文件
```shell
python gen_two_double_input_hex.py
```
1. 或可以直接使用`data`目录下已生成的测试数据文件
### 生成对比数据文件
#### 生成`float`型对比数据文件
1. 将`input_two_float_hex.dat`测试用例文件夹复制到`x86`服务器
2. 在`x86`目录复制到`x86`服务器
3. 使用以下命令生成对比数据文件
```shell
cd x86 && make REMAINDERF=TRUE
./remainderf_acc  xxx/input_two_float_hex.dat
```
#### 生成`double`型对比数据文件
1. 将`input_two_double_hex.dat`测试用例文件夹复制到`x86`服务器
2. 在`x86`目录复制到`x86`服务器
3. 使用以下命令生成对比数据文件
```shell
cd x86 && make REMAINDER=TRUE
./remainder_acc  xxx/input_two_double_hex.dat
```
#### 生成`long double`型对比数据文件
1. 将`input_two_long_double80_hex.dat`测试用例文件夹复制到`x86`服务器
2. 在`x86`目录复制到`x86`服务器
3. 使用以下命令生成对比数据文件
```shell
cd x86 && make REMAINDERL=TRUE
./remainderl_acc  xxx/input_two_long_double80_hex.dat
```

1. 命令执行完成之后生成`output_float_imf_.dat、output_double_imf_.dat、output_longdouble_imf_.dat`文件
2. 将`output_float_imf_.dat、output_double_imf_.dat、output_longdouble_imf_.dat`复制到`arm`服务器
   
### 生成复现代码测试结果文件
#### 生成`float`类型测试结果文件
1. 将`test`以及`src`目录复制到`arm`服务器
2. 在`test`目录下执行以下指令生成复现代码测试结果文件
 ```shell
cd test && make REMAINDERF=TRUE
cd output && ./remainder_testf  xxx/input_two_float_hex.dat 
```
3. 执行完上述命令后，生成`output_float_arm_.dat`测试结果文件
#### 生成`double`类型测试结果文件
1. 将`test`以及`src`目录复制到`arm`服务器
2. 在`test`目录下执行以下指令生成复现代码测试结果文件
 ```shell
cd test && make REMAINDER=TRUE
cd output && ./remainder_test  xxx/input_two_double_hex.dat 
```
3. 执行完上述命令后，生成`output_double_arm_.dat`测试结果文件

#### 生成`long double`类型测试结果文件
1. 将`test`以及`src`目录复制到`arm`服务器
2. 在`test`目录下执行以下指令生成复现代码测试结果文件
 ```shell
cd test && make REMAINDERL=TRUE
cd output && ./remainder_testl  xxx/input_two_long_double128_hex.dat 
```
3. 执行完上述命令后，生成`output_longdouble_arm_.dat`测试结果文件
     
### 精度对比
1. 精度对比程序在`accuracy_compare`目录下，`comparef.c、compare.c、comparel.c`分别为`float、double、long double`精度对比实现
2. 在`arm`服务器执行以下命令进行精度对比
```shell
cd accuracy_compare && make FLOAT=TRUE && make DOUBLE=TRUE && make LDOUBLE=TRUE 
./compare  xxx/output_double_arm_.dat  xxx/output_double_imf_.dat
./comparef  xxx/output_float_arm_.dat  xxx/output_float_imf_.dat
./comparel  xxx/output_longdouble_arm_.dat  xxx/output_longdouble_imf_.dat
```

