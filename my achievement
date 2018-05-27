#include <iostream>
#include "stdafx.h"
sqlite3_stmt *stmtU;
sqlite3 *db = 0; // хэндл объекта соединение к БД

using namespace std;
const char* SQL = "CREATE TABLE IF NOT EXISTS Labs(num_lab, name, lab_ex, name_meas, meas_1, meas_2, meas_3, meas_4, meas_5, meas_6, meas_7,meas_8); ";

static int callback(void *NotUsed, int argc, char **argv, char **azColName)
{
	int i;
	for (i = 0; i<argc; i++)
	{
		cout << azColName[i] << " = " << (argv[i] ? argv[i] : "NULL") << "\n";
	}
	cout << "\n";
	return 0;
}

int main(int argc, char **argv) 
{
	ifstream lab("C://project/lab.txt");
	ofstream result("C://project/result.txt");
	string str;
	char *err = 0;
	char *sql;
	int rc, Meas;
	const char * cname = str.c_str(), *cnameMeas = str.c_str();//перевод из string в char  
	sql = "INSERT INTO Labs (num_lab, name, lab_ex, name_meas, meas_1, meas_2, meas_3, meas_4, meas_5, meas_6, meas_7,meas_8)\
			 VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?);";
	sqlite3_prepare16(db, sql, -1, &stmtU, 0);
	//---------------------
	// открываем соединение
	//---------------------
	if (sqlite3_open16(L"Labs.db", &db))
		fprintf(stderr, "Ошибка открытия/создания БД: %s\n", sqlite3_errmsg(db));
	//--------------
	// выполняем SQL
	//--------------
	else if (sqlite3_exec(db, SQL, 0, 0, &err))
	{
		fprintf(stderr, "Ошибка SQL: %sn", err);
		sqlite3_free(err);
	}
	//---------------
	//work with files
	//---------------
	
	if (!lab.is_open())
	{
		cout << "Такого файла нет." << endl;
		return -1;
	}
	
	while (!lab.eof())
	{
		
		getline(lab, str);
		if (str == "push")
		{
			//sqlite3_prepare16(db, sql, -1, &stmtU, 0);//подготовка к выполнению запроса
			//заполнение "?" в запросе sql
			for (int i = 1; i < 5; i++)
			{
				if (i % 2 != 0)
				{
					getline(lab, str,';');
					Meas = atoi(str.c_str());//преобразование из string в int
					sqlite3_bind_int(stmtU, i, Meas);
				}
				else
				{
					getline(lab, str,';');
					sqlite3_bind_text(stmtU, i, cname, -1, 0);
				}
			}
			for (int i = 5; i < 13; i++)
			{
				getline(lab, str, ' ');
				Meas = atoi(str.c_str());//преобразование из string в int
				sqlite3_bind_int(stmtU, i, Meas);
			}
			//for (int i = 0; i < 13; i++)
			//	cout << stmtU << endl;
			sqlite3_step(stmtU);//выполняем команду
			cout << sqlite3_column_value(stmtU, 0)<<endl;
			sqlite3_reset(stmtU);//сброс параметром команды (для реализации следующих запросов)
			sqlite3_finalize(stmtU);//завершение работы команды
			//while (!lab.eof())
			//{
			//	sql = "INSERT INTO Labs (num_lab, name, lab_ex, name_meas, meas_1, meas_2, meas_3, meas_4, meas_5, meas_6, meas_7,meas_8) VALUES ()";
				//sqlite3_prepare16(db, sql, -1, &stmtU, 0);
					
			//	sqlite3_step(stmtU);//выполняем команду
			//	sqlite3_reset(stmtU);//сброс параметром команды (для реализации следующих запросов)
			//	sqlite3_finalize(stmtU);//завершение работы команды
			//}
		}
	
		
		if (str == "result")
		{

		}
	}
	// закрываем соединение
	sqlite3_close(db);
	system("pause");
	return 0;
}
