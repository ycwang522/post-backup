---
title: C++ STL常用方法笔记
date: 2016-08-23 10:47:25
categories: C++
tags:
	- STL
---
------

<font size=3 color="green">关于STL中算法库的更详细的用法说明请移步cppreference查询：http://zh.cppreference.com/w/cpp/algorithm</font>

</br>
<font size=3 color="blue">STL容器库：http://zh.cppreference.com/w/cpp/container</font>

------

STL的代码从广义上讲分为三类：algorithm（算法）、container（容器）和iterator（迭代器），几乎所有的代码都采用了模板类和模版函数的方式.

在C++标准中，STL被组织为下面的13个头文件：
`<algorithm>、<deque>、<functional>、<iterator>、<vector>、<list>、<map>、<memory>、<numeric>、<queue>、<set>、<stack>和<utility>`。

## 1. vector

从后面快速的插入与删除，直接访问任何元素。
可以看作动态数组。自动分配一块连续的内存空间进行数据存储。
当存储的数据超过分配的空间时，会重新分配一块内存块：

首先，会申请一块更大的内存块；然后，将原来的数据拷贝到新的内存块中；
其次，销毁掉原内存块中的对象（调用对象的析构函数）；最后，将原来的
内存空间释放掉。

随机访问方便，支持[]/vector.at()。
vector被设计成只能在后端进行追加和删除操作。
只能在最后进行push和pop，不能在头进行。

vector 类中定义了4中种构造函数:
默认构造函数:构造一个初始长度为0的空向量.
vector<int>v1;
构造函数:有一个可选参数,如果预定义了size,他的成员都被赋为0.
vector<int>v2(init_size,0);
复制构造函数:构造一个新的向量,作为已存在的向量的完全复制.
vector<int>v3(v2);
带两个常量参数的构造函数:
vector<int>v4(first,last);

<!-- more -->

方法:

	c.assign(beg,end) c.assign(n,elem)　将(beg; end)区间中的数据赋值给c。将n个elem的拷贝赋值给c。 　　
	c. at(idx)　传回索引idx所指的数据，如果idx越界，抛出out_of_range。 　
	c.back()　传回最后一个数据，不检查这个数据是否存在。 　　
	c.begin()　传回迭代器中的第一个数据地址。 　　
	c.capacity()　返回容器中数据个数。 　　
	c.clear()　移除容器中所有数据。 　　
	c.empty()　判断容器是否为空。 　　
	c.end() // 指向迭代器中末端元素的下一个，指向一个不存在元素。 　　
	c.erase(pos) // 删除pos位置的数据，**传回下一个数据的位置**。 　　
	c.erase(beg,end)　//删除[beg,end)区间的数据，传回下一个数据的位置。 　
	c.front()　//传回第一个数据。 　　
	get_allocator　使用构造函数返回一个拷贝。 　　
	c.insert(pos,elem) // 在pos位置插入一个elem拷贝，传回新数据位置　 　
	c.insert(pos,n,elem) // 在pos位置插入n个elem数据,无返回值　 　　
	c.insert(pos,beg,end) // 在pos位置插入在[beg,end)区间的数据。无返回值c.max_size()　返回容器中最大数据的数量。　 　　
	c.pop_back()　删除最后一个数据。　 　　
	c.push_back(elem)　在尾部加入一个数据。　 　　
	c.rbegin()　传回一个逆向队列的第一个数据。　 　　
	c.rend()　传回一个逆向队列的最后一个数据的下一个位置。　 　　
	c.resize(num)　重新指定队列的长度。　 　　
	c.reserve()　保留适当的容量。　 　　
	c.size()　返回容器中实际数据的个数。　 　　
	c1.swap(c2) // 将c1和c2元素互换




## 2. list

双链表，从任何地方快速插入与删除。

数据由若干个节点构成，每一个节点都包括一个信息块、一个前驱指针、一个后驱指针。
不使用连续的内存空间这样可以随意地进行动态操作。
可以在内部任何位置快速地插入或删除，当然也可以在两端进行push和pop。
不能进行内部的随机访问，不支持[]/vector.at()。

五个构造函数:
list<int> c0;//空链表
list<int> c1(3);  //建一个含三个默认值是0的元素的链表
list<int> c2(5,2);//建一个含五个元素的链表,值都是2
list<int> c4(c2);//建一个c2的copy链表
list<int> c5(c1.begin(),c1.end());//含c1一个区域的元素[first,last).


方法:

	assign() //分配值，有两个重载： 　　
	c1.assign(++c2.begin(), c2.end())  　　
	c1.assign(7,4) //c1中现在为7个4。 　　
	back() //返回最后一元素的引用： 　　
	begin() //返回第一个元素的指针(iterator) 　　
	clear() //删除所有元素 　　
	empty() //判断是否链表为空 　　
	end() //返回最后一个元素的下一位置的指针(list为空时end()=begin()) 　erase() //删除一个元素或一个区域的元素(两个重载) 　　
	front() //返回第一个元素的引用： 　　
	insert() //在指定位置插入一个或多个元素(三个重载)： 　　
	max_size() //返回链表最大可能长度(size_type就是int型)： 　　
	merge() //合并两个链表并使之默认升序(也可改)： 　
	　pop_back() //删除链表尾的一个元素 　　
	pop_front() //删除链表头的一元素 　　
	push_back() //增加一元素到链表尾 　　
	push_front() //增加一元素到链表头 　　
	rbegin() //返回链表最后一元素的后向指针(reverse_iterator or const) 　rend() //返回链表第一元素的下一位置的后向指针 　　
	remove //()删除链表中匹配值的元素(匹配元素全部删除) 　　
	remove_if() //删除条件满足的元素(会遍历一遍链表) 　　
	resize() //重新定义链表长度(两重载)： 　　
	reverse() //反转链表: 　　
	size() //返回链表中元素个数 　　
	sort() //对链表排序，默认升序(可自定义) 　　
	splice() //对两个链表进行结合(三个重载) 　　
	swap() //交换两个链表(两个重载) 　　
	unique() //删除相邻重复元素(断言已经排序，因为它不会删除不相邻的相同元素) 

```cpp
example:
// 输出一个链表
void ShowList(list<int>& listTemp)
{    
//  size()返回链表中元素个数    
cout << listTemp.size() << endl;    
for (list<int>::iterator it = listTemp.begin(); it != listTemp.end(); ++it)    
{        
cout << *it << ' ';    
}    
cout << endl;
}
```

## 3. map
一对多映射，基于关键字快速查找，不允许重复值。
由于其按链表的方式存储，它也继承了链表的优缺点。

构造函数:
Template<class T1,class T2>
map();//默认构造函数
map(const map& m)//拷贝构造函数
map(iterator begin,iterator end);//区间构造函数
map(iterator begin,iterator end,const traits&_compare)
//带比较谓词的构造函数
map(iterator begin,iterator end,const traits&_compare,
const allocator&_all)//带分配器

example:
```cpp
1.
#include <iostream> 　　
#include <map> 　　
using namespace std; 　　
int main(void) 　　
{ 　　
map<char,int,less<char> > map1; 　　
map<char,int,less<char> >::iterator mapIter;
map1[''c'']=3; 　　
map1[''d'']=4; 　　
map1[''a'']=1; 　　
map1[''b'']=2; 　　
for(mapIter=map1.begin();mapIter!=map1.end();++mapIter) 　　
cout<<" "<<(*mapIter).first<<": "<<(*mapIter).second;
map<char,int,less<char> >::const_iterator ptr; 　　
ptr=map1.find(''d''); 　　
cout<<''\n''<<" "<<(*ptr).first<<" 键对应于值："<<(*ptr).second; 　　cin.get(); 　　
return 0; 　　
} 


2.
#include<map>
#include<string>
#include<iostream>
using namespace std;
int main()
{
    map<string,int>  m;
    m["a"]=1;
    m["b"]=2;
    m["c"]=3;
    map<string,int>::iterator it;
    for(it=m.begin();it!=m.end();++it)
        cout<<"key: "<<it->first <<" value: "<<it->second<<endl;
    return   0;
}
```




应用篇:

```cpp
1.不用STL:
#include "stdafx.h"
#include <stdlib.h>
#include <iostream>
using namespace std;
int compare(const void *arg1, const void *arg2);

void main(void)
{
	const int max_size = 10;		
	int num[max_size];
	int n;
	for (n = 0; cin >> num[n]&&n<max_size-2; n ++);	
	cout<<'\n';
	qsort(num, n+1, sizeof(int), compare);	
	for (int i = 0; i < n+1; i ++)
		cout << num[i] << "\n";
	system("pause");
}
int compare(const void *arg1, const void *arg2)
{
	return	(*(int *)arg1 < *(int *)arg2) ? -1 :
		(*(int *)arg1 > *(int *)arg2) ? 1 : 0;
}


2.STL:
#include "stdafx.h"
#include <iostream>
#include <vector>
#include <algorithm>
#include <stdlib.h>

using namespace std;

void main(void)
{
	vector<int> num;		
	int element;
	while (cin >> element)
		num.push_back(element);
	sort(num.begin(), num.end());
	for (int i = 0; i < num.size(); i ++)
		cout << num[i] << "\n";
	system("pause");
}


3.第三版:
#include<iostream>
#include<stdlib.h>
#include<vector>
#include<algorithm>
#include<iterator>

using namespace std;

void main(void)
{
	typedef vector<int> int_vector;
	typedef istream_iterator<int> istream_itr;
	typedef ostream_iterator<int> ostream_itr;
	typedef back_insert_iterator<int_vector> back_ins_itr;
	int_vector num;
	copy(istream_itr(cin),istream_itr(),back_ins_itr(num));
	sort(num.begin(),num.end());
	copy(num.begin(),num.end(),ostream_itr(cout,'\n'));
	system("pause");
}

4.
#include "stdafx.h"
#include<stdlib.h>
#include <iostream>
#include <algorithm>

using namespace std;

#define SIZE 100
int iarray[SIZE];

int main()
{
	iarray[20] = 50;
	int* ip = find(iarray, iarray + SIZE, 50);
	if (ip == iarray + SIZE)
		cout << "50 not found in array" << endl;
	else
		cout << *ip << " found in array" << endl;
	
	system("pause");
}


5.
#include "stdafx.h"
#include<stdlib.h>
#include <iostream>
#include <algorithm>

using namespace std;
 vector<int>  intVector(100);

void main()
{
	intVector[20] = 50;
	vector<int>::iterator intIter =
		find(intVector.begin(), intVector.end(), 50);
	if (intIter != intVector.end())
		cout << "Vector contains value " << *intIter << endl;
	else
		cout << "Vector does not contain 50" << endl;
	system("pause");
}



6.
int main()
{
vector<string> v;
string tmp;
while(getline(cin,tmp))
v.push_back(tmp);
sort(v.begin(),v.end());
copy(v.begin(),v.end(),ostream_iterator<string>(cout,"\n"));
}
```
## 4. STL算法

STL算法部分主要由头文件`<algorithm>,<numeric>,<functional>`组成。要使用 STL中的算法函数必须包含头文件`<algorithm>`，对于数值算法须包含`<numeric>，<functional>`中则定义了一些模板类，用来声明函数对象。
STL中算法大致分为四类：
1）、非可变序列算法：指不直接修改其所操作的容器内容的算法。
2）、可变序列算法：指可以修改它们所操作的容器内容的算法。
3）、排序算法：包括对序列进行排序和合并的算法、搜索算法以及有序序列上的集合操作。
4）、数值算法：对容器内容进行数值计算。

 

以下对所有算法进行细致分类并标明功能：

### <一>查找算法(13个)：判断容器中是否包含某个值


- adjacent_find:在iterator对标识元素范围内，查找一对相邻重复元素，找到则返回指向这对元素的第一个元素的ForwardIterator。否则返回last。重载版本使用输入的二元操作符代替相等的判断。

- binary_search:            在有序序列中查找value，找到返回true。重载的版本实用指定的比较函数对象或函数指针来判断相等。

- count:                    利用等于操作符，把标志范围内的元素与输入值比较，返回相等元素个数。

- count_if:                 利用输入的操作符，对标志范围内的元素进行操作，返回结果为true的个数。
```cpp	
	#include<iostream>
	#include<string.h>
	#include<algorithm>
	#include<vector>
	using namespace std;
	int main()
	{
		int a[] = { 1, 2, 4, 5, 6 };
		vector<int>ivec(a, a + 5);
		cout << count_if(ivec.begin(), ivec.end(), [](int i){return i % 2 == 0; });
		return 0;
	}
```


- equal_range:              功能类似equal，返回一对iterator，第一个表示lower_bound，第二个表示upper_bound。
```cpp
#include <algorithm>
#include <vector>
#include <iostream>

struct S {
    int number;
    char name;
    S ( int number, char name  )
        : number ( number ), name ( name )
    {}
    // only the number is relevant with this comparison
    bool operator< ( const S& s ) const {
        return number < s.number;
    }
};
int main() {
    // note: not ordered, only partitioned w.r.t. S defined below
    std::vector<S> vec = { {1,'A'}, {2,'B'}, {2,'C'}, {2,'D'}, {4,'G'}, {3,'F'} };
    S value ( 2, '?' );
    auto p = std::equal_range(vec.begin(),vec.end(),value);
    for ( auto i = p.first; i != p.second; ++i )
        std::cout << i->name << ' ';
}
```

- find:                     利用底层元素的等于操作符，对指定范围内的元素与输入值进行比较。当匹配时，结束搜索，返回该元素的一个InputIterator。
```cpp
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;
int main()
{
	int a[] = { 1, 4, 4, 5, 6 };	
	vector<int>ivec(a, a + 5);
	vector<int>::iterator it= find(ivec.begin(), ivec.end(), 5);
	cout << *it << endl;	//查找之后返回iterator
	return 0;
}
```

- find_end:                 在指定范围内查找"由输入的另外一对iterator标志的第二个序列"的最后一次出现。找到则返回最后一对的第一个ForwardIterator，否则返回输入的"另外一对"的第一个ForwardIterator。重载版本使用用户输入的操作符代替等于操作。
```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main() {
std::vector<int> v{1, 2, 3, 4, 1, 2, 3, 4, 1, 2, 3, 4};
std::vector<int>::iterator result;

std::vector<int> t1{1, 2, 3};

result = std::find_end(v.begin(), v.end(), t1.begin(), t1.end());
if (result == v.end()) {
    std::cout << "subsequence not found\n";
} else {
    std::cout << "last subsequence is at: "
              << std::distance(v.begin(), result) << "\n";
}
```

- find_first_of:            在指定范围内查找"由输入的另外一对iterator标志的第二个序列"中任意一个元素的第一次出现。重载版本中使用了用户自定义操作符。

- find_if:                  使用输入的函数代替等于操作符执行find。
```cpp
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

bool isOdd(int i)
{
	if (i % 2 == 0)
		return 0;
	else
		return 1;
}
int main()
{
	int a[] = { 2, 4, 4, 5, 6 };	
	vector<int>ivec(a,a+5);
	vector<int>::iterator it=find_if(ivec.begin(), ivec.end(), isOdd);
	cout << "The first odd is " << *it << endl;
	return 0;
}
```

- lower_bound:              返回一个ForwardIterator，指向在有序序列范围内的可以插入指定值而不破坏容器顺序的第一个位置。重载函数使用自定义比较操作。

- upper_bound:              返回一个ForwardIterator，指向在有序序列范围内插入value而不破坏容器顺序的最后一个位置，该位置标志一个大于value的值。重载函数使用自定义比较操作。

- search:                   给出两个范围，返回一个ForwardIterator，查找成功指向第一个范围内第一次出现子序列(第二个范围)的位置，查找失败指向last1。重载版本使用自定义的比较操作。
- search_n:                 在指定范围内查找val出现n次的子序列。重载版本使用自定义的比较操作。
 
### <二>排序和通用算法(14个)：提供元素排序策略

- inplace_merge:            合并两个有序序列，结果序列覆盖两端范围。重载版本使用输入的操作进行排序。
- merge:                    合并两个有序序列，存放到另一个序列。重载版本使用自定义的比较。
- nth_element:              将范围内的序列重新排序，使所有小于第n个元素的元素都出现在它前面，而大于它的都出现在后面。重载版本使用自定义的比较操作。
- partial_sort:             对序列做部分排序，被排序元素个数正好可以被放到范围内。重载版本使用自定义的比较操作。
- partial_sort_copy:        与partial_sort类似，不过将经过排序的序列复制到另一个容器。
- partition:                对指定范围内元素重新排序，使用输入的函数，把结果为true的元素放在结果为false的元素之前。
- random_shuffle:           对指定范围内的元素随机调整次序。重载版本输入一个随机数产生操作。
- reverse:                  将指定范围内元素重新反序排序。
- reverse_copy:             与reverse类似，不过将结果写入另一个容器。
- rotate:                   将指定范围内元素移到容器末尾，由middle指向的元素成为容器第一个元素。
- rotate_copy:              与rotate类似，不过将结果写入另一个容器。
- sort:                     以升序重新排列指定范围内的元素。重载版本使用自定义的比较操作。
- stable_sort:              与sort类似，不过保留相等元素之间的顺序关系。
- stable_partition:         与partition类似，不过不保证保留容器中的相对顺序。
 
### <三>删除和替换算法(15个)
- copy:                     复制序列
- copy_backward:            与copy相同，不过元素是以相反顺序被拷贝。
- iter_swap:                交换两个ForwardIterator的值。
- remove:                   删除指定范围内所有等于指定元素的元素。注意，该函数不是真正删除函数。内置函数不适合使用remove和remove_if函数。
- remove_copy:              将所有不匹配元素复制到一个制定容器，返回OutputIterator指向被拷贝的末元素的下一个位置。
- remove_if:                删除指定范围内输入操作结果为true的所有元素。
- remove_copy_if:           将所有不匹配元素拷贝到一个指定容器。
- replace:                  将指定范围内所有等于vold的元素都用vnew代替。
- replace_copy:             与replace类似，不过将结果写入另一个容器。
- replace_if:               将指定范围内所有操作结果为true的元素用新值代替。
- replace_copy_if:          与replace_if，不过将结果写入另一个容器。
- swap:                     交换存储在两个对象中的值。
- swap_range:               将指定范围内的元素与另一个序列元素值进行交换。
- unique:                   清除序列中重复元素，和remove类似，它也不能真正删除元素。重载版本使用自定义比较操作。
- unique_copy:              与unique类似，不过把结果输出到另一个容器。

### <四>排列组合算法(2个)：提供计算给定集合按一定顺序的所有可能排列组合
- next_permutation:         取出当前范围内的排列，并重新排序为下一个排列。重载版本使用自定义的比较操作。
- prev_permutation:         取出指定范围内的序列并将它重新排序为上一个序列。如果不存在上一个序列则返回false。重载版本使用自定义的比较操作。
 

### <五>算术算法(4个)
- accumulate:               iterator对标识的序列段元素之和，加到一个由val指定的初始值上。重载版本不再做加法，而是传进来的二元操作符被应用到元素上。
- partial_sum:              创建一个新序列，其中每个元素值代表指定范围内该位置前所有元素之和。重载版本使用自定义操作代替加法。
- inner_product:            对两个序列做内积(对应元素相乘，再求和)并将内积加到一个输入的初始值上。重载版本使用用户定义的操作。
- adjacent_difference:      创建一个新序列，新序列中每个新值代表当前元素与上一个元素的差。重载版本用指定二元操作计算相邻元素的差。
 
### <六>生成和异变算法(6个)
- fill:                     将输入值赋给标志范围内的所有元素。
- fill_n:                   将输入值赋给first到first+n范围内的所有元素。
- for_each:                 用指定函数依次对指定范围内所有元素进行迭代访问，返回所指定的函数类型。该函数不得修改序列中的元素。
- generate:                 连续调用输入的函数来填充指定的范围。
- generate_n:               与generate函数类似，填充从指定iterator开始的n个元素。
- transform:                将输入的操作作用与指定范围内的每个元素，并产生一个新的序列。重载版本将操作作用在一对元素上，另外一个元素来自输入的另外一个序列。结果输出到指定容器。


### <七>关系算法(8个)
- equal:                    如果两个序列在标志范围内元素都相等，返回true。重载版本使用输入的操作符代替默认的等于操作符。
- includes:                 判断第一个指定范围内的所有元素是否都被第二个范围包含，使用底层元素的<操作符，成功返回true。重载版本使用用户输入的函数。
- lexicographical_compare:  比较两个序列。重载版本使用用户自定义比较操作。
- max:                      返回两个元素中较大一个。重载版本使用自定义比较操作。
- max_element:              返回一个ForwardIterator，指出序列中最大的元素。重载版本使用自定义比较操作。
- min:                      返回两个元素中较小一个。重载版本使用自定义比较操作。
- min_element:              返回一个ForwardIterator，指出序列中最小的元素。重载版本使用自定义比较操作。
- mismatch:                 并行比较两个序列，指出第一个不匹配的位置，返回一对iterator，标志第一个不匹配元素位置。如果都匹配，返回每个容器的last。重载版本使用自定义的比较操作。
 

### <八>集合算法(4个)
- set_union:                构造一个有序序列，包含两个序列中所有的不重复元素。重载版本使用自定义的比较操作。
- set_intersection:         构造一个有序序列，其中元素在两个序列中都存在。重载版本使用自定义的比较操作。
- set_difference:           构造一个有序序列，该序列仅保留第一个序列中存在的而第二个中不存在的元素。重载版本使用自定义的比较操作。
- set_symmetric_difference: 构造一个有序序列，该序列取两个序列的对称差集(并集-交集)。

### <九>堆算法(4个)
- make_heap:                把指定范围内的元素生成一个堆。重载版本使用自定义比较操作。
- pop_heap:                 并不真正把最大元素从堆中弹出，而是重新排序堆。它把first和last-1交换，然后重新生成一个堆。可使用容器的back来访问被"弹出"的元素或者使用pop_back进行真正的删除。重载版本使用自定义的比较操作。
- push_heap:                假设first到last-1是一个有效堆，要被加入到堆的元素存放在位置last-1，重新生成堆。在指向该函数前，必须先把元素插入容器后。重载版本使用指定的比较操作。
- sort_heap:                对指定范围内的序列重新排序，它假设该序列是个有序堆。重载版本使用自定义比较操作。


