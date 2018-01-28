Programming an ATM in C++.

## Functionalities:
1. Welcome message and prompt the user to enter a correct password (otherwise they will not proceed further)
4. Possibilities once logged in: Open a new account, Delete account, Modify account, Transaction, View account, List of accounts and Exit.
5. After performing desired transaction by the user, it will take the user to the menu screen.

#### Extract: Options
![png](images/opt.PNG)

#### Extract: View of the account
![png](images/view.PNG)

#### Extract: Withdrawing money
![png](images/withdraw.PNG)


```cpp
#include<time.h>
#include<fstream.h>
#include<iostream.h>
#include<conio.h>
#include<ctype.h>
#include<process.h>
#include<iomanip.h>
#include<stdio.h>
#include<string.h>
#include<dos.h>
#include<stdlib.h>
void welcome_screen();
void welcome_screen()
     {
	clrscr();
	gotoxy(20,10);
	cputs("**************** W E L C O M E ****************");
	gotoxy(25,12);
	cputs("*** T O  S W I F T  B A N K  A T M *** " );
	gotoxy(23,14);
	gotoxy(25,25);
	textcolor(BLACK+BLINK);
	cputs(" *** PRESS ANY KEY TO CONTINUE ***");
	getch();
	return;
     }
class atm
     {
       public:
	       long ini;
	       int money;
	       int mon;
	       int record;
	       long ph1;
	       char address[50];
	       char name_ver[50];
	       char name[20];
	       void modification();
	       void op_acc();
	       void del();
	       void menu();
	       void view_acc();
	       void view1();
	       void init();
	       void display();
	       void view();
	       char check(char *);
	       void loadingbar();
	       void trans();
	       void help();
	       int test();
	       void passcheck();
	    }obj;
void atm::op_acc() 			 //ADDING  INFORMATION
    {
      ofstream fout;
      fout.open("harry2",ios::out|ios::app);
      init();
      fout.write((char*)&obj,sizeof(obj));
      fout.close();
    }
int atm::test() 				//FIND NO. OF RECORDS
    {
      ifstream fin;
      fin.open("harry2");
      fin.seekg(0,ios::end);
      int n;
      n=fin.tellg()/sizeof(obj);
      cout<<" \n NUMBER OF ACCOUNTS = "<<n ;
      return n ;
    }
void atm::view_acc()  			//SEARCHING FOR A PARTICULAR NUMBER
    {
      ifstream fin;
      fin.open("harry2");
      if(fin.fail())
	{
	  cout<<" \n FILE NOT FOUND ";
	  getch();
	  return;
	}
      clrscr();
      textcolor(BLACK+BLINK);
      gotoxy(30,1);
      cprintf(" SEE ACCOUNT ");
      cout<<" \n ENTER PHONE NUMBER : ";
      long ph;
      cin>>ph;
      int n;
      n=test();
      for(int i=0;i<n;i++)
	{
	  fin.read((char*)&obj,sizeof(obj));
	  if(ph==ph1)
	    {
	      view1();
	      return;
	    }
	}
      if(fin.eof())
	{
	  cout<<" \n ACCOUNT NOT FOUND ";
	}
    }
void atm::init()  				// ENTERING THE DETAILS
    {
      clrscr();
      char ch;
      textcolor(BLACK+BLINK);
      gotoxy(30,1);
      cprintf(" OPEN A ACCOUNT ");
      textcolor(BLACK);
      time_t tim;
	time(&tim);
	cout<<endl<<endl<<" DATE AND TIME IS:"<<ctime(&tim);
	gotoxy(1,10);
	cout<<"Enter 10 digit phone number";
	gotoxy(1,3);
      cout<<" \n ENTER  PHONE NUMBER : ";
      cin>>ph1;
	cin.get(ch);
	gotoxy(1,10);
	cout<<"Name should not more than 25 letters";
	gotoxy(1,4);
      cout<<" \n ENTER NAME : ";
      cin.getline(name,20,'\n');
      gotoxy(1,10);
      cout<<"Address should not more than 50 letters";
      gotoxy(1,5);
      cout<<" \n ENTER YOUR ADDRESS : ";
      cin.getline(address,50,'\n');
      gotoxy(1,10);
      cout<<"Name should not more than 25 letters";
      gotoxy(1,6);
      cout<<" \n ENTER NAME OF VERIFYING PERSON: ";
      cin.getline(name_ver,50,'\n');
      gotoxy(1,10);
      cout<<"Balance should not less than 1000";
      gotoxy(1,7);
      cout<<" \n ENTER INITIAL DEPOSIT : ";
      cin>>ini;
      }
void atm::view1() 				//TO DISPLAY ALL THE RECORDS
    {
      cout<<"\n";
      cout<<" PHONE NUMBER1 : "<<obj.ph1<<"\n";
      cout<<" NAME : "<<obj.name<<"\n";
      cout<<" YOUR ADDRESS: "<<obj.address<<"\n";

      cout<<" NAME OF VERIFYING PERSON: "<<obj.name_ver<<"\n";
      cout<<" INITIAL DEPOSIT : "<<obj.ini<<"\n";
      getch();
    }
void atm::modification() 			//TO MODIFY ANY DATA IN  THE RECORD IF NECESSARY
   {
     clrscr();
     textcolor(BLACK+BLINK);
     gotoxy(30,1);
     cprintf(" EDIT ACCOUNT ");
     textcolor(BLACK);
     long ph;
     int n,i;
     ifstream fin;
     ofstream fout;
     fin.open("harry2");
     if(fin.fail())
       {
	 cout<<"\n FILE NOT FOUND !";
	 fout.close();
	 exit(-1);
       }
    fout.open("new1");
    n=test();
    if(n==0)
      {
	cout<<"\n FILE IS EMPTY ! ";
	getch();
	return;
      }
   while(fin.good())
      {
	fin.read((char*)&obj,sizeof(obj));
	fout.write((char*)&obj,sizeof(obj));
      }
   fin.close();
   fout.close();
   fout.open("harry2",ios::trunc);
   fin.open("new1");
   if(fin.fail())
     {
      cout<<"\n FILE NOT FOUND !";
      exit(-1);
     }
   char ch;
   cout<<"\n ENTER PHONE NUMBER :";
   cin>>ph;
   ch=cin.get();
   for(i=0;i<n;i++)
	{
	   fin.read((char*)&obj,sizeof(obj));
	   char d;
	   if(ph==ph1)
	      {
		view1();
		d=check(" PHONE NUMBER ");
		if((d=='y') || (d=='Y'))
		  {
		    cout<<"\n ENTER NEW PHONE NUMBER :";
		    cin>>ph1;
		    ch=cin.get();
		    }
		if(check("NAME")=='y')
		  {
		    cout<<"\n ENTER NEW NAME : ";
		    cin.getline(name,20,'\n');
		  }
		if(check(" ADDRESS")=='y')
		  {
		    cout<<"\n ENTER NEW ADDRESS :";
		    cin.getline(address,50,'\n');
		  }
		if(check(" VERIFYING PERSON NAME:")=='y')
		  {
		    cout<<"\n ENTER NEW NAME :";
		    cin.getline(name_ver,50,'\n');
		  }
		if(check( "INITIAL DEPOSIT :")=='y')
		{
		  cout<<"\n ENTER INITIAL DEPOSIT:";
		  cin>>ini;

		}
	      }
	   fout.write((char*)&obj,sizeof(obj));
	}
   fout.close();
   fin.close();
    }
char  atm::check(char *s)
    {
       cout<<"\n WANT TO MODIFY \t "<<s<<"\t"<<"Y/N";
       char ch;
      ch =getch();
       if((ch=='y')||(ch=='Y'))
	return 'y';
       else
	return 'n';
    }
void  atm::del()  				//DELETE ACCOUNT
    {
       clrscr();
       textcolor(BLACK+BLINK);
       gotoxy(30,1);
       cprintf("DELETE A ACCOUNT");
       long ph;
       int n,i;
       ifstream fin;
       ofstream fout;
       fin.open("harry2");
       if(fin.fail())
	{
	  cout<<"\n FILE NOT FOUND ! ";
	  getch();
	  return;
	}
       fout.open("new1");
       n=test();
       if(n==0)
	{
	  cout<<"\n FILE IS EMPTY ! ";
	  getch();
	  return;
	}
      for(i=0;i<n;i++)
	{
	  fin.read((char*)&obj,sizeof(obj));
	  fout.write((char*)&obj,sizeof(obj));
	}
      fin.close();
      fout.close();
      fout.open("ch1",ios::trunc);
      fin.open("new1");
      if(fin.fail())
	{
	  cout<<"\n FILE NOT FOUND ! ";
	  getch();
	  return;
	}
     cout<<endl<<"ENTER PHONE NUMBER:"<<endl;
     cin>>ph;
     for(i=0;i<n;i++)
       {
	 fin.read((char*)&obj,sizeof(obj));
	 if(ph!=ph1)
	    fout.write((char*)&obj,sizeof(obj));
	cout<<endl<<”ACCOUNT HAS BEEN DELETED”;	
       }

     fout.close();
     fin.close();
    }
 void atm::view()			//DISPLAYS LIST OF ACCOUNTS
   {
     ifstream fin;
     int n,j;
     fin.open("harry2");
     if(fin.fail()||fin.bad())
       {
	  cout<<"\n FILE NOT FOUND ! ";
	  getch();
	  return;
       }
     clrscr();
     int i=0;
     n=test();
     for(j=0;j<n;j++)
       {
	 cout<<"\n ACCOUNT NUMBER "<<i+1<<"\n";
	 fin.read((char*)&obj,sizeof(obj));
	 cout<<"\n PHONE NUMBER :"<<obj.ph1<<"\n";
	 cout<<"\n NAME :"<<obj.name<<"\n";
	 cout<<"\n YOUR ADDRESS:"<<obj.address<<"\n";
	 cout<<"\n NAME OF VERIFYING PERSON:"<<obj.name_ver<<"\n";
	 cout<<"\n INITIAL DEPOSIT :"<<obj.ini<<"\n";

	 i+=1;
       }
      fin.close();
      getch();
}
void  atm::menu()			//DISPLAYS MENU ITEMS
    {
       char ch;
       clrscr();
       textbackground(WHITE);
       textcolor(BLACK);
       gotoxy(18,6);
       cprintf(" CHOOSE ONE OPTION FROM THE GIVEN OPTIONS: ");
       gotoxy(30,8);
       cprintf(" O:OPEN AN ACCOUNT ");
       gotoxy(30,10);
       cprintf(" D:DELETE ACCOUNT \n  \r ");
       gotoxy(30,12);
       cprintf(" M:MODIFY ACCOUNT\r ");
       gotoxy(30,14);
       cprintf(" L:LIST OF ACCOUNTS \n \r ");
       gotoxy(30,16);
       cprintf(" V:VIEW ACCOUNT \n \r ");
       gotoxy(30,18);
       cprintf(" T:TRANSECTIONS \n \r");
       gotoxy(30,20);
       cprintf(" H:HELP \n \r ");
       gotoxy(30,22);
       cprintf(" E:EXIT \n \r ");
       ch=getch();
      switch(ch)
	   {
	     case 'o':
	     case 'O':
		op_acc();
		break;
	     case 'd' :
	     case 'D' :
		del();
		break;
	     case 'm':
	     case 'M':
		modification();
		break;
	     case 'l':
	     case 'L':
		view();
		break;
	     case 'v':
	     case 'V':
		view_acc();
		break;
	     case 'T':
	     case 't':
		trans();
		break;
	     case 'h':
	     case 'H':
		  help();
	     case 'e':
	     case 'E':
		system("cls");
		exit(0);
	 }
}
void  atm::trans()				//CALL TO OTHER FUNCTIONS
{
clrscr();
char e;
gotoxy(18,6);
cout<<"CHOOSE OPTION FROM THE GIVEN OPTIONS: "<<endl;
gotoxy(30,8);
cout<<"D. DEPOSIT BALANCE"<<endl ;
gotoxy(30,10);
cout<<"W. WITHDRAW MONEY"<<endl;
cin>>e;
switch(e)
{
 case 'd':
 case 'D':
 clrscr();
 cout<<endl<<"YOUR INITIAL BALANCE IS:"<<obj.ini<<endl;;
 cout<<endl<<"ENTER THE AMOUNT YOU WANT TO DEPOSIT:";
 cin>>money;
obj.ini=obj.ini+money;
 cout<<endl<<"YOUR UPDATED BALANCE IS:: "<<obj.ini;
  getch();
  break;
   case 'w':
   case 'W':
    clrscr();
   cout<<endl<<"YOUR INITIAL BALANCE IS:"<<obj.ini<<endl;
   cout<<endl<<"PLEASE ENTER AMOUNT YOU WANT TO WITHDRAW:";
   cin>>mon;
   obj.ini=obj.ini-mon;
   cout<<endl<<"YOUR BALANCE AFTER WITHDRAWAL IS:"<<obj.ini<<endl;
   getch();
   break;
}
 }
void atm::help(void)				// HELP FUNCTION
{
clrscr();
textcolor(BLACK+BLINK);
gotoxy(27,3);
cout<<"WELCOME TO HELP OPTION";
textcolor(BLACK);
delay(10);
gotoxy(10,5);
cout<<"1.IN THIS PROJECT YOU CAN KEEP RECORD OF DAILY BANKING ";
delay(10);
gotoxy(10,6);
cout<<"   TRANSACTIONS.		";
delay(10);
gotoxy(10,8);
cout<<"2.THIS PROGRAMME IS CAPABLE OF HOLDING ANY NO. OF ACCOUNTS ";
delay(10);
gotoxy(10,10);
cout<<"3.IN FIRST OPTION YOU CAN OPEN A NEW ACCOUNT ";
delay(10);
gotoxy(10,12);
cout<<"4.PERSON BY ENTERING SIMPLY PERSONAL DETAILS";
delay(10);
gotoxy(10,14);
cout<<"5.IN SECOND OPTION YOUN CAN DELETE YOUR ACCOUNTS. ";
delay(10);
gotoxy(10,16);
cout<<"6.THROUGH THIRD OPTION YOU CAN VIEW YOUR ACCOUNT ";
delay(10);
gotoxy(10,18);
cout<<"7.IN FOURTH OPTION YOU CAN VIEW LIST OF ACCOUNTS. ";
delay(10);
gotoxy(10,20);
cout<<"8.IN THE FIFTH OPTION YOU CAN DO TRANSECTION. ";
delay(10);
gotoxy(10,21);
cout<<"(DEPOSIT / WITHDRAWAL) ";
delay(10);
gotoxy(10,23);
cout<<"AND LAST OPTION IS QUIT (EXIT TO DOS).";
delay(10);
textcolor(WHITE+BLINK); textbackground(BLACK);
gotoxy(20,25);
cprintf(" PRESS ANY KEY TO CONTINUE ");
gotoxy(30,26);
getch();
for(int i=25;i>=1;i--)
   {
   delay(20);
   gotoxy(1,i);clrscr();
   }
}
void loadingbar();
void loadingbar()  			//MAKING A FUNCTION OF LOADING BAR.
{
char r=221;        
char s=220;       
char t=219;       
for (int k=2; k>0; k--)
{
clrscr();
gotoxy(30,11);    
delay(60);     
cout<<"LOADING,AUTHENTICATING USER";  
gotoxy(30,12);
for (int j=0; j<27;j++)
{
delay(60);
cout<<t;
}
}
}
void passcheck();
void passcheck()				//PASSWORD VALIDATION FUNCTION
{
char ch[9];
clrscr();
cout<<"Enter the password <any 8 characters>:";
w1:ch[0]=getch();
cout<<"*";
ch[1]=getch();
cout<<"*";
ch[2]=getch();
cout<<"*";
ch[3]=getch();
cout<<"*";
ch[4]=getch();
cout<<"*";
ch[5]=getch();
cout<<"*";
ch[6]=getch();
cout<<"*";
ch[7]=getch();
cout<<"*";
ch[8]='\0';
if(strcmp(ch,"harpreet") == 0)
{
loadingbar();
cout<<endl<<"LOGIN SUCCESSFULL";
}
else  
{
cout<<endl<<"LOGIN FAILED"<<endl;
cout<<endl<<"Please Enter Correct Password:";
goto w1;
}
}
int main()
    {
      welcome_screen();
       passcheck();
      for(;;)
      obj.menu();
      return 0;
   }
```
