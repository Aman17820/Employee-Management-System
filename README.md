# Employee-Management-System
//A program for managing employees. One can store ,edit and delete details about employees in file system on your desktop.
#include <iostream>
#include<fstream>
#include<ios>
#include<limits>
#include<conio.h>
#include<stdlib.h>
#include<string>
#include<iomanip>
using namespace std;
class Employee
{
public:
  int eid;
  char ename[20];
  char eage[20];
  char esalary[20];
  char edepartment[20];
  void input()
  {
    cout << "Enter id of Employee : ";
    cin >> eid;
    cout << "Enter name of Employee : ";
    cin>>ename;
    cout << "Enter Age of Employee : ";
    cin >> eage;
    cout << "Enter Salary of Employee : ";
    cin>>esalary;
    cout << "Enter Department of Employee : ";
    cin>>edepartment;
    cout << endl;

  }
  void output ()
  {
    cout<<setw(10)<<eid<<setw(22)<<ename<<setw(22)<<eage<<setw(22)<<esalary<<setw(22)<<edepartment<<endl;

  }

};

int main ()
{
  Employee emp;
  string logid;
  string logpass;
  cout<<"\n\n\n\n\n\n\n\t\t\t----------------------------------------------------------------\n";
  cout<<"\t\t---------------------WELCOME TO EMPLOY MANAGEMENT SYSTEM----------------------\n";
  cout<<"\t\t------------------------------------------------------------------------------\n";
  cout<<"\n\t\tEnter ID and PASSWORD :\n";
  cout<<"\t\tID :";
  cin>>logid;
  cout<<"\t\tPASSWORD:";
  cin>>logpass;
  if(logid=="srmu@gmail.com"&&logpass=="aman")
  {
     cout<<"Login Successfull!\n";
     char ans = 'y';
     int ch;
     do
    {
      system("CLS");
      cout<<"Enter '0' to create New database(Carefull:- All previously saved data will be Erased)"<<endl;
      cout << "Enter '1' to add records " << endl;
      cout << "Enter '2' to view all records" << endl;
      cout << "Enter '3' to search an Record ." << endl;
      cout << "Enter '4' to Modify an Record ." << endl;
      cout << "Enter '5' to delete a Record ." << endl;
      cout << "Enter '6' to Exit ." << endl;
      cout<<"(-----Tip:- Use _ for spacing purpose-----)"<<endl;
      cout << "\nEnter your choice : ";
      cin >> ch;
      if (ch == 0)

	{
    ofstream fout;
	  fout.open ("Employeee.txt", ios::trunc);
	  char con;
	  do
	    {
	      emp.input();
	      fout.write ((char *) &emp, sizeof (emp));
	      cout << "Want to add more record? ";
	      cin >> con;
	    }
	  while (con == 'y');
	  fout.close ();
	}

     else if (ch == 1)

	{
	  ofstream foute;
	  foute.open ("Employeee.txt", ios::app|ios::binary);
	  char con;
	  do
	    {
	      emp.input ();
	      foute.write ((char *) &emp, sizeof (emp));
	      cout << "Want to add more record? ";
	      cin >> con;
	    }
	  while (con == 'y');
	  foute.close();
	}
      else if (ch == 2)
	{
	  ifstream fin;
	  fin.open ("Employeee.txt", ios::in|ios::binary);
	  cout << setw(10)<<"Id" << setw(22)<< "Name" << setw(22) << "Age"<<setw(22)<<"Salary"<<setw(22)<<"Department"<<endl;
	  cout<<"---------------------------------------------------------------------------------------------------"<<endl;
	  while (fin.read ((char *) &emp, sizeof (emp)))
	    {
	        emp.output ();
	    }
	}

	else if(ch==3)
    {
        int sid;
        int found=0;
        cout<<"Enter Employee ID to search :";
        cin>>sid;
        ifstream fin;
        fin.open("Employeee.txt",ios::in|ios::binary);
        while(fin.read ((char *) &emp, sizeof (emp)))
        {
            if (emp.eid==sid)
            {
                 cout << setw(10)<<"Id" << setw(22)<< "Name" << setw(22) << "Age"<<setw(22)<<"Salary"<<setw(22)<<" Department"<<endl;
                 cout<<"---------------------------------------------------------------------------------------------------"<<endl;
                 emp.output();
                 found=1;
                 break;
            }
        }
        if (found==0)
        {
            cout<<"Record not found \n";
        }
        fin.close();
    }

    else if(ch==4)
    {
        int sid;
        cout<<"Enter ID to be Modified :";
        cin>>sid;
        long pos;
        fstream fio;
        fio.open("Employeee.txt",ios::in|ios::out|ios::binary);
        while(!fio.eof())
        {
            pos=fio.tellg();
            fio.read((char*)&emp,sizeof(emp));
            if(emp.eid==sid)
            {
                emp.input();
                fio.seekg(pos);
                fio.write((char *)&emp,sizeof(emp));
                cout<<"\nRecord Modification Completed.\n";
            }
            else
                {
                    cout<<"Record not found..\n";
                }

        }
    }

 else if(ch==5)
    {
        int sid;
        cout<<"Enter id to be Deleted :";
        cin>>sid;
        fstream fio;
        fstream temp;
        temp.open("temp.txt",ios::out|ios::binary);
        fio.open("Employeee.txt",ios::in|ios::binary);
        while(fio.read((char *)&emp,sizeof(emp)))
        {
            if(sid==emp.eid)
            {
                if (emp.eid!=sid)
                {
                    temp.write((char *)&emp,sizeof(emp));

                }
            }
            else
                {
                    cout<<"Record not found..\n";
                }
        }

        temp.close();
        fio.close();
        remove("Employeee.txt");
        rename("temp.txt","Employeee.txt");
    }

    else if(ch==6)
    {
        exit(6);
    }
    else
    {
        cout<<"\nWrong Character Entered\n";
    }

    cout << "FOR MAIN MENU PRESS 'Y'?";
    cin >> ans;

    }
  while (ans == 'y'||ans=='Y');
  }
  else
  {
      cout<<"Wrong credentials";
  }
  cout<<"\nPress any key to Exit the App....";
  getch();
  return 0;
}
