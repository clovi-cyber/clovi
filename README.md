cstore.h
#include<iostream>
#include<string>
#include<fstream>

using namespace std;

class commodity//商品
{
public:
protected:
	string name;
	double price;
};

class computer:public commodity//计算机
{
public:
protected:
	string CPU;
	string GPU;
};

class laptop :public computer//笔记本
{
public:
	laptop() = default;
	friend ostream& operator<<(ostream& os, laptop& item);
	friend istream& operator>>(istream& is, laptop& item);
private:
	double screen_size;
	double weight;
};

ostream& operator<<(ostream& os, laptop& item)
{
	os << item.name << " " << item.price << " " << item.CPU << " "
		<< item.GPU << " " << item.screen_size << " " << item.weight;
	return os;
}

istream& operator>>(istream& is, laptop& item)
{
	is >> item.name >> item.price >> item.CPU >> item.GPU
		>> item.screen_size >> item.weight;
	return is;
}
class desktop :public computer//台式机
{
public:
	desktop() = default;
	friend ostream& operator<<(ostream& os, desktop& item);
	friend istream& operator>>(istream& is, desktop& item);
private:
	double TDP;
};

ostream& operator<<(ostream& os, desktop& item)
{
	os << item.name << " " << item.price << " " << item.CPU << " "
		<< item.GPU << " " << item.TDP;
	return os;
}

istream& operator>>(istream& is, desktop& item)
{
	is >> item.name >> item.price >> item.CPU >> item.GPU
		>> item.TDP;
	return is;
}

class periphe :public commodity//外设
{
public:
	
protected:
	string colour;
	string size;//格式:xx*xx*xx
};

class mouse :public periphe//鼠标
{
public:
	mouse() = default;
	friend ostream& operator<<(ostream& os, mouse& item);
	friend istream& operator>>(istream& is, mouse& item);
private:
	int DPI;
	string material;
};

ostream& operator<<(ostream& os, mouse& item)
{
	os << item.name << " " << item.price << " " << item.colour << " " << item.size << " "
		<< item.DPI << " " << item.material;
	return os;
}

istream& operator>>(istream& is, mouse& item)
{
	is >> item.name >> item.price >> item.colour >> item.size >> item.DPI >> item.material;
	return is;
}

class keyboard :public periphe//键盘
{
public:
	keyboard() = default;
	friend ostream& operator<<(ostream& os, keyboard& item);
	friend istream& operator>>(istream& is, keyboard& item);
private:
	int keynum;//按键数
	string axis;//轴体
};

ostream& operator<<(ostream& os, keyboard& item)
{
	os << item.name << " " << item.price << " " << item.colour << " " << item.size << " "
		<< item.keynum << " " << item.axis;
	return os;
}

istream& operator>>(istream& is, keyboard& item)
{
	is >> item.name >> item.price >> item.colour >> item.size >> item.keynum >> item.axis;
	return is;
}


cstore.cpp
#include"cstore.h"
#include<list>
#include<algorithm>
#include<vector>
using namespace std;

void add(list<laptop>& lap,list<desktop>& des,list<mouse>& mou,list<keyboard>& key);
void show(list<laptop>& lap, list<desktop>& des, list<mouse>& mou, list<keyboard>& key);
int main()
{
	list<laptop> lap;
	list<desktop> des;
	list<mouse> mou;
	list<keyboard> key;
	ifstream ilap(".//laptop.txt", ios::app), ides(".//desktop.txt", ios::app),
		imou(".//mouse.txt", ios::app), ikey(".//keyboard.txt", ios::app);
	ofstream olap(".//laptop.txt",ios::app), odes(".//desktop.txt",ios::app),
		omou(".//mouse.txt",ios::app), okey(".//keyboard.txt",ios::app);
	laptop l; desktop d; mouse m; keyboard k;
	while (ilap >> l)
	{
		lap.push_back(l);
	}
	while (ides >> d)
	{
		des.push_back(d);
	}
	while (imou >> m)
	{
		mou.push_back(m);
	}
	while (ikey >> k)
	{
		key.push_back(k);
	}
	while (1)
	{
		cout << "请输入想要执行的操作（1，添加商品--2，展示商品--3,退出）" << endl;
		int op = 0;
		cin >> op;
		switch (op)
		{
		case 1:add(lap, des, mou, key); break;
		case 2:show(lap, des, mou, key); break;
		case 3:break;
		}

	}
	for (auto i : lap)
	{
		olap << i;
	}
	for (auto i : des)
	{
		odes << i;
	}
	for (auto i : mou)
	{
		omou << i;
	}
	for (auto i : key)
	{
		okey << i;
	}

	return 0;
}

void add(list<laptop>& lap, list<desktop>& des, list<mouse>& mou, list<keyboard>& key)
{
	cout << "add---";
	cout << "请输入想要添加商品的类别：1笔记本 2台式机 3鼠标 4键盘" << endl;
	int kind = 1;
	cin >> kind;
	switch (kind)
	{
	case 1:
	{
		laptop la;
		cout << "依次输入，品名，价格，CPU，GPU，屏幕大小，重量" << endl;
		cin >> la;
		lap.push_back(la);
	}break;
	case 2:
	{
		desktop de;
		cout << "依次输入，品名，价格，CPU，GPU,TDP" << endl;
		cin >> de;
		des.push_back(de);
	}break;
	case 3:
	{
		mouse mo;
		cout << "依次输入，品名，价格，颜色，尺寸（xx*xx*xx),DPI，材质" << endl;
		cin >> mo;
		mou.push_back(mo);
	}break;
	case 4:
	{
		keyboard ke;
		cout << "依次输入，品名，价格，颜色，尺寸（xx*xx*xx),按键数，轴体" << endl;
		cin >> ke;
		key.push_back(ke);
	}break;
	default:break;
	}
}

void show(list<laptop>& lap, list<desktop>& des, list<mouse>& mou, list<keyboard>& key)
{
	cout << "请输入想要展示的商品列表类比：1笔记本 2台式机 3鼠标 4键盘" << endl;
	int kinds = 1;
	cin >> kinds;
	switch (kinds)
	{
	case 1:
	{
		for (auto iter : lap)
		{
			cout << iter << endl;
		}
	}break;
	case 2:
	{
		for (auto iter : des)
		{
			cout << iter << endl;
		}break;
	}
	case 3:
	{
		for (auto iter : mou)
		{
			cout << iter << endl;
		}break;
	}
	case 4:
	{
		for (auto iter : key)
		{
			cout << iter << endl;
		}break;
	}
	default:break;
	}
}
