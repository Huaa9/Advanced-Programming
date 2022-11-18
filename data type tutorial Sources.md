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
   // YOUR CODE HERE 
}

int main() {

    // 2. Create a vector global_cases and initialize
    // it with dummy data using populate_vector (task 3).
    // Once you have done so, uncomment the last line in main:
    // i.e. call "write_to_csv"

    /* YOUR CODE HERE */

    // 4. Find out which country had the highest number of cases today
    // and print both the no. of cases and the name of the country
    // print today's case numbers. 
    
    /* YOUR CODE HERE */

    // write_to_csv( global_cases );

}
```
