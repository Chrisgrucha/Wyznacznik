#include <iostream>
#include <cstdlib>
#include <fstream>
#include <ctime>


using namespace std;

double **read(const char*, int*);
void display(double **, int);
double det(double **, int);
double **alokacja(int);
void zwalnianie(double **, int);


int main(int argc, char *argv[])
{

    double **tab;
    int w=0;
    tab=read(argv[1], &w);
    cout<<endl<<det(tab, w)<<endl;
    zwalnianie(tab,w);
    return 0;
}

double **alokacja(int w)
{
    double **tab;
    tab=(double**)calloc(w, sizeof(double*));
    for(int i=0;i<w;i++)
    {
        tab[i]=(double*)calloc(w,sizeof(double));
    }
    return tab;
}

void zwalnianie(double **tab, int w)
{
    for(int i=0;i<w;i++)
    {
        free(tab[i]);
    }
    free(tab);
}


double **read(const char* filename, int* w)
{

    fstream plik(filename, ios::in);
    if(plik.good()==false)
    {
        cout<<"BLAD WCZYTYWANIA PLIKU!"<<endl;
        plik.close();
    }
    plik>>*(w);
    double **tab;
    tab=alokacja(*w);
    for(int i=0;i<*w;i++)
    {
        for(int j=0;j<*w;j++)
        {
            plik>>tab[i][j];
        }
    }
    display(tab,*w);
    return tab;
}

void display(double **tab, int w)
{
    for(int i=0;i<w;i++)
    {
        for(int j=0;j<w;j++)
        {
            cout<<tab[i][j]<<"\t";
        }
        cout<<"\n"<<endl;
    }
}

double det(double ** matrix, int n)
{
  double **minor;
  double sum=0.0;
  double sign=1.0;
  int i, j;
  if(n==1) return matrix[0][0];
  if(n==2) return ((matrix[0][0]*matrix[1][1]) - (matrix[0][1]*matrix[1][0]));
  minor=(double**)calloc(n-1,sizeof(double));
  for(i=0;i<n-1;i++)
  {
        minor[i]=(double*)calloc(n-1,sizeof(double));
  }
  for(i=0;i<n;i++)
  {
      for(int k=0, x=0;x<n-1;k++,x++)
      {
	      if(i==k) k++;
          for(j=0;j<n-1;j++)
          {
              minor[j][x]=matrix[j+1][k];
          }
      }

      sum+=sign*matrix[0][i]*det(minor,n-1);
      sign = -sign;
  }
  zwalnianie(minor,n-1);
  return sum;
}
