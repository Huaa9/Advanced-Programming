## gcd-solution.cpp
```c++
// g++ gcd.cpp -o gcd

#include <iostream>

// swap variables using arithmetic operations
void swap(int &a, int &b)
{
    b += a;
    a = b - a;
    b -= a;
}

// Alternative: swap variables using bitwise operations, only meant if you know what this is.
// Please avoid bitwise operations, they may offer some advantages in low-level code,
// but they are error-prone and reduce readability.
void swapXor(int &a, int &b)
{
    b = a ^ b;
    a = a ^ b;
    b = a ^ b;
}

int gcdRecursive(int a, int b)
{
    if (0 == b)
    {
        return a;
    }
    else
    {
        return gcdRecursive(b, a % b);
    }
}

int gcdIterative(int a, int b)
{
    while (b != 0)
    {
        int temp = a % b;
        a = b;
        b = temp;
    }
    return a;
}

int gcdIterative2(int a, int b)
{
    while (b != 0)
    {
        a %= b;
        swap(a, b);
    }
    return a;
}

int gcdIterative3(int a, int b)
{
    while (b != 0)
    {
        a %= b;
        std::swap(a, b);
    }
    return a;
}

int main()
{
    int a = 8;
    int b = 12;

    std::cout << "gcdRec=" << gcdRecursive(a, b) << std::endl;
    std::cout << "gcdIter=" << gcdIterative(a, b) << std::endl;
    std::cout << "gcdIter2=" << gcdIterative2(a, b) << std::endl;
    std::cout << "gcdIter3=" << gcdIterative3(a, b) << std::endl;
    std::cout << "a=" << a << " b=" << b << std::endl;

    // Side-note: example for swapping operations using logical operations.
    // Please avoid this.
    int c = 245;
    int d = -43;

    swapXor(c, d);
    std::cout << "c=" << c << " d=" << d << std::endl;

    return 0;
}
```
## matrix operation part2 solution.cpp
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

bool test_matrix_vector_product() {
    /*
    * Test implementation of operator* for Matrix-vector product.
    */
    bool tests_passed = true;

    std::vector<std::vector<double>> matrix = { {1. , 2. }, {3. , 4. }};
    std::vector<double> vec = {1., 1.};

    std::vector<double> mat_vec = matrix * vec;

    std::vector<double> reference = {3. ,7.};

    double tol = 1e-8;
    for(auto i = 0; i < reference.size(); i++){
        // floating point values are "equal" if their
        // difference is small 
        if( std::abs(reference.at(i) - mat_vec.at(i) ) > tol ){
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
        std::cout << "Computed: ";
        print_vector(mat_vec, false);
    }
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
