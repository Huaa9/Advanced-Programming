## Matrix Operation.cpp
```c++
#include <iostream>
#include <vector>

void print_vector(const std::vector<int> &v) {
  std::cout << " [ ";
  for (const auto &elem : v) {
    std::cout << elem << " ";
  }
  std::cout << "]" << std::endl;
}

void print_matrix(const std::vector<std::vector<int>> &A) {
  std::cout << "[" << std::endl;
  for (const auto &row : A) {
    for (const auto &elem : row) {
      std::cout << elem << " ";
    }
    std::cout << std::endl;
  }
  std::cout << "]" << std::endl;
}

int main() {

  // Get the vector elements from the user
  std::vector<int> v{};
  std::cout << "Give elements to the vector v\n";
  int x;
  while (std::cin >> x) {
    v.push_back(x);
  }
  std::cin.clear();
  std::cin.ignore();

  // Print the vector v
  std::cout << "Vector v is:\n";
  print_vector(v);

  // Get a square matrix A
  const auto N = v.size();
  std::cout << "Give the " << N * N << " elements of matrix A (row-first)\n";
  std::vector<std::vector<int>> A(N); // We define just the lines for now
  for (auto i = 0; i < N; ++i) {
    for (auto j = 0; j < N; ++j) {
      int y;
      std::cin >> y;
      A[i].push_back(y);
    }
  }

  // Print the matrix A
  std::cout << "Matrix A is:" << std::endl;
  print_matrix(A);

  // Multiply A * v and store it to a vector m
  std::vector<int> m(N, 0);
  for (auto i = 0; i < N; ++i) {
    for (auto j = 0; j < N; ++j) {
      m[i] += A[i][j] * v[j];
    }
  }

  // Print the vector m
  std::cout << "Vector m = A * v is:" << std::endl;
  print_vector(m);

  return 0;
}
```
## vis_app_sol.cpp
```c++
#include<iostream>
#include<cassert>
#include<fstream>
#include<vector>
#include<array>
#include<random>
#include<string>
#include<tuple>


const auto num_countries = 195;
const std::array<std::string, num_countries> countries = {
"Afghanistan", "Albania", "Algeria", "Andorra", "Angola", 
"Antigua and Barbuda", "Argentina", "Armenia", "Australia", 
"Austria", "Azerbaijan", "Bahamas", "Bahrain", "Bangladesh", 
"Barbados", "Belarus", "Belgium", "Belize", "Benin","Bhutan", 
"Bolivia", "Bosnia and Herzegovina", "Botswana", "Brazil", 
"Brunei", "Bulgaria", "Burkina Faso", "Burundi","Côte d'Ivoire",
"Cabo Verde", "Cambodia", "Cameroon", "Canada", "Central African Republic", 
"Chad", "Chile", "China","Colombia", "Comoros", "Congo", 
"Costa Rica", "Croatia", "Cuba", "Cyprus", "Czechia", 
"Democratic Republic of the Congo", "Denmark", "Djibouti", 
"Dominica", "Dominican Republic", "Ecuador", "Egypt", "El Salvador", 
"Equatorial Guinea", "Eritrea", "Estonia", "Eswatini", "Ethiopia", 
"Fiji", "Finland", "France", "Gabon", "Gambia", "Georgia","Germany", 
"Ghana", "Greece", "Grenada", "Guatemala", "Guinea", "Guinea-Bissau", 
"Guyana", "Haiti", "Holy See","Honduras", "Hungary", "Iceland", "India", 
"Indonesia", "Iran", "Iraq", "Ireland", "Israel", "Italy", "Jamaica",
"Japan", "Jordan", "Kazakhstan", "Kenya", "Kiribati", "Kuwait", 
"Kyrgyzstan", "Laos", "Latvia", "Lebanon", "Lesotho","Liberia", 
"Libya", "Liechtenstein", "Lithuania", "Luxembourg", "Madagascar", 
"Malawi", "Malaysia", "Maldives", "Mali","Malta", "Marshall Islands",
"Mauritania", "Mauritius", "Mexico", "Micronesia", "Moldova", "Monaco",
"Mongolia","Montenegro", "Morocco", "Mozambique", "Myanmar", "Namibia", 
"Nauru", "Nepal", "Netherlands", "New Zealand","Nicaragua", "Niger",
"Nigeria", "North Korea", "North Macedonia", "Norway", "Oman", 
"Pakistan", "Palau", "Palestine State", "Panama", "Papua New Guinea", 
"Paraguay", "Peru", "Philippines", "Poland", "Portugal", "Qatar",
"Romania","Russia", "Rwanda", "Saint Kitts and Nevis", "Saint Lucia", 
"Saint Vincent and the Grenadines", "Samoa", "San Marino",
"Sao Tome and Principe", "Saudi Arabia", "Senegal", "Serbia", 
"Seychelles", "Sierra Leone", "Singapore", "Slovakia","Slovenia", 
"Solomon Islands", "Somalia", "South Africa", "South Korea", 
"South Sudan", "Spain", "Sri Lanka", "Sudan","Suriname", 
"Sweden", "Switzerland", "Syria", "Tajikistan", "Tanzania", 
"Thailand", "Timor-Leste", "Togo", "Tonga","Trinidad and Tobago", 
"Tunisia", "Turkey", "Turkmenistan", "Tuvalu", "Uganda", "Ukraine", 
"United Arab Emirates","United Kingdom", "United States of America",
"Uruguay", "Uzbekistan", "Vanuatu", "Venezuela", "Vietnam", "Yemen","Zambia", 
"Zimbabwe"
};

void write_to_csv (std::vector<unsigned int>& global_cases, 
                   std::string filename = "./global_cases.csv"){
    /*
    * Generates a csv (comma separated values) of (country, cases).
    * This creates a file "global_cases.csv" by default in the 
    * current folder. We use the <fstream> header for writing to the  
    * file. 
    */ 

   // open file stream for output
   std::ofstream csv_file(filename);

   // csv column headers
   csv_file << "country,cases\n";

   for( auto i = 0; i < global_cases.size(); i++ ){

       /* We can write to a file in the same way as we write to std::cout.
        * This is because both are implemented as "streams" 
        *
        * We do not need to pass the "countries" array to our 
        * "function" since it is global. Avoid doing this,
        * we will learn better ways to organize our code later.
        */ 
       csv_file << countries[i] << "," << global_cases[i] << "\n"; 
   }
   
   csv_file.close();
}

void populate_vector( std::vector<unsigned int>& global_cases) {
    /*
    * Populate a vector with pseudo-random data generated using the mersenne
    * twister engine from <random>. The seed is set to 0 so that everyone
    * gets the same sequence of 'random' numbers.
    */

    auto seed = 0;
    auto min = 0;
    auto max = 10000;

    // mt19337: a pseudo random generator using the mersenne 
    // twister engine ( from <random> )
    // The value of gen will be updated every time we access it.
    std::mt19937 gen(seed);
    // uniform_int_distribution: use the mersenne twister 
    // engine to generate a uniform random distribution over (min, max)
    std::uniform_int_distribution<unsigned int> unif_distrib(min, max);

    /* 3. populate vector global_cases with random data
    * You can generate samples from the uniform distribution above like so:
    * unif_distrib(gen)
    */ 

   // 3. solution
   for( auto& elem : global_cases){
       elem = unif_distrib(gen);
   }
}

// 6. idea exercise
/*
 * Some of you may know tuples from other programming languages. Here's 
 * how we can work with them in C++. You'll need to compile this with --std=c++17.
 */
std::tuple<std::string, int> compute_max_cases(std::vector<unsigned int>& global_cases){

    assert(global_cases.size() != 0);
    
    auto max_at = 0;
    auto curr_max = global_cases[max_at];
    for(auto i = 1; i < global_cases.size(); i++){
        if(global_cases[i] > curr_max){
            curr_max = global_cases[i];
            max_at = i;
        }
    }
    return std::make_tuple( countries[max_at], curr_max);
}

int main() {

    // 2. solution
    std::vector<unsigned int> global_cases(num_countries, 0);
    populate_vector(global_cases);

    // 4. Find out which country had the highest number of cases today
    // and print both the no. of cases and the name of the country
    auto curr_max = global_cases[0];
    auto max_at = 0;

    // 4. solution
    for(auto i = 1; i < global_cases.size(); i++) {
        if ( global_cases[i] > curr_max) {
            curr_max = global_cases[i];
            max_at = i;
        }
    }
    for(const auto& elem : global_cases) {
        std::cout << elem << " ";
    }
    std::cout << '\n';

    // 6. solution
    //  This way (auto [var1, var2]) of pulling out information from a tuple
    // is called destructuring: You'll need to compile with c++17 support:
    // ( gcc --std=c++17)
    auto [ country, max ] = compute_max_cases(global_cases);

    std::cout << "Today's maximum of " << max << " was recorded in " 
        << country << '\n';


//    std::cout << " Today's maximum of " << curr_max 
//        << " was recorded in " << countries[max_at];

    write_to_csv( global_cases );

}
```
