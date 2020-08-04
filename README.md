1、结构体占用内存空间大小与实际分配大小不一样原因
答、在存储过程中，为了提高CPU的存储速度，编译器会对变量的起始地址做对齐处理，各个变量的起始地址相对于结构体的起始地址的偏移量是变量类型所占字节的整数倍，并且结构体总的字节数是最大变量字节数的整数倍。
比如 struct Person{
int a;
long b;
char c;
}，
内存分配大小为24字节。a占4个字节，但是long b八个字节，所以a会补齐成8个字节，又因为总大小得是8的整数倍，所以得是24个字节

2、举例什么情况会栈溢出?
递归调用一个函数的时候，且没有打断递归停止，或者递归嵌套特别深的时候
- (void)funcA{
int a = 10;
[self funcA];
} 
这种情况会一直递归调用分配新的内存地址，会发生栈溢出

-(void)funcB{
int b = 10;
}

-(void)funcC{
int c = 10;
}

- (void)mainfunc
{
int a = 10;
{
int b = 10;
}
{
int c = 20;
}
//这种情况b与c的内存地址不一样

[self funcB];
[self funcC];
这种情况b与c的内存地址一样，因为离开作用范围函数及变量就释放了，这时候内存地址会重用上面的

for （int i = 0; i < 30; i++）{
int a = 10;
//这时候每次a的内存地址都相等，因为出了循环对象就会释放
}
}

3、iOS中Xcode查看内存未释放的N种方式
首先构造内存未释放情景
- (void)testLeaks{
    ClassA *aa = [[ClassA alloc] init];
    ClassB *b = [[ClassB alloc] init];
    aa.b = b;
    b.a = aa;
}  用debug memory graph
