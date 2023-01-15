一些常用的板子

# STL常用

## **字符串**

**读一整行**

getline(cin,s);

```cpp
string s;
getline(cin,s);
```

**查找**

.find(substr) 返回匹配子串的第一个下标(int类型),若没有找到返回-1

```cpp
string s = "123.2331.123";
int idx = s.find(".23");
cout<<idx;   //输出 3
```

顺带提一嘴, 这个API是KMP算法的一个实现, 关于[[KMP算法]]

**子串**

.substr(idx,len) 从idx开始(包括idx),子串长度为length

.substr(idx) 从idx开始到该串的结尾

调用入口 :

```cpp
string s = "012345678";
string sub1 = s.substr(3,5);
cout<<sub1<<endl; //输出  34567
string sub2 = s.substr(5);
cout<<sub2<<endl; // 输出 5678
string insert = s.substr(0,5) + "*" + s.substr(5); //在4与5之间插入*
cout<<insert<<endl;  //输出 01234*5678
```

**反转字符串**

reverse(s.begin(),s.end());

**删除子串**

.erase(idx,len)

.erase(idx)

**字符串换数字**

1. stoi() - 将string转化为int
2. stof() - 将string转换为float.
3. stod() - 将string转换为double.
4. stold() - 将string转换为long double

其他类型转成字符串: 
`std::to_string()

***字符串流**

```cpp
stringstream ss;
string s ="你想作为流输入的字符串"
ss<<s;
```

## 自定义排序

**法1. 自定义cmp()函数**

1. 返回值为 bool 类型，用来表示当前的排序是否正确。
2. 参数为两个相同类型的变量，且类型与要排序的容器模板类型相同。

```cpp
struct MyStruct {
    int weigth;
    string str;
};
bool cmp(const MyStruct& ms1, const MyStruct& ms2) {
    return ms1.weight < ms2.weight;
}
int main(){
    vector<MyStruct> msVector;
    sort(msVector.begin(), msVector.end(), cmp);
}
```

**法2. 重载运算符**

**如果用到map/priority_queue等底层为则必须重载运算符**

这里只记录结构体外重载的, 原理与cmp()类似

```cpp
bool operator < (const edge &e1,const edge &e2) {
    return e1.w < e2.w;
}
```

## 常用基础数据结构

### vector queue stack 略

注意其.begin() .end() 返回的迭代器并不是最后一个元素的，而是**前闭后开**的

所有的STL都具有的方法： size/empty/begin/end/clear

### dequeue 双端队列

如果频繁在队首队尾进行操作,可以考虑使用双端队列

| 函数                         | 作用                      |
| ---------------------------- | ------------------------- |
| .push_back() / .push_front() | 从队首/队尾 插入          |
| .pop_back() / .pop_front()   | 从队首/队尾 取出          |
| .front() / .back()           | 取队首/队尾的元素(不取出) |

### priority_queue

优先队列默认是大根堆(权值高的在前),其实现方式是**大根二叉堆**，要求其维护的数据结构**必须重载<运算符**，若想要实现小根堆 重载运算符时改变开口方向即可。

### set && multiset

两者的实现都是**红黑树**，前者不允许重复元素出现，后者允许

| 函数              | 作用                                                |
| ----------------- | --------------------------------------------------- |
| .count（）        | 查询集合中包含的元素个数（multiset才可以用）        |
| .find（）         | 在集合中查找，如果集合中没有该元素 返回.end()迭代器 |
| .lower_bound（x） | 返回<=x的第一个元素                                 |
| .upper_bound(x)   | 返回<的第一个元素                                   |
| .insert()         | 插入新的元素                                        |

### map && unordered_map

map本质上是以key为关键字的一颗红黑树，存储一个键值对，**key必须定义<运算符**

.insert() 与 .erase()

insert与erase的对象都可以是pair （make_pair）

erase也支持迭代器删除

.find() 查询，若不存在返回.end()

**重载的[]运算符**

map[key] 返回其value值，**如果key不存在则会默认把value置为一个零值，会白白增加维护的空间而且会引入一些错误，因此查询最好还是先用.find()判断一下是否存在值**

也可以直接用map[key]=value进行赋值（推荐）

unordered_map 底层是hash表，比map稍快

## 常用的内置算法

对于序列的算法 默认都是前闭后开 [begin,end)

### sort

如果自定义了cmp函数，sort的第三个参数为比较器

```
sort(v.begin(),v.end(),cmp)
```

### reverse

反转数组 `reverse(v.begin(),v.end())`

### unique

用于去重，原地去重 返回去重后的尾迭代器（不包含）

求重复的个数

```
int m=unique(v.begin(),v.end()) - v.begin()
```

### next_permutation && pre_permutation

返回下一个全排列,若不存在下一个排列（当前排列已经是字典序最大的排序）则返回false 否则返回true

pre_permutation同理

```cpp
//求数组的全排列 字典序从小到大打印
sort(v.begin(),v.end());
do{
    	for(int i=0;i<v.size();i++) cout<<v[i]<<" ";
    	cout<<endl;
}while(next_permutation(v.begin(),v.end()))
```

### lower_bound && upper_bound

要求数组一定是有序的，lower_bound返回<=当前值的第一个元素下标

upper_bound 返回<当前值的第一个下标

`int idx = lower_bound(v.begin(),v.end(),x)` 返回<=x的元素的第一个下标

# 快读

```cpp
inline int readInt()//读入一个整数  
{  
	char ch;
	bool flag = false;
	int a = 0;  
	while(!((((ch = getchar()) >= '0') && (ch <= '9')) || (ch == '-')));  
	if(ch != '-')
	{
		a *= 10;
		a += ch - '0';  
	}
	else
	{
		flag = true;
	}
	while(((ch = getchar()) >= '0') && (ch <= '9'))
	{
		a *= 10;
		a += ch - '0';
	}	
	if(flag)
	{
		a = -a;
	}
	return a;  
} 

inline string readStr()//读入一个字符串
{
	char ch = getchar();
	string st1 = "";
	while (!((ch >= 'a') && (ch <= 'z')))//把前面没用的东西去掉,当然,ch在什么范围内可以依据需要修改 
		ch = getchar();
	while ((ch >= 'a') && (ch <= 'z'))
		st1 += ch, ch = getchar();
	return st1;
}
```

# 数论算法

## 快速幂

```cpp
typedef long long ll;

ll q_pow(int a, int n, int mod) {//mod表示 把结果限制在[0,mod-1] 范围内
	ll ans = 1;
	while (n > 0) {
		if (n & 1) {
			ans = ans * a % mod;
		}
		a = a * a % mod;
		n >>= 1;
	}
	return ans;
}
```

## gcd and exgcd

exgcd 可以求出 ax+by = gcd(a,b)的一个最简解 (这里直接由&x &y表示)

辗转相除法: 除数为0吗 若为0直接返回被除数 否则求其余数 被除数与余数玩递归

```cpp
int gcd(int a, int b) {
	return b == 0 ? a : gcd(b, a % b);
}

int exgcd(int a, int b, int& x, int& y)
{
	if (b == 0) {
		x = 1, y = 0;
		return a;
	}
	int d = exgcd(b, a % b, x, y);
	int k = x;
	x = y;
	y = k - a / b * y;
	return d;
}
```

## 埃筛

时间复杂度O(n * loglogn)级别,接近线性筛

核心思想是质数的倍数一定不是质数

```cpp
vector<bool> eratosthenes(int upperbound) {
  std::vector<bool> flag(upperbound + 1, true);
  flag[0] = flag[1] = false;
  for (int i = 2; i <= sqrt(upperbound); ++i) {
    if (flag[i]) {
      for (int j = i * i; j <= upperbound; j += i)
        flag[j] = false;
    }
  }
  return flag;
}
```

## 米勒罗宾素数检验

概率算法,判断一个**大数**是否是素数, 当数够大时检测出错的结果极小

```cpp
const int times = 10;

ll prime[6] = { 2, 3, 5, 233, 331 };
ll qmul(ll x, ll y, ll mod) { // 乘法防止溢出， 如果p * p不爆ll的话可以直接乘； O(1)乘法或者转化成二进制加法
	return (x * y - (long long)(x / (long double)mod * y + 1e-3) * mod + mod) % mod;
	/*
	ll ret = 0;
	while(y) {
		if(y & 1)
			ret = (ret + x) % mod;
		x = x * 2 % mod;
		y >>= 1;
	}
	return ret;
	*/
}
ll qpow(ll a, ll n, ll mod) {
	ll ret = 1;
	while (n) {
		if (n & 1) ret = qmul(ret, a, mod);
		a = qmul(a, a, mod);
		n >>= 1;
	}
	return ret;
}
bool Miller_Rabin(ll p) {
	if (p < 2) return 0;
	if (p != 2 && p % 2 == 0) return 0;
	ll s = p - 1;
	while (!(s & 1)) s >>= 1;
	for (int i = 0; i < 5; ++i) {
		if (p == prime[i]) return 1;
		ll t = s, m = qpow(prime[i], s, p);
		while (t != p - 1 && m != 1 && m != p - 1) {
			m = qmul(m, m, p);
			t <<= 1;
		}
		if (m != p - 1 && !(t & 1)) return 0;
	}
	return 1;
}
```

质数筛与素数检测的结合运用

当较小时用素数筛直接开筛, 数较大时用素数检测单点检测

- [P1835 素数密度](https://opensourcehu.gitee.io/2022/05/12/板子/)

# 图论算法

## 最小生成树

### kruskal算法

```cpp
/*********注意:算法需要并查集支持,这里使用的是按秩压缩的UF*******/
struct edge//存图
{
    int u;
    int v;
    int weight = 0;
    bool operator < (edge& e) const {
        return weight < e.weight;
    }
};
vector<edge> G;
//返回最小生成树的总路径长
int kruskal() {
    auto arr = buildSet(N);//一开始默认每个点都不连通
    int ans = 0,cnt = 0;
    for (int i = 0; i < G.size(); i++)
    {
        if (cnt == N - 1)
            break;
        edge cur = G[i];
        if (Find(cur.u, cur.v, arr))//如果已经联通了(添加这条边就出现环了) 则不添加
            continue;
        else//若没有联通,则添加这条边
        {
            ans += cur.weight;
            Union(cur.u, cur.v, arr);
            cnt++;
        }
    }
    return ans;
}
```

## 单源最短路

### Dijkstra 算法

利用堆优化,要注意以下几个点

- 用fill初始化的时候 `fill(d + 1, d + V + 1, LLONG_MAX);` 点是几号就从几号开始初始化 不要只写个数组名字+长度上去 那个是memset的用法 `memset(arr,sizeof(arr),0);`
- 储存单源最短长度的数组要开ll pair也要开ll,不然图大了长度会爆int
- 第24. 25行的语句不能省略 不然会tle, d[]数组中存储的已经比当前点记录的要小了,说明这个点为中心已经计算过了 不用重复计算了

```cpp
#include <bits/stdc++.h>
#define MAX_V 10010
typedef long long ll;
using namespace std;
struct edge {
    int to;
    int weight;
};
int V;
typedef pair<ll, int> P; //fi最短距离 se编号
ll d[MAX_V];//存放到起始点的距离
vector<edge>  G[MAX_V];


void Dijkstra(int start) {
    priority_queue<P, vector<P>, greater<P> > que;
    fill(d + 1, d + V + 1, LLONG_MAX);
    que.push(P(0, start));
    d[start] = 0;
    while (!que.empty()) {
        P cur = que.top();
        que.pop();
        int u = cur.second;
        if (d[u] < cur.first)
            continue;
        for (int i = 0; i < G[u].size(); i++) {
            int v = G[u][i].to;
            if (d[v] > cur.first + G[u][i].weight) {
                d[v] = cur.first + G[u][i].weight;
                que.push(P(d[v], v));
            }
        }
    }
}
```

### floyd算法 O(n^3) 稠密图可用

```cpp
#define INF 0x3f3f3f3f   //一开始记得设置初始值
int d[MAX_V][MAX_V]; //d[u][v] 表示边e=(u,v)的权值, 不存在时设为INF,但d[i][i] = 0;
int V;

void init(){
    memset(d, INF, sizeof(d));
	for (int i = 1; i <= N; i++) {
		d[i][i] = 0;
	} 
}

void floyd() {//注意点是0还是1
	for (int k = 1; k <= V; k++)
		for (int i = 1; i <= V; i++)
			for (int j = 1; j <= V; j++)
				d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
}
```

# 数据结构

## 线段树板子

**1.支持区间修改的 懒标签写法**

- 比较长,但是可以支持区间修改(一个区间共同加上一个数)了
- 注意: **原数组和线段树数组都需要以[1,n]为下标**
- tree数组大小保险起见可以开为原数组的5倍

```cpp
//原始数组
int arr[MAX];
//线段树的节点信息
struct segmentTree {
    int l, r;
    ll sum, add;
#define l(x) tree[x].l
#define r(x) tree[x].r
#define sum(x) tree[x].sum
#define add(x) tree[x].add
} tree[MAX*5];
int n, m;

//建树  调用入口:build(1,1,n)  n为原始数组长度
void build(int cur, int L, int R) {
    l(cur) = L, r(cur) = R;
    if (L == R) {
        sum(cur) = arr[L];
        return;
    }
    int mid = (L + R) >> 1;
    build(cur * 2, L, mid);
    build(cur * 2 + 1, mid + 1, R);
    sum(cur) = sum(cur * 2) + sum(cur * 2 + 1);
}
//下推操作
void push_down(int cur) {
    //如果当前节点有标记
    if (add(cur)) {
        //标记下推  左右孩子节点值增加
        int lSon = cur * 2;
        int rSon = cur * 2 + 1;
        sum(lSon) += add(cur) * (r(lSon) - l(lSon) + 1);
        sum(rSon) += add(cur) * (r(rSon) - l(rSon) + 1);
        //左右孩子节点 打标记
        add(lSon) += add(cur);
        add(rSon) += add(cur);
        //清除当前节点的标记
        add(cur) = 0;
    }
}

//区间修改  调用入口change(1,L,R,addval) 在区间[L,R]的元素上全部加上addval
void change(int cur, int L, int R, int addval) {
    //如果完全覆盖  更新当前值 并在当前标签打标记
    if (L <= l(cur) && R >= r(cur)) {
        sum(cur) += (ll) addval * (r(cur) - l(cur) + 1);
        add(cur) += addval;
        return;
    }
    //标记下推
    push_down(cur);
    int mid = (l(cur) + r(cur)) >> 1;
    if (L <= mid) change(cur * 2, L, R, addval);
    if (R > mid) change(cur * 2 + 1, L, R, addval);
    //更新当前节点值
    sum(cur) = sum(cur * 2) + sum(cur * 2 + 1);
}

//查询入口  ask(1,L,R)  查询区间[L,R]
ll ask(int cur, int L, int R) {
    //完全覆盖 则直接返回节点值
    if (L <= l(cur) && R >= r(cur)) return sum(cur);
    //查询过程中 顺带着下推标记
    push_down(cur);
    int mid = (l(cur) + r(cur)) >> 1;
    ll val = 0;
    if (L <= mid) val += ask(cur * 2, L, R);
    if (R > mid) val += ask(cur * 2 + 1, L, R);
    return val;
}

int main() {
    cin >> n >> m;
    //注意原始数组下标是从1开始存的
    for (int i = 1; i <= n; ++i) {
        cin >> arr[i];
    }
    //记得要初始化  和树状数组不一样
    build(1, 1, n);
    for (int i = 0; i < m; ++i) {
        int t;
        cin >> t;
        if (t == 1) {
            int L, R, K;
            cin >> L >> R >> K;
            change(1, L, R, K);
        } else {
            int L, R;
            cin >> L >> R;
            cout << ask(1, L, R) << endl;
        }
    }
    return 0;
}
```

## 树状数组板子

注意, 单点修改树状数组就可以了 短 ,注意tree数组的下标是从1开始一直到n结束,不要写成从0开始的了 这样无法使用其下标性质

**arr数组(原数组)是不需要的,所有的修改和查询都在tree数组中进行 读入原数组的时候同步更新tree数组即可**

树状数组大小一般开为4~5倍的原数组大小

```cpp
using namespace std;
typedef long long ll;
//注意这里的MAX一般取为
ll tree[MAX];
int n;  //n的值即为树状数组长度,与数组长度相同
//注意tree和arr的index  都是从1开始到n结束
void update(int index,int val)
{
    for(int i=index; i<=n; i+= i&(-i))
        tree[i] +=val;
}

ll Find(int x)
{
    ll sum= 0;
    for(int i=x; i>0; i-= i&(-i))
        sum+= tree[i];
    return sum;
}

ll query(int L,int R)
{
    return Find(R)-Find(L-1);
}

int main(){
    // data init  注意arr的下标 要从1开始
    for(int i=1;i<=n;i++)
    {
        int k;
        cin>>k;
        update(i,k);
    }
    //查询入口  查询区间为[L,R]
    query(L,R);
}
```

## 并查集

路径压缩,简化版 (赛场一般用这个就行)

```cpp
#define MAX 5010//依据数据规模设定
int fa[MAX];
void init(int n) {//注意下标,看题意
	for (int i = 1; i < n; i++)  fa[i] = i;
}
int findFa(int x) {//核心代码
	return fa[x] = fa[x]==x? x:find(fa[x]);
}
void merge(int x,int y) {//合并
	int xfa = findFa(x); int yfa = findFa(y);
	fa[xfa] = yfa;
}
bool isUnion(int x,int y) {//查询
	return findFa(x) == findFa(y);
}
```

按秩压缩版 (卡常用) 一般用不到

```cpp
typedef struct ufnode
{
    int Rank;
    int val;
    int parents;
} UFNODE;

//注意这里需要用一个指针接收返回值
UFNODE* buildSet(int n)
{
    UFNODE* arr = new UFNODE[n + 1];
    for (int i = 1; i <= n; i++)
    {
        arr[i].Rank = 0;
        arr[i].val = i;
        arr[i].parents = i;
    }
    return arr;
}

int FindParents(int x, UFNODE* arr)
{
    int cur = x;
    while (arr[cur].parents != cur)
    {
        cur = arr[cur].parents;
    }
    return cur;
}

void Union(int x, int y, UFNODE* arr)
{
    int xF = FindParents(x, arr);
    int yF = FindParents(y, arr);
    if (arr[xF].Rank > arr[yF].Rank)
    {
        arr[yF].parents = xF;
    }
    else
    {
        arr[xF].parents = yF;
        if (arr[xF].Rank == arr[yF].Rank)
            arr[yF].Rank++;
    }
}

bool Find(int x, int y, UFNODE* arr)
{
    return FindParents(y, arr) == FindParents(x, arr);
}
/****用法****/
int main(){
    auto it = buildSet(scala);
    Find(x,y,scala);
    Union(x,y,scala);    
}
```