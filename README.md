# SoftSkills
для віддлагодження поекту 
#include <iostream>
#include <bitset>
#include <fstream>
#include <string>
#include <vector>
#include "librarys.h"
using namespace std;

// меняем линии 
void swap_line(vector<vector<string>>& arr, int first, int second) {
    for (int i = 0; i < arr.size(); ++i) {
        swap(arr[first][i], arr[second][i]); // swap меняет местами символы 
    }
}
// меняем колонки 
void swap_column(vector<vector<string>>& arr, int first, int second) {
    for (int i = 0; i < arr[0].size(); ++i) {
        swap(arr[i][first], arr[i][second]);
    }
}
// сортировка по алфавиту   
void gibrid_sort(vector<vector<string>>& arr) {
    for (int i = 1; i < arr[0].size() - 1; ++i)
    {
        for (int j = 0; j < arr[0].size() - 1; ++j)
            if (arr[0][j + 1] < arr[0][j])
                swap_column(arr, j, j + 1);
    }
    for (int i = 1; i < arr.size() - 1; ++i)
    {
        for (int j = 0; j < arr.size() - 1; ++j)
            if (arr[j + 1][0] < arr[j][0])
                swap_line(arr, j, j + 1);
    }
}


int main()
{
    setlocale(LC_ALL, "rus");
    int size = 0;
    int n, cols, q = 0, z = 0;
    int counter = 0;
    string** Mas;
    vector<vector<string>> FinalMas;
    cout << "Size is: ";
    cin >> size;
    n = sqrt(size);
    cols = n + 1;
    Mas = new string * [n];
    for (int i = 0; i < n; ++i)
        Mas[i] = new string[n];
    vector <int> arr(size);
    input_arr(arr);
    output_arr(arr);

    output_to_file(arr);
    vector <string> horizontal(n); // для записи в горизонтали и вертикаль 
    vector <string> vertical(n);  // 
    vector <string> alphabet;
    vector <string> alphabet_code;
    vector <string> string_arr;

    read_alphabet(alphabet);
    for (int i = 0; i < alphabet.size(); ++i) {
        cout << alphabet[i] << "\n";
    }

    read_nums(string_arr);
    for (int i = 0; i < string_arr.size(); ++i) {
        cout << string_arr[i] << "\n";
    }

    read_values(alphabet_code);
    for (int i = 0; i < alphabet_code.size(); ++i) {
        cout << alphabet_code[i] << "\n";
    }

    for (int i = 0; i < string_arr.size(); ++i)
    {
        for (int q = 0; q < alphabet_code.size(); ++q)
        {
            if (string_arr[i] == alphabet_code[q]) {
                string_arr[i] = alphabet[q];
            }
        }
    }
// для заполнения массива символами из бинарных чисел 
    for (int i = 0; i < string_arr.size(); ++i) {
        for (int k = 0; k < n; ++k) {
            for (int j = 0; j < n; ++j) {
                Mas[k][j] = string_arr[counter]; // нужен для перехода на следующую строчку после итерации  
                counter++;                                
                i++;    // для движения по строке массива 
            }
            cout << endl;
        }
    }
    // ввод слов вертикаль, горизонатль 
    cout << "Enter the first word(by letter): " << endl;
    for (int i = 0; i < n; ++i) {
        cin >> horizontal[i];
    }
    cout << "Enter the second word(by letter): " << endl;
    for (int i = 0; i < n; ++i) {
        cin >> vertical[i];
    }
   //

    for (int i = 0; i < n; ++i)
    {
        if (i == 0) {
            cout << "  " << horizontal[i]; // для отсупа в матрице между словами (горизонталь и вертикаль)
        }
        else
            cout << " " << horizontal[i];
    }
    cout << endl;
    // заполниние символами вертикали матрици  
    for (int i = 0; i < n; ++i) {
        cout << vertical[i];
        for (int j = 0; j < n; ++j) {
            cout << ' ' << Mas[i][j];
        }
        cout << endl;
    }

    for (int i = 0; i < cols; ++i) {
        vector <string> temp; // временный массив для без алфавитного расположения 
        for (int j = 0; j < cols; ++j) {
            if (i == 0 && j == 0) {
                temp.push_back("  "); // отступ [0][0] в объед. матрице 
            }
            else if (i == 0) {
                temp.push_back(horizontal[j - 1]); // [0][1] = [0][0] запись строки горизонатль 
            }
            if (i != 0 && j == 0) {
                temp.push_back(vertical[i - 1]);// [1] [0] = [0][0] запись строки вертикаль 
            }
            if (i != 0 && j != 0) {
                temp.push_back(Mas[q][z]); // запись символов (начало шифровки)
                z++;
            }
        }
        if (i != 0) {
            q++;
            z = 0;
        }
        FinalMas.push_back(temp); // заполняем динамический 2х вектор 
        cout << endl;
    }
    // цикл для выведения двумерного массива символов с отступом 
        for (int i = 0; i < cols; i++) {
            for (int j = 0; j < cols; j++) {
                if (i == 0 && j == 0)
                    cout << FinalMas[i][j];
                else
                    cout << FinalMas[i][j] << ' ';
            }
            cout << endl;
        }

    cout << endl;
    // вывод и сортировка по алфавиту 
    gibrid_sort(FinalMas);
    for (int i = 0; i < cols; i++) {
        for (int j = 0; j < cols; j++) {
            if (i == 0 && j == 0)
                cout << FinalMas[i][j];
            else
                cout << FinalMas[i][j] << ' ';
        }
        cout << endl;
    }
    cout << endl;
    return 0;
}
