# Batalla Naval
#include <iostream>
#include <cstdlib>
#include <ctime>
#include <cmath>

using namespace std;

#define N 10
#define alto 10
#define ancho 10

void imprimir(char matriz[N][N], bool disparos[N][N]);
void cargarDisparos(bool disparos[N][N]);
void cargarTablero(char tablero[N][N]);
void ingresarDisparo(bool disparos[N][N], int& x, int& y);
void realizarDisparo(bool disparos[N][N], int x, int y);
void separar(void);
void juegoGanado(int);
void juegoPerdido(void);
void resultadoJugada(char tablero[N][N], int x, int y, int& puntos);


int a,b,c,d,e,f,g,cont;

int main(){
	char tablero[N][N];
	int x,y,puntos=100;
	int jugada=0;
	a=b=c=d=e=f=g=0;
	cont=0;
	
	bool disparos[N][N];
	
	cargarDisparos(disparos);
	cargarTablero(tablero);
	
	do{
		jugada++;
		separar();
		
		cout<<"JUGADA #"<<jugada<<"		Puntos: "<<puntos<<endl;
		ingresarDisparo(disparos,x,y);
		realizarDisparo(disparos, x, y);
		
		cout<<"RESULTADO JUGADA #"<<jugada<<endl;
		resultadoJugada(tablero,x,y,puntos);
		
		cout<<"Historial de disparos"<<endl;
		imprimir(tablero, disparos);

	}while(cont<23 && puntos>0);
	
	if (cont==23) juegoGanado(puntos);
	else juegoPerdido();
}

void imprimir(char matriz[N][N], bool disparos[N][N]){
	cout<<endl;
	for(int i=-1 ; i<alto ; i++)
	{
		if (i==-1) cout<<"   1 2 3 4 5 6 7 8 9 10";
		else for(int j=-1 ; j<ancho ; j++)
		{
			if (j==-1) cout<<(char)(i+65)<<"  ";
			else
			{
				if(disparos[i][j]) cout<<matriz[i][j]<<" ";
				else cout<<"? ";
			}
		}
		cout<<endl;
	}
	return;
}

void cargarTablero(char tablero[N][N]){
	srand(time(NULL));
	
	int barcos[7]={5,4,4,3,3,2,2};
	char barco='a';
	int x,y,longitud,l,r;
	bool listo,horizontal;
	
	for(int i=0;i<N;i++)
		for(int j=0;j<N;j++)
			tablero[i][j]='.';
	
	for(int i=0;i<7;i++)
	{
		listo=false;
		do
		{
			longitud=barcos[i];
			x=rand()%N;
			y=rand()%N;
			horizontal=rand()%2;
			l=r=(horizontal)?x:y;
			if(tablero[x][y]=='.')
			{
				longitud--;
				for(int j=((horizontal)?x:y)+1;j<N && !listo && tablero[x+horizontal*(j-x)][y+(!horizontal)*(j-y)]=='.';j++)
				{
					longitud--;
					r=j;
					if(longitud==0) listo=true;
				}
				for(int j=((horizontal)?x:y)-1;j>=0 && !listo && tablero[x+horizontal*(j-x)][y+(!horizontal)*(j-y)]=='.';j--)
				{
					longitud--;
					l=j;
					if(longitud==0) listo=true;
				}
			}
		}
		while(!listo);
		for(int j=l;j<=r;j++) tablero[x+horizontal*(j-x)][y+(!horizontal)*(j-y)]=barco;
		barco++;
	}
	for(int i=0;i<10;i++)
	{
		do
		{
			x=rand()%N;
			y=rand()%N;
		}while(tablero[x][y]!='.');
		tablero[x][y]='m';
	}
	return;
}

void cargarDisparos(bool disparos[N][N]){
	
	for(int i=0 ; i<alto ; i++)
	{
		for(int j=0 ; j<ancho ; j++)
		{
			disparos[i][j]=false;
		}
	}
	
}

void ingresarDisparo(bool disparos[N][N], int& x, int& y){
	cout<<"Ingrese posicion a disparar: ";
	char a;
	bool listo=false;
	do{
		cin>>a>>y;
		if (a>='a'&& a<='j') a+='A'-'a';
		if (a>='A' && a<='J' && y>0 && y<=10)
		{
			x=(int)a-(int)'A';
			y--;
			if (!disparos[x][y]) listo=true;
			else cout<<endl<<"Ya disparaste en esa posicion, Ingresa otra coordenada: ";
		}
		else cout<<endl<<"Ingresa un valor dentro dentro del mapa: ";
	}while(!listo);
}

void realizarDisparo(bool disparos[N][N], int x, int y){
	disparos[x][y]=true;
	return;
}

void separar(void){
	cout<<endl<<endl<<"-----------------------------"<<endl<<endl;
}

void juegoGanado(int puntos){
	separar();
	cout<<endl<<"Felicitaciones, has ganado el juego, tu puntuacion fue de "<<puntos<<" puntos"<<endl;
}

void juegoPerdido(void){
	cout<<endl<<endl<<"Has perdido, mejor suerte la proxima"<<endl;
}

void resultadoJugada(char tablero[N][N], int x, int y, int& puntos){
	if (tablero[x][y]=='.')
	{
		cout<<"Agua";
		puntos--;
	}
	else
	{
		if (tablero[x][y]=='m')
		{
			cout<<"Has dado en una mina";
			puntos-=5;
		}
		else 
		{
			cout<<"Fuego";
			cont++;
			
			switch(tablero[x][y])
			{
				case 'a': 
					a++;
					if (a==5) cout<<" y hundido";
					break;
				case 'b':
					b++;
					if (b==4) cout<<" y hundido";
					break;
				case 'c':
					c++;
					if (c==4) cout<<" y hundido";
					break;
				case 'd': 
					d++;
					if (d==3) cout<<" y hundido";
					break;
				case 'e':
					e++;
					if (e==3) cout<<" y hundido";
					break;
				case 'f':
					f++;
					if (f==2) cout<<" y hundido";
					break;
				case 'g':
					g++;
					if (g==2) cout<<" y hundido";
					break;
			}
			cout<<"!";
		}
	}
	cout<<endl;
}

