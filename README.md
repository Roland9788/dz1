#include <iostream>
#include <fstream>
#include <iomanip>
#include <string>
#include <conio.h>
#include <windows.h>
////Максимальная длина пути к файлу
//#define MAX_PATH 260
////Количество столбцов на экране
//#define NUM_COLS 18
////Количество строк на экране
//#define NUM_ROWS 24



using namespace std;

template<typename T>
void Sort(T** a, int n, int m)
{
	for (int k = 0; k < n; k++)
		for (int p = 0; p < m; p++)
			for (int i = 0; i < n; i++)
				for (int j = 0; j < m; j++)
					if (a[i][j] > a[k][p])
						swap(a[i][j], a[k][p]);
}

void main()
{
	cout << (int)'А'<<endl;
	cout << (int)'Б' << endl;
	cout << (int)'В' << endl;
	cout << (int)'Г' << endl;
	cout << (int)'Я' << endl;
	srand(time(NULL));
	char Answer;
	const int MessageCount = 9;
	int i, j;
	//Подсказки
	enum { CHOICE = 3, INPUT_FILENAME, INPUT_DIMENSIONS, INPUT_ELEMENT, FILE_ERROR };
	//Сообщение 
	char Msg[MessageCount][50] =
	{
		"1. Вывести данные из текстового файла\n",
		"2. Записать данные в текстовый файл\n",
		"3. Выход из программы\n",
		"\n Ваш выбор: ",
		"Введите имя обрабатываемого файла: ",
		"Введите размерность матрицы: ",
		"Введите элементы матрицы: ",
		"Заполнить рандомно(да-1/нет-2)",
		"Невозможно открыть файл"
	};
	//Руссификация сообщений и вывод меню на экран
	for (i = 0; i < MessageCount; i++)
	{
		CharToOemA(Msg[i], Msg[i]);
	}
	do
	{
		for (int i = 0; i < 4; i++)
		{
			cout << Msg[i];
		}
		cin >> Answer;

	} while (Answer < '1' || Answer>'3');
	if (Answer == '3')
	{
		return;
	}
	//Переменная для имени файла
	char FileName[80];
	//Размерность матрицы
	int M, N;
	char num;
	cout << "\n" << Msg[INPUT_FILENAME];
	cin >> FileName;
	//Если выбран первый пункт меню, то выводим данные из текстового файла экран
	if (Answer == '1')
	{
		//Если файл с указанным именем уже существует выводим сообщение об ошибке
		ifstream inF(FileName, ios::in | ios::_Nocreate);
		if (!inF)
		{
			cout << "\n" << Msg[FILE_ERROR];
			return;
		}
		//Считываем размерность массива
		inF >> M;
		inF >> N;
		
		//Считываем элементы массива из файла и выводим их сразу на экран
		for (i = 0; i < M; i++)
		{
			for (j = 0; j < N; j++)
			{
				inF >> num;
				cout << setw(6) << num;
			}
			cout << "\n";
		}
		inF.close();
	}

	//Если выбран второй пункт меню, то выводим данные из текстового файла экран
	if (Answer == '2')
	{
		//Открываем файл для записи.
		// Если файла не существует то программа создает его
		ofstream outF(FileName, ios::out);
		if (!outF)
		{
			cout << "\n" << Msg[FILE_ERROR];
			return;
		}
		//Запрашиваем размерность матрицы и записываем данные в файл
		cout << Msg[INPUT_DIMENSIONS];
		cout << "\nM: ";
		cin >> M;
		cout << "\nN: ";
		cin >> N;

		outF << M << " " << N << "\n";
		char** matrix = new char* [M];
		for (int i = 0; i < M; i++)
		{
			matrix[i] = new char[N];
		}
		int answ;
		cout << Msg[7];
		cin >> answ;
		if (answ == 1)
		{
			//Запрашиваем элементы массива и записываем их в файл
			for (i = 0; i < M; i++)
			{
				for (j = 0; j < N; j++)
				{			
					matrix[i][j] = -64 + rand() % (-33 + -64);

					cout << matrix[i][j] << " ";
				}
			}

			Sort(matrix,M,N);
			
			for (i = 0; i < M; i++)
			{
				for (j = 0; j < N; j++)
				{
					outF << matrix[i][j] << " ";

				}
				outF << "\n";
			}

		}
		if (answ == 2)
		{
			cout << Msg[INPUT_ELEMENT];
			//Запрашиваем элементы массива и записываем их в файл
			for (i = 0; i < M; i++)
			{
				for (j = 0; j < N; j++)
				{
					cout << "Array[" << i << "]" << "[" << j << "] = ";
					cin >> num;
					outF << num << " ";
				}
				outF << "\n";
			}
			outF.close();
		}
	}

}
