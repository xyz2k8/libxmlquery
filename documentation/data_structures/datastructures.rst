.. highlight:: c

===============
Data Structures
===============

Generic List
------------

Generic array list optimized for fast access. This structure is defined in stack.h and implemented in stack.c. It can act as a stack, queue or random access list. Although they seem different, the datastructure is the same and every function of one type can be used in another. This means that you can have a random access list and start poping out elements as though it was a stack.

.. c:type:: struct generic_list_s
.. c:type:: list

This structure defines the random access list. Its structure definition is::

  typedef struct generic_list_s{
    struct list_bucket** array;
    int32_t start;
    int32_t count;
    int32_t capacity;
  } list;

As you can see this is just a list backed up by an array of :c:type:`struct list_bucket`. These buckets are defined as follows:

.. c:type:: struct list_bucket

::

  struct list_bucket
  {
    int16_t type;
    void* element;
  };

Finally, notice that the stack and queue type are just typedefs of :c:type:`list`.

::

  typedef struct generic_list_s stack;
  typedef struct generic_list_s queue;

The next sections will show how to use these types.

Function description
^^^^^^^^^^^^^^^^^^^^

.. c:function:: uint8_t generic_list_is_empty(struct generic_list_s* l)

   :c:member:`l` The generic list to check if is empty.

   This function checks if the generic list is empty. It return an integer bigger than 0 if true, or 0 if false.

.. c:function:: int32_t generic_list_get_count(struct generic_list_s* l)

   :c:member:`l` The generic list to obtain the count of elements.

   This function return the number of elements in the generic list.

.. c:function:: struct generic_list_s *new_generic_list(int32_t initial)

   :c:member:`initial` Initial capcity for a generic list.

   This function returns a generic list with a given initial capacity. Capacity dictates how much space the list will take. You don't need to
   worry about reallocation, but consider using small values for capacity when you know that the list will not grow that much. 

.. c:function:: stack* new_stack(int32_t initial)

   :c:member:`initial` Initial capcity for a stack.

   This function is just an alias for :c:func:`new_generic_list`.

.. c:function:: queue* new_queue(int32_t initial)

   :c:member:`initial` Initial capcity for a queue.

   This function is just an alias for :c:func:`new_generic_list`.

.. c:function:: void* set_element_with_type_at(list *l, void* obj, int16_t type, int32_t pos)

   :c:member:`l` List where to set the element.

   :c:member:`obj` New element to store in the list.

   :c:member:`type` The type of the new element.

   :c:member:`pos` The position of the element to be set.

   This function sets an already exiting element to a new one. A pointer to the old one is returned so the user can free the space used by it.

.. c:function:: void* set_element_at(list *l, void* obj, int32_t pos)

   :c:member:`l` List where to set the element.

   :c:member:`obj` New element to store in the list.

   :c:member:`pos` The position of the element to be set.

   This function does exactly the same as :c:func:`set_element_with_type_at`, but without the type feature.

.. c:function:: void insert_element_with_type_at(list* l, void* obj, int16_t type, int32_t pos)

   :c:member:`l` List where to insert the element.

   :c:member:`obj` New element to store in the list.

   :c:member:`type` The type of the new element.

   :c:member:`pos` The position of the element to be set.
   
   This function inserts an element at a given position with a given type. The list will be resized if necessary.

.. c:function:: void insert_element_at(list* l, void* obj, int32_t pos)

   :c:member:`l` List where to insert the element.

   :c:member:`obj` New element to store in the list.

   :c:member:`pos` The position of the element to be set.

   This function does exactly the same thing as :c:func:`insert_element_with_type_at`, but without the type feature.
   
.. c:function:: void sorted_insert_element_with_type(list* l, void* obj, int16_t type, int(*compare)(void* o1, int16_t type1, void* o2, int16_t type2))

   :c:member:`l` List where to insert the element.

   :c:member:`obj` New element to store in the list.

   :c:member:`type` The type of the new element.

   :c:member:`compare` A function pointer to a function that compares elements in the list.

   This function inserts an element into a list in sorted order, according to the function pointer passed as an argument.

.. c:function:: void append_element_with_type(list* l, void* obj, int16_t type)

   :c:member:`l` List where to append the element.

   :c:member:`obj` New element to store in the list.

   :c:member:`type` The type of the new element.

   This function inserts an element at the end of the list.

.. c:function:: void prepend_element_with_type(list* l, void* obj, int16_t type)

   :c:member:`l` List where to prepend the element.

   :c:member:`obj` New element to store in the list.

   :c:member:`type` The type of the new element.

   This function inserts an element at the beginning of the list.

.. c:function:: void add_element_with_type(list* l, void* obj, int16_t type)

   :c:member:`l` List where to add the element.

   :c:member:`obj` New element to store in the list.

   :c:member:`type` The type of the new element.

   This function adds an element to a list. It differs from :c:func:`prepend_element` and :c:func:`append_element` in the sense that the user doesn't need to know
   Where the element will be added.

.. c:function:: void add_element(list* l, void* obj)

   :c:member:`l` List where to add the element.

   :c:member:`obj` New element to store in the list.

   This function is exactly the same as :c:func:`add_element_with_type`, but without the type feature.

.. c:function:: void* get_element_at(const list* l, int32_t pos)

   :c:member:`l` List from where to get the element.

   :c:member:`pos` The element's position.

   This function returns the element stored at the given position.

.. c:function:: void* get_element_with_type_at(const list* l, int32_t pos, int16_t* type)

   :c:member:`l` List from where to get the element.

   :c:member:`pos` The element's position.

   :c:member:`type` An address to store the elements type.

   This function does the same as :c:func:`get_element_at`, but it also returns the element's type. This is stored in a location passed as the third argument
   to this function.

.. c:function:: int get_element_pos(const list* l, void* el)

   :c:member:`l` List from where to get the element's position.

   :c:member:`el` The element to look for in the list.

   This function returns the position of the element passed as an argument. If no element is found, then -1 is returned.

.. c:function:: int32_t remove_element(list *l, void* obj)

   :c:member:`l` List from where to remove the element.

   :c:member:`obj` The element to be removed.

   This function removes the first occurrence of a given element from a list. If the element doesn't exist then nothing will happen.

.. c:function:: int32_t remove_all(list *l, void* obj)

   :c:member:`l` List from where to remove the element.

   :c:member:`obj` The element to be removed.

   This function removes all occurrences of a given element from a list. If no element is found then nothing will happen.

.. c:function:: void* remove_element_at(list* l, int32_t pos)

   :c:member:`l` List from where to remove the element.

   :c:member:`pos` Position of the element to be removed.

   This function removes the element at a given position from a list.

.. c:function:: void enqueue_with_type(queue* q, void* obj, int16_t type)

   :c:member:`q` Queue where to enqueue the element.

   :c:member:`obj` Element to enqueue.

   :c:member:`type` The type of the element being enqueued.

   This function enqueues an element with a given type in a given queue.

.. c:function:: void enqueue(queue* q, void* obj)

   :c:member:`q` Queue where to enqueue the element.

   :c:member:`obj` Element to enqueue.

   This function is the same as :c:func:`enqueue_with_type`, but without the type feature.

.. c:function:: void* dequeue(queue* q)

   :c:member:`q` Queue from where to dequeue an element.

   This function dequeues an element from a given queue. If no element is present, then NULL is returned.

.. c:function:: void push_stack_type(stack* s, void* obj, int16_t type)

   :c:member:`s` Stack where to push the element.

   :c:member:`obj` Element to push onto the stack.

   :c:member:`type` The type of the element being push.

   This function pushes an element with a given type onto a given stack.

.. c:function:: void push_stack(stack* s, void* obj)

   :c:member:`s` Stack where to push the element.

   :c:member:`obj` Element to push onto the stack.

   This function is the same as :c:func:`push_stack_type`, but without the type feature.

.. c:function:: void* pop_stack(stack* s)

   :c:member:`s` Stack from where to pop an element.

   This function pops an element from a given stack. If no element is present, NULL is returned.

.. c:function:: list* remove_duplicates(list* l)

   :c:member:`l` List from where to remove duplicate elements.

   This function removes duplicate values based on their memory address.

.. c:function:: int16_t peek_element_type_at(list* l, int32_t pos)

   :c:member:`l` List from where to peek an element's type.

   :c:member:`pos` The element's position in the list.

   This function lets you see which type the element at a given position has. If no element is in that position the program will exit.

.. c:function:: int16_t peek_stack_type(stack *s)

   :c:member:`s` Stack from where to peek the element's type.

   This function is the same as :c:func:`peek_element_type_at`, but it applies only to stacks. This means that it will only check the element at the head of the stack.

.. c:function:: int16_t peek_queue_type(queue *s)

   :c:member:`s` Queue from where to peek the element's type.

   This function is the same as :c:func:`peek_stack_type`. However, it applies only to queues.

.. c:function:: struct generic_list_s *merge_lists(struct generic_list_s *l1, struct generic_list_s *l2)

   :c:member:`l1` Any list.

   :c:member:`l2` Any list.

   This function merges two list onto a single one. The list l2 will be append to l1. Beware that the input lists will be destroyed. The merged list is returned. If
   both lists are NULL, then NULL is returned.

.. c:function:: struct generic_list_s *duplicate_generic_list(const struct generic_list_s*)

   :c:member:`s` List to be duplicated.

   This function allocates and fills a new list with the same values as the given list. Only the buckets are duplicated, the elements on the list stay exactly the same.

.. c:function:: void destroy_generic_list(struct generic_list_s *s)

   :c:member:`s` List to be destroyed

   This function destroys a given list. Only the buckets get destroyed, the elements must be detroyed by the user.

   Example::

     #include <stdlib.h>
     #include <stdio.h>
     #include "stack.h"

     int main(){
        list* l = new_generic_list(2);
	int *a, *b;

	a = (int*) malloc(sizeof(int));
	b = (int*) malloc(sizeof(int));
	if(!a || !b){
	  printf("malloc failed\n");
	  return -1;
	}
	
	*a = 9;
	*b = 7;

	append_element(l, a, 0);
	append_element(l, b, 0);

	generic_list_iterator* it = new_generic_list_iterator(l);
	while(generic_list_iterator_has_next(it)){
	  int *c = (int*) generic_list_iterator_next(it);
	  free(c);
	}
	destroy_generic_list_iterator(it);
	
	destroy_generic_list(l);

	return 0;
     }

   You may compile it with

   .. code-block:: bash 

     gcc -o test <above_source_file> -I<folder path where stack.h is kept>

.. c:function:: generic_list_iterator* new_generic_list_iterator(struct generic_list_s* s)

   :c:member:`s` List to be iterated.

   This function creates an iterator for the given list.

.. c:function:: uint8_t generic_list_iterator_has_next(generic_list_iterator* i)

   :c:member:`i` A list iterator.

   This function checks if there are elements still to be iterated. If not 0 is returned, otherwise a value different from 0 is returned.

.. c:function:: void* generic_list_iterator_next(generic_list_iterator* i)

   :c:member:`i` A list iterator

   This function returns an element from the list and advances the iterator to the next position.

.. c:function:: void destroy_generic_list_iterator(generic_list_iterator* i)

   :c:member:`i` A list iterator

   This function frees a list iterator.

Generic Red Black Tree
----------------------
The file rbtree.c contains the source code for a generic red black tree. This data structure is capable of storing any kind of data and its defined in rbtree.h as:

.. c:type:: tree_root
.. c:type:: struct sroot

::

    typedef struct sroot{
       struct stree_node* root;
       void* (*key)(struct stree_node* node);
       int64_t (*compare)(void* keyA, void* keyB);
    }tree_root;

There are two things to notice here. First, this is only the root of the tree and points to the first tree_node of the rbtree. Second, it contains two function pointers. The first points to a function, which receives a :c:type:`tree_node` and should return a **pointer** to that node's key. The second is a pointer to a compare function, which compares two keys and should return:

- A negative integer if the first key is smaller than the second.
- 0 if the keys are the same.
- A positive integer if the first key is bigger than the second.

The compare function must be:

- Reflexive: Given an object a. compare(a, a) should always return 0.
- Symetric: Given two identical objects a and b, if compare(a,b) returns 0, then compare(b,a) must return 0.
- Transitive: Given three objects a, b, c. If compare(a,b) returns 0, and compare(b,c) returns 0, then compare(a,c) must return 0.
- Consistent: Multiple invocations of compare on the same objects in the same other, must return always the same value.

These properties should hold even if objects aren't equal.

The compare and key function pointers must be provided by the user. Why do we need these pointers? Because the data stored in the rbtree can be anything, but we still need to know how to sort it. Nevertheless, if you wish to use this data structure as a container and don't care how things are sorted, you can always use the method :c:func:`new_simple_rbtree`.

Each node in an rbtree is called a tree_node and is defined in rbtree.h as:

.. c:type:: tree_node
.. c:type:: struct stree_node

::

     typedef struct stree_node{
        void* node;

	uint8_t color;
  
        struct stree_node* parent;
  	struct stree_node* left;
  	struct stree_node* right;
      }tree_node;

There's not much to say about this structure, the only thing relevant is the field ``node``, which is used to store the actual data. The other fields are used to keep the rbtree intact.

Finally, there's one more structure, which is defined in rbtree.h as:

.. c:type:: tree_iterator
.. c:type:: struct siterator

::

      typedef struct siterator{
         struct stree_node* current;
      }tree_iterator;

As the name implies, this structure is an iterator to the tree nodes.

As a final note, remember that we provide functions to destroy our structures, but the actual data must be destroyed by you. Do not use iterators for this purpose.

Function description
^^^^^^^^^^^^^^^^^^^^

.. c:function:: tree_root* new_simple_rbtree()

   This function creates an rbtree, which sorts the data acording to its memory pointer. This function should be used when you just want the rbtree to behave as a container,
   but you still need O(log(n)) when accessing the data. Keep in mind that in order to retreive the stored data, you need to know it's memory pointer.
   
   The return value is a tree_root structure.

.. c:function:: tree_root* new_rbtree(void* (*key_function_pointer)(struct stree_node* node), int64_t (*compare_function_pointer)(void* keyA, void* keyB))

   :c:member:`key_function_pointer` A function that should return the address of the node's key.

   :c:member:`compare_function_pointer` A function that should compare two keys and return values as described above. It receives the addresses of each key.

   This function creates an rbtree, which sorts the data according to the given functions. The following example shows how to create an rbtree to store integers.

   Example::

     #include <stdio.h>
     #include "rbtree.h"

     void* key_address(tree_node* node){
        return node->node;
     }

     int64_t compare_integers(void* keyA, void* keyB){
        return *((int *) keyA) - *((int *) keyB);
     }

     int main(){
        tree_root* rbtree = new_rbtree(& key_address, & compare_integers);
	return 0;
     }

   You may compile it with

   .. code-block:: bash 

     gcc -o test <above_source_file> -I<folder path where rbtree.h is kept>

.. c:function:: void* rb_tree_insert(tree_root* root, void* node)

   :c:member:`root` A pointer to the tree root where to insert the data represented by ``node``.

   :c:member:`node` A pointer to the data, which will be inserted in the tree.

   As the name implies this function inserts data into the rbtree. In the eventuality that the inserted value is already in the tree, it will be replaced and a pointer to the older value is returned. This is done so the user can free the space stored by that data. The following example shows how to insert integers in an rbtree.

   Example::

     #include <stdio.h>
     #include "rbtree.h"

     void* key_address(tree_node* node){
        return node->node;
     }

     int64_t compare_integers(void* keyA, void* keyB){
        return *((int *) keyA) - *((int *) keyB);
     }

     int main(){
        tree_root* rbtree = new_rbtree(& key_address, & compare_integers);
	int a = 9, b = 6, c = 10, d = 6;
	
	rb_tree_insert(rbtree, &a);
        rb_tree_insert(rbtree, &b);
	rb_tree_insert(rbtree, &c);
	int older = *((int *) rb_tree_insert(rbtree, &d));
	
	//don't free older because it was "alloched" by the compiler.
	printf("Found a %d already stored in the tree.\n", older);

	return 0;
     }

   You may compile it with

   .. code-block:: bash 

     gcc -o test <above_source_file> -I<folder path where rbtree.h is kept>

.. c:function:: void* rb_tree_delete(tree_root* root, void* key)

   :c:member:`root` A pointer to the tree root where to delete the data with key pointed by ``key``.

   :c:member:`key` A pointer to the key of the node to be deleted.

   This function deletes a node from an rbtree. If a node with a key equal to the one pointed by ``key`` does not exist, NULL will be return. However, if such a node is found, then a pointer to the data is returned. This is done so the user can free the space used by that data. The following example shows how to use this function on an rbtree that stores integers.

   Example::

     #include <stdio.h>
     #include "rbtree.h"

     void* key_address(tree_node* node){
        return node->node;
     }

     int64_t compare_integers(void* keyA, void* keyB){
        return *((int *) keyA) - *((int *) keyB);
     }

     int main(){
        tree_root* rbtree = new_rbtree(& key_address, & compare_integers);
	int a = 9, b = 6, c = 10;
	
	rb_tree_insert(rbtree, &a);
        rb_tree_insert(rbtree, &b);
	rb_tree_insert(rbtree, &c);
	
	int d = 10, stored;
	stored = *((int *) rb_tree_delete(rbtree, &d));
	
	//don't free stored because it was "alloched" by the compiler.
	printf("Found a %d stored in the tree.\n", stored);		

	return 0;
     }

   You may compile it with

   .. code-block:: bash 

     gcc -o test <above_source_file> -I<folder path where rbtree.h is kept>



.. c:function:: void* search_rbtree(tree_root root, void* key)

   :c:member:`root` The root of the tree where to perform the search.

   :c:member:`key` A pointer to the key of the node to be searched.

   This function searchs an rbtree for a node. It returns NULL if nothing is found, or the data stored in the tree with a key equal to the value pointed by ``key``. The following example shows how to search a tree that stores integers.

   Example::

     #include <stdio.h>
     #include "rbtree.h"

     void* key_address(tree_node* node){
        return node->node;
     }

     int64_t compare_integers(void* keyA, void* keyB){
        return *((int *) keyA) - *((int *) keyB);
     }

     int main(){
        tree_root* rbtree = new_rbtree(& key_address, & compare_integers);
	int a = 9, b = 6, c = 10;
	
	rb_tree_insert(rbtree, &a);
        rb_tree_insert(rbtree, &b);
	rb_tree_insert(rbtree, &c);
	
	int d = 10, stored;
	stored = *((int *) search_rbtree(*rbtree, &d));
	
	printf("Found a %d stored in the tree.\n", stored);		

	return 0;
     }

   You may compile it with

   .. code-block:: bash 

     gcc -o test <above_source_file> -I<folder path where rbtree.h is kept>

.. c:function:: void destroy_rbtree(tree_root* root)

   :c:member:`root` A pointer to the tree to be destroyed.

   This function destroys an rbtree. Note that this doesn't free the user stored data. The following example shows how to use this in a tree that stores integers.

   Example::

     #include <stdio.h>
     #include "rbtree.h"

     void* key_address(tree_node* node){
        return node->node;
     }

     int64_t compare_integers(void* keyA, void* keyB){
        return *((int *) keyA) - *((int *) keyB);
     }

     int main(){
        tree_root* rbtree = new_rbtree(& key_address, & compare_integers);
	int a = 9, b = 6, c = 10;
	
	rb_tree_insert(rbtree, &a);
        rb_tree_insert(rbtree, &b);
	rb_tree_insert(rbtree, &c);
	
	destroy_rbtree(rbtree);
	//We do not need to free the stored data because it was "alloched" by the compiler.

	return 0;
     }

   You may compile it with 

   .. code-block:: bash 

     gcc -o test <above_source_file> -I<folder path where rbtree.h is kept>

   Notice that running the command 

   .. code-block:: bash 

      valgrind --show-reachable=yes --leak-check=full ./test

   produces the ouput::

      ==1188== HEAP SUMMARY:
      ==1188==     in use at exit: 0 bytes in 0 blocks
      ==1188==   total heap usage: 4 allocs, 4 frees, 72 bytes allocated
      ==1188== 
      ==1188== All heap blocks were freed -- no leaks are possible

   Which means that there are no memory leaks and you should always use this function to free the space stored by any rbtree you use.

.. c:function:: tree_iterator* new_tree_iterator(tree_root* root)

   :c:member:`root` A pointer to a tree root, which the iteration will be performed.

   This function creates an iterator to an rbtree. Note that when you create an iterator, you should not insert or delete nodes from the tree before the iteration is over. Otherwise, the behaviour of the program will be unpredictable. It returns pointer to the created iterator. The following example shows how to create an iterator for a tree that stores integers.

   Example::

     #include <stdio.h>
     #include "rbtree.h"

     void* key_address(tree_node* node){
        return node->node;
     }

     int64_t compare_integers(void* keyA, void* keyB){
        return *((int *) keyA) - *((int *) keyB);
     }

     int main(){
        tree_root* rbtree = new_rbtree(& key_address, & compare_integers);
	int a = 9, b = 6, c = 10;
	
	rb_tree_insert(rbtree, &a);
        rb_tree_insert(rbtree, &b);
	rb_tree_insert(rbtree, &c);
	
	tree_iterator* it = new_tree_iterator(rbtree);

	return 0;
     }

   You may compile it with 

   .. code-block:: bash 

     gcc -o test <above_source_file> -I<folder path where rbtree.h is kept>

.. c:function:: uint8_t tree_iterator_has_next(tree_iterator* it)

   :c:member:`it` A tree iterator created by calling :c:func:`new_tree_iterator`.

   This function returns 1 if there are more elements in the tree to be iterated. The following code shows a simple usage of this function.

   Example::

     #include <stdio.h>
     #include "rbtree.h"

     void* key_address(tree_node* node){
        return node->node;
     }

     int64_t compare_integers(void* keyA, void* keyB){
        return *((int *) keyA) - *((int *) keyB);
     }

     int main(){
        tree_root* rbtree = new_rbtree(& key_address, & compare_integers);
	int a = 9, b = 6, c = 10;
	
	rb_tree_insert(rbtree, &a);
        rb_tree_insert(rbtree, &b);
	rb_tree_insert(rbtree, &c);
	
	tree_iterator* it = new_tree_iterator(rbtree);

	if(tree_iterator_has_next(it))
	  printf("There are still elements to be iterated.\n");
	return 0;
     }

   You may compile it with 

   .. code-block:: bash 

     gcc -o test <above_source_file> -I<folder path where rbtree.h is kept>

.. c:function:: void* tree_iterator_next(tree_iterator* it)

   :c:member:`it` A tree iterator created by calling :c:func:`new_tree_iterator`.

   This functions returns the current element pointed by iterator ``it`` and advances to the next element in the iteration. This function should be used with :c:func:`tree_iterator_has_next`. Note that there is **no** guaranty about the order of iteration. The following code shows how to use it.

   Example::

     #include <stdio.h>
     #include "rbtree.h"

     void* key_address(tree_node* node){
        return node->node;
     }

     int64_t compare_integers(void* keyA, void* keyB){
        return *((int *) keyA) - *((int *) keyB);
     }

     int main(){
        tree_root* rbtree = new_rbtree(& key_address, & compare_integers);
	int a = 9, b = 6, c = 10;
	
	rb_tree_insert(rbtree, &a);
        rb_tree_insert(rbtree, &b);
	rb_tree_insert(rbtree, &c);
	
	tree_iterator* it = new_tree_iterator(rbtree);

	while(tree_iterator_has_next(it)){
	  int d = *((int *) tree_iterator_next(it));
	  printf("%d\n", d);
	}
	return 0;
     }

   You may compile it with 

   .. code-block:: bash 

     gcc -o test <above_source_file> -I<folder path where rbtree.h is kept>

.. c:function:: void destroy_iterator(tree_iterator* it)

   :c:member:`it` A tree iterator created by calling :c:func:`new_tree_iterator`.

   This function frees the iterator pointed by ``it``. The following example shows how to use it.

   Example::

     #include <stdio.h>
     #include "rbtree.h"

     void* key_address(tree_node* node){
        return node->node;
     }

     int64_t compare_integers(void* keyA, void* keyB){
        return *((int *) keyA) - *((int *) keyB);
     }

     int main(){
        tree_root* rbtree = new_rbtree(& key_address, & compare_integers);
	int a = 9, b = 6, c = 10;
	
	rb_tree_insert(rbtree, &a);
        rb_tree_insert(rbtree, &b);
	rb_tree_insert(rbtree, &c);
	
	tree_iterator* it = new_tree_iterator(rbtree);

	while(tree_iterator_has_next(it)){
	  int d = *((int *) tree_iterator_next(it));
	  printf("%d\n", d);
	}
	destroy_iterator(it);

	destroy_rbtree(rbtree);
	return 0;
     }

   You may compile it with 

   .. code-block:: bash 

     gcc -o test <above_source_file> -I<folder path where rbtree.h is kept>

   .. code-block:: bash 

      valgrind --show-reachable=yes --leak-check=full ./test

   produces the ouput::

      ==4432== HEAP SUMMARY:
      ==4432==     in use at exit: 0 bytes in 0 blocks
      ==4432==   total heap usage: 5 allocs, 5 frees, 76 bytes allocated
      ==4432== 
      ==4432== All heap blocks were freed -- no leaks are possible

   Which means that there are no memory leaks and you should always use this function when iterating.
