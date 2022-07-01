#include<iostream>
#include<fstream>
#include<curses.h>
#include<stdio.h>
#include<iomanip>
#include<stdlib.h>
#include<string.h>
using namespace std;
#define filename "std4.txt"
fstream ifile;
class student
{
char usn[15],name[20],age[5],branch[6],sem[5];
public:
void opener(fstream &ifile, char *fn , ios_base::openmode mode);
void read();
void pack();
void display();
void unpack();
int search();
};
void student::opener(fstream &sfile, char *fn , ios_base::openmode mode)
{
sfile.open(fn,mode);
if(!sfile)
{
cout<<"unable to open a file"<<endl;
getch();
exit(1);
}
}
//function to read the student record
void student::read()
{
cout<<"enter the usn number:";
scanf("%s",usn);
cout<<"enter the name:";
scanf("%s",name);
cout<<"enter the age:";
scanf("%s",age);
cout<<"enter the branch:";
scanf("%s",branch);
cout<<"enter the sem:";
scanf("%s",sem);
pack();
}
// function to pack the student record using delimiter
void student::pack()
{
char buffer[75];
strcpy(buffer,usn);
strcat(buffer,"|");
strcat(buffer,name);
strcat(buffer,"|");
strcat(buffer,age);
strcat(buffer,"|");
strcat(buffer,branch);
strcat(buffer,"|");
strcat(buffer,sem);
strcat(buffer,"|");
ifile<<buffer<<"#";
}
//function to display student record
void student::display()
{
int count=0;
cout<<setiosflags(ios::left);
cout<<setw(5)<<"RRN"<<setw(15)<<"USN"<<setw(20)<<"NAME"<<setw(5);
cout<<"AGE"<<setw(10)<<"BRANCH"<<setw(5)<<"SEM"<<endl;
while(1)
{
ifile.getline(usn,15,'|');
if(ifile.eof())
break;
unpack();
count++;
cout<<setw(5)<<count<<setw(15)<<usn<<setw(20)<<name<<setw(5)<<age;
cout<<setw(10)<<branch<<setw(5)<<sem<<endl;
}
}
// function to unpack
void student::unpack()
{
char dummy[75];
ifile.getline(name,20,'|');
ifile.getline(age,5,'|');
ifile.getline(branch,6,'|');
ifile.getline(sem,5,'|');
ifile.getline(dummy,75,'#');
}
//function to search student record based on rrn.
int student::search()
{
int rrn,count=0;
char dummy[75];
cout<<"enter the rrn to be searched:";
cin>>rrn;
cout<<"RRN:"<<rrn;
while(1)
{
ifile.getline(usn,15,'|');
if(ifile.eof())
break;
count++;
if(rrn==count)
{
cout<<"\nRecord found\n";
unpack();
cout<<"USN:"<<usn<<"\n"<<"NAME:"<<name<<"\n"<<"AGE:"<<age;
cout<<"\n"<<"BRANCH:"<<branch<<"\n"<<"SEM:"<<sem<<"\n";
return 1;
}
else
ifile.getline(dummy,100,'#');
}
return -1;
}
// MAIN PROGRAM
int main()
{
int ch,pos;
student s;
curscr;
for(;;)
{
cout<<endl<<"1.for read\t2.for display\t3.for search\t4.for exit\n";
cout<<"Enter the choice:";
cin>>ch;
switch(ch)
{
case 1: s.opener(ifile,filename,ios::app);
cout<<"enter the student details\n";
s.read();
break;
case 2: s.opener(ifile,filename,ios::in);
cout<<"The student details are:"<<endl;
s.display();
break;
case 3:s.opener(ifile,filename,ios::in);
cout<<"To search record based on Relative record number(RRN) \n";
pos=s.search();
if(pos==-1)
cout<<"\nRRN number is out of range-Record not found\n";
break;
default:exit(0);
}
ifile.close();
}
return 0;
}
