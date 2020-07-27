# FNC-Desafio-Lab
Exercício proposto

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void prazo(int code){
	int dias=0;
	char situacao[100];
	
	if(code<=2){
		dias=2;
		printf("Prazo maximo de %d dias uteis para analise.\n",dias);
		printf("Data da entrega: em %d dias uteis.",dias+1);
		
		
		//strcpy(situacao, "");
	}
		
	if(code>2 && code<6){
	    dias=3;		
		printf("Prazo maximo de %d dias uteis para analise.\n",dias);
     	printf("Data da entrega: em %d dias uteis.",dias+1);		
		
	}
	
	

    if (code>=6 && code<11 )
	{
		dias=7;
			printf("Prazo maximo de %d dias uteis para analise.\n",dias);
          	printf("Data da entrega: em %d dias uteis.",dias+1);	
		
	}

	
	if(code>=11){
		printf(" ERRO, o codigo inserido eh invalido.");
	}

}

//Cada contato possui um nome e telefone.

 

struct Procedimento{
  char nomePr[50];
  int  code; 
  float dias;
};


struct Paciente{
	 char nomePa[50],email[200];
     int cpf; 
};


struct Exame{
	int dataDaEntrega;
	int dataDaColeta;
	struct Procedimento Pr;
	struct Paciente Pa;
	
};

//Exame structExame;









int main(){
  //Ponteiro para estrutura. Vai ser o nosso vetor de 
  //estruturas dinamicas com os dados da agenda.
  struct Procedimento *agenda;
  struct Paciente *agendaPa;
  struct Exame *agendaEx;
  int cap; //Capacidade do vetor (tamanho real do vetor).
  int n;   //Numero de contatos cadastrados (tamanho usado).
  int opc; //Opcao do menu selecionada.
  int i;
  FILE *fp;

  //Abrindo arquivo em modo de leitura ("r" = read).
  fp = fopen("agenda.dat","r");

  //Se retorna NULL eh pq nao encontrou o arquivo.
  //Isso acontece na primeira vez que rodamos o programa.
  if(fp==NULL){
    n = 0;    //Base de dados inicialmente vazia.
    
    
    //a capacidade eh a quantidade de vezes que vou repetir o tamanho da struct contato, ou seja, quantas pessoas
    //serão cadastradas
    cap = 20; //Fixamos uma capacidade inicial (ex: 20).

    //Alocammos o vetor de estruturas de forma
    //dinamica conforme a capacidade.
    agenda = (struct Procedimento *)malloc(sizeof(struct Procedimento)*cap);
    agendaPa = (struct Paciente *)malloc(sizeof(struct Paciente)*cap);
    agendaEx = (struct Exame *)malloc(sizeof(struct Exame)*cap);
    
	  }
  else{ //Arquivo encontrado, logo procedemos com a leitura.

    //Le o numero de registros.
    fread(&n, sizeof(int), 1, fp);
    
    //A capacidade deve ser maior ou igual a "n" (ex: n*2).
    cap = n*2; 

    //Alocammos o vetor de estruturas de forma
    //dinamica conforme a capacidade.
    agenda = (struct Procedimento*)malloc(sizeof(struct Procedimento)*cap);
    agendaPa = (struct Paciente *)malloc(sizeof(struct Paciente)*cap);
    agendaEx = (struct Exame *)malloc(sizeof(struct Exame)*cap);

    //Leitura dos dados dos registros do arquivo para o vetor.
    fread(agenda, sizeof(struct Procedimento), n, fp);
    fread(agendaPa, sizeof(struct Paciente), n, fp);
    fread(agendaEx, sizeof(struct Exame), n, fp);
     
     
    //Fecha arquivo apos leitura.
    fclose(fp);
  }
  
  do{
    //Exibe o menu de opcoes.
    printf("- - - - -Laboratorio FNC- - - - - - -\n");
    printf("- 11) Cadastrar um procedimento.     -\n");
    printf("- 21) Cadastrar um paciente.         -\n");
   // printf("- 31) Cadastrar um exame.            -\n");
    printf("- 12) Exibir procedimentos.          -\n");
    printf("- 22) Exibir lista de pacientes.     -\n"); 
    printf("- 32) Exibir exames.                 -\n"); 
    printf("- 13) Apagar um procedimento.        -\n");
    printf("- 23) Apagar um paciente da lista.   -\n");
   // printf("- 33) Apagar um exame.               -\n");
    printf("-  0) Salvar/Sair (d)os registros.   -\n");
    printf("- - - - - - - - - - - - - - - - - - -");
    
    //Le a opcao selecionada.
    scanf("%d",&opc);
    
    

    if(opc==11){
      //Inserir novo contato no final do vetor.

      if(n==cap){
	//Capacidade esgotada, devemos aumentar o 
	//vetor usando "realloc" (ex: dobrar capacidade).
	     cap *= 2;
         agenda = realloc(agenda, sizeof(struct Procedimento)*cap);
      }
      //Leitura dos dados.
      printf("Digite o nome: ");
      scanf(" %s",agenda[n].nomePr);
      //tentando apagar o lixo do sistema
      setbuf(stdin,NULL);
      //gets(agenda[n].nome);
      printf("Digite o codigo: ");
      scanf("%d",&agenda[n].code);
      
      //tentando apagar o lixo do sistema
     // fflush(stdout);
     setbuf(stdin,NULL);
      

     
     
      n++; //Incrementa numero de registros cadastrados.
    }
    else if(opc==12){
      //Exibe todos procedimentos cadastrados.
      for(i=0; i<n; i++){
		  printf("\n\n");
	    printf("******** Procedimentos %d ********\n",i+1);
        printf("Nome: %s\n",agenda[i].nomePr);
	    printf("Codigo: %d\n",agenda[i].code);
	    
	    
	    printf("Situacao: ");
	    prazo(agenda[i].code);
	    
	      
      }
      printf("\n\n");
    }
    else if(opc==13){
    	
    	 //Apaga um contato
      printf("Digite o nome do procedimento que deseja apagar: ");
      char nomePr[50];
      int j;
      scanf("%s",nomePr);
      for(i=0; i<n; i++){
             if (strcmp(agenda[i].nomePr, nomePr)==0){
             	for(j=i+1; j<n; j++){
                         strcpy(agenda[j-1].nomePr,agenda[j].nomePr);
                         agenda[j-1].dias=agenda[j].dias;
                 }
               n--;
               i=n;
              }
      }
    	
      
    }else if(opc==21){
      //Inserir novo contato no final do vetor.

      if(n==cap){
	//Capacidade esgotada, devemos aumentar o 
	//vetor usando "realloc" (ex: dobrar capacidade).
	     cap *= 2;
         agendaPa = realloc(agendaPa, sizeof(struct Paciente)*cap);
      }
      //Leitura dos dados.
      printf("Digite o nome: ");
      scanf(" %s",agendaPa[n].nomePa);
      //gets(agenda[n].nome);
      printf("Digite o CPF: ");
      scanf("%d",&agendaPa[n].cpf);
      printf("Digite o e-mail:");
      scanf("%s",agendaPa[n].email);
      
     
      
      n++; //Incrementa numero de registros cadastrados.
    }else if(opc==22){
      //Exibe todos procedimentos cadastrados.
      for(i=0; i<n; i++){
		  printf("\n\n");
	    printf("******** Paciente %d ********\n",i+1);
        printf("Nome: %s\n",agendaPa[i].nomePa);
	    printf("CPF: %d\n",agendaPa[i].cpf);
	    printf("E-mail: %d\n",agendaPa[i].email);
	    
	  
	    
      }
      printf("\n\n");
    }else if(opc==23){
    	
    	 //Apaga um contato
      printf("Digite o nome do contato que deseja apagar: ");
      char nomePa[50];
      int j;
      scanf("%s",nomePa);
      for(i=0; i<n; i++){
             if (strcmp(agendaPa[i].nomePa, nomePa)==0){
             	for(j=i+1; j<n; j++){
                         strcpy(agendaPa[j-1].nomePa,agendaPa[j].nomePa);
                         agendaPa[j-1].cpf=agendaPa[j].cpf;
                 }
               n--;
               i=n;
              }
      }
      
 /* }else if(opc==31){
      //Inserir novo contato no final do vetor.

      if(n==cap){
	//Capacidade esgotada, devemos aumentar o 
	//vetor usando "realloc" (ex: dobrar capacidade).
	     cap *= 2;
         agendaEx = realloc(agendaEx, sizeof(struct Exame)*cap);
      }
      //Leitura dos dados.
      printf("Digite o nome: ");
      scanf(" %s",agendaEx[n].);
      //gets(agenda[n].nome);
      printf("Digite o CPF: ");
      scanf("%d",&agendaEx[n].cpf);
      printf("Digite o e-mail:");
      scanf("%s",agendaEx[n].email);
      
     
      
      n++; //Incrementa numero de registros cadastrados.
      */
    }else if(opc==32){
      //Exibe todos procedimentos cadastrados.
      for(i=0; i<n; i++){
		  printf("\n\n");
	    printf("******** Exames %d ********\n",i+1);
	    printf("O paciente [%s] fara o procedimento [%s]\n",agendaPa[i].nomePa,agenda[i].nomePr);
	    prazo(agenda[i].code);
	    
	  
	    
      }
      printf("\n\n");
    }
	/*
	else if(opc==33){
    	
    	 //Apaga um contato
      printf("Digite o nome do exame que deseja apagar: ");
      char nomeEx[50];
      int j;
      scanf("%s",nomeEx);
      for(i=0; i<n; i++){
             if (strcmp(agendaEx[i].nomeEx, nomeEx)==0){
             	for(j=i+1; j<n; j++){
                         strcpy(agendaEx[j-1].nomeEx,agendaEx[j].nomeEx);
                         agendaEx[j-1].cpf=agendaEx[j].cpf;
                 }
               n--;
               i=n;
              }
      }
  }
*/

    
    
    //Enquanto nao for opcao de saida continua mostrando menu.
  }while(opc!=0); 

  if(n>0){
    //Se existe algum contato cadastrado 
    //entao grava para o disco.

    //Abre arquivo em modo de gravacao ("w" = write).
    fp = fopen("agenda.dat","w");

    //Grava o numero de contatos no inicio do arquivo.
    fwrite(&n, sizeof(int), 1, fp);
    
    //Grava os dados do vetor no arquivo.
    fwrite(agenda, sizeof(struct Procedimento), n, fp);
    fwrite(agendaPa, sizeof(struct Paciente), n, fp);

    //Fecha arquivo apos a gravacao.
    fclose(fp);
  }

  //Libera a memoria alocada do vetor.
  free(agenda);
  free(agendaPa);

  
  return 0;
}
