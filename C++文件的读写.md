# C++文件的读写

要对文件进行读写，首先需要包含fstream头文件，`#include <fstream>`

## 1、将数据写入文件

为了打开一个可供输出的文件，我们定义一个ofstream（供输出用的file stream）对象，并将文件名传入

```c++
//以供输出模式开启seq_data.txt
ofstream outfile("seq_data.txt");
```

声明outfie的同时，如果指定文件不存在，那么将创建该文件，并打开该文件供输出使用。

如果指定文件已经存在，默认的方式会将文件中的数据全部丢弃，再将输出的数据写入到文件。

```c++
//以追加模式(append mode)开启seq_data.txt
//新数据会被加到文件的末尾
ofstream outfile("seq_data.txt", ios_base:app);
```

文件还可能打开失败。在进行写入之前，我们必须确保文件已经成功打开，最简单的方式就是检验class object的真伪：

```c++
//如果outfile文件的值为false，那么说明文件并没有成功打开
if (!outfile)
	cerr << "Unable to open the file";	//cerr和cout一样，都是讲输出结果定向到用户的终端，唯一的区别是cerr的输出结果并没有缓冲
else
    outfile << user_name << " " << num_tries << " " << num_right;
```



## 2、将文件中的数据进行读入

如果要打开一个可供读取的文件，我们就定义一个ifstream（供输出的file stream对象），并将文件名传入。如果文件未能成功打开，该ifstream对象就位false。如果成功，该文件的写入文职会被设定在起始处

```c++
//以读取模式(input mode)打开infile
ifstream infile("seq_data.txt");
```

```c++
#include <iostream>
#include <fstream>
using namespace std;

int main()
{
    string name;
    int num_tries;
    int num_rights;
    ifstream infile("1_05.txt");

    if (!infile)
    {
        cerr << "Oops, unable to open file!" << endl;
    }else
    {
        infile >> name >> num_tries >> num_rights;
        cout << "name: " << name << endl;
        cout << "tot: " << num_tries << endl;
        cout << "right: " << num_rights << endl;
    }
    

    return 0;
}
```



## 3、对文件进行读写

如果想要同时读写一个文件，我们需要定义一个fstream对象。为了以追加模式打开（append mode），我们需要传入第二参数值ios_base::in | ios_base::app

```c++
fstream iofile("data.txt", ios_base::in | ios_base::app);
if(!iofile)
{
	cerr << "Oops, unable to open file!" << endl;
}else
{
    iofile << "andy " << 10 << ' ' << 7 << endl;
    cout << "______________________" << endl;
    string usr_name;
    int num_tries = 0;
    int num_rights = 0;

    // 由于ios_base::app的原因，一回家模式打开文件，文件的位置会位于末尾，所以开始读取之前，需要将文件重新定位到起始处
    iofile.seekg(0);
    iofile >> usr_name >> num_tries >> num_rights;
    cout << "name: " << usr_name << endl;
    cout << "tot: " << num_tries << endl;
    cout << "right: " << num_rights << endl;
}
```

