#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <ctype.h>
	struct instruc      //defines structure instruction to store
	{
		char s[5][10];  //instruction name and its arguments
		int type;       //type of instruction (3 reg/2 reg/1 reg/no reg)
		int k;          //stores number of arguments scanned
		
	}
    inst[100];  
	
	 struct symbol   //defines structure symbol to store symbol table
	 {
		 char s[4];  //stores label name
		 int line;   //stores line number at which the label is found
		 
	 }sym[6];
	 int a[100];
 int func ;	 
 int instruc,symbols,r[14];//number of instructions and symbols 
 void compute_mach(FILE *fp);
 int labely(char m[],int t);
 int integer(char p[])   /*checks whether given string is an integer*/
 {
	 int i;
	 for(i=1;p[i]!='\0';i++)
	 {
		if(p[i]-'0'>=0 && p[i]-'0'<=9)  //if the character is between 0-9 ,
		  continue;                    //its a digit
	
		else
		  return 0;               //if not, return 0 
	 }
	 
	 return 1;
 }
 
    int three_reg_instr(char p1[]) /*checks whether instruction is 3-register inst*/
	{
		if(strcmp(p1,"ADD")==0 || strcmp(p1,"SUB")==0 ||strcmp(p1,"MUL")==0 || strcmp(p1,"DIV")==0 )
			return 1;
		else
			return 0;
	}
	
	int two_reg_instr(char p1[]) /*checks whether instruction is 2-register inst*/
	{
		if(strcmp(p1,"CMP")==0 || strcmp(p1,"NOT")==0 ||strcmp(p1,"LSHIFT")==0 || strcmp(p1,"RSHIFT")==0 || strcmp(p1,"STR")==0 || strcmp(p1,"MOV")==0 ||strcmp(p1,"LOAD")==0 || strcmp(p1,"IN")==0 || strcmp(p1,"OUT")==0)
			return 1;
		else
		    return 0;		

	}
	
	int one_reg_instr(char p1[]) /*checks whether instruction is 1-register inst*/
	{
		if(strcmp(p1,"INC")==0 || strcmp(p1,"DEC")==0 ||strcmp(p1,"PUSH")==0 || strcmp(p1,"POP")==0 || strcmp(p1,"SP")==0 || strcmp(p1,"JME")==0 ||strcmp(p1,"JNZ")==0 || strcmp(p1,"JNE")==0 || strcmp(p1,"JMP")==0 || strcmp(p1,"JEQ")==0 || strcmp(p1,"JGT")==0 )
			return 1;
		
		else
			return 0;
			
	}
	
	int instruct(FILE *fp,char p1[]) /*checks whether given instruction is valid instruction*/
	{
			fp=fopen("Opcode.txt","r"); //open opcode file in read mode
			char l1[17],l2[17];
			
			while(!feof(fp))
			{
				fscanf(fp,"%s %s",l1,l2); //scan instruction name and address
				
				if(strcmp(p1,l1)==0) //if instruc name matches with p1
					return 1;		//instruc found! return 1
			}
			return 0;	
	} 
	int regis(char p1[]) /*checks whether given register is valid register or not*/
	{
			if(p1[0]=='R') //if 1st character is R
			{
				int i;
				for(i=0;i<strlen(p1);i++)
				  {
					  if(p1[i]-'0'>=0 && p1[i]-'0'<=9) //if all other digits are numbers
						  continue; //continue till you find a non-digit char
					  else
						  return 0; //if non-digit, returns 0
				  }
					return 1; // valid register, return 1
			}
			return 0;		
	}
	int label(char p1[]) /*checks whether p1 is valid label or not*/
	{
		if(p1[0]=='L' && p1[2]==':') //if 1st character is L
		{
			switch(p1[1]-'0')
			{
				case 1:         //If next char is b/w 1-6, return 1
				case 2:
				case 3:
				case 4:
				case 5:
				case 6:
				return 1;
				default:
				return 0; //if not,return 0
				
			}
			
		}
		return 0; //if 1st char is not L,invalid label
		
	}
	
	void fcheck(FILE *fp,char p1[],FILE *fout) /*finds & prints in output file if character p1 is in file fp*/
	{
			rewind(fp);
			char p2[17],p3[17];
			
			while(!feof(fp))
				{   
					fscanf(fp,"%s %s\n",p2,p3); //scan name and address
					
					if(strcmp(p1,p2)==0)     //if found
						{
							fprintf(fout,"%s",p3); //print the corresponding adress              
							break;				 // in output file
						}
					
				}
				
			
	}
	int freg_chk(char s[]) /*checks whether the register used is b/w R0-R13*/
	{   
		if(strlen(s)>3) 
			return 0;
		
		if(s[0]!='R')
			return 0;
		
		if(strlen(s)==3)
		{
			if(s[2]-'0'>3 || s[1]-'0'>1)
				return 0;
			else
			{
				if(isdigit(s[1]) && isdigit(s[2]))
					return 1;
				else
					return 0;
				
			}
			
		}
		if(strlen(s)==2)
		{
			if(isdigit(s[1]))
				return 1;
			else
				return 0;
		}
		return 0;
		
		
	}
	void enter_input(FILE *f_in) /*takes input from user and write in input file*/
	{
			char m[17];
			printf("Enter Assembly Instructions: \n");
			
			while(1)
			{
				fscanf(stdin,"%s",m);
				fprintf(f_in,"%s\n",m);
				if(strcmp(m,"HLT")==0) //loop continues till user enters HLT
					break;
				
			}
			
	
	}
	
	int overflow(char pl[]) /*checks whether user has entered a value more than 2^15*/
	{   
	    int j,i;
		j=strlen(pl);
		char s[j];
		
		for(i=1;pl[i]!='\0';i++)
		{
			s[i-1]=pl[i];
		}
		s[i]='\0';
		int k=atoi(s);
		if(k>32768)
			return 1;
		else
			return 0;
	
	}
	
	 void fconvert_to_hex(FILE* f_out,char pl[]) /*converts integer to 16-bit binary num*/
	 {
		 int k=atoi(pl),t=15;
		 //printf("%s \n",pl);
		 char m[17];
		 
		 while(k)
		 {
			m[t--]=(k%2)+'0';
			k=k/2;		
		 }
		 
		 while(t>=0)
			 m[t--]='0';
		 
		 m[16]='\0';
		 //printf("%s\n",m);
		 fprintf(f_out,"1110\n");
		// printf("yo:");puts(m);
		 fprintf(f_out,"%s",m);//write the binary code in output file
		 
	 }
	int ry;
	 void store(FILE *f_in,FILE *f_op,FILE *f_reg,FILE *f_lab,FILE *f_sym)
	 {
		 char m[17];
			
			char p1[17],p2[17],p3[17],p4[17],p5[17],prev[17];
			int i=-1,instr=0,flag,n,end=0,q,s,t,k=0,j=0;
			ry=0;
			for(i=0;i<100;i++)
				inst[i].k=0;
			i=-1;
			instruc=0;
			
			while(!feof(f_in))
			{   
					flag=0;
					rewind(f_op);
					rewind(f_reg);
					rewind(f_lab);
					
					fscanf(f_in,"%s\n",p1);
					//printf("%d : %s\n",instr,p1);
					if(strcmp(p1,"HLT")==0) //if encountered a HLT, assign 1 to end
						end=1;
					
					if(instruct(f_op,p1)) //if scanned string is instruction
					{   k=0;      
						i++;     //increment num of instructions
						strcpy(inst[i].s[k],p1);
						//printf("%s ",inst[i].s[k]);
						
						inst[i].k=k; 
						k++;
						
						if(three_reg_instr(p1)) //3-reg inst
							inst[i].type=3;     
						else if(two_reg_instr(p1)) //2-reg inst
							inst[i].type=2;
						else if(one_reg_instr(p1)) //1-reg inst
							inst[i].type=1;
						else                     //no arg inst
							inst[i].type=0;
						
					}
					else if(label(p1)) //if scanned string is a label
					{   
						if(strcmp(prev,"JMP")==0 || strcmp(prev,"JNE")==0 ||strcmp(prev,"JNZ")==0 ||strcmp(prev,"JME")==0 ||strcmp(prev,"JEQ")==0 || strcmp(prev,"JGT")==0) //if its encountered after loop instructions
						{
							strcpy(inst[i].s[k],p1); //store it in instruction row i
							inst[i].k=k;
							k++;
						}
						else
						{ 
					      i++; //increment num of inst and store it in separate instruction row
						  k=0;
						  strcpy(inst[i].s[k],p1);
						  strcpy(sym[j].s,inst[i].s[k]);
						   fprintf(f_sym,"%s",sym[j].s); //since its a label,store it in symbol table
						  sym[j++].line=i+1;    //line num is line at which label is found
						  inst[i].type=0; 
						   //j-->num of symbols
						  
						}
					}
					else if(regis(p1) || integer(p1)) //if scanned string is a register or integer
					{
						strcpy(inst[i].s[k],p1); //store it in the same instruction row
						inst[i].k=k;
						k++;
						
					}
					else
					{
				     k=0;      
						i++;     //increment num of instructions
						strcpy(inst[i].s[k],p1);
						//printf("%s ",inst[i].s[k]);
						a[ry++]=i;
						inst[i].k=k; 
						k++;
					}
					
					instr++; //increment number of lines
					strcpy(prev,p1);// store the current string in prev string 
			}
			symbols=j; //j->num of symbols =>store in symbols
		
			instruc=i; //i-->num of instructions
		 
		 
		 
	 }
	 int error=1;
	int compile(FILE *f_in,FILE *f_op,FILE *f_reg,FILE *f_lab) /*checks the error in instruction format entered by user*/
	{
		
				int p,y,u=0,j,end=0;
				error=0;
			

			for(p=0;p<=instruc;p++)	 //start scanning the stored instruction table
			{       
				if(p==0 && strcmp(inst[p].s[0],"START")!=0) //if start is not in line1
				{printf("Line 1:START not found!!\n");
			     error=1;}
				 if(p!=0 && strcmp(inst[p].s[0],"START")==0 )
				 {
					 printf("Line %d: START at wrong line!!\n",p+1);
					 error=1;
				 }
				//print error
			if(strcmp(inst[p].s[0],"HLT")==0)
				end=1;
				u=0; //change
					
				if(inst[p].k>inst[p].type) //if num of arguments user has entered is more than actual arg of instr
				{		
					printf("Line %d: Error too many arguments in instruction %s\n",p+1,inst[p].s[0]);
					error=1;
				}
				else if(inst[p].k<inst[p].type) //if num of args is less
				{
					printf("Line %d: Error too few arguments in instruction %s\n",p+1,inst[p].s[0]);
					error=1;
				}
				for(j=1;j<=inst[p].k;j++)
				   {
					   if(strcmp(inst[p].s[0],"JMP")==0 || strcmp(inst[p].s[0],"JNE")==0 ||strcmp(inst[p].s[0],"JNZ")==0 ||strcmp(inst[p].s[0],"JME")==0 || strcmp(inst[p].s[0],"JEQ")==0 || strcmp(inst[p].s[0],"JGT")==0) //if encountered a loop instruction
					   { 
					       if(strcmp(inst[p-1].s[0],"CMP")!=0 && strcmp(inst[p].s[0],"JMP")!=0)
						   { printf("Line %d: Error Condition not found to Jump \n",p);
					   error=1;}
						   if(label(inst[p].s[j])) // if it has label field after it
						   { 
							 for(y=0;y<symbols;y++) //check if that label is defined already in symbol table
							 {
								 if(strcmp(sym[y].s,inst[p].s[j])==0)
									 break;
							 }
							 if(y==symbols) //if not found in symbol table, print error
							    {
								 printf("Line %d: Error expected label %s not found!\n",p+1,inst[p].s[j]);
						         error=1;
						        }
						   }
					   }
					  else if((strcmp(inst[p].s[0],"MOV")==0 || strcmp(inst[p].s[0],"STR")==0 || strcmp(inst[p].s[0],"LOAD")==0 || strcmp(inst[p].s[0],"PUSH")==0) ) //if encountered a mov instruction with integer argument
					   { 
						   if(integer(inst[p].s[j]) && inst[p].s[j][0]!='R')
						   {	if(inst[p].s[j][0]!='#') // if the integer don't have '#' before it, print error
								{
								 printf("Line %d: Error expected '#' before integer in instr MOV\n",p+1);
							     error=1;
								}
								if(overflow(inst[p].s[j])) //if integer is more than 2^15,print error
								{printf("Line %d: Integer Overflow!! Only 0-2^15 allowed!\n",p+1); error=1;}
								
								  
								//denotes than 1st arg is an integer
								u=1;
								
						   }
						 else if(u==1) // if both arg are integer, print error
						   {
							printf("Line %d: Wrong Instruction format %s instruc\n",p+1,inst[p].s[0]);
						    error=1;
						   }
						   
					   
				  }					   
				  else if(!freg_chk(inst[p].s[j])) // if invalid register is entered
						{ 
						   printf("Line %d: Invalid register! Only R0-R13 allowed!!\n",p+1);
					       error=1;
					    }
					   
				   
				
				}
				for(j=0;j<ry;j++)
				{
					if(a[j]==p)
					{printf("Line %d: Invalid instruction.See ISA HELP\n",p+1);
				error=1;}
				}
			}
			if(end!=1)
			{
				printf("HLT not found!!\n"); //if end is not 1, HLT is not found
				error=1;
			}
		
		
		if(error!=1) // if code has no errors, compilation successful!
		printf("Successfully compiled! Generated binary code!\n");
	
	}
	
	
	void display_symbol_table(FILE *f_lab) /*displays symbol table*/
	{  	
		int y;
		char m1[17],m2[17];
		printf("Symbol   ILC(Line Num)   Label Address\n");

		
		for(y=0;y<symbols;y++)
		{  
			while(!feof(f_lab))
			{
				fscanf(f_lab,"%s %s",m1,m2);
				if(strcmp(sym[y].s,m1)==0)

					break;
			}
			printf("%s     %d       %s\n",sym[y].s,sym[y].line,m2); // print symbol name,line num and label address
		rewind(f_lab);
		}
	
	}
	void display_binary(FILE *f_out) /*displays our binary machine code output file*/
	{
		
		char p1[17],por[17],c;
		int k=0;
		
			while( (c = getc(f_out))!=EOF )
			{    
				putchar(c);
				
			}
	
	}
    

	void display_help(FILE *f_help) /*displays ISA help file*/
	{
			char p1[17],c;
			f_help=fopen("Help.txt","r");
			
			while( (c = getc(f_help))!= EOF )
				{    
					putchar(c);
				}
				fclose(f_help);
		
		
	}
	void show() /*Shows the instructions user has entered*/
	{
		int p,i,j;
			for(p=0;p<=instruc;p++)

			{       
					   for(j=0;j<=inst[p].k;j++)
					   {   
						   printf("%s ",inst[p].s[j]);
					   }
				printf("\n");
				
			}
			
	}
	
	void generate(FILE *f_op,FILE *f_out,FILE *f_reg,FILE *f_lab) /*generates binary machine code file*/
	{
		
		int p,i,j=1,b,t=0;
		char sp[4];
		for(p=0;p<=instruc;p++)
		{     
			if(strcmp(inst[p].s[0],"JME")==0 ||strcmp(inst[p].s[0],"JMP")==0 ||strcmp(inst[p].s[0],"JNZ")==0 ||strcmp(inst[p].s[0],"JNE")==0|| strcmp(inst[p].s[0],"JEQ")==0 || strcmp(inst[p].s[0],"JGT")==0) //if loop instruction
   					    {     
					           fcheck(f_op,inst[p].s[0],f_out);
							   
							   if(label(inst[p].s[1]))
							   {  
						   
							   fprintf(f_out,"1111\n");
							   fcheck(f_lab,inst[p].s[1],f_out);
							   fprintf(f_out,"\n");
							   
							   }
							   continue;
						} 
			
			if(instruct(f_op,inst[p].s[0])) // checks if instr,and outputs its binary address to file
				 fcheck(f_op,inst[p].s[0],f_out);
			 
			else if(label(inst[p].s[0])) //check if label and output its binary address to file
				 fcheck(f_lab,inst[p].s[0],f_out);
				 
			for(j=1;j<=inst[p].k;j++)
				{   t=0;
					if(inst[p].s[j][0]=='#') //if integer is found
						   {   
					           for(b=1;inst[p].s[j][b]!='\0';b++)
							   sp[t++]=inst[p].s[j][b];
						       sp[t]='\0';
						   
							   fconvert_to_hex(f_out,sp); //convert to 16-bit binary equivalent
						   }
						   
						   else
						   fcheck(f_reg,inst[p].s[j],f_out); //if register, output its binary add to file
				}
			fprintf(f_out,"\n");
				
		}
			
		
	}
	
int main()
{
			FILE *f_in;  // input file
			FILE *f_op; // opcode file
			FILE *f_reg;  // register adress file
			FILE *f_out;  //output file
			FILE *f_lab;  //label adress file
			FILE *f_help;
			FILE *f_sym;  //ISA help file
			
			f_in=fopen("Input.txt","w"); 
			f_op=fopen("Opcode.txt","r");
			f_reg=fopen("Reg.txt","r");
			f_lab=fopen("Labels.txt","r");
			f_out=fopen("Output.txt","w");
			f_help=fopen("Help.txt","r");
			f_sym = fopen("Symbol.txt","w");
			
			int choice=-1,k=-1,i,exit=0;
			
			printf("\n\n\t\tC.A.R.V.I.S: Smart assistant for cars : Developed by : 15ucs038 ; 15ucs070 ; 15ucs093 ; 15ucs073 .:\n");
			
		printf("\n\tMENU :\n\n");
		printf("\t1.Select The Function To Compute\n");
		printf("\t2.Display Assembly Language Code.\n"); 
		printf("\t3.Compile the Code.(First Pass)\n");
		printf("\t4.Display Symbol Table\n"); 
		printf("\t5.Generate & Display Machine Code.(Second Pass)\n");  
		printf("\t6.Compute the code\n");
		printf("\t7.Exit.\n"); 
		printf("\n\nEnter your choice?\n");
		
	while(choice!=7)
	{  

	



	printf("Enter choice: ");
		scanf("%d",&choice);
		switch(choice)
		{
			case 1:
			for(i=0;i<9;i++)
		r[i]=0;
			
			
			printf("\n\tFollowing Functions are available :\n\n\t1.Trip Cost Calculator\n\t2. Decrease Temperature\n\t3.Increase Temperature\n\t4.Door Lock assist\n\t5.Battery Left\n\t6.Tyre damage Assist\n\t7.Rear drive assist\n\t8.Estimated Time \n\t9.Pollution Indicator\n\t10.Automatic Headlight\n");
			scanf("%d",&k);
			switch(k)
			{
				case 1:
			f_in=fopen("OUR_FARE.txt","r");
			printf("Enter the distace to destination : ");
			scanf("%d",&r[1]);
	

			printf("Enter the petrol in the tank : ");
			scanf("%d",&r[2]);
			func = 1;
			break;



				case 2:
			f_in=fopen("temp_dec.txt","r");
			printf("Enter the current temperature : ");
			scanf("%d",&r[1]);
			printf("Enter temp by which u wanna decrease: ");
			scanf("%d",&r[2]);
			//printf("Enter default temp: ");
			func = 2;
			break;
			case 3:
			f_in=fopen("temp_inc.txt","r");
			printf("Enter the current temperature : ");
			scanf("%d",&r[1]);
			printf("Enter temp by which u wanna increase: ");
			scanf("%d",&r[2]);
			//printf("Enter default temp: ");
			func = 3;
			break;
			case 4:

			func = 4;
			f_in=fopen("OUR_doorstateour.txt","r");
			printf("Enter status for door 1 (0 if unlocked & 1 if locked) : ");
			scanf("%d",&r[1]);

			printf("Enter status for door 2 (0 if unlocked & 1 if locked) : ");
			scanf("%d",&r[2]);

			printf("Enter status for door 3 (0 if unlocked & 1 if locked) : ");
			scanf("%d",&r[3]);

			printf("Enter status for door 4 (0 if unlocked & 1 if locked) : ");
			scanf("%d",&r[4]);	
			
			break;
			case 7:
			f_in=fopen("OUR_reardrive.txt","r");
			printf("Enter the present distance of car from obstacle : ");
			scanf("%d",&r[1]);
			func = 7;
			break;
			case 6:
			f_in=fopen("OUR_RPM.txt","r"); 
			printf("Enter the present damage of the car : ");
			scanf("%d",&r[1]);

			
			func = 6;


			break;
			case 5:
			f_in=fopen("OUR_ENERGY SAVING.txt","r");
			printf("Enter battery consumed by lights : ");
			scanf("%d",&r[1]);

			printf("Enter battery consumed by A.C : ");
			scanf("%d",&r[2]);

			printf("Enter battery consumed by Music : ");
			scanf("%d",&r[3]);


			printf("Enter battery consumed by Misc : ");
			scanf("%d",&r[4]);



			func = 5;
			break;
			case 8:
			func = 8;
			f_in=fopen("OUR_TIME TOREACH.txt","r");
			printf("Enter speed of Car : ");
			scanf("%d",&r[1]);
			printf("Enter distance of destination of car : ");
			scanf("%d",&r[2]);
			
			break;
			case 9:
			f_in=fopen("OUR_POLLUTANT.txt","r");
			printf("Enter pollutation percentage of Co2 : ");
			scanf("%d",&r[1]);
		printf("Enter pollutation percentage of CO : ");
			scanf("%d",&r[2]);

			printf("Enter pollutation percentage of NO2 : ");
			scanf("%d",&r[3]);


			printf("Enter pollutation percentage of SO2 : ");
			scanf("%d",&r[4]);
			func = 9;
			break;
			}
			
			//enter_input(f_in);
			//fclose(f_in);
			//f_in=fopen("Input.txt","r");
			f_op=fopen("Opcode.txt","r");
			f_reg=fopen("Reg.txt","r");
			f_lab=fopen("Labels.txt","r");
			
			store(f_in,f_op,f_reg,f_lab,f_sym);
			
			//fclose(f_in);
			break;

			case 2:
			show();
			break;

			case 3:
			//f_in=fopen("Input.txt","r");
			f_op=fopen("Opcode.txt","r");
			f_reg=fopen("Reg.txt","r");
			f_lab=fopen("Labels.txt","r");
			
			compile(f_in,f_op,f_reg,f_lab);
			
			fclose(f_op);
		    fclose(f_reg);
		    fclose(f_lab);
		    fclose(f_in);
			break;
			
			case 4:
			f_lab=fopen("Labels.txt","r");
			display_symbol_table(f_lab);
			break;
			
			case 5:
			if(error==1)
			{
				printf("Can't generate binary code,compilation errors\n");
				break;
			}
			f_op=fopen("Opcode.txt","r");
			f_reg=fopen("Reg.txt","r");
			f_lab=fopen("Labels.txt","r");
			f_out=fopen("Output.txt","w");
			
			generate(f_op,f_out,f_reg,f_lab);
			
		    fclose(f_op);
		    fclose(f_reg);
		   	fclose(f_lab);
		    fclose(f_out);
			
			f_out=fopen("Output.txt","r");
			display_binary(f_out); 
			fclose(f_out);
			break;
			
			case 6:
			f_out=fopen("Output.txt","r");
			
			compute_mach(f_out);
			
			fclose(f_out);
			if(exit==1)
				return 0;
			break;
			
	
		}
			
	}
return 0;	
}
	

	
	
	
	
	int three_reg(char opc[])
{
	if(strcmp(opc,"0000")==0 || strcmp(opc,"0001")==0 || strcmp(opc,"0010")==0 ||strcmp(opc,"0011")==0)
	return 1;
    else
		return 0;
	
}
int two_reg(char opc[])
{
	if(strcmp(opc,"01000110")==0 || strcmp(opc,"01000000")==0)
		return 1;
	else
		return 0;
	
}
int one_reg(char opc[])
{
	if(strcmp(opc,"010010010000")==0 || strcmp(opc,"010010010001")==0)
		return 1;
	else
		return 0;
	
}
int loop(char opc[])
{
	if(strcmp(opc,"010010011010")==0 || strcmp(opc,"010010010110")==0 || strcmp(opc,"010010010111")==0 || strcmp(opc,"010010011000")==0 ||strcmp(opc,"010010011011")==0 )
		return 1;
	else
		return 0;
}
int labely(char m[],int x)
{

if(x!=1)
{
if(strcmp(m,"0011")==0)
	return 1;
if(strcmp(m,"0100")==0)
	return 2;
if(strcmp(m,"0101")==0)
	return 3;
if(strcmp(m,"0110")==0)
	return 4;
if(strcmp(m,"0111")==0)
	return 5;
if(strcmp(m,"1000")==0)
	return 6;
}
else
{
	int i,l=12;
char c[5];
for(i=0;i<4;i++,l++)
	c[i]=m[l];
c[i]='\0';

if(strcmp(c,"0011")==0)
	return 1;
if(strcmp(c,"0100")==0)
	return 2;
if(strcmp(c,"0101")==0)
	return 3;
if(strcmp(c,"0110")==0)
	return 4;
if(strcmp(c,"0111")==0)
	return 5;
if(strcmp(c,"1000")==0)
	return 6;

	
}	
}
int covert(char num[],int t)
{
	int dec=0,i,j,k,pow=1;
	if(t==1)
	{
		for(i=3;i>=0;i--)
	{    
        
		if(num[i]=='1')
			dec=dec+pow;
		pow=pow*2;
	}

    return dec;
	}
	for(i=15;i>=0;i--)
	{    
        
		if(num[i]=='1')
			dec=dec+pow;
		pow=pow*2;
	}

    return dec;
}
FILE *fp;
int seekLabel(char x[],FILE *fp)
{
	 char read[17];
	while(!feof(fp) && strcmp(x,read)!=0)
	{
		fscanf(fp,"%16s",read);
	}
	int l=ftell(fp);
	return l;
}
void compute_mach(FILE *fp)
{
	//fp=fopen("Output.txt","r");
	int i,l,m,n,k,v;
	int neg_stat,zero_stat,l1=0,l2=0,l3=0,l4=0,l5=0,l6=0,pos_stat;
	char opc[20],opc1[5],opc2[5],opc3[5],x[17],y[17],z[17],read[17];
	fscanf(fp,"%16s",opc);
	//printf("%d r1",r[1]);
	//fscanf(fp,"%16s",opc);
	neg_stat=0;
	zero_stat=0;
	pos_stat = 0 ;
	int co=1,flag=0;
	printf("Line %d: Executing....\n",co);
	while(!feof(fp))
	{
		
	fscanf(fp,"%4s",opc);
	//printf("Instr->");
	//puts(opc);
	//printf("\n");
	co++;
	printf("Line %d: Executing....\n",co);
	if(co==instruc+1)
	{printf("ALU signal: Halt\n",k);
	break;}
	
	//printf("%d\n",co);
	
	if(three_reg(opc))
	{
		fscanf(fp,"%4s%4s%4s",x,y,z);
		l=covert(x,1);
		m=covert(y,1);
		n=covert(z,1);
		if(strcmp(opc,"0000")==0)
		{r[l]=r[m]+r[n];
		printf("ALU signal: Add\n");}
		if(strcmp(opc,"0001")==0)
		{r[l]=r[m]-r[n];
		printf("ALU signal: Subtract\n");
	    }
		if(strcmp(opc,"0010")==0)
		{r[l]=r[m]*r[n];
	printf("ALU signal: Multiply\n");
		}
		if(strcmp(opc,"0011")==0)
		{r[l]=r[m]/r[n];
		printf("ALU signal: Divide\n");
		}

	}
	else
	{
		fscanf(fp,"%4s",opc1);
		strcat(opc,opc1);
		
		if(two_reg(opc))
		{  //printf("execut");

			if(strcmp(opc,"01000000")==0)
			{
				fscanf(fp,"%4s%4s",x,y);
				printf("ALU signal: Compare\n");
				//printf("execut");
				l=covert(x,1);
				//printf("%d ",l);
				m=covert(y,1);
				//printf("%d %d",l,m);
				if(r[l]>r[m])
				neg_stat=1;
				else if(r[l]==r[m])
				{zero_stat=1;
			
			}

	else if(r[l]<r[m])
				{pos_stat=1;
			
			}
			}
			if(strcmp(opc,"01000110")==0)
				{
					printf("ALU signal: Move\n");
					//puts(opc);
				fscanf(fp,"%4s%4s",x,z);
				
				fscanf(fp,"%16s",y);
				//printf("4-->");
				//puts(y);
				l=covert(x,1);
				m=covert(y,2);
				//printf("%d ",l);
				//printf("%d ",m);
				r[l]=m;
				//printf("r:%d",r[l]);
				
				}

	    }
		
		else
		{
			fscanf(fp,"%4s",opc2);
			strcat(opc,opc2);
			//printf("scan: %s",opc);
			if(one_reg(opc))
			{
			fscanf(fp,"%4s",x);
			l=covert(x,1);
				if(strcmp(opc,"010010010000")==0)
				{r[l]++;
			printf("ALU signal: Increment\n");
			
			}
				if(strcmp(opc,"010010010001")==0)
				{r[l]--;
			printf("ALU signal: Decrement\n");
				}
			}
			else if(loop(opc))
			{ fscanf(fp,"%4s");
				fscanf(fp,"%16s",x);
				//printf("yup");
				k=labely(x,1);
				int cat,y;
				for(y=0;y<symbols;y++)
				{
					if(k==sym[y].s[1]-'0')
						cat=y;
				}
			 if(strcmp(opc,"010010010111")==0)
			{
				 //printf("yup");
				if(zero_stat==0)
				{ //printf("yup");
			printf("ALU signal: Jump to L%d\n",k);
			co=sym[cat].line;
					switch(k)
					{
						case 1:
						if(l1==0)
							l1=seekLabel(x,fp);
						else
						fseek(fp,l1,SEEK_SET); //printf("gocha! %d l",l1);
						
						break;
		
				case 2:
						if(l2==0)
							l2=seekLabel(x,fp);
						else
						fseek(fp,l2,SEEK_SET); //printf("gocha!");
						break;
				
				case 3:
						if(l3==0)
							l3=seekLabel(x,fp);
						else
						fseek(fp,l3,SEEK_SET); //printf("gocha!");
					break;
		
				case 4:
						if(l4==0)
							l4=seekLabel(x,fp);
						else
						fseek(fp,l4,SEEK_SET); //printf("gocha!");
					break;
						
				case 5:
						if(l5==0)
							l5=seekLabel(x,fp);
						else
						fseek(fp,l5,SEEK_SET); //printf("gocha!");
					break;
		
				case 6:
						if(l6==0)
							l6=seekLabel(x,fp);
						else
						fseek(fp,l6,SEEK_SET);// printf("gocha!");
					break;

					}
				continue;
				}
			}
			 if(strcmp(opc,"010010011011")==0)           // Our JGT
			{
				 //printf("yup");
				if(pos_stat==1)
				{ //printf("yup");
			printf("ALU signal: Jump to L%d\n",k);
			co=sym[cat].line;
					switch(k)
					{
						case 1:
						if(l1==0)
							l1=seekLabel(x,fp);
						else
						fseek(fp,l1,SEEK_SET); //printf("gocha! %d l",l1);
						
						break;
		
				case 2:
						if(l2==0)
							l2=seekLabel(x,fp);

						else
						fseek(fp,l2,SEEK_SET); //printf("gocha!");
						break;
				
				case 3:
						if(l3==0)
							l3=seekLabel(x,fp);
						else
						fseek(fp,l3,SEEK_SET); //printf("gocha!");
					break;
		
				case 4:
						if(l4==0)
							l4=seekLabel(x,fp);
						else
						fseek(fp,l4,SEEK_SET); //printf("gocha!");
					break;
						
				case 5:
						if(l5==0)
							l5=seekLabel(x,fp);
						else
						fseek(fp,l5,SEEK_SET); //printf("gocha!");
					break;
		
				case 6:
						if(l6==0)
							l6=seekLabel(x,fp);
						else
						fseek(fp,l6,SEEK_SET);// printf("gocha!");
					break;

					}
				continue;
				}
			}
			else if(strcmp(opc,"010010011000")==0)
			{
           co=sym[cat].line;
		   printf("ALU signal: Jump to L%d\n",k);
				switch(k)
					{
						case 1:
						if(l1==0)
							l1=seekLabel(x,fp);
						else
						fseek(fp,l1,SEEK_SET); //printf("gocha! %d l",l1);
						break;
		
				case 2:
						if(l2==0)
							l2=seekLabel(x,fp);
						else
						fseek(fp,l2,SEEK_SET); //printf("gocha!");
						break;
				
				case 3:
						if(l3==0)
							l3=seekLabel(x,fp);
						else
						fseek(fp,l3,SEEK_SET); //printf("gocha!");
					break;
		
				case 4:
						if(l4==0)
							l4=seekLabel(x,fp);
						else
						fseek(fp,l4,SEEK_SET); //printf("gocha!");
					break;
						
				case 5:
						if(l5==0)
							l5=seekLabel(x,fp);
						else
						fseek(fp,l5,SEEK_SET); //printf("gocha!");
					break;
		
				case 6:
						if(l6==0)
							l6=seekLabel(x,fp);
						else
						fseek(fp,l6,SEEK_SET);// printf("gocha!");
					break;


					}
				continue;
			}
			
			else if(strcmp(opc,"010010011010")==0)
			{
				if(zero_stat==1)
				{
					printf("ALU signal: Jump to L%d\n",k);
					co=sym[cat].line;
					//printf("exect");
				switch(k)
					{
						case 1:
						if(l1==0)
							l1=seekLabel(x,fp);
						else
						fseek(fp,l1,SEEK_SET); //printf("gocha! %d l",l1);
						break;
		
				case 2:
						if(l2==0)
							l2=seekLabel(x,fp);
						else
						fseek(fp,l2,SEEK_SET); //printf("gocha!");
						break;
				
				case 3:
						if(l3==0)
							l3=seekLabel(x,fp);
						else
						fseek(fp,l3,SEEK_SET); //printf("gocha!");
					break;
		
				case 4:
						if(l4==0)
							l4=seekLabel(x,fp);
						else
						fseek(fp,l4,SEEK_SET); //printf("gocha!");
					break;
						
				case 5:
						if(l5==0)
							l5=seekLabel(x,fp);
						else
						fseek(fp,l5,SEEK_SET); //printf("gocha!");
					break;
		
				case 6:
						if(l6==0)
							l6=seekLabel(x,fp);
						else
						fseek(fp,l6,SEEK_SET);// printf("gocha!");
					break;

					}
				continue;
				}
			}
			else if(strcmp(opc,"010010010110")==0)
			{
				if(neg_stat==1)
				{
					printf("ALU signal: Jump to L%d\n",k);
					co=sym[cat].line;
				switch(k)
					{
						case 1:
						if(l1==0)
							l1=seekLabel(x,fp);
						else
						fseek(fp,l1,SEEK_SET); //printf("gocha! %d l",l1);
						break;
		
				case 2:
						if(l2==0)
							l2=seekLabel(x,fp);
						else
						fseek(fp,l2,SEEK_SET); //printf("gocha!");
						break;
				
				case 3:
						if(l3==0)
							l3=seekLabel(x,fp);
						else
						fseek(fp,l3,SEEK_SET); //printf("gocha!");
					break;
		
				case 4:
						if(l4==0)
							l4=seekLabel(x,fp);
						else
						fseek(fp,l4,SEEK_SET); //printf("gocha!");
					break;
						
				case 5:
						if(l5==0)
							l5=seekLabel(x,fp);
						else
						fseek(fp,l5,SEEK_SET); //printf("gocha!");
					break;
		
				case 6:
						if(l6==0)
							l6=seekLabel(x,fp);
						else
						fseek(fp,l6,SEEK_SET);// printf("gocha!");
					break;

					}
				continue;
				}
			}
			}


			else
			{
				//break;
			 fscanf(fp,"%4s",x);
			 //printf("ooh");
			 k=labely(x,2);
			 strcat(opc,x);
			 //if(strcmp(opc,"0100100110010001")==0)
		  //break;
			 
			 //printf("%d k",k);
			 switch(k)
					{
				case 1:
				l1=ftell(fp); //printf("read: %d\n",l1);
				break;
				case 2:
				l2=ftell(fp); break;
				case 3:
				l3=ftell(fp); break;
				case 4:
				l4=ftell(fp); break;
				case 5:
				l5=ftell(fp); break;
				case 6:
				l6=ftell(fp); break;
				default:
				break;

					}
			 
				
			}
			
		}
		
	  }
	  //if(strcmp(opc,"0100100110010001")==0)
		  //break;
	  for(v=0;v<14;v++)
	printf("r[%d]: %d ",v,r[v]);
printf("\n");
	 printf("Status: NZ: %d   Z: %d\n",neg_stat,zero_stat);  
	 printf("\n");
}

for(v=0;v<14;v++)
	printf("r[%d]: %d ",v,r[v]);
	 printf("Status: NZ: %d   Z: %d\n",neg_stat,zero_stat); 


//fclose(fp);


	
	

		if (func==1) printf("\n\n The trip cost is : %d Rs\n\n",r[8]) ;
				

		if(func==2)
		printf("\n\nThe final temperature is : %d degree celcius\n\n ",r[3]) ;

		if(func==3)
		printf("\n\nThe final temperature is : %d degree celcius\n\n ",r[3]) ;

		if(func==4)
		{
			int i ;
			for(i = 6 ; i < 10 ; i++)
			{
				if(r[i]==0)
				printf("\n\n Gate %d is not locked \n\n",i-5) ;
			}

		




		}

		if(func ==5)
		printf("\n\nThe battery can be used for %d hrs as per current battery consumption \n\n ",r[9]) ;


		if(func==9)
		{

		printf("\n\nOptimum pollution index is : 320 \n\n ") ;

		printf("\n\nYour pollution index is : %d  \n\n ",r[3]) ;


		}

		if(func==8)
		{


		printf("\n\nAt this average speed it will take %d hrs to reach your destination \n\n ",r[3]) ;



		}


		if(func==7)
		{


			printf("\n\nALERT! You have reached the minimum appropriate distance from the obstacle \n\n ") ;


		}
















}
 
