---
layout: post
keywords: Learn Python
description: Learn Python Exercise
title: "Learn Python Exercise Six"
categories: Python
tags: Python
group: archive
icon: key
---


Data Structure:

This session show how to use the list, set, tuple, dict, looping etc.

learn_datastructure.py

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-
    
    __author__ = "Huang Moyue"
    __version__ = "0.01"
    __date__ = "09/21/2014"
    __copyright__ = "Copyright 2014 Huang Moyue"
    
    """
    Data Type
    List example:
    ["list1","list2","list3"]
    
    list.append(x)
    Add an item to the end of the list; equivalent to a[len(a):] = [x].
    
    list.extend(L)
    Extend the list by appending all the items in the given list; equivalent to a[len(a):] = L.
    
    list.insert(i, x)
    Insert an item at a given position. The first argument is the index of the element before which to insert, so a.insert(0, x) inserts at the front of the list, and a.insert(len(a), x) is equivalent to a.append(x).
    
    list.remove(x)
    Remove the first item from the list whose value is x. It is an error if there is no such item.
    
    list.pop([i])
    Remove the item at the given position in the list, and return it. If no index is specified, a.pop() removes and returns the last item in the list. (The square brackets around the i in the method signature denote that the parameter is optional, not that you should type square brackets at that position. You will see this notation frequently in the Python Library Reference.)
    
    list.index(x)
    Return the index in the list of the first item whose value is x. It is an error if there is no such item.
    
    list.count(x)
    Return the number of times x appears in the list.
    
    list.sort(cmp=None, key=None, reverse=False)
    Sort the items of the list in place (the arguments can be used for sort customization, see sorted() for their explanation).
    
    list.reverse()
    Reverse the elements of the list, in place.
    """
    def listType(params):
        print("print list items below:")
        
        for i in params:
            print i, len(i)
        
    def my_reverse(x,y):
        if x > y:
            return -1
        if x < y:
            return 1
        return 0
    
    def filter_func(x):
        return x%2 != 0 and x%3 !=0
    
    def reduce_add(x,y):
        return  x + y
    
    def map_add(x):
        return x + x
    
    def char2int(str):
        return {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}[str]
    
    def str2int(str):
        return reduce(lambda x, y: x * 10 + y, map(char2int, str))
    
    """
    Tuple:
    tup = (1,2,3,4,5)
    Very like to list. But tuple is immutable!
    """
    def show_tuple_structure():
        
        tup_one = 1,2,3,4,5
        tup_two = 1235,'tuple_item'
        tup_three = tup_one, tup_two
        tup_list = ([1,2,3],[5,6,7])
        tup_four = 'hello_tuple',
        
        print "Print tuple:"
        print tup_one
        print tup_two
        print tup_three
        print tup_list
        print tup_four
    
    """
    Set:
    A set is an unordered collection with no duplicate elements. 
    Basic uses include membership testing and eliminating duplicate entries.
    Set objects also support mathematical operations like union, intersection, difference, and symmetric difference.
    set(list)
    """
    def show_set_structure():
        print "set:"
        basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
        print basket
        set(basket)
        print basket
        
        set_a = set('abracadabra')
        set_b = set('alacazam')
        # use operations, monitor the set operations
        print "set_a, set_b:",set_a, set_b
        print "set_a - set_b:", set_a - set_b
        print "set_a | b:", set_a|set_b
        print "set_a & set_b:", set_a&set_b
        print "set_a ^ set_b:", set_a ^ set_b
        
    
    """
    Data type
    Dictionary
        It define key-value relationship. {key, value}
        Example:
        {"server":"huangmoyue", "pwd":"12345"}
    """
    
    def dictionaryType():
        print("dic type:")
        
        dic_a = {"Server":"huangmoyue","Pwd":"861117"}
        dic_b = {'jack': 4098, 'sape': 4139}
        
        print dic_a.keys()
        print dic_b["jack"]
        
        for key, value in dic_a.items():
            print(";".join((key, value)))
            
            
testing_datastructure.py

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-
    
    __author__ = "Huang Moyue"
    __version__ = "0.01"
    __date__ = "09/21/2014"
    __copyright__ = "Copyright 2014 Huang Moyue"
    
    import unittest
    from unittest import TestCase
    
    from learnpy.learning_datastructure import listType
    from learnpy.learning_datastructure import my_reverse
    from learnpy.learning_datastructure import filter_func
    from learnpy.learning_datastructure import reduce_add
    from learnpy.learning_datastructure import map_add
    from learnpy.learning_datastructure import str2int
    from learnpy.learning_datastructure import show_tuple_structure
    from learnpy.learning_datastructure import show_set_structure
    from learnpy.learning_datastructure import dictionaryType
    
    class Test_DataType(TestCase):
        def test_listtype(self):
            test_list = ["list1","list2","list3"]
            listType(test_list)
        
        def test_list_inernal_function(self):
            test_list = [66.25, 333, 333, 1, 1234.5]
    
            #append 100 behind the test_list
            test_list.append(100)
            print "append 100 behind:", test_list
    
            #extend 
            ex_list = ["extend_item1", "extend_item2"]
            test_list.extend(ex_list)
            print "extend two items:", test_list
            
            #insert
            test_list.insert(3,"insert_item")
            print "insert one item:", test_list
    
            #remove
            test_list.remove(333)
            print "remove 333 from test_list:",test_list
            
            #pop
            test_list.pop()
            print "Use pop funtion:",test_list
            test_list.pop(0)
            print "Use pop to remove 66.25:",test_list
            
            #index
            print "Show index(0) position:", test_list.index(1)
            
            #count
            print "Show the extend_item1 position:", test_list.count("extend_item1")
    
            #sort
            print "Show before:",test_list
            test_list.sort()
            print "Show sort after:",test_list
            
            #sorted use user define function
            print "Show before:", test_list
            sorted(test_list,my_reverse)
            print "Show my own sort function:",sorted(test_list,my_reverse)
    
        #filter(function, sequence) returns a sequence consisting of those items from the sequence for which function(item) is true. If sequence is a string or tuple, the result will be of the same type; otherwise, it is always a list. 
        #map(function, sequence) calls function(item) for each of the sequence’s items and returns a list of the return values. 
        #reduce(function, sequence) returns a single value constructed by calling the binary function function on the first two items of the sequence, then on the result and the next item, and so on.
        def test_functional_tools(self):
            print "Show how to use the filter:", filter(filter_func,range(2,25))
            print "Show how to use the reduece:",reduce(reduce_add, [1, 3, 5, 7, 9])
            print "Show how to use the map:", map(map_add, [1, 2, 3, 4, 5, 6, 7, 8, 9])
            print "Show how the map and reduece work together:", str2int("123456")
            
        
        #List comprehensions
        def test_list_comprehensions(self):
            #The tranditional way to create the list
            
            print "List Comprehensions:"
            test_tra_list = []
            for x in range(10):
                test_tra_list.append(x)
            print test_tra_list
            
            #Obtain the same value in list
            test_comprehensions_list = [x for x in range(10)]
            print test_comprehensions_list
            
            #Another case
            test_comb_list = []
            for x in [1,2,3]:
                for y in [3,1,4]:
                    if x != y:
                        test_comb_list.append((x,y))
            
            print test_comb_list        
            
            test_pre_comb_list = [(x,y) for x in [1,2,3] for y in [2,1,4] if x != y]
            print test_pre_comb_list
            
        #Nested List Comprehensions
        def test_list_nested_compreh(self):
            print "nested List comprehension:"
            matrix = [
                      [1,2,3,4],
                      [5,6,7,8],
                      [9,10,11,12],
                      [13,14,15,16]
                      ]
            #Some matrix tranform methods:
            tran_one = [[row[i] for row in matrix] for i in range(4)]
            print "tran_one:", tran_one
            
            tran_two = zip(*matrix)
            print "tran_two:", tran_two
            
            tran_three = []
            for i in range(4):
                tran_three.append([row[i] for row in matrix])
            print "tran_three", tran_three
            
            tran_four = []
            for i in range(4):
                tran_four_row = []
                for row in matrix:
                    tran_four_row.append(row[i])
                tran_four.append(tran_four_row)
            print "tran_four:", tran_four
            
        #Use del to remove the list item
        def test_del(self):
            print "del item:"
            test_list = ["list1","list2","list3"]
            print test_list
            del test_list[0]
            print test_list
            
            del test_list       
            
        # tuple
        def test_tuple(self):
            show_tuple_structure()
            
        #set
        def test_set(self):
            show_set_structure()
            #set comprehensions
            test_set_compre = {x for x in 'abracadabra' if x not in 'abc'}
            print "set comprehensions:",test_set_compre
        
        #Dict
        def test_dict(self):
            dictionaryType()
            #The dict() constructor builds dictionaries directly from sequences of key-value pairs
            print "Use the dict() constructor to build the dict:"
            print dict(sape=4139, guido=4127, jack=4098)
            print dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])
            print {x: x**2 for x in (2, 4, 6)}
            
        #Looping Techniques
        def test_looping(self):
            print "test Loop:"
            print "Use Enumerate:"
            for i, v in enumerate(['a','b','c']):
                print i, v
            
            print "show two or more sequence one time:"
            questions = ['name', 'quest', 'favorite color']
            answers = ['lancelot', 'the holy grail', 'blue']
            for q, a in zip(questions, answers):
                print "what's your {0}? It is {1}.".format(q,a)
            
            print "Show reverse sequence: "
            for i in reversed(range(1,20,2)):
                print i
                
            print "Show sorted sequence:"
            basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
            for s in sorted(set(basket)):
                print s
            
            print "Show the dict item's key-value:"
            dic_a = {"Server":"huangmoyue","Pwd":"861117"}
            for k, v in dic_a.iteritems():
                print k, v
                print ";".join((k,v))
    
        #slicing operations
        def test_slicing(self):
            print "Use slicing:"
            words = ['cat', 'window', 'defenestrate']
            print words[:]
            print words[2:3]
            print words[-2:-1]
    
    
    if __name__ == '__main__':
        unittest.main()