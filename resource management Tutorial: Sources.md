## ecdc cases june.csv
一个excel表格
## matric operation part3.cpp
```c++
#include <iostream>
#include <vector>
#include <cassert>

template<typename T>
void print_vector(const std::vector<T>& vec, 
               bool print_vector_header = true){
    /*
    * Print vectors with elements of type T. Note
    * that this only works for objects of type 
    * T for which "std::cout << object" would work.
    */
   
   if(print_vector_header){
       std::cout << "Vector of size " << vec.size() << "\n";
   }

   std::cout << "[ ";
   // for each element in vector
   for (const auto& elem : vec ){
       std:: cout << elem << " ";
   }
   std::cout << " ]\n";
}

template<typename T>
void print_matrix(const std::vector<std::vector<T>>& matrix){

    std::cout << matrix.size() << " x " << matrix[0].size() << " Matrix: " << '\n';
    // for each row in matrix
    for(const auto& row : matrix){
        print_vector(row, false);
    }
}

std::vector<double> operator+( const std::vector<double>& vec_a,
                               const std::vector<double>& vec_b ){
    /*
    * Overload operator+ to allow addition of two double vectors 
    * of the same size. The return value is another vector formed by
    * adding the corresponding elements of vec_a and vec_b.
    */
    
    assert(vec_a.size() == vec_b.size() );
    
    std::vector<double> vec_sum( vec_a.size(), 0.0 );

    for ( auto i = 0; i < vec_a.size(); i++ ){
        vec_sum[i] = vec_a[i] + vec_b[i];
    }
    return vec_sum;
}

std::vector<double> operator*(const std::vector<std::vector<double>>& matrix,
                              const std::vector<double>& vec){
    /*
    * Overload of operator* for matrix vector multiplication
    * where a matrix is a vector of vector of doubles:
    * std::vector<std::vector<double>> 
    * and vector is std::vector<double>
    */
    auto N = vec.size();
    assert(N == matrix[0].size() );
    std::vector<double> res(N, 0.0);

    for(auto i = 0; i < N; i++){
        for(auto j = 0; j < N; j++){
            res[i] += vec[j] * matrix[i][j];
        }
    }
    return res;
}

double * mat_times_vec(double * matrix, size_t matrix_rows, size_t matrix_columns,
				       double * vec,    size_t vec_size){
	/*
	 * C/old-C++ way of multiplying matrix * vector.
	 * We need to also pass the sizes of each 
	 */
	assert(matrix_columns == vec_size);
	double * res = new double[matrix_rows];
	// By the way, C only has malloc instead of new.
	
	for(size_t i = 0; i < matrix_rows; i++){
		res[i] = 0;
		for(size_t j = 0; j < matrix_columns; j++){
			res[i] += vec[j] * matrix[i*matrix_columns+j];
		}
	}
	return res;
}

void mat_times_vec(double *matrix, size_t matrix_rows, size_t matrix_columns,
                   double *vec, size_t vec_size, double *res) {
  /*
   * C/old-C++ way of multiplying matrix * vector.
   * We need to also pass the sizes of each
   */
  assert(matrix_columns == vec_size);
  // By the way, C only has malloc instead of new.

  for (size_t i = 0; i < matrix_rows; i++) {
    res[i] = 0;
    for (size_t j = 0; j < matrix_columns; j++) {
      res[i] += vec[j] * matrix[i * matrix_columns + j];
    }
  }
}

bool test_matrix_vector_product() {
    /*
    * Test implementation of operator* for Matrix-vector product.
    */
    bool tests_passed = true;

    std::vector<std::vector<double>> matrix = { {1. , 2. }, {3. , 4. }};
    std::vector<double> vec = {1., 1.};

    std::vector<double> mat_vec = matrix * vec;

	// Alternative implementation with raw pointers.
	// matrix_raw is a pointer to an 1D array of 2x2 elements
	// vec_raw    is a pointer to an 1D array of 2   elements
	// res_raw    is a pointer without destination at the moment
	double * matrix_raw = new double[2*2];
	double * vec_raw    = new double[2];
	double * res_raw;
	
	// TInitialize matrix_raw and vec_raw with values
	matrix_raw[0] = 1;
	matrix_raw[1] = 2;
	matrix_raw[2] = 3;
	matrix_raw[3] = 4;
	vec_raw[0]    = 1;
	vec_raw[1]    = 1;
	
	// Compute matrix-vector product using mat_times_vec(...) 
	res_raw = mat_times_vec(matrix_raw, 2, 2, vec_raw, 2);

    std::vector<double> reference = {3. ,7.};

    double tol = 1e-8;
    for(auto i = 0; i < reference.size(); i++){
        // floating point values are "equal" if their
        // difference is small 
        if( (reference.at(i) - mat_vec.at(i) ) > tol ){
            tests_passed = false;
        }
    }
    for(auto i = 0; i < 2; i++){
		if ( (reference.at(i) - res_raw[i] ) > tol ){
			tests_passed = false;
		}
	}
    
    if(tests_passed){
        std::cout << "Tests passed!\n";
    }
    else{
        std::cout << "Tests failed \n";
        std::cout << "Reference: ";
        print_vector(reference, false);
        std::cout << "Computed (operator*): ";
        print_vector(mat_vec, false);
        std::cout << "Computed (mat_times_vec()): ";
        std::cout << res_raw[0] << " " << res_raw[1] << std::endl;
    }

    // TODO: Delete all memory allocated with new
    delete[] matrix_raw;
    delete[] vec_raw;

    return tests_passed;
}


int main(){
    std::vector<std::vector<double>> matrix = { {1. , 0. , 0.}, 
                                                {0. , 1. , 0.} ,
                                                {0. , 0. , 1.} };

    std::vector<double> vec_a = {1. , 2. , 3.};
    std::vector<double> vec_b = {4. , 5. , 6.};

    // adding two vectors
    auto vec_sum = vec_a + vec_b;
    print_vector(vec_sum);

    test_matrix_vector_product();

}
```
## vis app2.cpp
```c++
/*
** Yes, yes we know this is long but, you have
** seen most of this code before in worksheet 2.
** There are also some auxiliary functions here,
** which we could have put into a separate file.
** We'll see how to do this next week. ;)
** In the meantime, you can skip to the next long
** banner / comment.
 */

#include<iostream>
#include<sstream>
#include<cassert>
#include<fstream>
#include<vector>
#include<array>
#include<random>
#include<string>
#include<memory>
#include<algorithm>
#include<map>
#include<tuple>


const auto num_countries = 195;
// !!!this is different than before!!!
const std::vector<std::string> countries = {
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

   for( size_t i = 0; i < global_cases.size(); i++ ){

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


std::ostream& operator<<(std::ostream& os, std::vector<unsigned> vec)
{
    for(const auto& elem : vec)
    {
        os << elem << " " ;
    }
    os << '\n';
    return os;
}

std::ostream& operator<<(std::ostream& os, std::vector<double> vec)
{
    for(const auto& elem : vec)
    {
        os << elem << " " ;
    }
    os << '\n';
    return os;
}
/********************************************
** Welcome back!
*********************************************
**
 */

// Here's a struct: just a container / wrapper
// for data that should logically go together.
// This is like a class but we (usually) do not
// define methods / functions on it: we just
// use it to group together things.
//
// A struct names a type and you can create objects
// of this type like so: DataFrame data_frame;
// To access the members, use the 'dot' notation:
// data_frame.cases;
// data_frame.population; // and so on
struct DataFrame
{
    std::vector<std::string> regions;
    std::vector<unsigned> population;
    // cases[i] is a vector containing
    // daily cases observed in regions[i]
    // for multiple days.
    std::vector<std::vector<unsigned>> cases;
};

/*
** You do not "need" to read this function right now.
 */
std::unique_ptr<DataFrame> read_from_csv( std::string filename="ecdc_cases_june.csv")
{
    /*
    ** Read from a csv and populate a DataFrame.
    ** Return a unique_ptr to this DataFrame.
    */

    std::ifstream database(filename);

    std::unique_ptr<DataFrame> data = std::make_unique<DataFrame>();

    // parse data into these vars
    unsigned day_of_month;
    unsigned cases;
    std::string region;
    unsigned population;
    std::string curr_region = "";

    std::string line;
    // throw away first line: contains csv header
    std::getline(database, line);

    // keep track of region number
    // region names are repeated in csv
    auto region_num = -1;

    // while lines in csv
    while( std::getline(database, line) )
    {
        std::replace(line.begin(), line.end(), ',', ' ');
        std::stringstream line_stream(line);
        // stringstream breaks on spaces
        line_stream >> day_of_month >> cases >> region >> population;

        // new region in csv
        if( curr_region != region ){
            // -> operator: dereference pointer + access member / field
            // e.g. imagine: DataFrame* data;
            // then to access the cases field
            // we need: (*data).cases == data->cases
            curr_region = region;
            data->regions.push_back(region);
            data->population.push_back(population);
            data->cases.push_back(std::vector<unsigned>{});

            ++region_num;
        }

        data->cases[region_num].push_back(cases);
    }
    return data;
}

std::map<std::string, std::vector<double>> normalize_per_capita(std::unique_ptr<DataFrame>& data_frame)
{
    /*
    ** Normalize cases in data_frame by population i.e.
    ** compute cases per 100,000 people in each country.
    ** Returns a std::map<std::string, std::vector<double>>
     */

    // TODO: YOUR SOLUTION HERE

    /*
    ** 1. Extract references to regions, cases, and population
    **    from data_frame.
    ** 2. Declare a map<std::string, std::vector<double>>
    **    called cases_normalized. The std::string refers to the
    **    name of the region, while the associated vector contains
    **    normalized cases (per 100,000 people) in the region.
    ** 3. Populate the cases_normalized map by computing the cases
    **    per 100'000 people in the respective country. You'll need
    **    nexted loops( for(regions) { for(cases) } ).
    **    Hint: cases_normalized[name_of_region] gives you access
    **    to the associated vector.
    ** 4. Change the returned object to cases_normalized i.e.
    **    the map you just populated.
     */
    return std::map<std::string,std::vector<double>>{};
}

int main() {

    // populate vector from worksheet 2
    std::vector<unsigned int> global_cases(num_countries, 0);
    populate_vector(global_cases);


    std::cout << "**********Choose**********\n"
              << "Press (d) for dummy data\n"
              << "Press (r) for real data" << std::endl;
    char choice;
    std::cin >> choice;

    std::unique_ptr<DataFrame> data_frame;
    if( choice == 'd' )
    {
        // create DataFrame members for dummy data
        std::vector<std::vector<unsigned>> dummy_cases;
        std::vector<unsigned> dummy_population;

        for(size_t country_num = 0; country_num < countries.size(); ++country_num )
        {
            // reuse global_cases -
            dummy_cases.push_back( std::vector<unsigned>{global_cases[country_num]} );
            dummy_population.push_back(1);
        }

        // TODO: Make a unique_ptr and assign it to the
        // data_frame declared above.
        // Make sure that cases = dummy_cases
        // and population = dummy_population
        // and regions = countries.
        // Hint: You need to dereference the pointer first.

        // Since this is not yet implemented, throw an exception.
        // This would make more sense in another function, which we
        // could then wrap in a try{}catch(){} block.
        throw std::runtime_error("Tracking is not yet implemented for dummy data.\n");
        // TODO: Remove the above exception, once implemented.
    }
    else if (choice == 'r')
    {
        // TODO: get a unique_ptr from read_from_csv()
        // and assign it to the data_frame variable.
        // This unique pointer provides you real data.
        
        throw std::runtime_error("Tracking is not yet implemented for real data.\n");
        // TODO: Remove the above exception, once implemented.
    }
    else
    {
        std::cout << "It's fine if you cannot make up your mind. Maybe another time then...";
    }

    // TODO: Use a for loop to print "cases" for each country using the
    // data_frame pointer you just initialized. Hint: You can use the existing
    // operator<< overload (implemented above) for printing vectors.

    // TODO: After implementing normalize_per_capita()
    // 1. Call normalize_per_capita
    // 2. Print out the new normalized case numbers.
    // Hint: You can use the example in the worksheet to
    // iterate over the map.
}
```
