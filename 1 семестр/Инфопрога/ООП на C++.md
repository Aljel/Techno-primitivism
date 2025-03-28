ООП
1) Структура и ее парадигмы:
	1) Абстракция
	2) Инкапсуляция
	3) Наследование
	4) Полиморфизм2
2) Класс (По умолчанию private)

```c++
struct date{
	int d, m, y;
};

struct student{
	std::string F, I, O;
	date dateOfBirth;
	int course, group;
	int mark[5];
	void print(){
		std::cout << F << " " << I << " " << O << " ";
		std::cout << dateOfBirth.d << " " << dateOfBirth.m << dateOfBirth.y;
		std::cout << course << " " << group;
		for (int i = 0; i < 5; i++)
			std::cout << mark[i] << " ";
		std::cout << "\n";
	}
	student create();
}
```