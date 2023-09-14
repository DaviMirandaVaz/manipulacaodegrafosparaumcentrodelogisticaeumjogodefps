#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include <math.h>

/*
	Trabalho realizado por:Davi Miranda Vaz
	4?eiodo bcc 
	RA:202111000029
*/

#define MAX 100

int qtdfilial = 0;
int qtdlocal = 0;

//funcoes gerais
void inicializagrafo(char m[MAX][MAX]){
	int i, j;
	for(i=0;i<MAX;i++){
		for(j=0;j<MAX;j++){
			m[i][j] = '0';
		}
	}
}

void inicializastr(char m[MAX]){
	int i;
	for(i=0;i<MAX;i++){
		m[i] = '0';
	}
}

void inicializafloat(float m[MAX][MAX]){
	int i, j;
	for(i=0;i<MAX;i++){
		for(j=0;j<MAX;j++){
			m[i][j]= 0;
		}
	}
}

//Funcoes para filial



int pesquisafilial(char filiais[MAX][MAX], char filial[MAX]){
	int i = 0;
	for(i = 0; i < MAX; i++){
		if(strcmp(filiais[i], filial) == 0){
			return i;
		}
	}
	return -1;
	
}

void exibefilial(char filial[MAX][MAX]){
	int i = 0, j = 0;
	for(i = 0; i < qtdfilial; i++){
		printf("%d . ", i+1);
		for(j = 0;j < MAX; j++){
			if(filial[i][j] == '0'){
				printf("\n");
				break;
			}
			printf("%c", filial[i][j]);
		}
	}
	
		
}

void inserefilial( char filiais[MAX][MAX]){
	int i;
	char filial[MAX];
	inicializastr(filial);
	system("cls");
	exibefilial(filiais);
	printf("Qual o nome da cidade a ser inserida?\n");
	fflush(stdin);
	fgets(filial, MAX, stdin);
	if(pesquisafilial(filiais, filial) != -1){
		printf("Ja existe esta filial!\n");
		system("pause");
		return;
	}
	if(qtdfilial>MAX){
		printf("Matriz de filiais cheia! \n");
		system("pause");
		return;
	}
	strcpy(filiais[qtdfilial], filial);
	
	qtdfilial++;
}

void insererota(char filiais[MAX][MAX], float rotas[MAX][MAX]){
	int i, op123 = 0;
	char filial1[MAX], filial2[MAX];
	float dis;
	inicializastr(filial1);
	inicializastr(filial2);
	system("cls");
	exibefilial(filiais);
	printf("Qual nome da primeira cidade?\n");
	fflush(stdin);
	fgets(filial1, MAX, stdin);
	printf("Qual nome da segunda cidade?\n");
	fflush(stdin);
	fgets(filial2, MAX, stdin);
	if(pesquisafilial(filiais, filial1) == -1 || pesquisafilial(filiais, filial2) == -1){
		printf("Filial nao existe!\n");
		system("pause");
		return;
	}
	if(pesquisafilial(filiais, filial1) == pesquisafilial(filiais, filial2)){
		printf("Insercao de mesmas filiais!\n");
		system("pause");
		return;
	}
	printf("Qual o preco da movimentacao ?\n");
	scanf("%f", &dis);
	
	if(rotas[pesquisafilial(filiais, filial1)][pesquisafilial(filiais, filial2)] != 0 || rotas[pesquisafilial(filiais, filial2)][pesquisafilial(filiais, filial1)] != 0 ){
		printf("Atualizar preco? (1, sim) (2, nao)\n");
		scanf("%d", &op123);
		if(op123 == 1){
			rotas[pesquisafilial(filiais, filial1)][pesquisafilial(filiais, filial2)] = dis;
			rotas[pesquisafilial(filiais, filial2)][pesquisafilial(filiais, filial1)] = dis;
			return;
		}else{
			return;
		}
	}else{
		rotas[pesquisafilial(filiais, filial1)][pesquisafilial(filiais, filial2)] = dis;
		rotas[pesquisafilial(filiais, filial2)][pesquisafilial(filiais, filial1)] = dis;
		return;
	}
	
}

void listafiliaisp(char filiais[MAX][MAX], float rotas[MAX][MAX]){
	int i, p, var = 0;
	char filial[MAX];
	inicializastr(filial);
	system("cls");
	exibefilial(filiais);
	if(qtdfilial == 0){
		printf("Ainda nao existem filiais para serem mostradas!\n");
		system("pause");
		return;
	}
	printf("Qual nome da cidade a ser consultada?\n");
	fflush(stdin);
	fgets(filial, MAX, stdin);
	p = pesquisafilial(filiais, filial);
	if(p == -1){
		printf("Filial nao cadastrada!\n");
		system("pause");
		return;
	}
	system("cls");
	
	for(i=0;i<MAX;i++){
		if(rotas[p][i] != 0){
			var = 1;
		}
	}
	if(var == 0){
		printf("Filial nao tem destinos!\n");
		system("pause");
		return;
	}
	
	for(i=0;i<MAX;i++){
		if(rotas[p][i] != 0){
			printf("%sR$ %.2f.\n", filiais[i] , rotas[p][i]);
		}
	}
	system("pause");
	
}

void atualizap(char filiais[MAX][MAX], float rotas[MAX][MAX]){
	int i, p1 = 0, p2 = 0, op123 = 0;
	char filial1[MAX], filial2[MAX];
	float dis;
	inicializastr(filial1);
	inicializastr(filial2);
	system("cls");
	exibefilial(filiais);
	printf("Qual nome da primeira cidade?\n");
	fflush(stdin);
	fgets(filial1, MAX, stdin);
	printf("Qual nome da segunda cidade?\n");
	fflush(stdin);
	fgets(filial2, MAX, stdin);
	p1 = pesquisafilial(filiais, filial1);
	p2 = pesquisafilial(filiais, filial2);
	if(p1 == -1 || p2 == -1){
		printf("filial nao existe!\n");
		system("pause");
		return;
	}
	if(p1 == p2){
		printf("Insercao de mesmas filiais!\n");
		system("pause");
		return;
	}
	
	printf("Qual o novo preco da movimentacao ?\n");
	scanf("%f", &dis);
	
	if(rotas[p1][p2] == 0){
		printf("Rota ainda nao existe deseja incerir? (1. sim)(2. nao)\n");
		scanf("%d", &op123);
		if(op123 == 1){
			rotas[p1][p2] = dis;
			rotas[p2][p1] = dis;
		}else{
			return;
		}
	}else{
		rotas[p1][p2] = dis;
		rotas[p2][p1] = dis;
	}
}

void removefilial(char filiais[MAX][MAX], float rotas[MAX][MAX]){
	int i, p1 = 0, j;
	char filial[MAX];
	inicializastr(filial);
	system("cls");
	exibefilial(filiais);
	printf("Qual a filial a ser retirada?\n");
	fflush(stdin);
	fgets(filial, MAX, stdin);
	p1 = pesquisafilial(filiais, filial);
	if(p1 == -1){
		printf("Filial nao existe!\n");
		system("pause");
		return;
	}
	for(i=p1;i<qtdfilial;i++){
		for(j=0;j<MAX;j++){
			filiais[i][j] = filiais[i+1][j];
		}
	}
	
	for(i=p1;i<qtdfilial;i++){
		for(j=0;j<qtdfilial;j++){
			rotas[i][j] = rotas[i+1][j];
		}
	}
	for(i=p1;i<qtdfilial;i++){
		for(j=0;j<qtdfilial;j++){
			rotas[j][i] = rotas[j][i+1];
		}
	}
	qtdfilial--;

}

void calculacustos(char filiais[MAX][MAX], float rotas[MAX][MAX]){
	int i = 0, j = 0;
	float cust = 0;
	
	for(i = 0 ; i < qtdfilial ; i++){
		for(j = i ; j < qtdfilial ; j++){
			if(rotas[i][j] != 0){
				cust+=rotas[i][j];
			}
		}
	}
	
	if(cust < 0.00001){
		printf("Ainda nao existem custos a serem somados!\n");
		system("pause");
		return;
	}else{
		printf("O custo total das rotas e de %.2f\n", cust);
		system("pause");
		return;
	}
}

//funcoes para jogo

void exibelocais(char jogo[MAX][MAX]){
	int i = 0, j = 0;
	for(i = 0; i < qtdlocal; i++){
		printf("%d . ", i+1);
		for(j = 0;j < MAX; j++){
			if(jogo[i][j] == '0'){
				printf("\n");
				break;
			}
			printf("%c", jogo[i][j]);
		}
	}
	
		
}

int pesquisalocal(char jogo[MAX][MAX], char local[MAX]){
	int i = 0;
	for(i = 0; i < MAX; i++){
		if(strcmp(jogo[i], local) == 0){
			return i;
		}
	}
	return -1;
}

void inserelocaljogo(char jogo[MAX][MAX]){
	int i;
	char local[MAX];
	inicializastr(local);
	system("cls");
	exibelocais(jogo);
	printf("Qual o nome da cidade a ser inserida?\n");
	fflush(stdin);
	fgets(local, MAX, stdin);
	if(pesquisalocal(jogo, local) != -1){
		printf("Ja existe este local!\n");
		system("pause");
		return;
	}
	if(qtdlocal>MAX){
		printf("Matriz de locais cheia! \n");
		system("pause");
		return;
	}
	strcpy(jogo[qtdlocal], local);
	
	qtdlocal++;
}

void inseremovimentacao(char jogo[MAX][MAX],float tempo[MAX][MAX]){
	int i, op123 = 0, p1 = 0, p2 = 0;
	char local1[MAX], local2[MAX];
	float dis;
	inicializastr(local1);
	inicializastr(local2);
	system("cls");
	exibelocais(jogo);
	printf("Qual nome do primeiro local?\n");
	fflush(stdin);
	fgets(local1, MAX, stdin);
	printf("Qual nome do segundo local?\n");
	fflush(stdin);
	fgets(local2, MAX, stdin);
	p1 = pesquisalocal(jogo, local1);
	p2 = pesquisalocal(jogo, local2);
	if(p1 == -1 || p2 == -1){
		printf("local nao existe!\n");
		system("pause");
		return;
	}
	if(p1 == p2){
		printf("Incersao de mesmos locais!\n");
		system("pause");
		return;
	}
	printf("Qual o tempo da movimentacao em segundos?\n");
	scanf("%f", &dis);
	
	if(tempo[p1][p2] != 0 ){
		printf("Atualizar preco? (1, sim) (2, nao)\n");
		scanf("%d", &op123);
		if(op123 == 1){
			tempo[p1][p2] = dis;
			return;
		}else{
			return;
		}
	}else{
		tempo[p1][p2] = dis;
		return;
	}
	
	
}

void listalocaisdestino(char jogo[MAX][MAX], float tempo[MAX][MAX]){
	int i, p, var = 0;
	char local[MAX];
	inicializastr(local);
	system("cls");
	exibelocais(jogo);
	if(qtdlocal == 0){
		printf("Ainda nao existem locais para serem mostrados!\n");
		system("pause");
		return;
	}
	printf("Qual nome do local a ser consultado?\n");
	fflush(stdin);
	fgets(local, MAX, stdin);
	p = pesquisafilial(jogo, local);
	if(p == -1){
		printf("local nao cadastrado!\n");
		system("pause");
		return;
	}
	system("cls");
	
	for(i=0;i<MAX;i++){
		if(jogo[p][i] != 0){
			var = 1;
		}
	}
	if(var == 0){
		printf("local nao tem destinos!\n");
		system("pause");
		return;
	}
	
	for(i=0;i<MAX;i++){
		if(tempo[p][i] != 0){
			printf("%s %.0f s.\n", jogo[i] , tempo[p][i]);
			var = 2;
		}
	}
	if(var == 1){
			printf("local nao tem destinos!\n");
			system("pause");
			return;
	}
	system("pause");
}

void listalocaisorigem(char jogo[MAX][MAX], float tempo[MAX][MAX]){
	int i, p, var = 0;
	char local[MAX];
	inicializastr(local);
	system("cls");
	exibelocais(jogo);
	if(qtdlocal == 0){
		printf("Ainda nao existem locais para serem mostrados!\n");
		system("pause");
		return;
	}
	printf("Qual nome do local a ser consultado?\n");
	fflush(stdin);
	fgets(local, MAX, stdin);
	p = pesquisafilial(jogo, local);
	if(p == -1){
		printf("local nao cadastrado!\n");
		system("pause");
		return;
	}
	system("cls");
	
	for(i=0;i<MAX;i++){
		if(jogo[i][p] != 0){
			var = 1;
		}
	}
	if(var == 0){
		printf("local nao tem pontos de origem!\n");
		system("pause");
		return;
	}
	
	for(i=0;i<MAX;i++){
		if(tempo[i][p] != 0){
			printf("%s %.0f s.\n", jogo[i] , tempo[i][p]);
			var = 2;
		}
	}
	if(var == 1){
			printf("local nao tem pontos de origem!\n");
			system("pause");
			return;
	}
	system("pause");
	
	
	
}

void atualizamovimentacao(char jogo[MAX][MAX], float tempo[MAX][MAX]){
	int i, p1 = 0, p2 = 0, op123 = 0;
	char local1[MAX], local2[MAX];
	float dis;
	inicializastr(local1);
	inicializastr(local2);
	system("cls");
	exibelocais(jogo);
	printf("Qual nome do primeiro local?\n");
	fflush(stdin);
	fgets(local1, MAX, stdin);
	printf("Qual nome do segundo local?\n");
	fflush(stdin);
	fgets(local2, MAX, stdin);
	p1 = pesquisafilial(jogo, local1);
	p2 = pesquisafilial(jogo, local2);
	if(p1 == -1 || p2 == -1){
		printf("local nao existe!\n");
		system("pause");
		return;
	}
	if(p1 == p2){
		printf("Incersao de mesmos locais!\n");
		system("pause");
		return;
	}
	printf("Qual o novo tempo da movimentacao ?\n");
	scanf("%f", &dis);
	
	if(tempo[p1][p2] == 0){
		printf("movimentacao ainda nao existe deseja incerir? (1. sim)(2. nao)\n");
		scanf("%d", &op123);
		if(op123 == 1){
			tempo[p1][p2] = dis;
		}else{
			return;
		}
	}else{
		tempo[p1][p2] = dis;
	}
}

void removelocal(char jogo[MAX][MAX], float tempo[MAX][MAX]){
	int i, p1 = 0, j;
	char local[MAX];
	inicializastr(local);
	system("cls");
	exibelocais(jogo);
	printf("Qual o local a ser retirado?\n");
	fflush(stdin);
	fgets(local, MAX, stdin);
	p1 = pesquisafilial(jogo, local);
	if(p1 == -1){
		printf("Local nao existe!\n");
		system("pause");
		return;
	}
	
	for(i=p1;i<qtdlocal;i++){
		for(j=0;j<MAX;j++){
			jogo[i][j] = jogo[i+1][j];
		}
	}
	
	for(i=p1;i<qtdlocal;i++){
		for(j=0;j<qtdlocal;j++){
			tempo[i][j] = tempo[i+1][j];
		}
	}
	
	for(i=p1;i<qtdlocal;i++){
		for(j=0;j<qtdlocal;j++){
			tempo[j][i] = tempo[j][i+1];
		}
	}
	qtdlocal--;
}

void removetempo(char jogo[MAX][MAX], float tempo[MAX][MAX]){
	int i, p1 = 0, p2 = 0, op123 = 0;
	char local1[MAX], local2[MAX];
	float dis;
	inicializastr(local1);
	inicializastr(local2);
	system("cls");
	exibelocais(jogo);
	printf("Qual nome do local de origem?\n");
	fflush(stdin);
	fgets(local1, MAX, stdin);
	printf("Qual nome do local de destino?\n");
	fflush(stdin);
	fgets(local2, MAX, stdin);
	p1 = pesquisafilial(jogo, local1);
	p2 = pesquisafilial(jogo, local2);
	
	if(p1 == p2){
		printf("Incersao de mesmos locais!\n");
		system("pause");
		return;
	}
	
	if(p1 == -1 || p2 == -1){
		printf("local nao existe!\n");
		system("pause");
		return;
	}
	tempo[p1][p2] = 0;
}

void calculatempo(char jogo[MAX][MAX], float tempo[MAX][MAX]){
	int i = 0, j = 0;
	float cust = 0;
	
	for(i = 0 ; i < qtdlocal ; i++){
		for(j = 0 ; j < qtdlocal ; j++){
			if(tempo[i][j] != 0){
				cust+=tempo[i][j];
			}
		}
	}
	
	if(cust < 0.00001){
		printf("Ainda nao existem tempos a serem somados!\n");
		system("pause");
		return;
	}else{
		printf("O custo total do tempo e de %.0f s\n", cust);
		system("pause");
		return;
	}
	
	
	
}



//Funcoes do menu
int chamamenuprincipal(){
	int op;
	do{
		system("cls");
		printf("   MENU DE OPCOES \n");
		printf("0. Sair do programa.\n");
		printf("1. Modulo da logistica.\n");
		printf("2. Modulo do jogo.\n");
		printf("\nEscolha a opcao desejada:  \n");
		scanf("%d", &op);
		system("cls");
		switch(op){
			case 0:
				return 0;
			break;
			case 1:
				return op;
			break;
			case 2:
				return op;
			break;
		}
	}while(op != 1);
}

int chamamenufilial(){
	int op = 0;
	do{
		system("cls");
		printf("   MANU DE LOGISTICA \n");
		printf("0. Sair do programa.\n");
		printf("1. Menu principal.\n");
		printf("2. Menu de jogos.\n");
		printf("3. Inserir filiais.\n");
		printf("4. Inserir rotas.\n");
		printf("5. Exibir filiais proximas.\n");
		printf("6. Atualizar preco de movimentacao.\n");
		printf("7. Remover filial.\n");
		printf("8. Somar todos os custos de movimentacao.\n");
		printf("\nEscolha a opcao desejada:  \n");
		scanf("%d", &op);
		system("cls");
		switch(op){
			case 0:
				return 0;
			break;
			case 1:
				return op;
			break;
			case 2:
				return op;
			break;
			case 3:
				return op;
			break;
			case 4:
				return op;
			break;
			case 5:
				return op;
			break;
			case 6:
				return op;
			break;
			case 7:
				return op;
			break;
			case 8:
				return op;
			break;
		}
	}while(op != 1);
}

int chamamenujogo(){
	int op = 0;
	do{
		system("cls");
		printf("   MENU DE JOGO \n");
		printf("0. Sair do programa.\n");
		printf("1. Menu principal.\n");
		printf("2. Menu de logistica.\n");
		printf("3. Insere um local no jogo.\n");
		printf("4. Insere um tempo de locomocao.\n");
		printf("5. Listar locais de destino a partir de um local.\n");
		printf("6. Listar locais de origem a partir de um local.\n");
		printf("7. Atualizar tempo de movimentacao de um local a outro.\n");
		printf("8. Remover local do jogo.\n");
		printf("9. Remover temppo de movimentacao.\n");
		printf("10. Calcular todo o tempo de movimentacao pelo mapa do jogo.\n");
		printf("\nEscolha a opcao desejada:  \n");
		scanf("%d", &op);
		system("cls");
		switch(op){
			case 0:
				return 0;
			break;
			case 1:
				return op;
			break;
			case 2:
				return op;
			break;
			case 3:
				return op;
			break;
			case 4:
				return op;
			break;
			case 5:
				return op;
			break;
			case 6:
				return op;
			break;
			case 7:
				return op;
			break;
			case 8:
				return op;
			break;
			case 9:
				return op;
			break;
			case 10:
				return op;
			break;
		}
	}while(op != 1);
}


int main(){
	int op = 1, op1 = 1;
	char filial[MAX], filiais[MAX][MAX], jogo[MAX][MAX];
	float rotas[MAX][MAX], tempo[MAX][MAX];
	inicializagrafo(jogo);
	inicializagrafo(filiais);
	inicializafloat(rotas);
	inicializafloat(tempo);
	while(op != 0){
		if(op == 1){
			op = chamamenuprincipal();
			if(op == 0){
				break;
			}
			op++;
		}else if(op == 2){
			op = chamamenufilial();
			op1 = op;
			if(op1 == 2){
				op = 3;	
			}if(op1 == 3){
				inserefilial(filiais);
				op = 2;
			}else if(op1 == 4){
				insererota(filiais, rotas);
				op = 2;
			}else if(op1 == 5){
				listafiliaisp(filiais, rotas);				
				op = 2;
			}else if(op1 == 6){
				atualizap(filiais, rotas);				
				op = 2;
			}else if(op1 == 7){
				removefilial(filiais, rotas);				
				op = 2;
			}else if(op1 == 8){
				calculacustos(filiais, rotas);				
				op = 2;
			}
		}else if(op == 3){
			op = chamamenujogo();
			op1 = op;
			if(op1 == 2){
				op = 2;
			}else if(op1 == 3){
				inserelocaljogo(jogo);
				op = 3;
			}else if(op1 == 4){
				inseremovimentacao(jogo, tempo);
				op = 3;
			}else if(op1 == 5){
				listalocaisdestino(jogo, tempo);
				op = 3;
			}else if(op1 == 6){
				listalocaisorigem(jogo, tempo);
				op = 3;
			}else if(op1 == 7){
				atualizamovimentacao(jogo, tempo);
				op = 3;
			}else if(op1 == 8){
				removelocal(jogo, tempo);
				op = 3;
			}else if(op1 == 9){
				removetempo(jogo, tempo);
				op = 3;
			}else if(op1 == 10){
				calculatempo(jogo, tempo);
				op = 3;
			}
		}
	}
}




