# lab2_10670427
#include <iostream>
#include "mpi.h"
#include <chrono>

using namespace std;
using namespace std::chrono;
int main(int argc, char *argv[]){

int rank, size;
int sum, startval, endval, accum;


auto start= high_resolution_clock::now();
MPI::Init(argc, argv);

size = MPI::COMM_WORLD.Get_size();
rank = MPI::COMM_WORLD.Get_rank();
sum = 0; 

startval = 1000*rank/size+1;
endval = 1000*(rank+1)/size;
for(int i=startval; i<=endval; i=i+1){
   sum = sum + 1;
   } 
if(rank !=0){
   
   MPI::COMM_WORLD.Send(&sum, 1, MPI::INT, 0, 1);

   }else{

   for(int j=1; j<size; j=j+1){
      MPI::COMM_WORLD.Recv(&accum, 1, MPI::INT, j, 1);
sum = sum +accum;
   }

   }
if(rank == 0){
      cout<<"The sum from 1 to 1000 is: " << sum<< endl;

   }



MPI::Finalize();
auto stop = high_resolution_clock::now();

auto duration= duration_cast<microseconds>(stop-start);

cout<<"This taken by function: " <<duration.count()<< " microseconds" <<endl;   
  return 0;
    }
