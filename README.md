# tp2
#include <iostream>
#include <fstream>
#include "listasutil.hpp"
#include "listasorde.hpp"
#include "rwstring.hpp"

using namespace std;

struct Registro{
    int id;
    string cliente;
    string producto;
    float valor;

};

const int lcliente = 12;
const int lproducto = 10;

int critclientasc (Registro a, Registro b)
{
    if (a.cliente==b.cliente)
        return a.producto.compare(b.producto);
    else 
        return a.cliente > b.cliente ? 1 : -1;
} 

ostream& operator << (ostream &os, const Registro& reg)
{
	os << "id: " << reg.id << " - " << reg.cliente << " - " << reg.producto
	   << " - cant " << reg.valor;
	return os;
}

fstream& operator >> (fstream &fs, Registro& reg)
{
	fs.read(reinterpret_cast<char *>(&reg.id), sizeof(reg.id));
	reg.cliente = readstring(fs, lcliente);
	reg.producto = readstring(fs, lproducto);
	fs.read(reinterpret_cast<char *>(&reg.valor), sizeof(reg.valor));
	return fs;
}
	
void pedirNumero(int& opcion){
	
	
	cin>>opcion;
	if(opcion < 4){
		opcion = opcion;
	}
	else{
		while (opcion > 4){		
			cout<<"Por favor ingrese un numero de entre el 1 al 4 "<<endl;
			cin>>opcion;
		}
	}
	
}

int main() {
    int opcion;
	cout<<"---MENU DE OPCIONES---"<<endl;
	cout<<"1.Leer Datos"<<endl<<"2.Mostrar"<< endl <<"3.Despachar"<< endl <<"4.Salir"<<endl;
	cout<<"Elija una opcion:"<<endl;
	pedirNumero(opcion);
	
	//while (opcion<4 && opcion>0);
	//	*entra al switch*
	//cout<<"ingrese un numero entre el 1 y el 4 ";
	
	Registro reg;
	Nodo<Registro> *listaord = nullptr;
    fstream archi;
	
	switch (opcion)
	{
	case 1 :

		archi.open("Datos.bin", ios::in | ios::binary);
		if (!archi) {
			cout << "No se pudo abrir el archivo de lectura "
		     "Datos.bin" << endl;
			return EXIT_FAILURE;
		}
		while (archi >> reg) { // rellenamos listas
			cout << reg << endl;
			insertar(reg, listaord, critclientasc); // esta lista esta rellenada con criterio cliente
		}
	archi.close();
		break;
	case 2:
	cout<<"Mostrando los datos de la lista de pedidos: "<<endl;
		mostrar (listaord);
	cout<<"Lista de Despachos: "<<endl;
		//mostrar(listadespachos)
		break;
	case 3:
		break;
	case 4:
		break;
	
	default:
		cout<<"Por favor ingrese un numero de entre el 1 al 4 "<<endl;
		//NO SE SI ESTA BIEN ESTO LA VERDAD, PORQUE HABRIA QUE HACER QUE VUELVA A PEDIR UN NUMERO Y CREO QUE NO ESTARIA PASANDO
		//ahi tire arriba tire una idea para hacerlo con un while, pero habria que acomodarlo creo.
		break;
	}
    

    return 0;
}
