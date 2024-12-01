# Unordered Map

An unordered_map in C++ is an associative container that stores key-value pairs using a hash table.

## Constructors

Its constructors allow the creation and initialization of an unordered_map in various ways.

## Default Constructor

Syntax:

    std::unordered_map<key, T> umap;
Creates an empty unordered_map with no elements.

For example-

    std::unordered_map<int, std::string> umap;

## Range Constructor

Syntax:
    
    std::unordered_map<Key, T> umap(InputIterator first, InputIterator last);

Creates an unordered_map initialized with elements in the range [first, last)

- note in this iterator points to pair of values not a single variable value

For example -

    std::vector<std::pair<int, std::string>> vec = {{1, "one"}, {2, "two"}, {3, "three"}};
    std::unordered_map<int, std::string> umap(vec.begin(), vec.end());
The syntax will only work for this type of vector

## Copy Constructor

Syntax:

    std::unordered_map<Key, T> umap(const std::unordered_map<Key, T>& other);
Creates a new unordered_map as a copy of other.

For example-

    std::unordered_map<int, std::string> umap1 = {{1, "one"}, {2, "two"}};
    std::unordered_map<int, std::string> umap2(umap1);

## Move Constructor

Syntax:

    std::unordered_map<Key, T> umap(std::unordered_map<Key, T>&& other);
Moves elements from other to the new unordered_map. After the operation, other is left in a valid but unspecified state.

>What Does "Valid but Unspecified State" Mean?
>
>Valid:
>  * The object can still be destroyed (its destructor can safely run).
>  * The object can be assigned a new value or reused.
>  * Any member functions can still be called on the object without causing undefined behavior.
>
>Unspecified:
>  * The exact contents or value of the object after the move are not defined.
>  * The object is essentially "empty" or in some implementation-dependent state that cannot be relied upon.
>
>For example, after moving a container like an unordered_map, it might:
>  * Have zero elements.
>  * Have an unspecified number of buckets, though they are likely empty.
>  * Behave as an empty container for all practical purposes.

For example-

    std::unordered_map<int, std::string> umap1 = {{1, "one"}, {2, "two"}};
    std::unordered_map<int, std::string> umap2(std::move(umap1));

## Initializer List Constructor

Syntax:

    std::unordered_map<Key, T> umap(std::initializer_list<std::pair<const Key, T>> il);
Creates an unordered_map initialized with the elements in the initializer list il.

For example-

    std::unordered_map<int, std::string> umap = {{1, "one"}, {2, "two"}, {3, "three"}};
    
## Allocator Constructor

Syntax:

    std::unordered_map<Key, T> umap(const Allocator& alloc);
Creates an empty unordered_map using the specified allocator alloc.

For example-

    std::allocator<std::pair<const int, std::string>> alloc;
    std::unordered_map<int, std::string> umap(alloc);

## Custom Hash and Key Equality Constructors

Syntax:

    std::unordered_map<Key, T, Hash, KeyEqual> umap(
        size_t bucket_count = N,
        const Hash& hash = Hash(),
        const KeyEqual& equal = KeyEqual()
    );

    bucket_count: The initial number of buckets.
    hash: A callable object for hashing keys.
    equal: A callable object for comparing keys for equality.

The custom hash and key equality constructor in an unordered_map allows you to specify:

1. Initial Bucket Count:
+ The number of "buckets" the hash table initially allocates. Buckets store elements with identical hash values.
+ Specifying this can optimize performance if you know approximately how many elements you plan to store.

2. Custom Hash Function:
+ A function or functor used to compute the hash value of keys.
+ By default, std::hash<Key> is used. If you have a specific way to hash your keys, you can define and use a custom hash function.

3. Custom Key Equality Function:
+ A function or functor that determines whether two keys are considered equal.
+ By default, the operator== is used. If your key type requires special equality checks, you can define a custom equality function.


>Custom Hash

    #include <unordered_map>
    #include <iostream>
    
    // Define a custom hash function
    struct CustomHash {
    size_t operator()(int key) const {
        return key % 10; // Custom logic: hash based on remainder when divided by 10
    }
    };
    
    // Main function
    int main() {
    // Create an unordered_map with a custom hash function
      std::unordered_map<int, std::string, CustomHash> umap;
    
    // Insert elements into the unordered_map
    umap[1] = "one";
    umap[11] = "eleven"; // Will hash to the same bucket as 1 because of CustomHash
    
    // Print the unordered_map contents
    for (const auto &pair : umap) {
        std::cout << pair.first << ": " << pair.second << "\n";
    }
    return 0;
    }

Explanation:
* The custom hash function (CustomHash) computes the hash of a key as key % 10. This means keys like 1 and 11 will hash to the same bucket.
* The unordered_map is constructed with this hash function, and the map will behave according to the custom hashing logic.

>Key Equality Function

If two keys hash to the same value, the unordered_map uses the equality function to distinguish them. By default, it uses operator==. If you need custom logic, you can define your own.

    #include <unordered_map>
    #include <iostream>
    #include <string>
    
    // Custom equality function
    struct CustomEqual {
    bool operator()(const std::string& a, const std::string& b) const {
        return a.length() == b.length(); // Compare keys based on length
    }
    };
    
    // Main function
    int main() {
    // unordered_map with custom hash and equality
    std::unordered_map<std::string, int, std::hash<std::string>, CustomEqual> umap;
    
    umap["one"] = 1;     // Key "one" (length 3)
    umap["two"] = 2;     // Key "two" (length 3, treated as equal to "one")
    umap["three"] = 3;   // Key "three" (length 5)
    
    for (const auto& pair : umap) {
        std::cout << pair.first << ": " << pair.second << "\n";
    }
    
    return 0;
    }

Explanation:
* The equality function considers keys equal if they have the same length.
* "one" and "two" are treated as the same key because their lengths are equal.









## Capacity Functions

The capacity-related functions in std::unordered_map provide information about the number of elements it contains and its ability to store elements.

> size()

Description: Returns the number of elements currently stored in the unordered_map.
Syntax:
        
    std::size_t size() const noexcept;
Example-

    umap.size()
    
Return Value: The number of elements in the container.

> empty()

Description: Checks whether the unordered_map contains any elements.
Syntax:
        
    bool empty() const noexcept;
Example-

    if (umap.empty()) {
        std::cout << "The unordered_map is empty.\n";
    }
Return Value:
+ true if the unordered_map is empty (contains 0 elements).
+ false otherwise

> max_size()

Description: Returns the theoretical maximum number of elements the unordered_map can hold based on system and implementation constraints.
Syntax:

    std::size_t max_size() const noexcept;
Example-

    umap.max_size()
Return Value: The maximum number of elements that can be stored in the unordered_map.
