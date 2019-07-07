# Memory management notes

## Stack allocation:
Example:
```cpp
  Entity entity("oleguer");  // Value based: Local to this function stack frame
  std::cout << entity.GetName() << std::endl;
```

**Pros:**
- Faster allocation
- Automatically "destroyed" when scope is finished

**Cons:**
- Can only be used in this scope (between these brackets)
- Objects can't be very very big (STACK memory is fixed to ~2MB)


## Heap allocation:
Example of dinamically allocated memory:
```cpp
  Entity *entity = new Entity("oleguer");  // Created in heap
  std::cout << (*entity).GetName() << std::endl;  // Access pointer value
  std::cout << entity->GetName() << std::endl;  // Arrow automatically does this for us

  // As it is a pointer created in heap,
  // here we could save the pointer and use it in another scope
  some_entity_pointer = entity;
  // This is not copying values, just setting another pointer

  // When we are done with this variable,
  // we have to free the memory allocated
  delete entity;
  // OBS: if we had assigned it to another pointer,
  // we could have done:  delete some_entity_pointer
  // And would have deleted the same object
  // (we can do this outside this scope)
```

**Pros:**
- Control over life-span of the object
- Can hold bigger variables (HEAP memory is expandable)

**Cons:**
- Slower
- Can lead into memory leaks

### The "NEW" KeyWord:
- Finds the necessary memory for the specified data type in the HEAP memory.
- Returns a pointer to the found address
- It also calls the constructor of the object

**OBS:** The memory allocating part of **new**, is equivalent to **malloc**:
```cpp
  // Allocate memory and call constructor:
  Entity* entity = new Entity();

  // Allocate memory but don't call constructor
  Entity* entity = (Entity*)malloc(sizeof(Entity));  
```

### The "DELETE" KeyWord:
- Calls c function: **free()**, which frees the malloc block memory
- Calls destructor of the object

**OBS:** If we create a new array, we must delete by **delete[]**:
```cpp
  int* array = new int[50];
  destroy[] array;
```

## Pointers
- Integer that holds a memory adress.
**OBS**: type of pointer is irrelevant, its always an integer

Example:
```cpp
  int var = 9;
  int* ptr = &var;  // ptr holds where in memory we are storing var
  *ptr = 10;  // Access the variable from the pointer
  // Now var has a value of 10
```

**OBS:** Pointers are also variables, so we could have pointers of pointers (double pointers)
```cpp
  int var = 9;
  int* ptr = &var;  // ptr is memory address of var
  int** double_ptr = &ptr;  // double_ptr is memory address of ptr
```

## Smart pointers
- Wrapper around raw pointer that automates **new**/**delete** call process in heap memory.

### unique_ptr
- Can't be copied (if one dies, it will free the memory its pointing to, so the other will be pointing nowhere)

Example:
```cpp
  include<memory>  // Needs to be included to use cmart pointers
```